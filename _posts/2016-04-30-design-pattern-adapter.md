---
layout:	post
title:	"适配器(Adapter)模式"
categories: 设计模式
---

* content
{:toc}

### 简介

适配器模式应用场景:

**适配器模式将一个类的接口, 转换为客户期望的另一个接口. 适配器让原本接口不兼容的类可以合作无间**

1. 客户通过目标接口调用适配器的方法对适配器发出请求
2. 适配器使用被适配者接口把请求转换成被适配者的一个或多个调用接口
3. 客户受到调用的结果, 但并未察觉这一切是适配器在起转换作用

适配器模式的使用模板如下:

* 对象适配器:

    ![design-pattern-adapter-template]({{ site.url }}/assets/design-pattern/design-pattern-adapter-template.png)

* 类适配器:

    ![design-pattern-adapter-template1]({{ site.url }}/assets/design-pattern/design-pattern-adapter-template1.png)

### 应用例子

我们在院子里养了几种家禽, 包括鸭子, 火鸡等. 现在需要建立一个系统来管理它们, 第一步就需要给出这些家禽的定义, 很容易地我们就可以给出如下的类图:

![design-pattern-adapter-fowl]({{ site.url }}/assets/design-pattern/design-pattern-adapter-fowl.png)

#### 场景一

现在, 假设我们缺少鸭子对象, 想用一些火鸡对象来冒充鸭子(无良商家做的事).

显而易见, 因为火鸡的接口和鸭子的不同, 所以不能随便使用.

这时候, 起转换接口作用的适配器就很有用了, 我们可以得到如下的类图:

![design-pattern-adapter-fowl1]({{ site.url }}/assets/design-pattern/design-pattern-adapter-fowl1.png)

同理, 如果我们缺少火鸡对象, 同样可以借助鸭子来冒充火鸡, 类图如下:

![design-pattern-adapter-fowl2]({{ site.url }}/assets/design-pattern/design-pattern-adapter-fowl2.png)

#### 代码设计

* 鸭子接口Duck和绿头鸭MallardDuck

        public interface Duck {

            void quack();

            void fly();
        }

        public class MallarDuck implements Duck {

            @Override
            public void quack() {
                System.out.println("Quack");
            }

            @Override
            public void fly() {
                System.out.println("I'm flying");
            }

        }

* 火鸡接口Turkey和野生火鸡WildTurkey

        public interface Turkey {

            void gobble();

            void fly();
        }

        public class WildTurkey implements Turkey {

            @Override
            public void gobble() {
                System.out.println("Gobble gobble");
            }

            @Override
            public void fly() {
                System.out.println("I'm flying a short distance");
            }

        }

* 火鸡适配器TurkeyAdapter和鸭子适配器DuckAdapter

        public class TurkeyAdapter implements Duck {
            Turkey turkey;

            public TurkeyAdapter(Turkey turkey) {
                this.turkey = turkey;
            }

            @Override
            public void quack() {
                turkey.gobble();
            }

            @Override
            public void fly() {
                // 火鸡比鸭子不会飞, 飞5次才停止
                for (int i = 0; i < 5; i++) {
                    turkey.fly();
                }
            }

        }

        public class DuckAdapter implements Turkey {
            Duck duck;
            Random rand;

            public DuckAdapter(Duck duck) {
                this.duck = duck;
                rand = new Random();
            }

            @Override
            public void gobble() {
                duck.quack();
            }

            @Override
            public void fly() {
                // 鸭子比火鸡会飞, 平均5次才起飞
                if( rand.nextInt(5) == 0 ) {
                    duck.fly();
                }
            }

        }

* 测试代码

        public class FowlAdapterTest {

            public static void main(String[] args) {
                Duck duck = new MallarDuck();
                Turkey turkey = new WildTurkey();

                Duck fakeDuck = new TurkeyAdapter(turkey);
                fakeDuck.quack();
                fakeDuck.fly();

                Turkey fakeTurkey = new DuckAdapter(duck);
                fakeTurkey.gobble();
                fakeTurkey.fly();
            }

        }
        // 输出结果:
        // Gobble gobble
        // I'm flying a short distance(*5)
        // Quack
        // I'm flying(可能)

### 实际应用

#### 枚举器迭代器

旧世界的枚举器和新世界的迭代器:

![design-pattern-adapter-iterator]({{ site.url }}/assets/design-pattern/design-pattern-adapter-iterator.png)

如今, 我们经常面对遗留代码, 这些代码暴露出枚举接口, 但我们又希望在新的代码中只是用迭代器. 想要解决这个问题, 我们需要一个枚举器适配器.

类图设计如下:

![design-pattern-adapter-iterator1]({{ site.url }}/assets/design-pattern/design-pattern-adapter-iterator1.png)

看到这里, 应该会发现一个问题, 枚举器不支持`remove()`方法, 那么该如何处理呢? 幸好迭代器的设计者事先料到了, 所以将`remove()`方法定义成抛出`UnsupportedOperationException`.

代码设计如下:

    public class EnumerationIterator implements Iterator {
        Enumeration enumeration;

        @Override
        public boolean hasNext() {
            return enumeration.hasMoreElements();
        }

        @Override
        public Object next() {
            return enumeration.nextElement();
        }

        @Override
        public void remove() {
            throw new UnsupportedOperationException();
        }

    }