在 `Bash` 中，有多种将文本附加到文件的方法。


要将文本附加到文件，您需要对其具有写权限。 否则，您将收到一个被拒绝的权限错误。

## ( 使用重定向操作符(>>)

重定向允许您捕获命令的输出，并将其作为输入发送到另一个命令或文件。 重定向运算符将输出追加到给定文件。


您可以使用许多命令将文本打印到标准输出并将其重定向到文件，其中 `echo` 和 `printf` 是最常用的命令。

若要将文本附加到文件，请在重定向操作符后指定文件名:
```
echo "this is a new line" >> file.txt
```
![RRU1LT](https://gitee.com/chasays/mdPic/raw/master/uPic/RRU1LT.png)
当与 `-e` 选项一起使用时，`echo `命令解释反斜杠转义字符，如换行 `\n`:
```
echo -e "this is a new line \nthis is another new line" >> file.txt
```

![TFLfuC](https://gitee.com/chasays/mdPic/raw/master/uPic/TFLfuC.png)

如果你想生成更复杂的输出，可以使用 `printf` 命令来指定输出的格式:

```
printf "Hello, I'm %s.\n" $USER >> file.txt
```
![jogmpf](https://gitee.com/chasays/mdPic/raw/master/uPic/jogmpf.png)

另一种将文本附加到文件的方法是使用 Here 文档(Heredoc)。 它是一种重定向类型，允许您将多行输入传递给命令。


例如，您可以将内容传递给 `cat` 命令，并将其附加到文件中:

cat « EOF » file.txt The current working directory is: $PWD You are logged in as: $(whoami) EOF
```
cat filename > file.txt 
```


你可以将任何命令的输出附加到文件中:
```
date +"Year: %Y, Month: %m, Day: %d" >> file.txt

```

![MDiTEX](https://gitee.com/chasays/mdPic/raw/master/uPic/MDiTEX.png)
当使用重定向附加到文件时，请注意不要使用操作符覆盖重要的现有文件。
## 方法附加到文件中tee Command 命令

`tee` 是 Linux 中的命令行实用程序，它从标准输入读取数据，并同时写入标准输出和一个或多个文件。


默认情况下，tee 命令覆盖指定的文件。 要将输出附加到文件中，可以使用 tee 和 -a (--append)选项:
```
echo "this is a new line"  | tee -a file.txt
```
![J3UYRm](https://gitee.com/chasays/mdPic/raw/master/uPic/J3UYRm.png)


如果您不希望 `tee` 写入标准输出，可以将其重定向到 /dev/null:
```
echo "this is a new line"  | tee -a file.txt >/dev/null
```

使用 tee 命令优于操作符的优点是，tee 允许您将文本一次追加到多个文件，并将其他用户拥有的文件与 sudo 一起写入。


要将文本附加到没有写权限的文件，请在 tee 之前预置 sudo，如下所示:
```
echo "this is a new line" | sudo tee -a file.txt
```
![VzQBPs](https://gitee.com/chasays/mdPic/raw/master/uPic/VzQBPs.png)

tee 接收 echo 命令的输出，提高 sudo 权限，并写入文件。


要将文本附加到多个文件，请将这些文件指定为 tee 命令的参数:
```
echo "this is a new line"  | tee -a file1.txt file2.txt file3.txt
```
![EFeqzz](https://gitee.com/chasays/mdPic/raw/master/uPic/EFeqzz.png)
## 小结

在 Linux 中，要将文本附加到文件中，可以使用重定向操作符「>」或 tee 命令。

