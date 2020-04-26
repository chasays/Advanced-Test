在处理文本文件时，通常需要在一个或多个文件中查找和替换文本字符串。


`sed` 是一个流编辑器。 它可以对文件和输入流(如管道)执行基本的文本操作。 使用 `sed，您可以搜索、查找和替换、插入和删除单词和行。` 它支持基本的和扩展的正则表达式，允许您匹配复杂的模式。


接下来， 我将使用 sed 查找和替换字符串。 我还将向您展示如何执行递归搜索和替换。

## 查找和替换字符串sed

sed 有几个版本，它们之间有一些函数上的差异。 Macos 使用的是 BSD 版本，而且大多数 Linux 发行版默认都预装了 GNU。 下面默认的是 GNU 版本。

使用 sed 搜索和替换文本的一般形式如下:
```
sed -i 's/SEARCH_REGEX/REPLACEMENT/g' INPUTFILE
```

- -i  将其输出写入标准输出sed 
- s 替代命令，可能是 sed 中使用最多的命令
-/分隔符字符。它可以是任何字符，但通常是斜杠(/) 字符
- SEARCH_REGEX 要搜索的普通字符串或正则表达式
- REPLACEMENT 替换字符串
- g  全局替换标志。默认情况下一行一行地读取文件，只更改第一次出现的SEARCH_REGEX，当提供替换标志时，所有出现的情况都将被替换
- INPUTFILE 要在其上运行命令的文件名
最好在参数周围加上引号，这样 shell 元字符就不会扩展。


让我看一些示例，说明如何使用 sed 命令搜索文件中的文本，并使用其中一些最常用的选项和标志替换文件中的文本。


为了便于演示，我将使用以下文件 file.txt:
```

123 Foo foo foo 
foo /bin/bash Ubuntu foobar 456
```


如果省略了` g` 标志，那么每行中搜索字符串的第一个实例将被替换:
```
sed -i '' 's/foo/linux/' file.txt

123 Foo linux foo 

linux /bin/bash Ubuntu foobar 456
```
![I1CZtI](https://gitee.com/chasays/mdPic/raw/master/uPic/I1CZtI.png)
使用全局替换标志 sed 替换所有出现的搜索模式:
```
sed -i '' 's/foo/linux/g' file.txt
123 Foo linux linux
linux /bin/bash Ubuntu linuxbar 456
```
![XSP3mF](https://gitee.com/chasays/mdPic/raw/master/uPic/XSP3mF.png)


正如您可能已经注意到的，在前面的示例中，foobar 字符串中的子字符串 foo 也被替换了。 如果这不是想要的行为，请在搜索字符串的两端使用单词边界表达式(\b)。 这将确保部分词不匹配。
```
sed -i  's/\bfoo\b/linux/g' file.txt
123 Foo linux linux
linux /bin/bash Ubuntu foobar 456
```

![lCjovE](https://gitee.com/chasays/mdPic/raw/master/uPic/lCjovE.png)

若要使模式匹配不区分大小写，请使用 I 标志。 在下面的例子中，我同时使用了 g 和 I 标志:
```
sed -i  's/foo/linux/gI' file.txt
123 linux linux linux 
linux /bin/bash Ubuntu linuxbar 456
```



如果要查找和替换包含分隔符(/)的字符串，则需要使用反斜杠(\\)来转义斜杠。 例如，用/usr/bin/zsh 替换/bin/bash
```
sed -i '' 's/\/bin\/bash/\/usr\/bin\/zsh/g' file.txt
```


更简单和更易读的选项是使用另一个分隔符字符。 大多数人使用竖线(|)或冒号(:) ，但你可以使用任何其他字符:
```
sed -i 's|/bin/bash|/usr/bin/zsh|g' file.txt
123 Foo foo foo 
foo /usr/bin/zsh Ubuntu foobar 456
```


您还可以使用正则表达式。 例如，搜索所有的3位数字，并将它们替换为您将使用的字符串数字:
```
sed -i 's/\b[0-9]\{3\}\b/number/g' file.txt
number Foo foo foo 
foo /bin/bash demo foobar number
```
sed 的另一个有用特性是，您可以使用与匹配模式相对应的 & 符号。 该字符可以被多次使用。


例如，如果要在每个3位数字周围加上大括号{} ，请键入:
```
sed -i 's/\b[0-9]\{3\}\b/{&}/g' file.txt
{123} Foo foo foo 
foo /bin/bash demo foobar {456}
```


最后但并非最不重要的一点是，在使用 sed 编辑文件时进行备份总是一个好主意。 要做到这一点，只需要为 -i 选项提供一个扩展。 例如，要编辑 file.txt 并将原始文件保存为 file.txt.bak，可以使用:
```
sed -i.bak 's/foo/linux/g' file.txt
```


如果你想确保备份已经创建，用 ls 命令列出文件:
```
ls
file.txt file.txt.bak
```

##  递归查找和替换

有时，您希望递归地搜索目录中包含字符串的文件，并替换所有文件中的字符串。 这可以通过使用 find 或 grep 等命令递归地查找目录中的文件并将文件名管道化为 sed 来实现。

下面的命令将递归搜索当前工作目录文件夹中的文件，并将文件名传递给 sed。
```
find . -type f -exec sed -i 's/foo/bar/g' {} +
```

为了避免文件名中包含空格的问题，可以使用-print0选项，它告诉 find 打印文件名，然后使用空字符，并使用 xargs-0将输出管道传送到 sed:
```
find . -type f -print0 | xargs -0 sed -i 's/foo/bar/g'
```


要排除目录，请使用非路径选项。 例如，如果您正在替换本地 git repo 中的字符串，以排除所有以点(.)开头的文件 、使用:
```
find . -type f -not -path '*/\.*' -print0 | xargs -0 sed -i 's/foo/bar/g'
```

如果你只想搜索和替换具有特定扩展名的文件中的文本，你可以使用:
```
find . -type f -name "*.md" -print0 | xargs -0 sed -i 's/foo/bar/g'
```


另一种选择是使用 grep 命令递归地查找包含搜索模式的所有文件，然后将文件名通过管道传递给 sed:
```
grep -rlZ 'foo' . | xargs -0 sed -i.bak 's/foo/bar/g'
```
## 小结
虽然它看起来复杂和复杂，但实际上，用 sed 在文件中搜索和替换文本非常简单。