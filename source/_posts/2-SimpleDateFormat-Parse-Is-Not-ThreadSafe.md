---
title: JAVA SimpleDateFormat Not ThreadSafe Python In Ubuntue Usage 
date: 2019-02-20 23:58:45
tags:
	- java
	- concurrent 
---
# JAVA SimpleDateFormat Not ThreadSafe

SimpleDateFormat is not threadsafe, so when we use it in the a multiple threads environment, be careful.

Follow is a JAVA class to demo the error:

```sh
public class SimpleDateFormatThreadUnsafetyExample {

    private static SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd'T'HH:mm:ss");  ## the evil here,it is not thread safe

    public static void main(String[] args) {
        String dateStr = "2018-06-22T10:00:00";

        ExecutorService executorService = Executors.newFixedThreadPool(10);
        Runnable task = () -> parseDate(dateStr);

        for (int i = 0; i < 100; i++) {
            executorService.submit(task);
        }

        executorService.shutdown();
    }

    private static void parseDate(String dateStr) {
        try {
            Date date = simpleDateFormat.parse(dateStr);
            System.out.println("Successfully Parsed Date " + date);
        } catch (ParseException e) {
            System.out.println("ParseError " + e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

Run it, let's see what happeded?Following errors: 

```sh
java.lang.NumberFormatException: For input string: "20182018E"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
	at java.lang.Long.parseLong(Long.java:589)
	at java.lang.Long.parseLong(Long.java:631)
	at java.text.DigitList.getLong(DigitList.java:195)
	at java.text.DecimalFormat.parse(DecimalFormat.java:2084)
	at java.text.SimpleDateFormat.subParse(SimpleDateFormat.java:1869)
	at java.text.SimpleDateFormat.parse(SimpleDateFormat.java:1514)
	at java.text.DateFormat.parse(DateFormat.java:364)
	at io.qkits.dailyissues.concurrence.SimpleDateFormatThreadUnsafetyExample.parseDate(SimpleDateFormatThreadUnsafetyExample.java:28)
	at io.qkits.dailyissues.concurrence.SimpleDateFormatThreadUnsafetyExample.lambda$main$0(SimpleDateFormatThreadUnsafetyExample.java:17)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)
java.lang.NumberFormatException: multiple points
	at sun.misc.FloatingDecimal.readJavaFormatString(FloatingDecimal.java:1890)
	at sun.misc.FloatingDecimal.parseDouble(FloatingDecimal.java:110)
	at java.lang.Double.parseDouble(Double.java:538)
	at java.text.DigitList.getDouble(DigitList.java:169)
	at java.text.DecimalFormat.parse(DecimalFormat.java:2089)
	at java.text.SimpleDateFormat.subParse(SimpleDateFormat.java:2162)
	at java.text.SimpleDateFormat.parse(SimpleDateFormat.java:1514)
	at java.text.DateFormat.parse(DateFormat.java:364)
	at io.qkits.dailyissues.concurrence.SimpleDateFormatThreadUnsafetyExample.parseDate(SimpleDateFormatThreadUnsafetyExample.java:28)
	at io.qkits.dailyissues.concurrence.SimpleDateFormatThreadUnsafetyExample.lambda$main$0(SimpleDateFormatThreadUnsafetyExample.java:17)
	at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
	at java.util.concurrent.FutureTask.run(FutureTask.java:266)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
	at java.lang.Thread.run(Thread.java:748)

```

So move the SimpleDateFormat from static field to method level, it can solve this issue,or make the pareDate function sync.

## All the SimpleDateFormat method is not not thread safe?

if I change the pareDate method as follow:

```java
  private static void parseDate(String dateStr) {
        try {
            Date date = simpleDateFormat.parse(dateStr);
            System.out.println("Successfully Parsed Date " + date);
        } catch (ParseException e) {
            System.out.println("ParseError " + e.getMessage());
        } catch (Exception e) {
            e.printStackTrace();
        }

        System.out.println(formatDate(new Date()));
    }

    private static String formatDate(Date date){
        return simpleDateFormat.format(date);
    }
```

re-run it and even add more threads to run it, there is no error?
Why and what happened? Need to dig it more deeper.
