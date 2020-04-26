`rm` 是一个命令行工具，用于删除文件和目录。 这是每个 Linux 用户都应该熟悉的基本命令之一。


在本指南中，我们将通过最常见的 rm 选项的示例和说明来解释如何使用 rm 命令。

## 如何使用 `rm ` 命令

`rm` (remove)命令的一般语法如下:
```
rm [OPTIONS]... FILE...
```
![H9a2mr](https://gitee.com/chasays/mdPic/raw/master/uPic/H9a2mr.png)

默认情况下，当在没有任何选项的情况下执行时，rm 不删除目录，也不提示用户是否继续删除给定的文件。

若要删除单个文件，请使用 rm 命令后跟文件名作为参数:
```
rm filename
```

如果您在父目录上没有写权限，将会出现“ Operation not permitted”错误。

如果文件没有写保护，它将在没有通知的情况下删除。 在成功时，该命令不产生任何输出，并返回零。


当删除写保护文件时，命令会提示您进行确认，如下所示:
```
rm: remove write-protected regular empty file 'filename'?
```

键入` y `并按回车键可以删除该文件。


`-f `选项告诉 rm 永远不要提示用户并忽略不存在的文件和参数。
```
rm -f filename

```

如果您想获得有关正在删除的内容的信息，请使用-v (verbose)选项:
```
rm -v filename
 'filename'
```
![Hg72Uy](https://gitee.com/chasays/mdPic/raw/master/uPic/Hg72Uy.png)
##  删除多个文件
与 unlink 命令不同，rm 允许您一次删除多个文件。 为此，将文件名作为空格分隔的参数传递:
```
rm filename1 filename2 filename3
```

您可以使用正则表达式来匹配多个文件。 例如，删除所有。 在 png 文件的工作目录中，你可以输入:
```
rm *.png
```

使用正则表达式时，在运行 rm 命令之前。 使用 ls 命令列出文件始终是一个好主意，这样可以看到哪些文件将被删除。

## 删除目录(文件夹)

要删除一个或多个空目录，请使用 -d 选项:
```
rm -d dirname
```

`rm` -d 在功能上与 `rmdir` 命令相同。


要递归地删除非空目录及其中的所有文件，请使用 -r (递归)选项:
```
rm -r dirname
```
##  移除前提示

`-i` 选项告诉 rm 在删除每个文件之前提示用户:
```
rm -i filename1 filename2
```

要确认类型 y 并按回车键:
```
rm: remove regular empty file 'filename1'? 
rm: remove regular empty file 'filename2'? 
```


当移除三个以上的文件或递归移除一个目录时，为了得到整个操作的单个提示，使用-i 选项:
```
rm -i filename1 filename2 filename3 filename4
```
![dSjnFp](https://gitee.com/chasays/mdPic/raw/master/uPic/dSjnFp.png)
您将被要求确认删除所有给定的文件和目录:
```
rm -rf
```
如果给定的目录或目录中的文件是写保护的，rm 命令将提示您确认操作。 若要在没有提示的情况下删除目录，请使用-f 选项:
```
rm -rf dirname
```

`rm -rf` 命令非常危险，应该非常谨慎地使用！

## 小结

我们已经向您展示了如何使用 linuxrm 命令从 Linux 系统中删除文件和目录。

`删除重要文件或目录时要格外小心，因为一旦文件被删除，就无法轻易恢复`。

