vim 是许多在命令行上 Linux 下首选文本编辑器。 与其他编辑器不同，vim 有几种操作模式，这对于新用户来说有点吓人。

![F8J93X](https://gitee.com/stormzhang/mdPic/raw/master/uPic/F8J93X.png)
它的前身 vi 预装在 macOS 和几乎所有的 Linux 发行版上。 了解 vim 的基本知识将帮助您在遇到您最喜欢的编辑器不可用的情况时。


用法很多，在这里就简单说下常用的操作，如何在 vim / vi 中保存文件并退出编辑器。


## vim 模式
启动 vim 编辑器时，处于正常模式。 在这种模式下，您可以使用 vim 命令并在文件中导航。


为了能够输入文本，您需要进入插入模式按下 `i` 键。 这种模式允许您以在常规文本编辑器中相同的方式插入和删除字符。左下角会提示一个 `insert`。
![kBrTGY](https://gitee.com/stormzhang/mdPic/raw/master/uPic/kBrTGY.png)

要从任何其他模式回到正常模式，只需按 `Esc` 键。

## 打开文件

使用 vim 打开文件，后面跟着要编辑或创建的文件的名称:
```
vim file.text
```


## 保存文件

在 vim 中保存文件的命令是:`w`。


要在不退出编辑器的情况下保存文件，请按 Esc 键切换回正常模式，输入:w 并按 `Enter` 键。


1. 按键盘最左上角 Esc
2. :w
3.  按下 Enter

还有一个 update 命令:up，它只在文件中有未保存的更改时才将缓冲区写入文件。


要以不同的名称保存文件，输入:w new filename，然后按 Enter 键。

## 保存文件并退出
在 vim 中保存文件并退出编辑器的命令是:wq。


要保存文件并同时退出编辑器，请按 Esc 切换到正常模式，键入:wq 并按 Enter。

1. 按键盘最左上角 Esc
2. :wq
3.  按下 Enter

![IXQ5nI](https://gitee.com/stormzhang/mdPic/raw/master/uPic/IXQ5nI.png)

另一个保存文件并退出 vim 的命令是:x。

这两个命令之间的区别在于:x 只在有未保存的更改时才将缓冲区写入文件，而:wq 总是将缓冲区写入文件并更新文件修改时间。

![H0QQTG](https://gitee.com/stormzhang/mdPic/raw/master/uPic/H0QQTG.png)

## 退出不保存文件

若要退出编辑器，不保存更改，请按 Esc 切换到正常模式，键入:q! 并按回车键。感叹号是强制的意思。

1. 按键盘最左上角 Esc
2. :q!
3.  按下 Enter

![RFMvov](https://gitee.com/stormzhang/mdPic/raw/master/uPic/RFMvov.png)

## 小结

简单的展示了如何在 vim 中保存文件并退出编辑器。 如果您是 vim 的新手，推荐一个在线的体验 vim编辑。
>https://www.openvim.com/

![N4R1hX](https://gitee.com/stormzhang/mdPic/raw/master/uPic/N4R1hX.png)
