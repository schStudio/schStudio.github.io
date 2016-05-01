---
layout:	post
title:	"外观(Facade)模式"
categories: 设计模式
---

* content
{:toc}

### 简介

外观模式应用场景:

**外观模式提供了一个统一的接口, 用来访问子系统中的一群接口. 外观定义了一个高层接口, 让子系统更容易使用**

外观模式的使用模板如下:

![design-pattern-facade-template]({{ site.url }}/assets/design-pattern/design-pattern-facade-template.png)

### 应用例子

我是一个电影迷, 所以我要建立一个属于自己的家庭影院. 通过一番研究比较, 我组装了一套杀手级的系统, 内含DVD播放器, 投影机, 自动屏幕, 环绕立体声, 甚至还有爆米花机. 这些组件的组成如下:

![design-pattern-adapter-amplifier]({{ site.url }}/assets/design-pattern/design-pattern-facade-amplifier.png)

#### 场景一

花了好几个星期布线, 挂上投影机, 连接所有的装置并进行微调. 现在躺下来正准备享受一部电影......

愉快地挑选了一部DVD影片, 放松, 准备开始感受电影的魔幻魅力. OMG, 忘了一件事: 想看电影必须先执行一些任务.

* 打开爆米花机
* 开始爆米花
* 将灯光调暗
* 放下屏幕
* 打开投影机
* 将投影机的输入切换到DVD
* 将投影机设置宽屏模式
* 打开功放
* 将功放的输入切换到DVD
* 将功放设置到环绕立体声
* 将功放音量调到中
* 打开DVD播放器
* 开始播放DVD

想到这里, 心累, 但是还不止如此.

* 看完电影还需要把这一切都关掉
* 以后想要听CD或者广播, 也是这么麻烦

怎么办? 使用家庭影院竞变得如此复杂! 接下来看看外观模式是如何简化我们的需求的:

![design-pattern-adapter-amplifier1]({{ site.url }}/assets/design-pattern/design-pattern-facade-amplifier1.png)

现在, 我就可以直接调用`watchMovie()`来观看电影, 灯光, DVD播放器, 投影机, 功放, 屏幕, 爆米花, 一口气全部搞定.

> 外观模式除了简化接口之外,, 还将客户从组件的子系统中解耦.
> 
> 例如我中了彩票, 想要更新我的家庭影院, 很多设备的接口都改变了, 那么唯一需要改变的只是外观代码. 我依旧直接使用`watchMovie()`观看电影, 也就是客户代码不需要改变.

#### 代码设计

