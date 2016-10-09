# Description

## DOL框架描述

> The distributed operation layer (DOL) is a framework that enables the (semi-) automatic mapping of applications onto the multiprocessor SHAPES architecture platform. The DOL consists of basically three parts:

> * DOL Application Programming Interface: The DOL defines a set of computation and communication routines that enable the programming of distributed, parallel applications for the SHAPES platform. Using these routines, application programmers can write programs without having detailed knowledge about the underlying architecture. In fact, these routines are subject to further refinement in the hardware dependent software (HdS) layer.

> - DOL Functional Simulation: To provide programmers a possibility to test their applications, a functional simulation framework has been developed. Besides functional verification of applications, this framework is used to obtain performance parameters at the application level.

> * DOL Mapping Optimization: The goal of the DOL mapping optimization is to compute a set of optimal mappings of an application onto the SHAPES architecture platform. In a first step, XML based specification formats have been defined that allow to describe the application and the architecture at an abstract level. Still, all the information necessary to obtain accurate performance estimates is contained.



# How to install

## DOL安装笔记

#####安装一些必要的环境(ubuntu为例)：
	`$ sudo apt-get update`
	`$ sudo apt-get install ant`
	`$ sudo apt-get install openjdk-7-jdk`
	`$ sudo apt-get install unzip`

#####配置步骤：

1. 下载文件(使用Vmware虚拟机，也可以从主机拷贝到虚拟机中去[Tutorial](http://jingyan.baidu.com/article/c33e3f48a5c153ea15cbb5b2.html)

   - sudo wget [systemc-2.3.1.tgz](http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz)
   - sudo wget [dol_ethz.zip](http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip)

2. 解压文件

   - 新建dol的文件夹  
     `$	mkdir dol`
   - 将dolethz.zip解压到dol文件夹中  
     `$	unzip dol_ethz.zip -d dol`
   - 解压systemc  
     `$	tar -zxvf systemc-2.3.1.tgz`

     ![解压文件](http://p1.bqimg.com/4851/8571fe15d05f36d7.png)
     **图一 创建的文件夹dol**

3. 编译systemc

   - 解压后进入systemc-2.3.1的目录下
     `$	cd systemc-2.3.1`
   - 新建一个临时文件夹objdir
     `$	mkdir objdir`
   - 进入该文件夹objdir
     `$	cd objdir`
   - 运行configure(能根据系统的环境设置一下参数，用于编译)
     `$	../configure CXX=g++ --disable-async-updates`

     ![configure1](http://p1.bqimg.com/4851/2dff14dda735b6d8.png)
     ![configure2](http://p1.bqimg.com/4851/365df95e408f3f1b.png)
     **图二、三 运行configure后的结果**

   - 编译
     `$sudo make install`
   - 编译完后文件目录如下(能看到include, lib-linux64(对于32位系统，这里是lib-linux)
     `$ cd ..`
     `$ ls`

      ![ls](http://p1.bqimg.com/4851/b5f051d202a2d65e.png)
      **图四 systemc-2.3.1目录下的文件**

   - 记录当前的工作路径(会输出当前所在路径，记下来，待会有用)
     `$	pwd`

     ![path](http://p1.bqimg.com/4851/241fa5ead84e2f77.png)
     **图五 显示当前的工作路径**

4. 编译dol

   - 进入刚刚dol的文件夹

     `$  cd../dol`

   - 修改build_zip.xml文件

     **找到下面这段话，就是说上面编译的systemc位置在哪里**

     >  <property name="systemc.inc"value="YYY/include"/>

     >  <property name="systemc.lib"value="YYY/lib-linux/libsystemc.a"/>

     **把YYY改成上页pwd的结果（注意，对于64位系统的机器，lib-linux要改成lib-linux64）**

     ![modify](http://p1.bqimg.com/4851/82a567923e25eff3.png)
     **图六 修改build_zip.xml文件指定内容**

   - 编译(若成功会显示build successful)
     `$ant -f build_zip.xml all`

     ![compile](http://p1.bqimg.com/4851/5a2baa51807ffc47.png)
     **图七 编译成功**

   - 接着可以试试运行第一个例子
     **进入build/bin/mian路径下**
     `$cd build/bin/main`
     **然后运行第一个例子**
     `$ant -f runexample.xml -Dnumber=1`

     ![run successfully](http://p1.bpimg.com/4851/c7f6b2c64b1abf2a.png)

     **图八 运行结果**

     ![example1](http://p1.bqimg.com/4851/2d70c1dfd39832b0.png)
     **图九 example1.dot**

     ​



# Experimental experience

## 实验感想

本次实验学习如何进行DOL开发环境的配置，实验文档以及Q&A文档都十分详细，并且在前面的配置过程中都没有出现任何问题，但最后运行例子时build failed，问题出在中文系统中，只需要将在`dol/build/bin/main` 下的`runexample.xml`第215-217行：

    <tstamp>
    	<format property="touch.time"
              pattern="MM/dd/yyyy hh:mm aa"
              offset="-5" unit="second"/>
    </tstamp>
    <touch datetime="${touch.time}">
      <fileset dir="example${number}"/>
    </touch>

修改为：
    <!--     <tstamp>
    	<format property="touch.time"
              pattern="MM/dd/yyyy hh:mm aa"
              offset="-5" unit="second"/>
    </tstamp>
    <touch datetime="${touch.time}">
      <fileset dir="example${number}"/>
    </touch> -->
即注释掉或者删掉即可。

![problem](http://p1.bpimg.com/4851/1b3113b30c103519.png)