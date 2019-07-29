# Selenium Guide

[elemental-selenium-tips]

- upload files // https://saucelabs.com/resources/articles/selenium-file-upload
- download a file
```java
  FirefoxProfile profile = new FirefoxProfile();
        profile.setPreference("browser.download.dir", folder.getAbsolutePath());
        profile.setPreference("browser.download.folderList", 2);
        profile.setPreference("browser.helperApps.neverAsk.saveToDisk",
                "image/jpeg, application/pdf, application/octet-stream");
        profile.setPreference("pdfjs.disabled", true);
        driver = new FirefoxDriver(profile);
```
- work with frame
```java
        driver.switchTo().frame("frame-top");

```
- swith window

```java
  Object[] allWindows = driver.getWindowHandles().toArray();
        driver.switchTo().window(allWindows[0].toString());
```

- select drop down

```java
 WebElement dropdownList = driver.findElement(By.id("dropdown"));
        List<WebElement> options = dropdownList.findElements(By.tagName("option"));
        for (int i = 0; i < options.size(); i++) {
            if (options.get(i).getText().equals("Option 1")) {
                options.get(i).click();
            }
        }
```

- Page object
- Base Page Ob
ject
- Retry 
- ABTesting -with Cookies
- Screenshot

```java
File scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
            try {
                FileUtils.copyFile(scrFile,
                        new File("failshot_"
                                + description.getClassName()
                                + "_" + description.getMethodName()
                                + ".png"));
            } catch (IOException exception) {
                exception.printStackTrace();
            }
```

- Retries http status
BrowserMobProxyServer

- data-driven-testing
- locator strategy
- Dynamic PAge: WebDriverWait
- Tables
- ChromeDriver
- mobile drivers
- css-vs-xpath
- headless
- drag and drop
- Selector
- exception handle
- forget password
- one test,multiple browser
- chekboxes
- headless-ghostdriver
- waiting
- performance testing
- load testing
- Action Builder/Hover
- Javascript Alert
- Grid
- Javascript/executeScript
- logging
- wrapper
- junit xml
- tagging
- html report
- list tags
- keyboard-keys
- RightClick-ActionBuilder

```java
Actions action = new Actions(driver);
        action.contextClick(menu)
                .sendKeys(Keys.ARROW_DOWN)
                .sendKeys(Keys.ARROW_DOWN)
                .sendKeys(Keys.ARROW_DOWN)
                .sendKeys(Keys.ENTER)
                .perform();
        Alert alert = driver.switchTo().alert();

```

- limit bandwidth-proxy
- Hightlight item

```java
  js.executeScript(
                "arguments[0].setAttribute(arguments[1], arguments[2])",
                element,
                "style",
                "border: 2px solid red; border-style: dashed;");
```

- Blacklist(https://github.com/lightbody/browsermob-proxy)
- BrokenImage
- Load Testing Revisited
- Safari
- IE