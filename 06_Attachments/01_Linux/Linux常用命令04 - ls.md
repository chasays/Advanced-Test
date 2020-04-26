`ls` 命令是任何 Linux `用户都应该知道的基本命令之一`。 它用于列出有关文件系统中的文件和目录的信息。 `ls` 实用程序是安装在所有 Linux 发行版上的 Linux/Linux/Linux GNU核心工具组包的一部分。


在本教程中，我们将通过实际例子和最常见的 ls 选项的详细说明，向您展示如何使用 ls 命令。

## 如何使用ls Command 命令

`ls` 命令的语法如下:
```
ls [OPTIONS] [FILES]
```
![2b98f2](https://gitee.com/chasays/mdPic/raw/master/uPic/2b98f2.png)
当没有选项和参数时，ls 会显示当前工作目录中所有文件的名称列表:
```
ls
```
这些文件被列在字母顺序文档中:


若要列出特定目录中的文件，请将路径作为参数传递给 ls 命令。 例如，要列出/etc 目录的内容，您可以键入:
```
ls /etc
```
您还可以将多个目录和文件传递给以空格分隔的 ls 命令:
```
ls /etc/var /etc/passwd
```

如果你登录的用户没有读取该目录的权限，你会得到一条消息说 ls 不能打开该目录:
```
ls /root
ls: cannot open directory '/root': Permission denied
```

`ls` 命令有许多选项。在下面的部分中，我们将探讨最常用的选项。

## 单纯的list

`ls` 命令的默认输出只显示文件和目录的名称，这没有提供很多信息。


-l (小写l)选项使 ls 以长列表格式打印文件。

当使用长列表格式时，ls 命令将显示以下文件信息:

##  文件类型
- 文件权限
- 指向文件的硬链接数
- 文件所有者
- 文件组
- 文件大小
- 日期及时间
- 档案名称
-
考虑下面的例子:
```
ls -l /etc/hosts
-rw-r--r--  1 root  wheel  372  3  7 23:56 /etc/hosts
```
![fKIfjQ](https://gitee.com/chasays/mdPic/raw/master/uPic/fKIfjQ.png)
让我们解释一下输出中最重要的列。


第一个字符显示文件类型。 在我们的示例中，第一个字符是-，表示一个常规文件。 其他文件类型的值如下:

- `-` 普通档案
b -  阻塞特殊文件
c -  字符特殊文件
d -  目录
l -  符号链接
n - 网络档案

p - 先进先出法
s - 插座

接下来的九个字符显示文件权限。 前三个字符用于用户，后三个字符用于组，最后三个字符用于其他用户。 您可以使用 chmod 命令更改文件权限。 权限字符可以具有以下值:

r  - 读取文件的权限
w  - 写入文件的权限
x  - 执行文件的权限
s  - setgid 位
t  - 粘性钻头

在我们的示例中，rw-r -- r -- 意味着用户可以读写文件，组和其他用户只能读取文件。 权限字符后面的数字1是指向该文件的硬链接的数量。


接下来的两个字段 root 显示文件所有者和组，后面是文件的大小(337) ，以字节为单位显示。 如果要以人类可读的格式打印大小，请使用 -h 选项。 您可以使用 chown 命令更改文件所有者。

10月4日11:31是最后一次修改文件的日期和时间。

最后一列是文件的名称。

## 显示隐藏文件

默认情况下，ls 命令不会显示隐藏文件。 在 Linux 中，隐藏文件是任何以点(.)开头的文件 .

要显示包括隐藏文件在内的所有文件，请使用-a 选项:
```
ls -la ~/
drwxr-x--- 10 linuxize  linuxize  4096 Feb 12 16:28 .
drwxr-xr-x 18 linuxize  linuxize  4096 Dec 26 09:21 ..
-rw-------  1 linuxize  linuxize  1630 Nov 18  2017 .bash_history
drwxr-xr-x  2 linuxize  linuxize  4096 Jul 20  2018  bin
drwxr-xr-x  2 linuxize  linuxize  4096 Jul 20  2018  Desktop
drwxr-xr-x  4 linuxize  linuxize  4096 Dec 12  2017 .npm
drwx------  2 linuxize  linuxize  4096 Mar  4  2018 .ssh
```
##  对输出进行排序

如前所述，默认情况下 ls 命令列出了字母顺序文件。

排序选项允许你根据扩展、大小、时间和版本对输出进行排序:

--sort=extension(或-X ) 按扩展名的字母顺序排序
--sort=size /(或-S) 按文件大小排序
--sort=time  (或-t）按修改时间排序
--sort=version /(或-v) 版本号自然排序

如果希望以相反的排序顺序获得结果，请使用-r 选项。

例如，根据修改时间对/var 目录中的文件按相反的排序顺序进行排序:
```
ls -ltr /var
```
![UCDB6B](https://gitee.com/chasays/mdPic/raw/master/uPic/UCDB6B.png)
值得一提的是，ls 命令没有显示目录内容占用的总空间。 使用 du 命令获取目录的大小。

##  递归列出子目录

R 选项告诉 ls 命令递归地显示子目录的内容:
```
ls -R
```
![lONFXk](https://gitee.com/chasays/mdPic/raw/master/uPic/lONFXk.png)

## 小结

`ls` 命令列出有关文件和目录的信息。
有关 ls 的详细信息，请访问 GNU Coreutils 页面或在终端中键入 man ls。

>https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html#ls-invocation