---
title: 马桶测试-1： 创建干净的测试数据 
date: 2018-07-01 21:29:37
tags:
  - google_test_blog_translation
---

# 马桶测试-1： 创建干净的测试数据

Google Testing on the Toilet Series 翻译第一篇,创建干净的测试数据

原文 [Origin Testing on the Toilet: Cleanly Create Test Data](https://testing.googleblog.com/2018/02/testing-on-toilet-cleanly-create-test.html)

Helper方法让创建数据非常方便，但是随着时间的推移，不同的变量数据创建方法加入之后，代码可读性变得不那么好了.

```java
// This helper method starts with just a single parameter:
Company company = newCompany(PUBLIC);

// But soon it acquires more and more parameters.
// Conditionals creep into the newCompany() method body to handle the nulls,
// and the method calls become hard to read due to the long parameter lists:
Company small = newCompany(2, 2, null, PUBLIC);
Company privatelyOwned = newCompany(null, null, null, PRIVATE);
Company bankrupt = newCompany(null, null, PAST_DATE, PUBLIC);

// Or a new method is added each time a test needs a different combination of fields:
Company small = newCompanyWithEmployeesAndBoardMembers(2, 2, PUBLIC);
Company privatelyOwned = newCompanyWithType(PRIVATE);
Company bankrupt = newCompanyWithBankruptcyDate(PAST_DATE, PUBLIC);
```

为了让测试数据代码更据可读性，使用builder pattern，通过返回部分数据的builder类来创建数据，修改不同的状态是一个不错的方法。 创建数据的方法可以生成部分的默认数据，之后再修改需要修改的数据，可以让测试代码方便而且容易阅读，如下例：

```java
Company small = newCompany().setEmployees(2).setBoardMembers(2).build();
Company privatelyOwned = newCompany().setType(PRIVATE).build();
Company bankrupt = newCompany().setBankruptcyDate(PAST_DATE).build();
Company arbitraryCompany = newCompany().build();

// Zero parameters makes this method reusable for different variations of Company.
// It also doesn’t need conditionals to ignore parameters that aren’t set (e.g. null
// values) since a test can simply not set a field if it doesn’t care about it.
private static Company.Builder newCompany() {
  return Company.newBuilder().setType(PUBLIC).setEmployees(100); // Set required fields
}
```

同时测试不应该以来任何的默认值，如果有依赖就让使用者必须去了解具体的创建数据方法的实现了。

```java
// This test needs a public company, so explicitly set it.
// It also needs a company with no board members, so explicitly clear it.
Company publicNoBoardMembers = newCompany().setType(PUBLIC).clearBoardMembers().build();
```

更多详细信息可以参考：http://www.natpryce.com/articles/000714.html


## 附上原文：

This article was adapted from a Google Testing on the Toilet (TotT) episode. You can download a printer-friendly version of this TotT episode and post it in your office.
By Ben Yu
Helper methods make it easier to create test data. But they can become difficult to read over time as you need more variations of the test data to satisfy constantly evolving requirements from new tests:

```java
// This helper method starts with just a single parameter:
Company company = newCompany(PUBLIC);

// But soon it acquires more and more parameters.
// Conditionals creep into the newCompany() method body to handle the nulls,
// and the method calls become hard to read due to the long parameter lists:
Company small = newCompany(2, 2, null, PUBLIC);
Company privatelyOwned = newCompany(null, null, null, PRIVATE);
Company bankrupt = newCompany(null, null, PAST_DATE, PUBLIC);

// Or a new method is added each time a test needs a different combination of fields:
Company small = newCompanyWithEmployeesAndBoardMembers(2, 2, PUBLIC);
Company privatelyOwned = newCompanyWithType(PRIVATE);
Company bankrupt = newCompanyWithBankruptcyDate(PAST_DATE, PUBLIC);
```

Instead, use the test data builder pattern: create a helper method that returns a partially-built object (e.g., a Builder in languages such as Java, or a mutable object) whose state can be overridden in tests. The helper method initializes logically-required fields to reasonable defaults, so each test can specify only fields relevant to the case being tested:

```java
Company small = newCompany().setEmployees(2).setBoardMembers(2).build();
Company privatelyOwned = newCompany().setType(PRIVATE).build();
Company bankrupt = newCompany().setBankruptcyDate(PAST_DATE).build();
Company arbitraryCompany = newCompany().build();

// Zero parameters makes this method reusable for different variations of Company.
// It also doesn’t need conditionals to ignore parameters that aren’t set (e.g. null
// values) since a test can simply not set a field if it doesn’t care about it.
private static Company.Builder newCompany() {
  return Company.newBuilder().setType(PUBLIC).setEmployees(100); // Set required fields
}
```

Also note that tests should never rely on default values that are specified by a helper method since that forces readers to read the helper method’s implementation details in order to understand the test.
// This test needs a public company, so explicitly set it.
// It also needs a company with no board members, so explicitly clear it.
Company publicNoBoardMembers = newCompany().setType(PUBLIC).clearBoardMembers().build();
You can learn more about this topic at http://www.natpryce.com/articles/000714.html

