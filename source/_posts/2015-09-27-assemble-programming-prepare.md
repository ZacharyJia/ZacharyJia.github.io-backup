title: 8086汇编开发环境搭建
date: 2015-09-27 09:47:09
categories: 编程
tags: [汇编,开发环境]
---

本学期开始学习汇编了，但是无奈学的东西都是好久之前的老知识了，在现在的电脑上实践起来略有困难，连`debug`命令都没法用。
最后在参考老师给的资料和网上相关资料的基础上，完成了汇编开发环境的搭建，做下记录，以备后用。

<!--more-->

整个开发环境的搭建主要有以下几步：
1. Dos模拟器DosBox的安装和本地文件夹的挂载
2. masm、link、debug等工具的安装
3. 环境变量的配置
4. 自动运行脚本的配置

以上是基本步骤。文中所用到的各个软件可以在[这里](http://7xn2d3.com1.z0.glb.clouddn.com/file汇编开发环境搭建.jpg)下载

下面开始正式的安装和搭建过程。
### 0x0. Dos模拟器DosBox的安装和本地文件夹的挂载
从上面的压缩包中找到DosBox的安装文件，安装即可。安装完成后可以桌面或者开始菜单的图标打开DosBox.如下图：

![DosBox](http://7xn2d3.com1.z0.glb.clouddn.com/imgDosBox.png)

可以看到它默认的系统盘是Z盘。
下面我们可以通过以下命令将本地电脑的一个文件夹挂载到DosBox中：
```
mount c e:\masm611
```
这条命令的含义就是将本地的`e:\masm611`这个文件夹挂载到DosBox中的C盘里。
也就是说你在DosBox中访问C盘的资源就可以看到这个文件夹中的内容了。
可以通过
```
c: 
dir
```
这两条命令查看到其中的内容。

### 0x1. masm、link、debug等工具的安装
我们在进行汇编编程的过程中，上面说的这几个工具都是非常常用的。Masm是编译器，link是链接器，debug是用来调试程序的。

首先我们来安装masm，这里我使用的是Masm6.11的安装版，我没有尝试过只使用一个masm.exe文件是否能够成功，可以自行尝试。
把压缩包中的Masm611.zip中的几个文件夹解压到刚才挂载的文件夹中（e:\masm611），然后从DosBox中进入DISK1文件夹，运行其中的setup.exe:
```
cd DISK1 回车
setup 回车
```
这时你就可以看到上古时代的软件安装界面了^_^

![安装](http://7xn2d3.com1.z0.glb.clouddn.com/img安装.png)

按照提示首先按回车继续，然后选择`Install the Macro Assembler using defaults`：

![安装2](http://7xn2d3.com1.z0.glb.clouddn.com/img安装2.png)

然后选择安装位置，我们选择刚刚挂载的C盘。


然后确认我们的选择。我们选择`NO CHANGES`继续安装即可。

![安装3](http://7xn2d3.com1.z0.glb.clouddn.com/img安装3.png)

最后等待一会儿之后就可以安装完成了。（确实要等*一会儿*）

回到命令输入界面后，我们可以通过cd命令进入到`c:\masm611\bin`这个目录，然后通过dir命令查看所有文件，如果你能找到
`masm.exe`这个文件，那么你这一步你就成功了。

接着我们安装link和debug工具。其实不算安装，只要在windows中将压缩包里的两个exe文件复制到`E:\masm611\masm611\bin`这个文件夹里就可以了。
然后在DosBox里再次查看`c:\masm611\bin`这个目录，能看到刚复制进去的两个文件就OK了。如果看不到的话，就执行下面这条命令再查看：
```
rescan
```
这条命令是用来清除磁盘缓存的。

最后，你的文件夹里应该是这样的：

![bin](http://7xn2d3.com1.z0.glb.clouddn.com/imgbin.png)


### 0x2. 环境变量的配置
我们现在已经可以使用使用masm、Link、debug这些命令进行汇编的编程了，但是每次都要输入全路径很麻烦，所以要通过配置环境变量来简化我们的操作。
相信学习过java开发的人都了解环境变量的作用了。在dos下设置环境变量只需要一条命令就行：
```
set PATH=%PATH%;c:\masm611\bin\;
```
也就是在原来的PATH的基础上，再加入c:\masm611\bin这个路径，也就是我们的masm等文件所在的路径。
执行完上面的命令之后，你应该就可以直接在里面输入:
```
masm
```
来运行masm程序了。like this:

![masm](http://7xn2d3.com1.z0.glb.clouddn.com/imgmasm.png)

这样这一步也成功完成了。

### 0x3. 自动运行脚本的配置
到这里，其实你已经可以愉快的开发了。然而当你下次再打开DosBox的时候，你会发现：
```
"WTF？C盘怎么不见了！？"
"我的masm命令怎么运行不了了？！"
...
```
这是因为你刚才做的配置都不能保存，关闭DosBox之后就都没了~_~
为了解决这个问题，我们可以对DosBox进行配置，让它在每次启动的时候自动为我们执行上面的操作。

在打开DosBox的时候，你应该会看到除了DosBox的窗口之外，还启动了另一个命令行窗口。

![config](http://7xn2d3.com1.z0.glb.clouddn.com/imgconfig.png)

然后我画红线的位置就是配置文件所在的地方，使用文本(ji)编(shi)辑(ben)器打开这个文件，然后找到最后的[autoexec]部分，在后面追加以下命令：
```
mount c e:\masm611
set PATH=%PATH%;c:\masm611\bin\;
```
别忘记保存。这时，你再重新打开DosBox试试，发现启动完成之后，它已经自动执行了这两条命令了。这时就可以直接使用masm等命令了！

### 0x4. 备注
我的安装过程中并没有在DosBox安装文本编辑器来编辑代码，因为我不想在Dos的命令行中写代码。
代码可以直接在e:\masm611这个文件夹中写，然后进DosBox编译链接调试就行，我相信大家还是更爱windows下的文本编辑器吧。

当然如果你非要在Dos下编辑文件的话，可以去网上找一个Dos下的edit.exe复制到DosBox里面使用。