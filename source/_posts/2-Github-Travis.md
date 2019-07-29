---
title: Travis - GithubToolChain-1
date: 2018-07-01 21:34:04
tags:
    - CI/CD
---

# Github工具系列-Travis

Travis 是一个持续集成工具，github上的每一次提交都可以触发一次build. 在说Travis之前，先说说持续集成，Continuous Integration(CI)的概念。

[CI](https://martinfowler.com/articles/continuousIntegration.html) 这是martin flow关于持续集成的一片文章。CI简而言之就是一个开发的方式，这个方式有别于瀑布式开发中在所有功能(大量代码)完成之后才进行验证，他提倡通过不停的提交一个个小功能(代码2量不那么大)进行验证。很显然采用这种开发方式，会带来频繁的build和test，因此也催生了Jenkins等持续集成的工具。Travis也是这种工具，和Jenkins不同的是，Travis更像一种云服务，不需要你在本地部署。

回到CI这个话题，为什么要采用CI这个方式呢？他有什么好处呢？CI的好处是：

···
通过不停的build,test可以对功能，代码获取快速的反馈，进行快速的修复，因此项目的进度实际上是非常稳定的，出现在产品开发完之后，产品，需求方向出现错误会尽可能的避免
···

从目前来看，国内很多公司大都采用了CI的方式，大型一些的公司每天都有成千上万的build，test在自动化的进行，没有一个工具来做这些的话，可以想象成本会有多高，也就无从谈起或者实施CI了。有一点比较佩服老外，他们如果觉得方向是正确的，就会想办法通过工具或者其他手段来实现，软件行业的各种思想，大部分都是老外提出的。(critical thinking这个方面也是我自己要好好提高的）

 
## Run Travis

.travis.yml file for JAVA

## .travis,yml detail
[reference](https://docs.travis-ci.com/user/for-beginners/)

- Building,testing,deployment
- Builds,Jobs,Stages and Phases 
  * phases, sequential steps of a jab
  * build, a group of jobs
  * job, serveral phases
  * stage
- travis phases:
  * before_install
  * install
  * before_script
  * script
