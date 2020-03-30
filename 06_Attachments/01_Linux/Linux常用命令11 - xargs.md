xargs 实用程序允许您从标准输入构建和执行命令。 它通常通过管道与其他命令组合使用。


使用 xargs，可以将标准输入作为参数提供给 mkdir 和 rm 等命令行实用程序。


## 如何使用 xargs 命令

xargs 从标准输入中读取参数(由空格或换行符分隔) ，并使用输入作为命令的参数执行指定的命令。 如果没有提供命令，则默认为/bin/echo。

xargs 命令的语法如下:
```
xargs [OPTIONS] [COMMAND [initial-arguments]]
```

使用 xargs 的最基本示例是使用管道向 xargs 传递以空格分隔的几个字符串，并运行一个将这些字符串用作参数的命令。
```
echo "file1 file2 file3" | xargs touch
```



在上面的示例中，接下来将标准输入管道输送到 xargs，并为每个参数运行 touch 命令，创建三个文件。 这和你跑步的时候是一样的:
```
touch file1 file2 file3
```

### 如何查看命令和提示用户

要在执行命令之前在终端上打印该命令，请使用-t (--verbose)选项:
```
echo  "file1 file2 file3" | xargs -t touch
```
```
touch file1 file2 file3
```


如果您希望得到一个提示，在执行每个命令之前是否运行它，请使用-p (--interactive)选项:
```
echo  "file1 file2 file3" | xargs -p touch
```

键入 y 或 Y 以确认并运行命令:
```
touch file1 file2 file3 ?...y
```

此选项在执行破坏性命令时非常有用, 比如 `rm`，还有这个命令千万不要在服务器上运行

## 如何限制参数的数量
默认情况下，传递给命令的参数数量由系统的限制决定。


- n (--max-args)选项指定传递给给定命令的参数数目。 xargs 根据需要多次运行指定的命令，直到所有参数都用完为止。


在下面的示例中，从标准输入中读取的参数数目被限制为1。
```
echo  "file1 file2 file3" |  xargs -n 1 -t touch
```

从下面的详细输出中可以看到，touch 命令针对每个参数分别执行:
```
touch file1
touch file2
touch file3
```

## 如何运行多个命令
要使用 xargs 运行多个命令，请使用-i 选项。 它通过在-i 选项后定义一个 replace-str 来工作，并且所有 replace-str 的出现都被传递给 xargs 的参数替换。


下面的 xargs 示例将运行两个命令，首先使用 touch 创建文件，然后使用 ls 命令列出文件:
```
echo "file1 file2 file3" | xargs -t -I % sh -c '{ touch %; ls -l %; }'
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file1
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file2
-rw-r--r-- 1 linuxize users 0 May  6 11:54 file3
```

`需要注意的是在MacOS上并没有-i这个参数，只有-I，用法差不多。`
Replace-str 的一个常见选择是% 。但是，您可以使用另一个占位符，例如 ARGS:
```
echo "file1 file2 file3" | xargs -t -I ARGS sh -c '{ touch ARGS; ls -l ARGS; }'
```

##  如何指定分隔符

使用-d (--delimiter)选项设置自定义分隔符，可以是单个字符，也可以是以开始的转义序列。


接下来正在使用下面的示例作为分隔符:
```
echo  "file1;file2;file3" | xargs -d \; -t touch
```
```
touch file1 file2 file3
```
##  如何从文件中读取项目
xargs 命令还可以从文件而不是标准输入中读取项。 为此，请使用-a (--arg-file)选项后跟文件名。


在下面的示例中，xargs 命令将读取 ips.txt 文件并 ping 每个 IP 地址。
```
ips.txt
8.8.8.8
1.1.1.1
```


接下来还使用-l1选项，它指示 xargs 一次读取一行。 如果省略此选项，xargs 将把所有 ip 传递给单个 ping 命令。
```
xargs -t -L 1 -a ips.txt ping -c 1
ping -c 1 8.8.8.8 
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=50 time=68.1 ms

...
ping -c 1 1.1.1.1 
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=59 time=21.4 ms

```
## 使用xargs 与find

xargs 最常与 find 命令结合使用。 您可以使用 find 搜索特定的文件，然后使用 xargs 对这些文件执行操作。


为了避免包含换行符或其他特殊字符的文件名出现问题，始终使用 find-print0选项，这会导致 find 打印完整的文件名后面跟一个空字符。 xargs 可以使用-0，(-null)选项正确地解释这个输出。

在下面的示例中，find 将打印/var/www/中所有文件的完整名称。 Cache directory 和 xargs 将把文件路径传递给 rm 命令:
```
find /var/www/.cache -type f -print0 | xargs -0 rm -f
```
## 使用 xargs 修剪空白字符

xargs 还可以用作从给定字符串的两侧删除空格的工具。 只需通过管道将字符串传递给 xargs 命令，它就会执行修整操作:
```
echo "  Long line " | xargs
Long line
```
这在比较 shell 脚本中的字符串时非常有用。
```
#!/bin/bash
VAR1=" chasays "
VAR2="chasays"

if [[ "$VAR1" == "$VAR2" ]]; then
    echo "Strings are equal."
else
    echo "Strings are not equal."
fi
## Using xargs to trim VAR1
if [[ $(echo "$VAR1" | xargs) == "$VAR2" ]]; then
    echo "Strings are equal."
else
    echo "Strings are not equal."
fi
```

```
Strings are not equal.
Strings are equal.
```
## 小结
xargs 是 Linux 上的命令行实用工具，能够搭配其他命令，使用出惊人的效果。