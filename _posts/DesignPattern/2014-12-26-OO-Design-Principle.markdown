---
layout: post
title: 面向对象设计的原则
---

>本文引用Robert C. Martin所著的敏捷软件开发（原则、模式与实践）

* **SPR  单一职责原则**  
	一个类应该只有一个发生变化的原因

* **OCP  开放－封闭原则**  
	软件实体（类、模块、函数等）应该是可以扩展的，但是不可以修改。

* **LSP  Liskov替换原则**  
	子类型必须能够替换它们的基类型。

* **DIP  依赖倒置原则**  
	抽象不应该依赖于细节。细节应该依赖于抽象。

* **ISP  接口隔离原则**  
	不应该强迫客户依赖并未使用的方法。接口属于客户，而不属于它所在的类层次结构。

* **REP  重用－发布等价原则**  
	重用的粒度就是发布的粒度。
  
* **CCP  共同封闭原则**  
	包中的所有类对于同一种性质的变化应该是共同封闭的。一个变化若对一个封闭的包产生影响，则将对该包中的所有类产生影响，而对于其他包则不造成任何影响。

* **CRP  共同重用原则**  
	一个包中的所有类应该是共同重用的。如果重用了包中的一个类，那么也就相当于重用了包中的所有类。

* **ADP  无环依赖原则**  
	在包的依赖关系图中不允许存在环。
  
* **SDP  稳定依赖原则**  
	朝着稳定的方向进行依赖。

* **SAP  稳定抽象原则**  
	包的抽象程度与其稳定程度一致。

- - -

####参考

[PrinciplesOfOod](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)   
[solid](http://code.tutsplus.com/series/the-solid-principles--cms-634)  
[oodesign](http://www.oodesign.com/)  
[c2](http://c2.com/cgi/wiki?PrinciplesOfObjectOrientedDesign)  
