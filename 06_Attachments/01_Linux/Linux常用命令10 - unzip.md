`zip` 是最广泛使用的归档文件, 除了linux，windows也是非常的广泛。，支持无损数据压缩。 zip 文件是包含一个或多个压缩文件或目录的数据容器。


接下来，我将解释如何使用 `unzip` 命令通过命令行解压缩 Linux 系统中的文件。
还有与之对应就是 zip。
![0FPVdt](https://gitee.com/chasays/mdPic/raw/master/uPic/0FPVdt.png)

## 安装unzip
在大多数 Linux 发行版中，unzip 不是默认安装的，但是您可以使用您的发行版的包管理器轻松地安装它。

- 在 Ubuntu 和 Debian 上
```
sudo apt install unzip
```
- Fedora 和 Fedora
```
sudo yum install unzip
```
## 如何解压 `ZIP` 文件

最简单的形式是，当不带任何选项使用时，unzip 命令将指定 `ZIP` 归档文件中的所有文件解压缩到工作目录文件夹中。


举个例子，假设你下载了 Wordpress 安装 `ZIP` 文件。 要将这个文件解压到工作目录文件夹，你只需运行以下命令:
```
unzip latest.zip
```

zip 文件不支持 linux 样式的所有权信息。提取的文件属于运行命令的用户。


您必须对解压压缩 `ZIP` 归档文件的目录具有写权限。

## 静默运行

默认情况下，解压缩将打印所提取的所有文件的名称，并在提取完成时打印一个摘要。


使用 -q 开关禁止打印这些消息。
```
unzip -q filename.zip
```

## 将 `ZIP` 文件解压缩到另一个目录

要将 `ZIP` 文件解压缩到与当前目录不同的目录，请使用 -d 开关:
```
unzip filename.zip -d /path/to/directory
```


例如，要将 WordPress 归档 latest.zip 解压缩到/var/www/目录，可以使用以下命令:
```
sudo unzip latest.zip -d /var/www
```
在上面的命令中，我使用 sudo 是因为我登录的用户通常没有对/var/www 目录的写权限。 当使用 sudo 对 `ZIP` 文件进行解压缩时，提取的文件和目录归用户根所有。

## 解压密码保护的 `ZIP` 文件


要解压缩受密码保护的文件，请调用 unzip 命令，并在 -P 选项后面加上密码:
```
unzip -P PasswOrd filename.zip
```


在命令行中键入密码是不安全的，应该避免。 一个更安全的选择是正常地提取文件而不提供密码。 如果 `ZIP` 文件是加密的，解压缩会提示你输入密码:
```
unzip filename.zip
archive:  filename.zip
[filename.zip] file.txt password: 
```

只要是正确的，unzip 将对所有加密文件使用相同的密码。

## 解压缩 `ZIP` 文件时排除文件
要排除特定的文件或目录进行解压缩，请使用-x 选项，然后使用空格分隔的存档文件列表排除解压缩:
```
unzip filename.zip -x file1-to-exclude file2-to-exclude
```


在下面的示例中，我将从 `ZIP` 归档文件中提取除. git 目录以外的所有文件和目录:
```
unzip filename.zip -x "*.git/*"
```

##  覆盖现有文件

假设您已经解压缩了一个 `ZIP` 文件，并且再次运行相同的命令:
```
unzip latest.zip
```


默认情况下，解压缩将询问您是否只覆盖当前文件、覆盖所有文件、跳过当前文件的提取、跳过所有文件的提取，或者重命名当前文件。
```
Archive:  latest.zip
replace wordpress/xmlrpc.php? [y]es, [n]o, [A]ll, [N]one, [r]ename:
```


如果您想在没有提示的情况下覆盖现有文件，请使用-o 选项:
```
unzip -o filename.zip
```

谨慎使用此选项。如果对文件做了任何更改，更改将丢失。

## 解压 `ZIP` 文件而不改写现有文件
假设您已经解压缩了一个 `ZIP` 文件，并且对一些文件进行了更改，但是不小心删除了一些文件。 您希望保留更改并从 `ZIP` 归档文件中还原已删除的文件。


在这种情况下，使用-n 选项强制 unzip 跳过提取已经存在的文件:
```
unzip -n filename.zip
```
## 解压多个 `ZIP` 文件

您可以使用正则表达式来匹配多个归档文件。


例如，如果你当前的工作目录文件夹中有多个 `ZIP` 文件，你可以只用一个命令解压所有文件:
```
unzip '*.zip'
```

注意 * 旁边的单引号。 如果你忘记引用参数，shell 会展开通配符，你会得到一个错误。

## 列出 zip 文件的内容

若要列出 `ZIP` 文件的内容，请使用-l 选项:
```
unzip -l filename.zip
```


在下面的例子中，我列出了所有的 WordPress 安装文件:
```
unzip -l latest.zip
```


输出结果如下:

Archive:  latest.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2019-08-02 22:39   test/
     3065  2019-08-31 18:31   test/xmlrpc.php
      364  2019-12-19 12:20   test/wp-blog-header.php
     7415  2019-03-18 17:13   test/readme.html
...
...
    21323  2019-03-09 01:15   test/wp-admin/themes.php
     8353  2019-09-10 18:20   test/wp-admin/options-reading.php
     4620  2019-10-24 00:12   test/wp-trackback.php
     1889  2019-05-03 00:11   test/wp-comments-post.php
---------                     -------
 27271400                     1648 files

## 小结

Unzip 是一个实用工具，可以帮助您列出、测试和解压缩 `ZIP` 文档。

要在 Linux 系统上创建 `ZIP` 归档文件，您需要使用 `ZIP` 命令。

![ZCoBLj](https://gitee.com/chasays/mdPic/raw/master/uPic/ZCoBLj.png)