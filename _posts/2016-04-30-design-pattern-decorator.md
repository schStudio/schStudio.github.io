---
layout:	post
title:	"装饰者(Decorator)模式"
categories: 设计模式
---

* content
{:toc}

### 简介

装饰者模式应用场景:

**装饰者模式动态地将责任附加到对象上. 若要扩展功能, 装饰者模式提供了比继承更有弹性的替代方案**

* 装饰者和被装饰对象有相同的超类型
* 可以用一个或多个装饰者包装一个对象
* 在被装饰对象可以使用的地方, 装饰者也可以使用
* 装饰者在被装饰对象的行为之前/之后, 可以加上自己的行为, 达到特定的目的

装饰者模式的使用模板如下:

![design-pattern-decorator-template]({{ site.url }}/assets/design-pattern/design-pattern-decorator-template.png)

### 应用例子

星巴兹(Starbuzz)是以扩展速度最快而闻名的咖啡连锁店. 因为扩展速度实在太快了, 他们准备更新订单系统, 以合乎他们的饮料供应要求.

他们原先的类图如下所示:

![design-pattern-decorator-beverage1]({{ site.url }}/assets/design-pattern/design-pattern-decorator-beverage1.png)

#### 场景一

现在, 购买咖啡时, 要求可以在其中加入各种调料, 例如: 蒸奶(Steamed Milk), 豆浆(Soy), 摩卡(Mocha), 奶泡等. 星巴兹会根据加入的调料收取不同的费用, 所以订单系统必须考虑到这些调料成分.

他们做了第一次尝试, 结果如下:

![design-pattern-decorator-beverage2]({{ site.url }}/assets/design-pattern/design-pattern-decorator-beverage2.gif)

很明显, 星巴兹为自己制造了一个维护恶梦. 如果牛奶的价钱上扬, 怎么办? 新增加一种焦糖调料, 怎么办?

#### 场景二

接下来有人提出了另一种解决方案, 她说利用实例变量和继承, 就可以追踪这些调料.

那么就根据这个解决方案来尝试以下. 先从Beverage基类下手, 加上实例变量, 代表是否加上调料(牛奶, 豆浆, 摩卡, 奶泡...). 结果如下:

![design-pattern-decorator-beverage3]({{ site.url }}/assets/design-pattern/design-pattern-decorator-beverage3.png)

这个设计看起来简洁很多, 但是从长远的角度思考, 会有什么问题呢?

* 调料价钱的改变需要修改现有代码
* 加入新调料需要加入新的实例变量和修改cost()
* 加入新饮料, 某些调料可能并不适合, 例如茶和奶泡
* 顾客想要双倍摩卡, 价钱难以修改

#### 场景三

场景一和场景二让我们认识到, 利用继承无法完全解决问题. 在星巴兹遇到的问题有: 类数量爆炸, 设计死板, 以及基类加入的新功能并不适用于所有的子类.

所以, 在这里要采用不一样的做法: 我们要以饮料为主体, 然后在运行时以调料"装饰"饮料. 比如, 如果顾客想要摩卡和奶泡深焙咖啡, 那么, 要做的就是:

1. 拿一个深焙咖啡(DarkRoast)对象
2. 以摩卡(Mocha)对象装饰它
3. 以奶泡(Whip)对象装饰它
4. 调用cost()方法, 并依赖委托(delegate)将调料的价钱加上去

因此根据该解决方案, 我们可以得到以下结果:

![design-pattern-decorator-beverage4]({{ site.url }}/assets/design-pattern/design-pattern-decorator-beverage4.png)

#### 代码设计

* 饮料基类Beverage

        public abstract class Beverage {
            String description = "Unknown Beverage";

            public String getDescription() {
                return description;
            }

            public abstract double cost();
        }

* 饮料子类HouseBlend, DarkRoast, Decaf, Espresso

        public class HouseBlend extends Beverage {

            public HouseBlend() {
                description = "House Blend Coffee";
            }

            @Override
            public double cost() {
                return .89;
            }
        }
        ///////////////////////////////////////////
        public class DarkRoast extends Beverage {

            public DarkRoast() {
                description = "Dark Roast Coffee";
            }

            @Override
            public double cost() {
                return .99;
            }
        }

        ///////////////////////////////////////////
        public class Decaf extends Beverage {

            public Decaf() {
                description = "Decaf Coffee";
            }

            @Override
            public double cost() {
                return 1.05;
            }
        }

        ///////////////////////////////////////////
        public class Espresso extends Beverage {

            public Espresso() {
                description = "Espresso";
            }

            @Override
            public double cost() {
                return 1.99;
            }
        }

