---
layout: post
title: iPhone 上 RPG 游戏的设计和实现 (3)
published: false
tags: [Game, Cocos2d, RPG, Design]
catogory: 教程
---

由于本人对游戏实现认识不足, 总结一下几种游戏设计模式的区别仅供参考, 也许你有更深入的认识,
欢迎交流. 对于我这种菜鸟来说, Entity-System 比较适合.

基本上所有的运行期动态游戏对象实现的目的大部分是为了实现数据驱动的方式. 数据驱动的好处就
在于可以明确分离`程序`-`策划`在游戏实现环节的工作.也很方便`程序`开发一些编辑工具给`策划`
使用,提高`策划`的工作效率,满足`策划`高度定制游戏的需求.

<!--more-->

- Object-Centric/OO
  - 运行期游戏对象为静态对象.
  - 游戏对象的所有数据和行为在同一个类中.
  - 调试方便.
  - 不利于扩展和重用.

- Component-Based
  - 运行期游戏对象为动态对象.
  - 游戏的对象仅仅是一个 Component 的容器外加一个游戏对象的ID.
  - 游戏对象的行为通过 Component 来体现.
  - Component 内部包含了其所用的数据,也定义了组件的行为.
  - Component 之间不允许有依赖关系.
  - 必须有一个事件或者消息系统来对Component之间的依赖和交互进行解耦.
  - 必须精心定义各种消息来满足 Component 之间或者游戏对象之间的交互.
  - 本身框架实现简单, 但是消息设计比较复杂.
  - 调试不方便.

- Property-Centric/Attribute-Behaviour
  - 运行期游戏对象为动态对象.
  - 为了解决 Component 的依赖问题.
  - 为了解决 Component 之间重叠属性导致的内存占用问题.
  - 以数据为中心, 在游戏对象内部增加属性.
  - 游戏对象的行为由其属性决定, 属性再决定Behaviour.
  - 设计模式复杂.
  - 调试不方便.

- Entity-System
  - 运行期游戏对象为动态对象.
  - 游戏的对象仅仅是一个 Component 的容器外加一个游戏对象的ID.
  - 游戏对象的行为通过 Component 来体现.
  - Component 内部仅仅包含本组件所用的数据.
  - Component **不** 包含任何行为(setter/getter等必须品除外).
  - Component 的行为由 Subsystem 来实现.
  - Component 的交互和依赖可以通过不同的 Subsystem 来组合处理.
  - 事件或者消息系统不是必须的.
  - Entity 和 Component 组成了一个类似关系数据库的表, Entity 是主键, Component 是列.
  - 设计模式简单.
  - 运行期计算量比较大, 需要做一些优化.
  - 调试不方便.
