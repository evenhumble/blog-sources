---
title: Unit Testing Intro
date: 2018-07-18 22:35:49
tags:
    - ut
    - justdoit
---

# Just DO It - Unit Testing

This is first blog for ***just do it*** series. The purpose for the ***just do it*** series is quit personal. As an aged tester, I have tried a bunch of different testing. And obviously,I am good at some types, and only have concepts in some other types.But There is no notes taken for these, sometimes it bothers me a lot when I need to explain or demo. Anthoer main reason is that I believe that when you want to test something, you need to understand what you test, it is not only the superficial workflow or business logic, but also some techincal inside. ***just do it*** is for all the testings or components I have  experienced and learned. It is like footprints of my long enough tester jouney. And Just Do it is more than any words.

ps: for learning English, I decided to write it in English at first.

## Unit Testing Paradox

There are many blogs out there claiming Unit Testing is such a good thing for improve product quality,and it is quick and cheap. Some Evangelists even claimed unit testing plays very import role in agile,CI/CD process. But in reality, writing unit testing is very rare in a large amount of companies here China. Why? Why good thing doesn't happen? It is quick,improving quality, more important is cheap? Why so many people/companies don't buy in? I don't know, but I want to dive it for seeking the reason.

## JUnit Try

JUnit is a very popular unit testing framework, so Let me try from this.

### Build a Simple Demo Application

But hold on,what application is for testing? OK, Let me build to simple ToDoApplication. What Features are included:

- Create ToDoItem : create a ToDoItem which status is TODO
- Update ToDoItme: update a ToDoItem 
- Delete ToDoItem: set ToDoItem status to deleted
- Complete ToDoItem: set ToDoItem status to completed

I try to make it as simple as possible, so the demonstrate these futures, I create three classes:

- ToDOItem: it is an entity, which is used to save to database. Just treat it as a database table
- ToDoItemDTO: it is a DTO entity, treat is as input
- ToDoItemService: do the transformation, transfer ToDoItemDTO to ToDoItem

Here is very simple codes:

```java
public class ToDoItem {
    private String id;
    private String desc;
    private String details;
    private int status;
    private Long userId;
}
```

```java
public class ToDoItemDTO  {
    private String id;
    private String desc;
    private String details;
    private int status=ToDoStatusEnum.TODO.getStatusId();
    private Long userId;
}
```

```java
public class ToDoService {

    public ToDoItem createToDoItem(ToDoItemDTO dto){
        ToDoItem toDoItem = new ToDoItem();
        toDoItem.setId(UUIDGenerator.generateToDoId());
        toDoItem.setStatus(dto.getStatus());
        toDoItem.setDesc(dto.getDesc());
        toDoItem.setDetails(dto.getDetails());
        toDoItem.setUserId(dto.getUserId());
        return toDoItem;
    }

    public ToDoItem updateToDoItem(ToDoItemDTO dto){
        return createToDoItem(dto);
    }
    //todo: should make todo item statue to DELETED
    public ToDoItem deleteToDoItem(ToDoItemDTO dto){
        return createToDoItem(dto);
    }

   //todo: should make todo item statue to Completed
    public ToDoItem completeToDoItem(ToDoItemDTO dto){
        return createToDoItem(dto);
    }
}

```

```java
public enum ToDoStatusEnum {
    TODO(0),WIP(1),DONE(2),PENDING(3),DELETED(4);

    public int getStatusId() {
        return statusId;
    }

    private final int statusId;

    ToDoStatusEnum(int statusId) {
        this.statusId= statusId;
    }
}

```
### Let's create first JUNIT Test cases

It is obviously the input is an instance of ToDoDTO, and the output is ToDoItem and the most important part to test is ToDoService. So first thing first, here is the first one test case for ToDoServicce, to test create todoitem successfully, and the content in DTO should be same as the new created ToDoItem.

```java
import org.junit.Assert;
import org.junit.BeforeClass;
import org.junit.Test;

public class ToDoServiceTest {
  
    @Test
    public void testCreateToDoItem_successful() {
        ToDoService toDoService; = new ToDoService();
        ToDoItemDTO dto = new ToDoItemDTO();
        dto.setDesc("test todo");
        dto.setDetails("test todo details");
        ToDoItem toDoItem =toDoService.createToDoItem(dto);
        Assert.assertEquals(toDoItem.getDesc(),"test todo");
        Assert.assertEquals(toDoItem.getDetails(),"test todo details");
        Assert.assertEquals(toDoItem.getStatus(),ToDoStatusEnum.TODO.getStatusId());

    }
}
```

Here I only use two JUNIT elements, these two elements will be used many many times, if you are writing test codes.

- @Test: annotation test mark the method as test method
- Assert.assertEquals: compare the expected value and actual value

