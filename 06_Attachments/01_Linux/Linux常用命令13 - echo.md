`echo` 命令是 Linux 中`最基本和最常用`的命令之一。 传递给 echo 的参数被打印到标准输出中。


`echo` 通常用于 shell 脚本中，用于显示消息或输出其他命令的结果。

## echo 命令
`echo` 是 Bash 和其他大多数流行的 shell，如 Zsh 和 Ksh 中的一个 shell 内置程序。 它的行为在不同的 shell 中略有不同。


还有一个独立的/usr/bin/echo 实用程序，但通常会优先使用 shell 内置版本。 我们将介绍 Bash 内置版本的 echo。

![wNoubz](https://gitee.com/stormzhang/mdPic/raw/master/uPic/wNoubz.png)

`echo` 命令的语法如下:
```
echo [-neE] [ARGUMENTS]
```

- 当-n 选项，则取消尾随换行符
- 如果-e 选项，则将解释以下反斜杠转义字符:
- \\  显示反斜杠字符
- \a 警报(BEL)
- \b 显示退格字符
- \c  禁止任何进一步的输出
- \e 显示转义字符
- \f 显示窗体提要字符
- \n  显示新行
- \r 显示回车
- \t 显示水平标签
- \v  显示垂直标签
- 这个-E 项禁用转义字符的解释。这是默认值

在使用 echo 命令时`，不过有几点需要考虑`。

方法传递参数之前，shell 将替换所有变量、通配符匹配和特殊字符echo. 命令
 虽然没有必要，但是将传递给的参数包含起来是一个很好的编程实践双引号或单引号
当使用单引号时`''` 将保留引号内每个字符的字面值。不展开变量和命令
## 举个栗子

下面的例子展示了如何使用 echo 命令:


在标准输出上显示一行文本。
```
echo Hello, World!
Hello, World!
```

显示一行包含双引号的文本。


若要打印双引号，请将其包含在单引号内，或用反斜杠字符进行转义。
```
echo 'Hello "Linuxize"'
echo "Hello \"Linuxize\""
Hello "Linuxize"
```


显示一行包含单引号的文本。


要打印单引号，请将其包含在双引号内或使用 ANSI-C 引号。
```
echo "I'm a Linux user."
echo $'I\'m a Linux user.'
I'm a Linux user
```

显示包含特殊字符的消息。


使用-e 选项启用转义字符的解释。
```
echo -e "You know nothing, Jon Snow.\n\t- Ygritte"
You know nothing, Jon Snow.
    - Ygritte
```

模式匹配字符。


`echo` 命令可以与模式匹配字符一起使用，比如通配符。 例如，下面的命令将返回所有。 工作目录中的 php 文件。
```
echo The PHP files are: *.php
The PHP files are: index.php contact.php functions.php
```


重定向到一个文件


您可以使用，操作符将输出重定向 > 或者 >> 到一个文件，而不是显示在屏幕上。

```
echo -e 'The only true wisdom is in knowing you know nothing.\nSocrates' >> /tmp/file.txt
```

如果 file.txt 不存在，命令将创建它。 当使用该文件时将被覆盖，而将把输出附加到该文件。


使用 cat 命令查看文件内容:
```
cat /tmp/file.txt
The only true wisdom is in knowing you know nothing.
Socrates
```
Displaying variables

显示变量


`echo` 还可以显示变量。在下面的示例中，我们将输出当前登录用户的名称:
```
echo $USER
admin
```
![ZR4XQn](https://gitee.com/stormzhang/mdPic/raw/master/uPic/ZR4XQn.png)

$USER 是一个保存用户名的 shell 变量。


显示命令的输出


使用 $(command)表达式将命令输出包含在 echo 的参数中。 下面的命令将显示当前日期:
```
echo "The date is: $(date +%D)"
The date is: 04/01/20
```
![gVMB8U](https://gitee.com/stormzhang/mdPic/raw/master/uPic/gVMB8U.png)

以彩色显示


使用 ANSI 转义序列更改前景色和背景色或设置文本属性，如下划线和粗体。

- echo -e "\033[1;37mWHITE"
- echo -e "\033[0;30mBLACK"
- echo -e "\033[0;34mBLUE"
- echo -e "\033[0;32mGREEN"
- echo -e "\033[0;36mCYAN"
- echo -e "\033[0;31mRED"
- echo -e "\033[0;35mPURPLE"
- echo -e "\033[0;33mYELLOW"
- echo -e "\033[1;30mGRAY"

## 小结
By now, you should have a good understanding of how the echo command works.

现在，您应该已经很好地理解了 echo 命令是如何工作的。