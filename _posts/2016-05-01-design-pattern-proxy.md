---
layout:	post
title:	"代理(Proxy)模式"
categories: 设计模式
---

* content
{:toc}

### 简介

代理模式应用场景:

**代理模式为一个对象提供一个替身或占位符以控制对这个对象的访问**

代理模式的使用模板如下:

![design-pattern-proxy-template]({{ site.url }}/assets/design-pattern/design-pattern-proxy-template.png)

### Java RMI

简介: RMI提供了客户辅助对象和服务辅助对象, 为客户辅助对象创建和服务对象相同的方法.RMI的好处是你不必编写网络和IO相关的代码.

RMI提供了所有运行时的基础设施, 好让一切正常工作. 其中包括了查找服务(lookup).

虽然调用远程方法就如同调用本地方法一样, 但是客户辅助对象会通过网络发送方法调用, 所以网络和IO的确是存在的. 我们知道网络和IO是有风险的, 容易失败的, 所以随时都可能抛出异常, 因此客户端必须意识到这种风险, 所以代码中会看到客户端程序处理异常.

RMI的术语:

* RMI将客户辅助对象称作stub(桩)
* RMI将服务辅助对象称作skeleton(骨架)

RMI的服务模型如下图所示:

![design-pattern-rmi-model]({{ site.url }}/assets/design-pattern/design-pattern-rmi-model.png)

服务步骤如下:

1. 定义远程接口
2. 实现远程接口
3. 利用rmic工具产生stub和skeleton
4. 启动RMI registry(rmiregistry)
5. 开启远程服务

代码实例:

1. 定义远程接口: MyRemote

        import java.rmi.Remote;
        import java.rmi.RemoteException;

        public interface MyRemote extends Remote {

            public String sayHello() throws RemoteException;
        }

2. 实现远程接口: MyRemoteImpl

        import java.rmi.Naming;
        import java.rmi.RemoteException;
        import java.rmi.server.UnicastRemoteObject;

        public class MyRemoteImpl extends UnicastRemoteObject implements MyRemote {

            protected MyRemoteImpl() throws RemoteException {
                super();
                // TODO Auto-generated constructor stub
            }

            @Override
            public String sayHello() throws RemoteException {
                return "Server says, 'Hey'";
            }

            public static void main(String[] args) {
                try {
                    MyRemote service = new MyRemoteImpl();
                    Naming.rebind("RemoteHello", service);
                } catch(Exception ex) {
                    ex.printStackTrace();
                }
            }
        }

3. 利用rmic工具产生stub和skeleton

		%rmic MyRemoteImpl

4. 启动RMI registry(rmiregistry)

		%rmicregistry

5. 开启远程服务

		%java MyRemoteImpl

6. 客户端测试

		%java MyRemoteClient

> 客户端如何获取stub对象呢?
>
> * 把可序列化的类移交到客户端
> * 动态类下载: 使用Web服务器提供类的下载地址, 由客户端RMI系统启动获取

对于RMI, 程序员最常犯的三个错误是:

1. 启动服务之前没有启动rmiregistry
2. 没有让变量和返回值类型成为可序列化的类型
3. 没有给客户端提供stub类

### 动态代理

Java在java.lang.reflect包中有自己的代理支持, 利用这个可以在运行时动态地创建一个代理类, 实现一个或多个接口, 并将方法的调用转发到指定的类. 因为实际的代理类是在运行时创建的, 所以称此Java技术为: 动态代理

动态代理模式的模板如下图:

![design-pattern-proxy-dynamic-template]({{ site.url }}/assets/design-pattern/design-pattern-proxy-dynamic-template.png)

因为Java已经为你创建了Proxy类, 所以你需要借助某种方法来告诉Proxy类你要做什么. 现在不能像以前一样把代码放在Proxy类中, 因为Proxy不是你实现的. 既然这样代码要放在哪里呢? 放在InvocationHandler中.

**InvocationHandler的工作是响应代理的任何调用**