* 调料装饰者基类CondimentDecorator

        public abstract class CondimentDecorator extends Beverage {
            public abstract String getDescription();
        }

* 调料装饰者子类Milk, Mocha, Soy, Whip

        public class Milk extends CondimentDecorator {
            Beverage beverage;

            public Milk(Beverage beverage) {
                this.beverage = beverage;
            }

            @Override
            public String getDescription() {
                return beverage.getDescription() + ", Milk";
            }

            @Override
            public double cost() {
                return .10 + beverage.cost();
            }

        }
        ///////////////////////////////////////////
        public class Mocha extends CondimentDecorator {
            Beverage beverage;

            public Mocha(Beverage beverage) {
                this.beverage = beverage;
            }

            @Override
            public String getDescription() {
                return beverage.getDescription() + ", Mocha";
            }

            @Override
            public double cost() {
                return .20 + beverage.cost();
            }

        }
        ///////////////////////////////////////////
        public class Soy extends CondimentDecorator {
            Beverage beverage;

            public Soy(Beverage beverage) {
                this.beverage = beverage;
            }

            @Override
            public String getDescription() {
                return beverage.getDescription() + ", Soy";
            }

            @Override
            public double cost() {
                return .15 + beverage.cost();
            }
        }
        ///////////////////////////////////////////
        public class Whip extends CondimentDecorator {
            Beverage beverage;

            public Whip(Beverage beverage) {
                this.beverage = beverage;
            }

            @Override
            public String getDescription() {
                return beverage.getDescription() + ", Whip";
            }

            @Override
            public double cost() {
                return .10 + beverage.cost();
            }
        }

* 测试代码

        public class StarbuzzCoffee {

            public static void main(String[] args) {
                // 未加调料, 浓缩咖啡
                Beverage beverage = new Espresso();
                System.out.println(beverage.getDescription()
                        + " $" + beverage.cost());
                // 双倍摩卡, 奶泡, 深焙咖啡
                Beverage beverage2 = new DarkRoast();
                beverage2 = new Mocha(beverage2);
                beverage2 = new Mocha(beverage2);
                beverage2 = new Whip(beverage2);
                System.out.println(beverage2.getDescription()
                        + " $" + beverage2.cost());
                // 豆浆, 摩卡, 奶泡, 综合咖啡
                Beverage beverage3 = new HouseBlend();
                beverage3 = new Soy(beverage3);
                beverage3 = new Mocha(beverage3);
                beverage3 = new Whip(beverage3);
                System.out.println(beverage3.getDescription()
                        + " $" + beverage3.cost());
            }
        }
        // 测试结果:
        // Espresso $1.99
        // Dark Roast Coffee, Mocha, Mocha, Whip $1.49
        // House Blend Coffee, Soy, Mocha, Whip $1.34

### 实际应用

#### Java I/O

Java I/O中的装饰者模式:

![design-pattern-decorator-java-io]({{ site.url }}/assets/design-pattern/design-pattern-decorator-java-io.png)

说明:

* `FileInputStream`,`StringBufferInputStream`,`ByteArrayInputStream`这些是可以被装饰者包装的具体组件
* `FilterInputStream`是一个抽象装饰者
* `PushbackInputStream`,`BufferedInputStream`,`DataInputStream`,`LineNumberInputStream`这些是具体的装饰者

### 应用实践

了解了Java I/O中的装饰者模式, 现在我们来编写自己的Java I/O装饰者: 

把输入流内的所有大写字符转成小写

代码设计如下:

* LowerCaseInputStream

        public class LowerCaseInputStream extends FilterInputStream{

            public LowerCaseInputStream(InputStream in) {
                super(in);
            }

            public int read() throws IOException {
                int c = super.read();
                return (c == -1 ? c : Character.toLowerCase((char)c));
            }

            public int read(byte[] b, int off, int len) throws IOException {
                int result = super.read(b, off, len);
                for (int i = off; i < off+result; i++) {
                    b[i] = (byte)Character.toLowerCase((char)b[i]);
                }
                return result;
            }
        }

* InputTest

        public static void main(String[] args) {
            int c;
            try {
                InputStream in = new LowerCaseInputStream(
                        new BufferedInputStream(
                        new FileInputStream("test.txt")));
                while ((c = in.read()) >= 0) {
                    System.out.println((char)c);
                }
                in.close();
            } catch(IOException e) {
                e.printStackTrace();
            }
        }