It is quit straightforward, when you test something, give the input, process it, and compare the output to the expected value, if meet, pass, otherwise failed.

### More cases

After Complete the first case, let's do more. We want to test more functionalities, like update,delete, complete todoitem. Here you go:

```java
public class ToDoServiceTest {

    public static ToDoService toDoService;
    public static  ToDoItemDTO dto ;
    @BeforeClass
    public static void setupToDoService(){
        toDoService = new ToDoService();
        dto = new ToDoItemDTO();
    }
    @Test
    public void testCreateToDoItem_successful() {

        dto.setDesc("test todo");
        dto.setDetails("test todo details");
        ToDoItem toDoItem =toDoService.createToDoItem(dto);
        Assert.assertEquals(toDoItem.getDesc(),"test todo");
        Assert.assertEquals(toDoItem.getDetails(),"test todo details");
        Assert.assertEquals(toDoItem.getStatus(),ToDoStatusEnum.TODO.getStatusId());

    }

    @Test
    public void updateToDoItem_successful() {
        dto.setDesc("update");
        dto.setDetails("update");
        ToDoItem toDoItem =toDoService.createToDoItem(dto);
        Assert.assertEquals(toDoItem.getDesc(),"update");
        Assert.assertEquals(toDoItem.getDetails(),"update");
    }

    @Test
    public void deleteToDoItem() {
        ToDoItem toDoItem =toDoService.deleteToDoItem(dto);
        Assert.assertEquals(toDoItem.getStatus(),ToDoStatusEnum.DELETED.getStatusId());
    }

    @Test
    public void completeToDoItem() {
        ToDoItem toDoItem =toDoService.deleteToDoItem(dto);
        Assert.assertEquals(toDoItem.getStatus(),ToDoStatusEnum.DELETED.getStatusId());
    }
}
```

In above codes, I use a new junit annonations:

- @BeforeClass

@BeforeClass is invoked before the test class initialized. So the method and should be static. Here why I used @BeforeClass is because I want to resue the toDoService accross all the test methods. So it should be a static.

And another Junit annotation @Before is simmilar with @BeforeClass.
It is invoked before every test method. Here is a sample codes to show it:

```java
public class JunitAnnotationDemo {

    String testString="First";

    @Before
    public void setUp(){
        testString = this.testString + ",setup";
        System.out.println(this);
    }

    @Test
    public void testTestString_First(){
        System.out.println(testString);
    }

    @Test
    public void testTestString_Second(){
        System.out.println(testString);
    }
}
```

From the result of System.out.println(this), actually JUNIT use different instance to run the test methods

```java
io.hedwig.justdoit.demo.JunitAnnotationDemo@17d10166
First,setup
io.hedwig.justdoit.demo.JunitAnnotationDemo@4f8e5cde
First,setup
```

### Run the tests

Back to the ToDoItem unit tests. Run the ToDoServiceTest, there are several errors, here is an example for deleteToDoItem: 

```java
java.lang.AssertionError: 
Expected :0
Actual   :4
 <Click to see difference>
```

The target to unit test is to make sure you codes pass the tests quickly. Look back to ToDoService, it is obviously, deleteToDoItem should set the ToDoItem status to DELETE, but the function is omitted. So add the codes to fix it, and re-run the tests to make it all passed. 

## Unit Test Thinking

Actully this process is as known as TDD (Test Driven Development). Before actually complete the real working codes, write test codes first. The purpose of these test codes, I think they could be :

- Features Definition
- Task Tracking Tool
- Boundary Checking Tool

Why? I think in reality, there are many undocumented features in PRD, no one actually mantained it. Unit Test documents these features.

Also if writes test codes before complete the real codes, then you pass test methods one by one, that means developer complete the sub-tasks one by one, so It is Task Tracking tool. Imaging that, if you want to complete 10 sub-tasks, but you are interruptted every hour by meetings or chattings. Every time develop want to find where to start, just run the unit tests, then start from the failed tests.

There are always boundary cases, write into codes to check, it helps develop to find bugs, and also it could be checked if develop have some new changes.

## What should be paid for Unit Tests

Above cases looks so fascination, Unit Tests could tracking features,tasks, and check the codes when changes happened to ensure your changes are correct. But Why developers still don't want to write the Unit tests?  Let's just counting the lines of code in above examples, the truth is that the test codes are more lines than real codes. It is not joke, it happened. So I can understand why developer don't want to write unit tests, maybe the workload!!

Understand what the develop thoughts are, it is also one of targets in ***just do it*** series, try it and then find where the issues are.

Because write testing codes need to take more times, pick what to test and use less test codes to achieve coverages is more critical and practical. 


[code repo](https://github.com/evenhumble/justdo-it/tree/master/ut-cases)
