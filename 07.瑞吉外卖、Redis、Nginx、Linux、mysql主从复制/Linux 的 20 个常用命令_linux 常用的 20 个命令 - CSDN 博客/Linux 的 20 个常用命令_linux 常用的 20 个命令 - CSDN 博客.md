---
url: https://blog.csdn.net/qq_40991313/article/details/126258465?spm=1001.2014.3001.5501
title: Linux 的 20 个常用命令_linux 常用的 20 个命令 - CSDN 博客
date: 2025-02-20 09:49:11
tag: 
summary: 
---
 **导航：**

[【Java 笔记 + 踩坑汇总】Java 基础 + JavaWeb+SSM+SpringBoot+SpringCloud + 瑞吉外卖 / 谷粒商城 / 学成在线 + 设计模式 + 面试题汇总 + 性能调优 / 架构设计 + 源码解析](https://blog.csdn.net/qq_40991313/article/details/126646289?csdn_share_tail=%7B%22type%22%3A%22blog%22%2C%22rType%22%3A%22article%22%2C%22rId%22%3A%22126646289%22%2C%22source%22%3A%22qq_40991313%22%7D "【Java笔记+踩坑汇总】Java基础+JavaWeb+SSM+SpringBoot+SpringCloud+瑞吉外卖/谷粒商城/学成在线+设计模式+面试题汇总+性能调优/架构设计+源码解析")

**目录**

[0. tab 键：代码补全](#t0)

[1. ls：列出文件列表 list](#t1)

[2. cd：切换目录 change directory](#t2)

[3. cp：复制粘贴文件 copy](#t3)

[4. mv：移动 move](#t4)

[5. rm：删除文件、目录 remove](#t5)

[6. mkdir：创建目录 make directory](#t6)

[7. rmdir：删除空目录 remove directory](#t7)

[8. chown：更改所有者 change owner](#t8)

[9. chmod：更改文件的权限模式 change mode](#t9)

[10. find：查找文件](#t10)

[11. |：管道](#t11)

[12. grep：查找文件内容，按行查找并匹配](#t12)

[13. tar：打包，压缩，解压](#t13)

[13.3 touch: 创建空文件](#t14)

[13.6 vim 编辑器: 创建修改文件](#t15)

[13.9 clear: 清空命令行](#t16)

[14. cat（more,less,tail）：查看文件，打印文件内容](#t17)

[15. ps：查看进程 process select](#t18)

[16. kill：杀死进程](#t19)

[17. passwd：修改密码 password](#t20)

[18. pwd：显示当前目录路径 print work directory](#t21)

[19. tee：显示并保存](#t22)

[20. reboot：重启](#t23)

## 0. tab 键：代码补全

例如输入文件夹 cd con，按 tab 键可以自动补全成该目录下 config。

## 1. ls：列出文件列表 list

**ls** 命令是**列出目录内容** (List Directory Contents) 的意思。

“**ls -l”**，简写成 **ll**。命令以**详情模式** (long listing fashion) 列出文件夹的内容。

**"ls -a"** 命令会列出文件夹里的**所有内容**，包括以 "." 开头的隐藏文件。 

![](<assets/1740016151715.png>)

注意：在 **Linux** 中，文件以 “**.**” 开头的就是隐藏文件，并且每个文件，文件夹，设备或者命令都是以文件对待。 

## 2. cd：切换目录 change directory

文件夹输到一半时候按 “tab” 键是可以自动补全的。

**cd.. **       ：退回上一级目录。

**cd /**       : 退回根目录。

**cd ～**      : 会改变工作目录为 root 目录

**cd - **       ：返回上一**次**目录 

## 3. cp：复制粘贴文件 copy

 **cp [拷贝前路径] 文件 路径 [拷贝并重命文件名]**

示例： 

![](<assets/1740016151977.png>)

## 4. mv：移动 move

## 5. rm：删除文件、目录 remove

**rm a.txt **       ：回车后输入 y 确认删除，n 取消删除

![](<assets/1740016152219.png>)

**rm -r xxx       **  删除文件或递归删除目录

**rm -f xxx**       删除目录，**无提示，不建议用**

**rm -rf xxx** 不带提示删除文件，是由 - f 和 - r 合并的

**rm -rf /***           ** 很危险，删库跑路**，无提示递归删除该路径下所有文件目录

![](<assets/1740016152502.png>)

## 6. mkdir：创建目录 make directory

**mkdir -p xxx/xxx**        ：创建多级目录

## 7. rmdir：删除空目录 remove directory

**rmdir xxx       ：删除名为 xxx 的空目录**

只能删除空目录，非空目录会报错：

![](<assets/1740016152751.png>)

先删除目录下文件再删除目录：

![](<assets/1740016152997.png>)

## 8. chown：更改所有者 change owner

## 9. chmod：更改文件的权限模式 change mode

## 10. find：查找文件

find / -name aaa.txt        ：递归查找文件 

![](<assets/1740016153385.png>)

 其他命令，引号可以去除。

![](<assets/1740016153561.png>)

**示例：**

查找 MySQL 配置文件：

```
find / -name "my.cnf"

``` 

## 11. |：管道

![](<assets/1740016153877.png>)

ls --help | more        左边是列表查看帮助信息，右边是分段回车查看文件。 

## 12. grep：查找文件内容，按行查找并匹配

![](<assets/1740016154137.png>)

## 13. tar：打包，压缩，解压

![](<assets/1740016154378.png>)

tar -cvf xxx.tar 目录 /                打包 

**tar -zcvf xxx.tar.gz 待压缩目录 /**         打包并压缩特定目录。

![](<assets/1740016154645.png>)

tar -zxvf xxx.tar.gz                 解压

解压到特定目录：

![](<assets/1740016154849.png>)

一般下载网站，linux 下载方式文件后缀名都是 tar.gz，意思是打包加压缩

![](<assets/1740016155148.png>)

## 13.3 touch: 创建空文件

![](<assets/1740016155395.png>)

## 13.6 vim 编辑器: 创建修改文件

**三种模式：**

命令行、插入、底层模式（命令行模式时按冒号）。

**进入 vim 编译器：**

`vim hello.txt` 

**vim 编辑模式：**

然后按 **i** 键进入 INSERT 进行编辑。

**vim 删除一行：**

先 esc 退出编辑模式，光标移到删除的行，输入 **dd**

**vim 删除给定范围的行**  
① 删除从第 3 行到第 5 行  
按 ESC，然后输入下面的命令，然后回车。

:3,5d

  
② 删除最后一行  
按 ESC，然后输入下面的命令，然后回车。

:$d

  
③ 删除当前行之前的所有行  
按 ESC，然后输入下面的命令，然后回车。

:1,.-1d

  
④ 删除当前行之后的所有行  
按 ESC，然后输入下面的命令，然后回车。

:.+1,$d

**vim 复制粘贴：**

先按 esc 键退出编辑模式，之后 **`yy`** 复制一行，**`p`** 粘贴一行

**vim 保存：**

先 esc 退出 insert 模式，再输入: wq 进行保存

## 13.9 clear: 清空命令行

清空命令行。输入回车即可。或者 ctrl+L

## 14. cat（more,less,tail）：查看文件，打印文件内容

如果文件较大，查看不完全要用 more，分段回车查看

**cat xxx.xxx**             ：查看文件，打印文件内容 

![](<assets/1740016155706.png>)

cat a.txt > b.txt        ：a 的内容覆盖复制粘贴到 b.txt

cat a.txt >> b.txt        ：a 的内容追加复制粘贴到 b.txt

**more xxx.txt **       ：大文件分段回车查看，按 q 或者 Ctrl+c 退出

**less xxx.txt**                ：大文件逐行查看，空格或回车或下方向键查看下一行，上方向键查看上一行，按 q 或者 Ctrl+c 退出。按 G 看最后一页，按 g 看第一页。

**tail -10 xxx.txt **               ：查看最后 10 行，数字可改，适用于看日志

**tail -n 10 xxx.txt**             ：查看最后 10 行，数字可改，适用于看日志

**tail -f xxx.txt** ：动态查看日志

**案例：**实时查看日志文件最后 100 行：

```
tail -f -n 100 zcy_backend.log


``` 

## 14.5 nohup：不挂起运行命令 no hang up

**后台运行并指定日志：** 

```
nohup /root/runoob.sh > runoob.log 2>&1 &

```

**2>&1** 解释：

将标准错误 2 重定向到标准输出 &1 ，标准输出 &1 再被重定向输入到 runoob.log 文件中。

*   0 – stdin (standard input，标准输入)
*   1 – stdout (standard output，标准输出)
*   2 – stderr (standard error，标准错误输出)

## 15. ps：查看进程 process select

![](<assets/1740016156271.png>)

ps -ef | grep ssh        查找某一进程，中间竖杠是管道，左边输入作为右边输出。 

## 16. kill：杀死进程

kill 进程号：告诉进程，你需要被关闭，请自行停止运行并退出。

kill -9 进程号：强制退出进程，表示 “无条件终止”；这个信号不能被捕获或忽略，同时接收这个信号的进程在收到这个信号时不能执行任何清理。

## 17. passwd：修改密码 password

## 18. pwd：显示当前目录路径 print work directory

![](<assets/1740016156476.png>)

## 19. tee：显示并保存

## 20. reboot：重启