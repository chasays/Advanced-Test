
grep 命令代表“全局正则表达式 print” ，它是 Linux 中最强大和最常用的命令之一。

![1mxfNj](https://gitee.com/stormzhang/mdPic/raw/master/uPic/1mxfNj.png)
grep 在一个或多个输入文件中搜索与给定模式匹配的行，并将每个匹配行写入标准输出。 如果没有指定文件，`grep` 将从标准输入读取，这通常是另一个命令的输出。

在本文中，我们将通过实例和对最常见的 GNU `grep` 选项的详细说明，向您展示如何使用 `grep` 命令。
![Bm1gUK](https://gitee.com/stormzhang/mdPic/raw/master/uPic/Bm1gUK.png)


## `grep`  命令语法

grep 命令的语法如下:
```
grep [OPTIONS] PATTERN [FILE...]
```
方括号中的项目是可选的。

OPTIONS - 既然可选，就是可以要可不要。
PATTERN - 搜寻模式
FILE - 零个或多个输入文件名

>为了能够搜索该文件，运行该命令的用户必须具有对该文件的读访问权。

## 搜索文件中的字符串
grep 命令最基本的用法是在文件中搜索字符串(文本)。

For example, to display all the lines containing the string bash from the /etc/passwd file, you would run the following command:

例如，要显示/etc/passwd 文件中包含字符串 bash 的所有行，可以运行以下命令:
```
grep bash /etc/passwd
```
输出应该是这样的:
```
root:x:0:0:root:/root:/bin/bash
linuxize:x:1000:1000:linuxize:/home/linuxize:/bin/bash
```
如果字符串包含空格，需要用单引号或双引号将其括起来:
```
grep "Gnome Display Manager" /etc/passwd
```
## 反相匹配(排除)

若要显示与模式不匹配的行，请使用-v (或 --invert-match)选项。

例如，要打印不包含字符串 nologin 的行，可以使用:

```
grep -v nologin /etc/passwd
root:x:0:0:root:/root:/bin/bash
colord:x:124:124::/var/lib/colord:/bin/false
git:x:994:994:git daemon user:/:/usr/bin/git-shell
linuxize:x:1000:1000:linuxize:/home/linuxize:/bin/bash
```
##  使用 `grep` 筛选命令的输出

命令的输出可以通过管道使用 `grep` 进行过滤，并且只有与给定模式匹配的行才会打印在终端上。


例如，要查找系统中作为用户 www-data 运行的进程，可以使用以下 ps 命令:
```
ps -ef | grep www-data
www-data 18247 12675  4 16:00 ?        00:00:00 php-fpm: pool www
root     18272 17714  0 16:00 pts/0    00:00:00 `grep` --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn www-data
www-data 31147 12770  0 Oct22 ?        00:05:51 nginx: worker process
www-data 31148 12770  0 Oct22 ?        00:00:00 nginx: cache manager process
```
您还可以根据命令连接多个管道。 正如您在上面的输出中看到的，还有一行包含 `grep` 进程。 如果不希望显示该行，则将输出传递给另一个 `grep` 实例，如下所示。
```
ps -ef | grep www-data | grep -v grep
www-data 18247 12675  4 16:00 ?        00:00:00 php-fpm: pool www
root     18272 17714  0 16:00 pts/0    00:00:00 `grep` --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn www-data
www-data 31147 12770  0 Oct22 ?        00:05:51 nginx: worker process
www-data 31148 12770  0 Oct22 ?        00:00:00 nginx: cache manager process
```
## 递归搜索
要递归搜索模式，可以使用 -r 选项(或 --recursive)调用 `grep`。 当使用此选项时，`grep` 将搜索指定目录中的所有文件，递归地跳过遇到的符号链接。


若要跟踪所有符号链接，请使用-R 选项，而不是-r。


下面的示例演示如何在/etc 目录中的所有文件中搜索字符串 chasays.github.io:
```
grep -r chasays.github.io /etc
```
输出将包括以文件的完整路径为前缀的匹配行:
```
/etc/hosts:127.0.0.1 node2.chasays.github.io
/etc/nginx/sites-available/chasays.github.io:    server_name chasays.github.io   www.chasays.github.io;
```
如果使用-r 选项，`grep` 将跟随所有符号链接:
``
grep -R chasays.github.io /etc
``
注意下面输出的最后一行。 当使用-rmr 调用 `grep` 时，不会打印该行，因为 Nginx 启用站点的目录中的文件是到 sites-available 目录中的配置文件的符号链接。

```
/etc/hosts:127.0.0.1 node2.chasays.github.io
/etc/nginx/sites-available/chasays.github.io:    server_name chasays.github.io   chasays.github.io;
/etc/nginx/sites-enabled/chasays.github.io:    server_name chasays.github.io   chasays.github.io;
```
##  只显示文件名
若要禁止默认 `grep` 输出并只打印包含匹配模式的文件名，请使用-l (或 --files-with-matches)选项。


下面的命令搜索所有以。 在当前工作目录中输出包含字符串 linuxize. com 的文件名:
```
grep -l chasays.github.io *.conf
```
输出结果如下:
```
tmux.conf
haproxy.conf
```
The -l option is usually used in combination with the recursive option -R:

-l 选项通常与递归选项 -R 结合使用:
```
grep -Rl chasays.github.io /tmp
```
## 不区分大小写的搜索

默认情况下，`grep` 区分大小写，这意味着大小写字符被视为不同字符。


若要在搜索时忽略大小写，请使用-i 选项(或 --ignore-case)调用 `grep`。


例如，当搜索没有任何选项的 Zebra 时，下面的命令不会显示任何输出，即有匹配的行:
```
grep Zebra /usr/share/words
```
但是如果使用-i 选项执行不区分大小写的搜索，它将匹配大小写字母:
```
grep -i Zebra /usr/share/words
```
指定“ Zebra”将匹配“ Zebra”、“ Zebra”或该字符串的任何其他大小写字母组合。
```
zebra
zebra's
zebras
```

## 搜索全文
在搜索字符串时，`grep` 将显示字符串嵌入较大字符串中的所有行。


例如，如果搜索“ gnu” ，所有“ gnu”嵌入在较大单词中的行，如“ cygnus”或“ magnum”将被匹配:
```
grep gnu /usr/share/words

cygnus
gnu
interregnum
lgnu9d
lignum
magnum
magnuson
sphagnum
wingnut
```

若要仅返回指定字符串为整个单词(由非单词字符括起来)的那些行，请使用-w (或 --word-regexp)选项。

>字符包括字母数字字符(a-z, A-Z, and ，及0-9)  ( )及下划线(_). 所有其他字符都视为非字符

如果您运行与上面相同的命令(包括 -w 选项) ，`grep` 命令将只返回 gnu 作为单独的单词包含的那些行。
```
grep -w gnu /usr/share/words
gnu
```

##  显示行号

-n (或 --line-number)选项告诉 `grep` 显示包含与模式匹配的字符串的行的行号。 使用此选项时，`grep` 将匹配内容打印到以行号为前缀的标准输出。


例如，要显示/etc/services 文件中包含以匹配行号作为前缀的字符串 bash 的行，可以使用以下命令:
```
grep -n 10000 /etc/services
```
下面的输出显示匹配项在第10423和10424行。
```
10423:ndmp            10000/tcp
10424:ndmp            10000/udp
```

## 计数匹配

若要将匹配行数打印到标准输出，请使用 -c (或 --count)选项。


在下面的示例中，我们计算了将/usr/bin/zsh 作为 shell 的帐户数量。
```
grep -c '/usr/bin/zsh' /etc/passwd

4
```

## 安静模式

Q (或 --quiet)告诉 `grep` 在安静模式下运行，不要在标准输出上显示任何内容。 如果找到匹配项，则该命令退出状态为0。 在 shell 脚本中使用 `grep` 时，这非常有用，您希望检查文件是否包含字符串，并根据结果执行特定操作。


下面是一个在静默模式下使用 `grep` 作为 if 语句中的测试命令的示例:
```
if `grep` -q PATTERN filename
then
    echo pattern found
else
    echo pattern not found
fi
```



## 基本正则表达式

Gnu`grep` 有三个正则表达式特性集，Basic、 Extended 和 perl 兼容。


默认情况下，`grep` 将模式解释为基本正则表达式，其中除元字符外的所有字符实际上都是匹配自身的正则表达式。


下面是最常用的元字符列表:


使用 ^ (插入符号)符号来匹配行开头的表达式。 在下面的示例中，只有当字符串 kangaroo 出现在行的开头时，它才会匹配。
```
grep "^kangaroo" file.txt
```
使用 $(dollar)符号来匹配行尾的表达式。 在下面的示例中，只有当字符串 kangaroo 出现在行的末尾时，它才会匹配。
```
grep "kangaroo$" file.txt
```

使用。 (句号)符号来匹配任何单个字符。 例如，要匹配以 kan 开头，然后有两个字符和以字符串 roo 结尾的任何内容，您可以使用以下模式:
```
grep "kan..roo" file.txt
```

使用[](方括号)匹配括在方括号中的任何单个字符。 例如，找到包含 accept 或者 accent 的行，你可以使用以下模式:
```
grep "acce[np]t" file.txt
```

使用[ ^ ](方括号)匹配括在方括号中的任何单个字符。 下面的模式将匹配包含 co (除了 l 以外的任何字母) a 的任何字符串组合，如可可、钴等，但不匹配包含可乐的线,
```
grep "co[^l]a" file.txt

```
若要转义下一个字符的特殊含义，请使用(反斜杠)符号。

##  扩展的正则表达式

若要将模式解释为扩展正则表达式，请使用-e (或 --extended-regexp)选项。 扩展的正则表达式包括所有基本元字符，以及用于创建更复杂、更强大的搜索模式的附加元字符。 以下是一些例子:


匹配并提取给定文件中的所有电子邮件地址:
```
grep -E -o "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,6}\b" file.txt
```
匹配并提取给定文件中的所有有效 IP 地址:
```
grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' file.txt
```
O 选项仅用于打印匹配的字符串。

## 搜索多个字符串(模式)
可以使用 OR 操作符 | 连接两个或多个搜索模式。


默认情况下，`grep` 将模式解释为一个基本的正则表达式，其中 | 等元字符失去了它们的特殊含义，必须使用它们的反斜线版本。


在下面的例子中，我们正在 Nginx 日志错误文件中搜索出现的词汇 fatal，error，critical:
```
grep 'fatal\|error\|critical' /var/log/nginx/error.log
```
如果使用扩展正则表达式选项-e，则不应转义运算符 | ，如下所示:
```
grep -E 'fatal|error|critical' /var/log/nginx/error.log
```
##  在匹配之前打印行

若要在匹配行之前打印特定行数，请使用-b (或 --before-context)选项。


例如，要在匹配行之前显示五行前导上下文，可以使用以下命令:
```
grep -B 5 root /etc/passwd
```
![zwEsXm](https://gitee.com/stormzhang/mdPic/raw/master/uPic/zwEsXm.png)
##  匹配后打印行

若要在匹配行之后打印特定行数，请使用 -a (或 --after-context)选项。


例如，要在匹配行之后显示五行尾随上下文，可以使用以下命令:
```
grep -A 5 root /etc/passwd
```
![hnWXx0](https://gitee.com/stormzhang/mdPic/raw/master/uPic/hnWXx0.png)
## 小结

grep 命令允许您在文件内搜索模式。 如果找到匹配项，`grep` 将打印包含指定模式的行。
在 `grep` 用户手册页面上有很多关于 `grep` 的信息。



>https://www.gnu.org/software/`grep`/manual/`grep`.html