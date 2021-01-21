---
layout: post
title: "灵活 - 软件架构设计的原则"
date: 2020-12-12 18:52:05
comments: true
categories: 
- 软件架构 
tags: 
- Apache
- ZooKeeper
- Ubuntu
---

能适应变化是支撑企业生存和发展的重要能力, 这些变化包含：客户的需求,开拓新的市场，新技术的应用。 支撑企业运转的软件必须反映这些。

一个灵活的软件架构设计是至关重要的，鱼与熊掌不可兼得，在这里我们将通过一个非常高的架构视角来解释如何平衡和折中。

### **设计一个好的架构**

早在欧洲古典时代，人们已经知道并且描述精心设计建筑的原理，例如古罗马建筑师 [维特鲁威](https://en.wikipedia.org/wiki/Vitruvius) 在 [De Architectura](https://en.wikipedia.org/wiki/De_architectura). "firmitas, utilitas, venustas" - 稳定，实用，美丽

<img src="/static/image/Vitruvius.jpg" title="Vitruvius" width="600"/>
    Vitruvius

<img src="/static/image/De_architectura.jpg" title="De_architectura" width="600"/>
   《建筑十书》 De architectura

<img src="/static/image/Pont_du_Gard_BLS.jpg" title="Pont_du_Gard_BLS" width="600"/>
加尔水道桥（Pont du Gard）位于法国加德省靠近雷穆兰的地方，是古罗马所建造的输水系统。

对于软件架构来说，稳定并且实用的原则代表系统的弹性和生命周期，并且与体系结构承受和随时间变化的方式密切相关。 当建筑与周围环境和谐，没有不必要的混乱和复杂性时，就会产生美。


灵活的软件体系结构能够适应环境和需求的变化，而无需涵盖结构上的变化。 它也没有刚性结构，否则会阻碍功能的演化和增长。

## 参考链接：
[How To Install Apache ZooKeeper On Ubuntu](https://phoenixnap.com/kb/install-apache-zookeeper#htoc-configuring-replicated-zookeeper)
[尚硅谷Zookeeper教程(zookeeper框架精讲)](https://www.bilibili.com/video/BV1PW411r7iP?p=7)
