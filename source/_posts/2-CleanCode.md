---
title: Clean Code Reading Note
date: 2018-07-01 21:29:37
tags:
  - Notes
---

# Clean Code/The Art of Readable Code Reading Notes

```
知易行难
理念可能都有，那么怎么做呢？
```
匆匆看过两本书，两本书提供了一些如何写好代码的纲领，当时很明显真要写好不是读完两本书就可以的。多练，多思考才可以。两本书有不少比较相似的内容，所以很明显这些相似的内容就是日常需要关注的地方，不管是注释，方法命名，变量命名，重构这些都是日常工作中时刻注意的。

Clean Code和The Art of Readable Code看完对于我来说，更多的是对于作者面面俱到细节的描述惊讶，居然有人能把这些琐碎的事情说的这么完整和逻辑感十足。可以说健壮的代码关于细节的处理是相当的繁琐，把繁琐的事情写成Clean Code不是一件容易事情，我们不缺理念，我们缺的是细节，谁都说我要写漂亮整齐的代码，然后怎么做到呢？我们抱怨别人的代码混乱，但是我们重写真的可以写的刚好吗？如果我们想写的好，那要如何去做呢？这本书给出了一些答案，而这些就是我学习的要点。


## Clean Code-chapter 1介绍

这个章节讲述了几个关于混乱代码的伤害(实际上是关于公司/产品的生存问题)，引用了不同技术大牛对于Clean Code的一些看法同时引出下面作者要写的关于Clean code的内容。大多数大牛们的说法都比价抽象，所以能记住的不多，下面是个印象比较的说法：

- ```Later Equals Never```
- 没有测试代码，就没有Clean Code(当然是TDD派)
- 没有重复代码，可读性好，小块代码
- 童子军军规：让营地比你来的时更干净，一点点清理，一点点消除，慢慢变成clean code
- 读代码：写代码=10:1，读远远超过写，为什么需要Clean Code了？

## Clean Code 的一些基本常识

- 起名字的艺术，有意义的名字，变量，函数，参数，类，包等等
   * 名副其实
   * 避免误导，缩写/拼硬真的都知道吗
   * 有意义的区分
   ```java
    copy(int[] a1,int a2[])
    //不如
    copy(int[] source,int[] dest)
   ```
   * 读的出来的名字
   * 每个概念对应一个词，在一个应用中保持一致
   * 使用已有解决方案领域的名称，比如遵循常用Design-pattern中的名称
   * 使用业务领域中名称
   * 添加有意义的语境名称，不使用无意义的语境名称

- 函数
  * 函数名称，描述性，动词和关键字
  * 函数变量，参数名称尽量有意义，数量尽量小于3个，善用可变参数
  * 短小尤其是高层的函数，尽量有意义的函数组合而成(<3000 行)
  * 只做一件事情
  * 少用switch
  * 无副作用，比如调用有前提条件，需要注意如何更优雅的处理
  * setIfExist
  * Exception 替代返回错误代码
  * 抽离try/catch的代码
  * DRY:Don't Repeat Yourself
  * 一个入口一个出口(一个return)

- 注释
  * 法律信息
  * 代码就是注释，给变量，函数起个好名字胜过很多注释
  * 提供信息的注释，避免无意义或者和代码不同步的注释
  * 警示，提醒可能修改时遇到问题
  * TODO
  * 放大某种看来不合理的问题，说明trade-off的情况或者特别处理的意义
  *  修改的日志，修改的comments

- 格式
格式是没有太多好说的，或许用一个stylecheck的工具更为好.

- 对象和数据结构
  * 模块不应该了解它内部的对象情形
  * DTO 通信/ActiveRecord-find/search， 这些是数据结构，不要有业务逻辑
  * 对象暴露行为，隐藏数据/数据结构暴露数据，不要有明显的行为

- 异常处理
  * 异常而非返回码
  * 使用Runtime Exception
  * 异常信息
  * 定义常规流程，不要返回NULL

- 单元测试
  * TDD，测试代码和业务代码，保持测试代码整齐(让人可以维护)
  * 测试代码可读性，确保一致可以延续下去和业务保持同步(code-review,如何解决代码量打得大的问题)
  * DSL Domain测试语言
  * FIRST: Fast/Independent/Repeatable/Self-Validating/Timely

- 类
  * 短小，SRP
  * 接口隔离/隔离修改
  * DIP

- 系统
  * DIP
  * 工厂方法
  * scale out
  * JDK代理/Proxy invocation handler/AOP/AspectJ

- 迭代
  * Run all the tests
  * Refactor

- Concurrence

  * 并发防御： SRP，Immutable Data,数据副本
  * ReentrantLock/Sempaphore/CoundDownLatch
  * 执行模型
  
  |互斥|每一个时刻只有一个线程访问共享数据和资源|
  |线程饥饿|比如总是执行快的线程执行|
  |死锁|两个或者多个线程相互等待|
  |活锁|执行次序一致的线程|
  
  * producer-consumer
  * reader-writer
  * 保持同步区域微小
  * 关闭资源
  * 测试线程代码/注意偶发时间(design for failure)
  * 多平台/多处理器
  * stub codes/yield??

- 逐步改进
  * work first
  * 一步一步Refactor，from one big method to well-orgainized
  * unit testing for refactor

- 味道和启发，需要细读
  * 移除不必要的注释
  * 函数参数不要过多，移除没有使用的函数
  * extract重复代码
  * 测试不足，使用覆盖率工具，忽律的测试是不确定的，边界条件
  * 缺陷扎堆
  
## The Art of Readable Code

### 表面层次的改进
  * naming，变量，函数，参数，类，包命名，有意义不含糊，特定，和业务领域命名一致
  * 注释，简单，实例，描述为什么这样的理由，todo，站在读者的立场

### 简化循环和逻辑

- if/esle的考量，正逻辑还是负逻辑更好理解
- 避免do/while
- 提前返回
- 最小嵌套
- 拆分巨大的语句
- 变量命名，缩小变量作用域
  
### 重新组织代码

- 抽取不想关的子逻到工具类或者其他相关领域
- 小函数也不需要太多
- 一次只做一件事情，通过组合函数或者类来解决大问题

### 想法变成代码
使用自然语言程序描述功能，描述解决问题的流程

## 少些代码

- 少量代码解决最重要的问题，介绍需求
- 使用现有工具，unix tools/现有代码

```
cat *.log | awk `{print $5 " " $7}` | egrep "[45]..$" \
| sort | uniq -c | sort -nr
```

## 精选话题

- 测试可读性
- 详细的错误信息
- 测试函数命名
- 可测性衡量-Hard
  * 全局变量
  * 外部组件大量依赖代码
  * 代码不确定行为
- 可测性衡量-好
  * 内部状态很少
  * 类函数只做一件事情
  * 依赖很少，可管理
  * 函数接口简单
- 测试代码需要描述清楚测试的功能，高层定义越简单越好

### 深入阅读推荐

- Code Complete
- Refactoring
- The Pragmatic Programer
- Javascript - The Good Parts
- Effective Java
- Design-Pattern: Elements of Reusable Object-Oriented Software
- Programming Pearls
- [Joel on Software](http://www.joeonsoftware.com)
- Donald E.Knuth books 