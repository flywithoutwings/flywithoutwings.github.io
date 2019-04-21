# Linux history命令介绍与不保存操作记录

## 一、history命令介绍

history命令在Linux系统中可以说是为我们提供了很大的便利，详细了解其用法可以有效提高工作效率。  
在默认环境下，系统（以Centos6.2为例）中默认history命令执行的效果是：  
983ls  
984catiptables  
985ifconfig  
986mysql-uroot-p  
987sshroot@192.168.0.51  
988df-h  
989history  
以上是截取的部分结果，下面来详细说说其用法：

### 1.命令的用法及参数usage:history\[-c\]\[-doffset\]\[n\]orhistory-anrw\[filename\]orhistory-psarg\[arg…\]

参数：  
n：数字，要列出最近的若干命令列表  
-c：将目前的shell中的所有history内容全部消除  
-a：将目前新增的history指令新增入histfiles中，若没有加histfiles，则预设写入~/.bash\_history  
-r：将histfiles的内容读到目前这个shell的history记忆中  
-w：将目前的history记忆内容写入histfiles

重复以前的指令：  
\[root@keystone~\]\#history10\#列出最后10条执行的命令  
\[root@keystone~\]\#!988\#执行第988条指令  
\[root@keystone~\]\#!!\#执行最后一条指令  
\[root@keystone~\]\#history-c\#清除所有的指令记录  
history命令的用途确实很大，但需要小心安全的问题，尤其是root的历史纪录档案，因为这将很多的重要资料在执行的过程中会被纪录在~/.bash\_history当中，需要使用过程中恰当的管理。

### 2.使用HISTTIMEFORMAT显示时间戳在/etc/bashrc中编辑：

export HISTTIMEFORMAT='%F%T'  
export HISTTIMEFORMAT="%Y-%m-%d%H:%M:%Swhoami"\#这将显示执行命令的用户  
这个功能只能用在当HISTTIMEFORMAT这个环境变量被设置之后，之后的那些新执行的bash命令才会被打上正确的时间戳。在此之前的所有命令，都将会显示成设置HISTTIMEFORMAT变量的时间。

### 3.使用HISTSIZE控制历史命令记录的总行数

在/etc/bashrc中编辑：  
export HISTSIZE=100\#history命令的记录数量。  
The number of commands to remember in the command history.The default value is 500.  
export HISTFILESIZE=100\#记录文件的行数。  
The maximum number of lines contained in the history file.When this variable is assigned avalue,the history file is truncated,if necessary,by removing the old estentries,to contain no more than that number of lines.The default value is 500.The history file is also truncated to this size after writing it when an interactive shell exits.  
如果你想禁用history，可以将HISTSIZE设置为0。

### 4.使用HISTFILE更改历史文件名称

默认情况下，命令历史存储在~/.bash\_history文件中。  
在/etc/bashrc中编辑：  
export HISTFILE=/var/log/command\_history

### 5.使用HISTCONTROL从命令历史中剔除连续重复的条目

在/etc/bashrc中编辑：

export HISTCONTROL=ignoredups\#ignoredups只能剔除连续的重复条目。要清除整个命令历史中的重复条目，可以将HISTCONTROL设置成erasedups  
export HISTCONTROL=erasedups

\#\#6.使用HISTCONTROL强制history不记住特定的命令  
在/etc/bashrc中编辑：

export HISTIGNORE='ls'\#A colonseparated list of patterns used to decide which command lines should be saved on the history list.Each pattern is anchored at the beginning of the line and must match the complete line

## 二、history不保存记录与隐藏操作

### 1、备份与恢复

