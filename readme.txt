https://blog.csdn.net/jackailson/article/details/24230991

      我的个人博客:www.only2fire.com，里面的其他内容可能有你需要的哦 大笑。

      这篇文章在我博客的网址是:http://www.only2fire.com/archives/30.html,排版更好，不信的话，打开看看吧 ^_^。好了，如果不愿去我的博客看，那么下面就是正文。

注：本文参考了网上相关资料，且通过了本人的尝试。

如果你的电脑安装了CentOS后无法用有线上网,那么就请执行这个命令:

lspci| grep -i eth

我的终端显示的结果为:02:00.0 Ethernet controller: Qualcomm Atheros AR8161 Gigabit Ethernet (rev 08)

接着再执行一下命令：ifconfg

终端显示的连接中没有eth0或者ethx(x代表1～……)的相关信息，

如果你的电脑终端显示的也是上诉类似的情况,那么不用纠结了,这是网卡驱动没装好,好吧,接下来就由我来分享一下我的解决方法吧:

注:我电脑的网卡是Atheros AR8161这个,可能你的电脑不是这个,那么就请你去下载与你相关的驱动吧，不过安装方法都大同小异啦！

我这里针对的是安装Atheros AR8161。

首先需要准备的：与你电脑相关的网卡驱动


1、下载Atheros AR8161驱动，下面是网址

http://pan.baidu.com/s/1gd3hNvd

下载好后，进行解压，解压得到目录：alx-linux-v2.0.0.6


2、CentOS默认是没有安装gcc的，你可以执行:gcc

如果终端显示：bash: gcc: comman not found

那么就说明你的电脑还没安装gcc，此时执行下面的命令来安装gcc

执行命令：yum install gcc

等待gcc安装完成。

3、安装kernel-headers 和 kernel-devel：

在终端执行如下命令：

yum install kernel-headers-$(uname -r) kernel-devel-$( uname -r) -y


4、在终端下切换路径到刚才解压的文件下，即将路径切换到文件夹alx-linux-v2.0.0.6里面

接着执行命令:cd ./src/

接着执行命令：make  此时在src文件夹中产生了一个alx.ko文件

接着执行命令：mkdir /lib/modules/2.6.32-358.6.2.el6.x86_64/kernel/net/wired不必敲全，用TAB键补全，只要找对自己的内核目录就行，这一步只是创建一个wired目录，因此用TAB键补全，找到自己对应的目录是很重要的。


5、拷贝alx.ko到上一步创建的文件夹里面

接着执行命令：vi /lib/modules/2.6.32-358.6.2.el6.x86_64/modules.networking  不必敲全，用TAB键补全，只要找对自己的内核目录就行，找到自己对应的目录是很重要的

在modules.networking的第一行加入alx.ko,如下图：



6、执行命令：vi /etc/sysconfig/modules/alx.modules

在alx.modules中添加如下内容：

#!/bin/sh

if [ ! -c /dev/input/alx.ko ] ; then

exec /sbin/modprobe alx >/dev/null 2>&1

fi





7、最后执行其他命令：

首先执行命令：depmod -a

接着执行命令：modprobe alx

好了至此，网卡驱动安装就大功告成了，看看你的网络是否连接上了呢，反正我的是连上了呢^_^

注：转载请注明出处，谢谢！^_^