> 使用动态代理实现的例子: [动态代理实例](#section-4)

### 应用例子: 远程代理

一家糖果公司的CEO想要在自己的办公室里面监控各地糖果销售情况, 这就需要用到Java RMI技术了, 而前面提到RMI技术就是代理模式应用场景.

首先要在服务器端创建Remote接口, 还有Remote接口实现类. 这里CEO需要监控糖果销售情况, 那么远程接口就由糖果销售机来提供:

* 糖果销售机远程接口`GumballMachineRemote`

        import java.rmi.*;

        public interface GumballMachineRemote extends Remote {
            public int getCount() throws RemoteException;
            public String getLocation() throws RemoteException;
            public State getState() throws RemoteException;
        }

* 糖果销售机远程接口实现`GumballMachine`

        import java.rmi.*;
        import java.rmi.server.*;

        public class GumballMachine
            extends UnicastRemoteObject implements GumballMachineRemote {
        private static final long serialVersionUID = 2L;
        State soldOutState;
        State noQuarterState;
        State hasQuarterState;
        State soldState;
        State winnerState;

        State state = soldOutState;
        int count = 0;
        String location;

        public GumballMachine(String location, int numberGumballs) throws RemoteException {
            soldOutState = new SoldOutState(this);
            noQuarterState = new NoQuarterState(this);
            hasQuarterState = new HasQuarterState(this);
            soldState = new SoldState(this);
            winnerState = new WinnerState(this);

            this.count = numberGumballs;
            if (numberGumballs > 0) {
                state = noQuarterState;
            } 
            this.location = location;
        }

        public void insertQuarter() {
            state.insertQuarter();
        }

        public void ejectQuarter() {
            state.ejectQuarter();
        }

        public void turnCrank() {
            state.turnCrank();
            state.dispense();
        }

        void setState(State state) {
            this.state = state;
        }

        void releaseBall() {
            System.out.println("A gumball comes rolling out the slot...");
            if (count != 0) {
                count = count - 1;
            }
        }

        public void refill(int count) {
            this.count = count;
            state = noQuarterState;
        }

        public int getCount() {
            return count;
        }

        public State getState() {
            return state;
        }

        public String getLocation() {
            return location;
        }

        public State getSoldOutState() {
            return soldOutState;
        }

        public State getNoQuarterState() {
            return noQuarterState;
        }

        public State getHasQuarterState() {
            return hasQuarterState;
        }

        public State getSoldState() {
            return soldState;
        }

        public State getWinnerState() {
            return winnerState;
        }

        public String toString() {
            StringBuffer result = new StringBuffer();
            result.append("\nMighty Gumball, Inc.");
            result.append("\nJava-enabled Standing Gumball Model #2014");
            result.append("\nInventory: " + count + " gumball");
            if (count != 1) {
                result.append("s");
            }
            result.append("\n");
            result.append("Machine is " + state + "\n");
            return result.toString();
        }

* 启动服务

        import java.rmi.*;

        public class GumballMachineTestDrive {

            public static void main(String[] args) {
                GumballMachineRemote gumballMachine = null;
                int count;

                if (args.length < 2) {
                    System.out.println("GumballMachine <name> <inventory>");
                    System.exit(1);
                }

                try {
                    count = Integer.parseInt(args[1]);

                    gumballMachine = 
                        new GumballMachine(args[0], count);
                    Naming.rebind("//" + args[0] + "/gumballmachine", gumballMachine);
                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        }

* 客户:

        import java.rmi.*;

        public class GumballMonitor {
            GumballMachineRemote machine;	// 代理对象

            public GumballMonitor(GumballMachineRemote machine) {
                this.machine = machine;
            }

            public void report() {
                try {
                    System.out.println("Gumball Machine: " + machine.getLocation());
                    System.out.println("Current inventory: " + machine.getCount() + " gumballs");
                    System.out.println("Current state: " + machine.getState());
                } catch (RemoteException e) {
                    e.printStackTrace();
                }
            }
        }

* 测试:

        import java.rmi.*;

        public class GumballMonitorTestDrive {

            public static void main(String[] args) {
                String[] location = {"rmi://santafe.mightygumball.com/gumballmachine",
                                     "rmi://boulder.mightygumball.com/gumballmachine",
                                     "rmi://seattle.mightygumball.com/gumballmachine"}; 

                if (args.length >= 0)
                {
                    location = new String[1];
                    location[0] = "rmi://" + args[0] + "/gumballmachine";
                }

                GumballMonitor[] monitor = new GumballMonitor[location.length];

                for (int i=0;i < location.length; i++) {
                    try {
                        GumballMachineRemote machine = 
                                (GumballMachineRemote) Naming.lookup(location[i]);
                        monitor[i] = new GumballMonitor(machine);
                        System.out.println(monitor[i]);
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }

                for(int i=0; i < monitor.length; i++) {
                    monitor[i].report();
                }
            }
        }

### 应用例子: 虚拟代理

**虚拟代理作为创建开销大的对象的代表. 虚拟代理经常直到我们真正需要一个对象的时候才创建它. 当对象在创建前和创建中时, 由虚拟代理待扮演对象. 对象创建后, 代理就会将请求直接委托给对象**

我们打算建立一个应用程序, 用来展示你最喜欢的CD封面. 你可以建立一个CD标题菜单, 然后从Amazon.com等网站的在线服务中取得CD封面的图. 如果你使用Swing, 可以创建一个Icon接口从网络上加载图像. **唯一的问题是, 局限于连接带宽和网络负载, 下载可能需要一些时间, 所以在等待图像加载的时候, 应该显示一些东西**. 我们也不希望在等待图像时将整个应用程序挂起. 一旦图像加载完成, 刚才显示的东西应该小时, 图像显示出来.

想做到这里, 简单的方式就是利用虚拟代理. 虚拟代理可以代理Icon, 管理背景的加载, 并在加载未完成时显示"CD封面加载中, 请稍候......", 一旦加载完成, 代理就把显示的职责委托给Icon接口.

根据上面的分析, 可以得到如下的类图:

![design-pattern-proxy-icon]({{ site.url }}/assets/design-pattern/design-pattern-proxy-icon.png)

代码设计如下:

* 代理对象ImageProxy

        import java.net.*;
        import java.awt.*;
        import javax.swing.*;

        class ImageProxy implements Icon {
            volatile ImageIcon imageIcon;
            final URL imageURL;
            Thread retrievalThread;
            boolean retrieving = false;

            public ImageProxy(URL url) { imageURL = url; }

            public int getIconWidth() {
                if (imageIcon != null) {
                    return imageIcon.getIconWidth();
                } else {
                    return 800;
                }
            }

            public int getIconHeight() {
                if (imageIcon != null) {
                    return imageIcon.getIconHeight();
                } else {
                    return 600;
                }
            }

            synchronized void setImageIcon(ImageIcon imageIcon) {
                this.imageIcon = imageIcon;
            }

            public void paintIcon(final Component c, Graphics  g, int x,  int y) {
                if (imageIcon != null) {
                    imageIcon.paintIcon(c, g, x, y);
                } else {
                    g.drawString("Loading CD cover, please wait...", x+300, y+190);
                    if (!retrieving) {
                        retrieving = true;

                        retrievalThread = new Thread(new Runnable() {
                            public void run() {
                                try {
                                    setImageIcon(new ImageIcon(imageURL, "CD Cover"));
                                    c.repaint();
                                } catch (Exception e) {
                                    e.printStackTrace();
                                }
                            }
                        });
                        retrievalThread.start();
                    }
                }
            }
        }

* 代理对象显示组件ImageComponent

        import java.awt.*;
        import javax.swing.*;

        class ImageComponent extends JComponent {
            private static final long serialVersionUID = 1L;
            private Icon icon;

            public ImageComponent(Icon icon) {
                this.icon = icon;
            }

            public void setIcon(Icon icon) {
                this.icon = icon;
            }

            public void paintComponent(Graphics g) {
                super.paintComponent(g);
                int w = icon.getIconWidth();
                int h = icon.getIconHeight();
                int x = (800 - w)/2;
                int y = (600 - h)/2;
                icon.paintIcon(this, g, x, y);
            }
        }

* 主界面ImageProxyTestDrive

        import java.net.*;
        import javax.swing.*;
        import java.util.*;

        public class ImageProxyTestDrive {
            ImageComponent imageComponent;
            JFrame frame = new JFrame("CD Cover Viewer");
            JMenuBar menuBar;
            JMenu menu;
            Hashtable<String, String> cds = new Hashtable<String, String>();

            public static void main (String[] args) throws Exception {
                ImageProxyTestDrive testDrive = new ImageProxyTestDrive();
            }

            public ImageProxyTestDrive() throws Exception{
                cds.put("Buddha Bar","http://images.amazon.com/images/P/B00009XBYK.01.LZZZZZZZ.jpg");
                cds.put("Ima","http://images.amazon.com/images/P/B000005IRM.01.LZZZZZZZ.jpg");
                cds.put("Karma","http://images.amazon.com/images/P/B000005DCB.01.LZZZZZZZ.gif");
                cds.put("MCMXC A.D.","http://images.amazon.com/images/P/B000002URV.01.LZZZZZZZ.jpg");
                cds.put("Northern Exposure","http://images.amazon.com/images/P/B000003SFN.01.LZZZZZZZ.jpg");
                cds.put("Selected Ambient Works, Vol. 2","http://images.amazon.com/images/P/B000002MNZ.01.LZZZZZZZ.jpg");

                URL initialURL = new URL((String)cds.get("Selected Ambient Works, Vol. 2"));
                menuBar = new JMenuBar();
                menu = new JMenu("Favorite CDs");
                menuBar.add(menu);
                frame.setJMenuBar(menuBar);

                for (Enumeration<String> e = cds.keys(); e.hasMoreElements();) {
                    String name = (String)e.nextElement();
                    JMenuItem menuItem = new JMenuItem(name);
                    menu.add(menuItem); 
                    menuItem.addActionListener(event -> {
                        imageComponent.setIcon(new ImageProxy(getCDUrl(event.getActionCommand())));
                        frame.repaint();
                    });
                }

                // set up frame and menus

                Icon icon = new ImageProxy(initialURL);
                imageComponent = new ImageComponent(icon);
                frame.getContentPane().add(imageComponent);
                frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
                frame.setSize(800,600);
                frame.setVisible(true);

            }

            URL getCDUrl(String name) {
                try {
                    return new URL((String)cds.get(name));
                } catch (MalformedURLException e) {
                    e.printStackTrace();
                    return null;
                }
            }
        }

### 应用例子: 保护代理

每个城镇都需要配对服务, 你负责帮对象村实现约会服务系统. 你有一个好点子, 就是在服务中加入"Hot"和"Not"的评鉴, "Hot"就表示喜欢对方, "Not"就表示不喜欢. 你希望这套系统能鼓励你的顾客找到可能的配对对象, 这也会让事情更有趣.

那么可以很容易得到下面的类图:

![design-pattern-proxy-person-bean]({{ site.url }}/assets/design-pattern/design-pattern-proxy-person-bean.png)

但是, 根据我们定义PersonBean的方式, 任何客户都可以调用任何方法.

这是一个我们可以使用保护代理的绝佳例子. **什么是保护代理? 这是一种根据访问权限决定客户可否访问对象的代理**. 比方说, 如果你有一个雇员对象, 保护代理允许雇员调用对象上的某些方法, 经理还可以多调用一些其他的方法(例如setSalary()), 而人力资源处的雇员可以调用对象上的所有方法.

在我们的约会服务系统中, 我们希望顾客可以设置自己的信息, 同时又防止他人更改这些信息. HotOrNot评分则相反, 顾客不能更改自己的评分, 但是他人可以设置你的评分.

因此, 我们使用保护代理可以得到如下的类图:

![design-pattern-proxy-person-bean-invocation-handler]({{ site.url }}/assets/design-pattern/design-pattern-proxy-person-bean-invocation-handler.png)

代码设计如下:

* PersonBean

        public interface PersonBean {

            String getName();
            String getGender();
            String getInterests();
            int getHotOrNotRating();

            void setName(String name);
            void setGender(String gender);
            void setInterests(String interests);
            void setHotOrNotRating(int rating); 

        }

* PersonBeanImpl

        public class PersonBeanImpl implements PersonBean {
            String name;
            String gender;
            String interests;
            int rating;
            int ratingCount = 0;

            public String getName() {
                return name;	
            } 

            public String getGender() {
                return gender;
            }

            public String getInterests() {
                return interests;
            }

            public int getHotOrNotRating() {
                if (ratingCount == 0) return 0;
                return (rating/ratingCount);
            }


            public void setName(String name) {
                this.name = name;
            }

            public void setGender(String gender) {
                this.gender = gender;
            } 

            public void setInterests(String interests) {
                this.interests = interests;
            } 

            public void setHotOrNotRating(int rating) {
                this.rating += rating;	
                ratingCount++;
            }
        }

* OwnerInvocationHandler

        import java.lang.reflect.*;

        public class OwnerInvocationHandler implements InvocationHandler { 
            PersonBean person;

            public OwnerInvocationHandler(PersonBean person) {
                this.person = person;
            }

            public Object invoke(Object proxy, Method method, Object[] args) 
                    throws IllegalAccessException {

                try {
                    if (method.getName().startsWith("get")) {
                        return method.invoke(person, args);
                    } else if (method.getName().equals("setHotOrNotRating")) {
                        throw new IllegalAccessException();
                    } else if (method.getName().startsWith("set")) {
                        return method.invoke(person, args);
                    } 
                } catch (InvocationTargetException e) {
                    e.printStackTrace();
                } 
                return null;
            }
        }

* NonOwnerInvocationHandler

        import java.lang.reflect.*;

        public class NonOwnerInvocationHandler implements InvocationHandler { 
            PersonBean person;

            public NonOwnerInvocationHandler(PersonBean person) {
                this.person = person;
            }

            public Object invoke(Object proxy, Method method, Object[] args) 
                    throws IllegalAccessException {

                try {
                    if (method.getName().startsWith("get")) {
                        return method.invoke(person, args);
                    } else if (method.getName().equals("setHotOrNotRating")) {
                        return method.invoke(person, args);
                    } else if (method.getName().startsWith("set")) {
                        throw new IllegalAccessException();
                    } 
                } catch (InvocationTargetException e) {
                    e.printStackTrace();
                } 
                return null;
            }
        }

* 测试驱动

        import java.lang.reflect.*;
        import java.util.*;

        public class MatchMakingTestDrive {
            HashMap<String, PersonBean> datingDB = new HashMap<String, PersonBean>();

            public static void main(String[] args) {
                MatchMakingTestDrive test = new MatchMakingTestDrive();
                test.drive();
            }

            public MatchMakingTestDrive() {
                initializeDatabase();
            }

            public void drive() {
                PersonBean joe = getPersonFromDatabase("Joe Javabean"); 
                PersonBean ownerProxy = getOwnerProxy(joe);
                System.out.println("Name is " + ownerProxy.getName());
                ownerProxy.setInterests("bowling, Go");
                System.out.println("Interests set from owner proxy");
                try {
                    ownerProxy.setHotOrNotRating(10);
                } catch (Exception e) {
                    System.out.println("Can't set rating from owner proxy");
                }
                System.out.println("Rating is " + ownerProxy.getHotOrNotRating());

                PersonBean nonOwnerProxy = getNonOwnerProxy(joe);
                System.out.println("Name is " + nonOwnerProxy.getName());
                try {
                    nonOwnerProxy.setInterests("bowling, Go");
                } catch (Exception e) {
                    System.out.println("Can't set interests from non owner proxy");
                }
                nonOwnerProxy.setHotOrNotRating(3);
                System.out.println("Rating set from non owner proxy");
                System.out.println("Rating is " + nonOwnerProxy.getHotOrNotRating());
            }

            PersonBean getOwnerProxy(PersonBean person) {

                return (PersonBean) Proxy.newProxyInstance( 
                        person.getClass().getClassLoader(),
                        person.getClass().getInterfaces(),
                        new OwnerInvocationHandler(person));
            }

            PersonBean getNonOwnerProxy(PersonBean person) {

                return (PersonBean) Proxy.newProxyInstance(
                        person.getClass().getClassLoader(),
                        person.getClass().getInterfaces(),
                        new NonOwnerInvocationHandler(person));
            }

            PersonBean getPersonFromDatabase(String name) {
                return (PersonBean)datingDB.get(name);
            }

            void initializeDatabase() {
                PersonBean joe = new PersonBeanImpl();
                joe.setName("Joe Javabean");
                joe.setInterests("cars, computers, music");
                joe.setHotOrNotRating(7);
                datingDB.put(joe.getName(), joe);

                PersonBean kelly = new PersonBeanImpl();
                kelly.setName("Kelly Klosure");
                kelly.setInterests("ebay, movies, music");
                kelly.setHotOrNotRating(6);
                datingDB.put(kelly.getName(), kelly);
            }
        }
        // 测试结果
        // Name is Joe Javabean
        // Interests set from owner proxy
        // Can't set rating from owner proxy
        // Rating is 7
        // Name is Joe Javabean
        // Can't set interests from non owner proxy
        // Rating set from non owner proxy
        // Rating is 5