* 功放Amplifier

        public class Amplifier {
            String description;
            Tuner tuner;
            DvdPlayer dvd;
            CdPlayer cd;

            public Amplifier(String description) {
                this.description = description;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void setStereoSound() {
                System.out.println(description + " stereo mode on");
            }

            public void setSurroundSound() {
                System.out.println(description + " surround sound on (5 speakers, 1 subwoofer)");
            }

            public void setVolume(int level) {
                System.out.println(description + " setting volume to " + level);
            }

            public void setTuner(Tuner tuner) {
                System.out.println(description + " setting tuner to " + dvd);
                this.tuner = tuner;
            }

            public void setDvd(DvdPlayer dvd) {
                System.out.println(description + " setting DVD player to " + dvd);
                this.dvd = dvd;
            }

            public void setCd(CdPlayer cd) {
                System.out.println(description + " setting CD player to " + cd);
                this.cd = cd;
            }

            public String toString() {
                return description;
            }
        }

* CD播放器CdPlayer

        public class CdPlayer {
            String description;
            int currentTrack;
            Amplifier amplifier;
            String title;

            public CdPlayer(String description, Amplifier amplifier) {
                this.description = description;
                this.amplifier = amplifier;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void eject() {
                title = null;
                System.out.println(description + " eject");
            }

            public void play(String title) {
                this.title = title;
                currentTrack = 0;
                System.out.println(description + " playing \"" + title + "\"");
            }

            public void play(int track) {
                if (title == null) {
                    System.out.println(description + " can't play track " + currentTrack + 
                            ", no cd inserted");
                } else {
                    currentTrack = track;
                    System.out.println(description + " playing track " + currentTrack);
                }
            }

            public void stop() {
                currentTrack = 0;
                System.out.println(description + " stopped");
            }

            public void pause() {
                System.out.println(description + " paused \"" + title + "\"");
            }

            public String toString() {
                return description;
            }
        }

* DVD播放器DvdPlayer

        public class DvdPlayer {
            String description;
            int currentTrack;
            Amplifier amplifier;
            String movie;

            public DvdPlayer(String description, Amplifier amplifier) {
                this.description = description;
                this.amplifier = amplifier;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

                public void eject() {
                movie = null;
                        System.out.println(description + " eject");
                }

            public void play(String movie) {
                this.movie = movie;
                currentTrack = 0;
                System.out.println(description + " playing \"" + movie + "\"");
            }

            public void play(int track) {
                if (movie == null) {
                    System.out.println(description + " can't play track " + track + " no dvd inserted");
                } else {
                    currentTrack = track;
                    System.out.println(description + " playing track " + currentTrack + " of \"" + movie + "\"");
                }
            }

            public void stop() {
                currentTrack = 0;
                System.out.println(description + " stopped \"" + movie + "\"");
            }

            public void pause() {
                System.out.println(description + " paused \"" + movie + "\"");
            }

            public void setTwoChannelAudio() {
                System.out.println(description + " set two channel audio");
            }

            public void setSurroundAudio() {
                System.out.println(description + " set surround audio");
            }

            public String toString() {
                return description;
            }
        }

* 灯光TheaterLight

        public class TheaterLights {
            String description;

            public TheaterLights(String description) {
                this.description = description;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void dim(int level) {
                System.out.println(description + " dimming to " + level  + "%");
            }

            public String toString() {
                return description;
            }
        }

* 爆米花机PopcornPopper

        public class PopcornPopper {
            String description;

            public PopcornPopper(String description) {
                this.description = description;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void pop() {
                System.out.println(description + " popping popcorn!");
            }


                public String toString() {
                        return description;
                }
        }

* 屏幕Screen

        public class Screen {
            String description;

            public Screen(String description) {
                this.description = description;
            }

            public void up() {
                System.out.println(description + " going up");
            }

            public void down() {
                System.out.println(description + " going down");
            }


            public String toString() {
                return description;
            }
        }

* 投影仪Projector

        public class Projector {
            String description;
            DvdPlayer dvdPlayer;

            public Projector(String description, DvdPlayer dvdPlayer) {
                this.description = description;
                this.dvdPlayer = dvdPlayer;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void wideScreenMode() {
                System.out.println(description + " in widescreen mode (16x9 aspect ratio)");
            }

            public void tvMode() {
                System.out.println(description + " in tv mode (4x3 aspect ratio)");
            }

                public String toString() {
                        return description;
                }
        }

* 收音机Tuner

        public class Tuner {
            String description;
            Amplifier amplifier;
            double frequency;

            public Tuner(String description, Amplifier amplifier) {
                this.description = description;
            }

            public void on() {
                System.out.println(description + " on");
            }

            public void off() {
                System.out.println(description + " off");
            }

            public void setFrequency(double frequency) {
                System.out.println(description + " setting frequency to " + frequency);
                this.frequency = frequency;
            }

            public void setAm() {
                System.out.println(description + " setting AM mode");
            }

            public void setFm() {
                System.out.println(description + " setting FM mode");
            }

            public String toString() {
                return description;
            }
        }

* 家庭影院外观模式

        public class HomeTheaterFacade {
            Amplifier amp;
            Tuner tuner;
            DvdPlayer dvd;
            CdPlayer cd;
            Projector projector;
            TheaterLights lights;
            Screen screen;
            PopcornPopper popper;

            public HomeTheaterFacade(Amplifier amp, 
                         Tuner tuner, 
                         DvdPlayer dvd, 
                         CdPlayer cd, 
                         Projector projector, 
                         Screen screen,
                         TheaterLights lights,
                         PopcornPopper popper) {

                this.amp = amp;
                this.tuner = tuner;
                this.dvd = dvd;
                this.cd = cd;
                this.projector = projector;
                this.screen = screen;
                this.lights = lights;
                this.popper = popper;
            }

            public void watchMovie(String movie) {
                System.out.println("Get ready to watch a movie...");
                popper.on();
                popper.pop();
                lights.dim(10);
                screen.down();
                projector.on();
                projector.wideScreenMode();
                amp.on();
                amp.setDvd(dvd);
                amp.setSurroundSound();
                amp.setVolume(5);
                dvd.on();
                dvd.play(movie);
            }


            public void endMovie() {
                System.out.println("Shutting movie theater down...");
                popper.off();
                lights.on();
                screen.up();
                projector.off();
                amp.off();
                dvd.stop();
                dvd.eject();
                dvd.off();
            }

            public void listenToCd(String cdTitle) {
                System.out.println("Get ready for an audiopile experence...");
                lights.on();
                amp.on();
                amp.setVolume(5);
                amp.setCd(cd);
                amp.setStereoSound();
                cd.on();
                cd.play(cdTitle);
            }

            public void endCd() {
                System.out.println("Shutting down CD...");
                amp.off();
                amp.setCd(cd);
                cd.eject();
                cd.off();
            }

            public void listenToRadio(double frequency) {
                System.out.println("Tuning in the airwaves...");
                tuner.on();
                tuner.setFrequency(frequency);
                amp.on();
                amp.setVolume(5);
                amp.setTuner(tuner);
            }

            public void endRadio() {
                System.out.println("Shutting down the tuner...");
                tuner.off();
                amp.off();
            }
        }

* 测试代码

        public class HomeTheaterTestDrive {
            public static void main(String[] args) {
                Amplifier amp = new Amplifier("Top-O-Line Amplifier");
                Tuner tuner = new Tuner("Top-O-Line AM/FM Tuner", amp);
                DvdPlayer dvd = new DvdPlayer("Top-O-Line DVD Player", amp);
                CdPlayer cd = new CdPlayer("Top-O-Line CD Player", amp);
                Projector projector = new Projector("Top-O-Line Projector", dvd);
                TheaterLights lights = new TheaterLights("Theater Ceiling Lights");
                Screen screen = new Screen("Theater Screen");
                PopcornPopper popper = new PopcornPopper("Popcorn Popper");

                HomeTheaterFacade homeTheater = 
                        new HomeTheaterFacade(amp, tuner, dvd, cd, 
                                projector, screen, lights, popper);

                homeTheater.watchMovie("Raiders of the Lost Ark");
                homeTheater.endMovie();
            }
        }
        // 测试结果:
        // watchMovie()
        // Get ready to watch a movie...
        // Popcorn Popper on
        // Popcorn Popper popping popcorn!
        // Theater Ceiling Lights dimming to 10%
        // Theater Screen going down
        // Top-O-Line Projector on
        // Top-O-Line Projector in widescreen mode (16x9 aspect ratio)
        // Top-O-Line Amplifier on
        // Top-O-Line Amplifier setting DVD player to Top-O-Line DVD Player
        // Top-O-Line Amplifier surround sound on (5 speakers, 1 subwoofer)
        // Top-O-Line Amplifier setting volume to 5
        // Top-O-Line DVD Player on
        // Top-O-Line DVD Player playing "Raiders of the Lost Ark"

        // endMovie()
        // Shutting movie theater down...
        // Popcorn Popper off
        // Theater Ceiling Lights on
        // Theater Screen going up
        // Top-O-Line Projector off
        // Top-O-Line Amplifier off
        // Top-O-Line DVD Player stopped "Raiders of the Lost Ark"
        // Top-O-Line DVD Player eject
        // Top-O-Line DVD Player off