步骤如下:  
1、建立一个文件来存储常用命令，例如/root/history.txt,把常用命令当成文本写进去，每个命令占一行  
2、在终端运行history -c，清除杂乱的历史记录  
3、运行history -r /root/history.txt,把命令读进来作为当前bash的历史记录  
4、运行history,就得到一个整洁的命令列表了，例如:  
\[root@localhost windata\]\# history -c  
\[root@localhost windata\]\# history -r /root/history.txt  
\[root@localhost windata\]\# history  
1 history -r /root/history.txt  
2 mount -t msdos -o iocharset=gb2312 /dev/sda1 /mnt/usb  
3 mount -t vfat -o iocharset-gb2312 /dev/hda5 /mnt/windata  
4 umount /mnt/windata  
5 mount -t vfat -o iocharset-gb2312 /dev/hda5 /mnt/windata  
6 cd /mnt/windata  
7 history  
\[root@localhost windata\]\#  
5、以后命令乱了，重复1-4的步骤，又可以使命令很清晰了。  
可以用history命令来查看。  
其它的记录你在/var/log这个文件夹下去找吧。它记录了所有的记录。  
实际上。history命令也是读的/var/log的东西  
历史命令都在 .bash\_history 文件中呀，每个用户的家目录中都有这个文件的，去看他们的历史命令吧  
在每个用户的家目录里，看文本命令很多，如： cat  more  less  vi 等等这些命令都可以看某个用户的 .bash\_history 文件。  
例：\# more ~user/.bash\_history 看 user 用户的历史命令，但是你要访问的权限才行。  
.bash\_history在每个用户的$HOME下  
在每个用户的home目录下的.bash\_history  
6.把一些命令给去掉回写回/root/.bash\_history的方法：  
history \|awk -F" " '{if\(NR&gt;0\){$1="";print $0}}'\|grep -v "multepollserver" &gt; /root/.bash\_history  
history -c //是把Linux下的/root/.bash\_history 全给清了，实现不保存，作为黑客是不地道的。  
再从其他终端倒腾回去：  
history \|awk -F" " '{if\(NR&gt;0\){$1="";print $0}}'\|grep -v "multepollserver" &gt; /root/.bash\_history

### 2、不留痕迹的办法,把指向给修改到/dev/null下：

\[root@localhost ~\]\# echo $HISTFILE  
/root/.bash\_history  
\[root@localhost ~\]\# HISTFILE=/dev/null  
\[root@localhost ~\]\# echo $HISTFILE  
/dev/null

\[root@test ~\]\# vi /root/.bash\_history  
对前面的：history \|awk -F" " '{print $3}' &gt; /root/.bash\_history 删除，退出即可。

请教一个关于linux下不保存命令历史记录的问题，不需要在任何文件中设置：  
ssh 登陆之后，在命令行下运行  
set HISTIGNORE=  
_或者  
export HISTIGNORE=_  
以后的命令就不会被保存了。

用 history -c 清空历史命令.  
在.bashrc的最后行追加  
unset HISTFILE  
这样做终端历史记录还是保存到了.bash\_history文件中，只是新打开的终端不能直接用上键调用而已，用"cat .bash\_history"仍能查看历史记录  
cat .bash\_history 看到的历史记录是 unset HISTFILE 之前保留的命令.  
unset HISTFILE 之后的命令并没有保留.  
用 history -c 清空历史命令.

### 3、总结

最简便不保留history操作记录的方法，在输入命令之前输入以下命令：  
unset HISTORY HISTFILE HISTSAVE HISTZONE HISTORY HISTLOG; export HISTFILE=/dev/null; export HISTSIZE=0; export HISTFILESIZE=0

### 4、参考文献

如何使linux系统下的root用户不保存终端历史记录到.bash\_history中 [https://blog.csdn.net/liushu\_it/article/details/43835709](https://blog.csdn.net/liushu_it/article/details/43835709)  
登录ssh之后不记录history [https://blog.51cto.com/0x007/1637909](https://blog.51cto.com/0x007/1637909)  
Linux 系统 history 命令详解 [https://blog.csdn.net/ichuzhen/article/details/30479435](https://blog.csdn.net/ichuzhen/article/details/30479435)

