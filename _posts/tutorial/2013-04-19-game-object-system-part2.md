---
layout: post
title: 游戏对象模型设计与实现(2)
published: true
tags: [Game, Design, Architecture]
catogory: 教程
---

严格来说，一个游戏的实现框架有三种：

- Object-Centric：面向对象方式的。
- Component-Based：面向组件的。
- Entity-System：实体系统？

我们依次来分析这三种实现方式的优点和缺点.

<!--more-->

Object-Centric
--------------

面向对象(OO)可能是我们每一个写过程序的人都耳熟能详的概念. 这里我就不废话了, 单说面向对象在游戏实现方面的不足之处.

众所周知, OO 的特性在于通过继承来实现代码的可重用性, 也许我们设计一个游戏, 游戏里面的对象有如下的继承结构:

![](/image/2013-04-19-rpg-game-part2.md/Object-Centric-Inherit-01.png)

这个结构看起来还可以, DrawableObject 处理可以在屏幕上显示的元素, 而 Trigger 触发器是不用在屏幕上显示的. 加入我们有一个游戏元素是可以动画的, 比如一个 NPC 怪物有走路的动作, 那我们需要一个负责处理动画的类 AnimatedObject, 显然 Animated 对象应该是一个可以显示的对象, 必然在 DrawableObject 下面. 那问题就来了, AnimatedObject 是直接继承 DrawableObject 呢还是 SimulatedObject ?

考虑到我们的 NPC 可能需要进行物理方面的模拟, 我们暂时挂在 SimulatedObject 类下面吧. 看起来我们的继承结构变成了这样:

![](/image/2013-04-19-rpg-game-part2.md/Object-Centric-Inherit-02.png)

问题是我们可能还有一些 NPC 会有触发效果的, 比如说接近了 NPC 以后, 玩家与 NPC 进入战斗状态. 那这种 NPC 怎么处理呢? 这种 NPC 同时继承 NPC 和 TriggerObject ?

一个明显的例子就是比如我们有一个交通工具的对象继承体系:

![](/image/2013-04-19-rpg-game-part2.md/Object-Centric-Inherit-03.png)

如果大家玩过`魔兽世界`肯定知道, 狮鹫是可以在陆地上跑, 也可以在天上飞的. 这种情况下, 狮鹫该继承哪个类呢? 还是多继承?

真是一个糟糕的想法! 我们在刚开始学习的时候, 也许老师就已经说过, 尽量不要用多重继承! 甚至有些语言在实现上就不支持多重继承. 也许我们硬着头皮使用多继承的方式来解决狮鹫的问题. 它继承于 AirVehicle 和 LandVehicle. 但是不可避免得, 我们需要增加狮鹫的接口来处理陆地行走(Run)和飞行(Fly)两个行为. 但是在 Vehicle 里面可能只有一个(MoveTo)接口, 好吧,我们再修改一下, 给 Vehicle 里面增加接口: Run, Fly, Swim 三个接口可以解决水陆空三种交通方式的问题.

如果我们都需要使用增加基类的接口这样来满足这样的需求, 那可能导致我们的基类无比庞大而难以维护, 而且, 面向对象的"一个接口多种形态"的理念又体现在哪里呢?

问题在于: 如果不使用多继承, 我们无法实现具体对象的行为, 如果使用多继承, 则导致基类需要定义N多的接口. 在单继承体系中, 我们对于对象的分类只能遵从一个标准, 如果一个对象通过一种标准获得了在继承体系中的地位, 就没有办法遵从另外一个标准来确定自己在继承体系中的地位. 还是拿狮鹫的例子来说: 狮鹫是一种交通工具, 同时, 它还是一种生物, 还是一种 NPC ...... 那狮鹫到底是继承交通工具, 还是生物, 还是 NPC 呢?

也许我们早就听到过这样的忠告: "*尽量使用**`组合`**方式而非**`继承`**方式来实现自己的类.*"


Component-Based
---------------

"你看到我手上拿的这个东西了吧，表面上看它是一个大哥大电话，但是你看这里有一层金属网膜，实际上，它是一个刮胡刀，这样在执行任务的时侯，也可以神不知鬼不觉地刮胡子。至于这个表面上看是一个刮胡刀，其实呢，它是一个吹风机。" -- 《国产007》

OO 不是一切, OO 也不是一无是处.

现实世界中的一个对象到底是因为他是某一具体的对象而具有了某些功能还是因为其具有了某些功能而是一个具体的对象? 太绕口了.

拿上面的刮胡刀(暂定名称)来说,它具有大哥大的外观,刮胡子的功能. 假如我们用 OO 的概念来定义的话, 它到底是什么? 大哥大亦或是刮胡刀? 两者都不是.我们唯一确定的就是, 它是一个"存在".:)

    class Mysticism : public Object {
    public:
        Aspect* aspect = new Aspect("大哥大");
        Function* function = new Function("刮胡子");
    };

也许这样更准确一些.

在组件编程的概念中, 一个对象由若干组件构成. 如果问这个对象到底是什么东西, 那要看你从哪方面问. 比如, 这个东西看上去是什么? "大哥大".
这个东西能干什么? "刮胡子". 如果你问, 这是什么? "一个存在". :)