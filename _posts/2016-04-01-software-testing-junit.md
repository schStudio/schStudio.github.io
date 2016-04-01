---
layout: post
title: "JUnit"
category: 软件测试
---

* content
{:toc}

### JUnit简介

JUnit就是为Java程序开发者实现单元测试提供一种框架，使得Java单元测试更规范有效，并且更有利于测试的集成

### JUnit的原理

#### JUnit结构

JUnit中有7个包, 每个包的简要说明如下表:

| 包名 | 说明 |
|--------|--------|
|junit.awtui|以awt界面组件来显示junit测试过程结果|
|junit.swingui|以swing界面组件来显示junit测试过程结果|
|junit.textui|以控制台输出来显示junit测试过程结果|
|junit.framework|junit的核心功能包, 包括Test/TestCase/TestSuite/TestResult/TestListener等类|
|junit.runner|junit核心功能包, 主要运行Test类|
|junit.extensions|junit扩展功能包|

#### JUnit核心类

![junit-main-class]({{ site.url }}/assets/software-testing/junit-main-class.jpg)

| 类名 | 说明 |
|--------|--------|
|Test|用于运行测试和收集测试结果, 还可以获取测试用例个数|
|TestCase|Test接口的一个实现类, 给出了测试的基本实现|
|TestSuite|Test接口的另一个实现类, 可以包含多个TestCase|
|TestResult|包含收集到的测试结果|
|TestListener|测试流程的监听接口, 可以开始/结束测试, 可以添加failures/errors|

#### JUnit生命周期

![junit-life]({{ site.url }}/assets/software-testing/junit-life.png)



#### JUnit中的设计模式

* Template Method

    实质就是首先建立方法的骨架，而尽可能地将方法的具体实现向后推移. 

    TestCase.runBare()就采用了这种模式，客户类均可以重载它的三个方法，这样使得测试的可伸缩性得到提高.
    
        public void runBare() throws Throwable{ 
            setUp(); 
            try {
                runTest();
            } finally {
                tearDown();
            }
        }
    
* Command

	实质就是将动作封装为一个对象，而不关心动作的接收者。这样动作的接收者可以一直到动作具体执行时才需确定.
    
    接口Test就是一个Command集，使得不同类的不同测试方法可以通过同一种接口Test构造其框架结构。这样对测试的集成带来了很多方便.