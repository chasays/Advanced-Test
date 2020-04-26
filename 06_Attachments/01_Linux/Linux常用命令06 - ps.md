在 Linux 中，程序的运行实例称为进程。 有时候，在 Linux 机器上工作时，您可能需要了解当前正在运行的进程。


您可以使用许多命令来查找正在运行的进程的信息，其中 ps、 pstree 和 top 是最常用的命令。


本文说明如何使用 ps 命令列出当前正在运行的进程并显示有关这些进程的信息。

## 如何使用 ps 命令

`ps` 命令的一般语法如下:
```
ps [OPTIONS]
```
出于历史和兼容性原因，`ps` 命令接受几种不同类型的选项:

- 样式选项，前面加一个破折号
- 风格的选项，使用无破折号
- 长选项，前面加两个破折号


不同的选项类型可以混合使用，但在某些特定情况下，可能会出现冲突，因此最好坚持使用一种选项类型。

可以对 BSD 和 UNIX 选项进行分组。


在最简单的形式中，当不使用任何选项时，ps 将为当前 shell 中运行的至少两个进程、 shell 本身以及调用命令时在 shell 中运行的进程打印四列信息。

## ps

输出包括有关 shell (bash)和在此 shell 中运行的进程的信息(ps，您键入的命令) :
```
  PID TTY          TIME CMD
 1809 pts/0    00:00:00 bash
 2043 pts/0    00:00:00 ps

这四列分别标记为 PID、 TTY、 TIME 和 CMD。
```
PID - 进程 ID。通常，当运行ps 命令时，用户寻找的最重要的信息是进程阻止一个故障进程.
TTY - 进程控制终端的名称
TIME - 进程的累计 CPU 时间，以分钟和秒表示
CMD - 用于启动进程的命令的名称

上面的输出不是很有用，因为它不包含很多信息。 ps 命令的真正威力来自于附加选项的启动。

`ps` 命令接受大量的选项，这些选项可用于显示特定的一组进程和关于进程的不同信息，但是在日常使用中只需要少量的选项。


`ps` 最常用于以下组合选项:


## BSD 表格:
```
ps aux
```
a - 这个 `a` 参数显示所有用户的进程。 只有与终端和组长的进程没有关联的进程没有显示
u - 代表一种面向用户的格式，它提供有关流程的详细信息
x - 列出没有控制终端的进程。这些进程主要是在启动时启动的running in the background 在后台运行.

该命令在十一列中显示信息，分别标记为 USER、 PID、% CPU、% MEM、 VSZ、 RSS、 STAT、 START、 TTY、 TIME 和 CMD。
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.8  77616  8604 ?        Ss   19:47   0:01 /sbin/init
root         2  0.0  0.0      0     0 ?        S    19:47   0:00 [kthreadd]
...
```

我们已经解释了 PID、 TTY、 TIME 和 CMD 标签，下面是其他标签的说明:

- USER - 运行进程的用户
- %CPU - CPU 进程的利用
- %MEM - 进程的驻留设置大小占计算机上物理内存的百分比
-  VSZ - KiB 中进程的虚拟内存大小
- RSS - 这个过程正在使用物理内存大小
- STAT - 进程状态代码，例如Z (zombie), (僵尸),S (sleeping), and (休眠) ，以及R (running). (运行)
- START - 命令开始的时间

选项告诉 ps 显示父进程到子进程的树视图:
```
ps auxf
```
The ps command also allows you to sort the output. For example, to sort the output based on the memory usage, you would use:

`ps` 命令还允许对输出进行排序。 例如，要根据内存使用情况对输出进行排序，可以使用:
```
ps aux --sort=-%mem
```

## Unix 格式:
```
ps -ef
```

- -e 显示所有进程
- -f 列出了完整格式的列表，它提供了有关进程的详细信息

该命令以八列显示信息，分别标记为 UID、 PID、 PPID、 c、 STIME、 TIME 和 CMD。
```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 19:47 ?        00:00:01 /sbin/init
root         2     0  0 19:47 ?        00:00:00 [kthreadd]
...
```

上面没有提到的标签有以下含义:

- UID 运行进程的用户
- PPID 父进程的 ID
- C 进程 CPU 利用率
- STIME 当命令开始的时候

若要只查看作为特定用户运行的进程，请键入以下命令，其中 linuxize 是用户名:
```
ps -f -U linuxize -u linuxize
```

## 自定义格式

O 选项允许您指定在运行 ps 命令时显示哪些列。


例如，为了只打印关于 PID 和 COMMAND 的信息，你可以运行以下命令之一:
```
ps -efo pid,comm
ps auxo pid,comm
```

![2OZIr6](https://gitee.com/chasays/mdPic/raw/master/uPic/2OZIr6.png)

## 使用其他命令

`ps` 可以通过管道与其他命令组合使用。

如果你想显示 ps 命令的输出，一次一页，通过管道将它传送到 less 命令:
```
ps -ef | less
```
![4Z1rEY](https://gitee.com/chasays/mdPic/raw/master/uPic/4Z1rEY.png)

`ps` 命令的输出可以用 grep 进行过滤。 例如，为了只显示属于 root 用户的进程，你可以运行:
```
ps -ef | grep root
```
![3p1Yi8](https://gitee.com/chasays/mdPic/raw/master/uPic/3p1Yi8.png)

## 小结
`ps` 命令是解决 Linux 系统问题时最常用的命令之一。 它有许多选项，但通常大多数用户使用 ps aux 或 ps-ef 来收集有关正在运行的进程的信息。

有关 `ps` 的详细信息，请在终端中键入 `man ps`。
![kXnTbz](https://gitee.com/chasays/mdPic/raw/master/uPic/kXnTbz.png)