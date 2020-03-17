移动文件和目录是您在 Linux 系统上经常需要执行的最基本的任务之一。

`mv` 命令(简称 move)用于将文件和目录从一个位置重命名并移动到另一个位置。 命令的语法如下:

```
mv [OPTIONS] SOURCE DESTINATION

```
Source `可以是一个或多个文件或目录`，DESTINATION `只能是是单个文件或目录`。

- 当`多个文件`或目录作为SOURCE, the 、`DESTINATION必须是一个目录`。所以文件被移动到目标目录
- 如果将单个文件指定为SOURCE, 目标是一个现有的目录，然后该文件被移动到指定的目录。
- 如果将单个文件指定为SOURCE, 一个单一的文件作为那么你就是目标`重命名文件`.
- 当SOURCE是一个目录，DESTINATION 根本不存在,SOURCE将被改名为DESTINATION. 反之，它被移动到`内部DESTINATION目录`

Note: 要移动文件或目录，您需要对 SOURCE 和 DESTINATION 都具有写权限。 否则，您将收到一个被拒绝的权限错误。

# Talk is cheap

# 简单用法

例如，要将文件 file1从当前工作目录文件夹移动到 / tmp 目录，您可以运行:

```

mv file1 /tmp
```

要重命名一个文件，你需要指定目标文件名:


```
mv file1 file2

```

移动目录的语法与移动文件时相同。 在下面的示例中，如果 dir2目录存在，则该命令将 dir1移动到 dir2中。 如果 dir2不存在，dir1将被重命名为 dir2:
```
mv dir1 dir2
```

## 移动多个文件和目录

若要移动多个文件和目录，请指定要移动的文件作为源文件。 例如，要将 file1和 file2文件移动到 dir1目录，您可以输入:
```
mv file1 file2 dir1
```

命令也允许你使用模式匹配。 例如，要将所有 pdf 文件从工作目录目录移动到 ~ / Documents 目录，你可以使用:
```
mv *.pdf ~/Documents
```

## 参数

`mv` 命令接受几个影响默认命令行为的选项。

在某些 Linux 发行版中，mv 可能是 mv 命令的别名，并带有一组自定义选项。 例如，在 CentOS 中，mv 是 mv-i 的别名。 您可以使用 type 命令查看 mv 是否是别名:
```
type mv 
```
![aF0FBl](https://gitee.com/stormzhang/mdPic/raw/master/uPic/aF0FBl.png)

如果 mv 是别名，输出结果如下:
```
mv is aliased to `mv -i'
```

如果给出了冲突的选项，则最后一个选项优先。

## 覆盖前的提示符

默认情况下，如果目标文件存在，它将被覆盖。要提示确认，使用-i 选项:
```
mv -i file1 /tmp
```

返回结果就是
```
mv: overwrite '/tmp/file1'?

```
覆盖文件类型 y 或 Y。

## 强制覆盖


如果您尝试覆盖只读文件，mv 命令将提示您是否要覆盖该文件:

```
mv -i file1 /tmp
mv: replace '/tmp/file1', overriding mode 0400 (r--------)? 
```
为了避免被提示，请使用-f 选项:
```
mv -f file1 /tmp

```

当您需要覆盖多个只读文件时，此选项特别有用。

## 不要覆盖现有的文件

n 选项告诉 mv 永远不要覆盖任何现有文件:
```
mv -f file1 /tmp
```

如果文件1存在，上面的命令将不执行任何操作，否则它将把文件移动到 / tmp 目录。

##  备份文件

如果目标文件存在，您可以使用-b 选项创建它的备份:
```
mv -b file1 /tmp
```
备份文件将具有与原始文件相同的名称，并附加一个波浪号(~)。


使用 ls 命令验证备份是否已创建:

```ls /tmp/file1*
/tmp/file1  /tmp/file1~
```
##  详细输出

另一个可能有用的选项是-v。 当使用此选项时，命令输出每个移动文件的名称:
```
mv -i file1 /tmp
renamed 'file1' -> '/tmp/file1'
```

## 小结

`mv` 命令用于移动和重命名文件和目录。
有关 mv 命令的详细信息，请查看手册页或在终端中键入` man mv`。
![zquN8k](https://gitee.com/stormzhang/mdPic/raw/master/uPic/zquN8k.png)
被命令行吓到的新 Linux 用户可以使用 GUI 文件管理器来移动他们的文件。

