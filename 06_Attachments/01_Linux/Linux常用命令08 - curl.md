`curl` 是一个命令行实用程序，用于将数据从服务器或传输到服务器，该服务器设计用于在没有用户交互的情况下工作。 使用 `curl`，您可以使用支持的协议(包括 HTTP、 HTTPS、 SCP、 SFTP 和 FTP)下载或上传数据。 `curl` 提供了许多选项，允许您恢复传输、限制带宽、代理支持、用户认证等等。


下面就介绍常见的用法， 将通过实际例子和最常见的 `curl` 选项的详细说明，向您展示如何使用 curl 工具。

## 安装 curl

现在大多数 Linux 发行版都预先安装了 curl 包。



要检查 `curl` 包是否已安装在系统上，请打开控制台，键入 curl，然后按回车键。 如果您安装了 curl，系统将打印 curl: 尝试‘curl --help’或‘ curl --manual’获取更多信息。 否则，您将看到类似 curl 命令的内容没有被找到。
![dsjqSu](https://gitee.com/stormzhang/mdPic/raw/master/uPic/dsjqSu.png)

如果没有安装 curl，您可以使用发行版的包管理器轻松地安装它。

###  在 Ubuntu 和 Debian 上安装 curl
```
sudo apt update
sudo apt install curl
```
###  在 CentOS 和 Fedora 上安装 curl
```
sudo yum install curl
```
##  如何使用 curl

curl 命令的语法如下:
```
curl [options] [URL...]
```
在其最简单的形式中，当不使用任何选项调用时，curl 将指定的资源显示到标准输出。


例如，要检索示例网站的主页，你可以运行:
```
curl chasays.github.io
```
![eqklbA](https://gitee.com/stormzhang/mdPic/raw/master/uPic/eqklbA.png)

该命令将在您的终端窗口中打印示例.com 主页的源代码。


如果没有指定协议，curl 会尝试猜测您想要使用的协议，它将默认为 HTTP。
##  将输出保存到文件中

若要保存 curl 命令的结果，请使用-o 或-O 选项。

Lowercase -o 使用一个预定义的文件名保存文件，在下面的示例中是 vue-v2.6.10. js:
```
curl -o vue-v2.6.10.js https://cdn.jsdelivr.net/npm/vue/dist/vue.js
```


大写 -O 保存文件和它的原始文件名:
```
curl -O https://cdn.jsdelivr.net/npm/vue/dist/vue.js
```

![cbHQFb](https://gitee.com/stormzhang/mdPic/raw/master/uPic/cbHQFb.png)

##  下载多个文件

要一次下载多个文件，请使用多个 -O 选项，后跟要下载的文件的 URL。


在下面的例子中，我们正在下载 Arch Linux 和 Debian iso 文件:

```
curl -O http://mirrors.edge.kernel.org/archlinux/iso/2018.06.01/archlinux-2018.06.01-x86_64.iso  \
     -O https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.4.0-amd64-netinst.iso
```

## 恢复下载
您可以使用 `-C` 选项恢复下载。 如果您的连接在下载一个大文件期间断开，而且您可以继续前一个文件而不是从头开始下载，那么这将非常有用。


例如，如果你正在下载 Ubuntu 18.04 iso 文件，使用以下命令:
```
curl -O https://updates.cdn-apple.com/2020/macos/061-44388-20200128-3badc52c-6391-412c-86d9-fc2aaf9514e0/macOSUpd10.15.3.dmg
```
然后你的连接突然断开，你可以用以下命令继续下载:
```
curl -C - -O https://updates.cdn-apple.com/2020/macos/061-44388-20200128-3badc52c-6391-412c-86d9-fc2aaf9514e0/macOSUpd10.15.3.dmg
```
![AFXvTE](https://gitee.com/stormzhang/mdPic/raw/master/uPic/AFXvTE.png)

##  获取 URL 的 HTTP 头

Http 头是冒号分隔的键值对，包含用户代理、内容类型和编码等信息。 头文件通过请求或响应在客户端和服务器之间传递。


使用 -I 选项仅获取指定资源的 HTTP 标头:
```
curl -I --http2 https://www.apple.com/
```

![b8dqcw](https://gitee.com/stormzhang/mdPic/raw/master/uPic/b8dqcw.png)
## 测试网站是否支援 http/2

要检查某个特定的 URL 是否支持新的 HTTP/2协议，请使用-i 和 --http2选项获取 HTTP header:
```
curl -I --http2 -s https://apple.com/ | grep HTTP
```
![9JSwNl](https://gitee.com/stormzhang/mdPic/raw/master/uPic/9JSwNl.png)
S 选项告诉 curl 以静音(quiet)运行，并隐藏进度表和错误消息。


如果远程服务器支持 http/2，curl 打印 http/2.0200:
```
HTTP/2 200
```
否则，回复就是 http/1.1200:
```
HTTP/1.1 200 OK
```
如果您使用的是 curl 版本7.47.0或更高版本，则不需要使用 -- http2选项，因为所有 HTTPS 连接都默认启用了 http/2。

##  遵循重定向
默认情况下，curl 不遵循 HTTP Location 头。


如果你尝试检索非 www 版本的 google. com，你会注意到，你不但没有获得页面的来源，反而会被重定向到 www 版本:
```
curl baidu.com
```

选项指示 curl 跟踪任何重定向，直到它到达最终目的地:
``
curl -L baidu.com
``
![nITkGy](https://gitee.com/stormzhang/mdPic/raw/master/uPic/nITkGy.png)

##  更改用户代理

有时在下载文件时，远程服务器可能被设置为阻止 curl User-Agent，或者根据访问者设备和浏览器返回不同的内容。


在这种情况下模拟不同的浏览器，使用 -a 选项。

例如，要模拟 Firefox 60，你可以使用:
```
curl -A "Mozilla/5.0 (X11; Linux x86_64; rv:60.0) Gecko/20100101 Firefox/60.0" https://baidu.org/
```

## 指定最大传输速率

--limit-rate 选项允许您限制数据传输速率。 该值可以用字节表示，k 后缀为千字节，m 后缀为兆字节，g 后缀为千字节。


在下面的例子中 curl 将下载 Go 二进制文件，并将下载速度限制在1 mb:
```
curl --limit-rate 1m -O https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz
```
此选项有助于防止 curl 占用所有可用带宽。
## 通过 FTP 传输文件

要使用 curl 访问受保护的 FTP 服务器，请使用-u 选项并指定用户名和密码，如下所示:
```
curl -u FTP_USERNAME:FTP_PASSWORD ftp://ftp.baidu.com/
```
登录后，该命令列出用户主目录中的所有文件和目录。


你可以使用以下语法从 FTP 服务器下载一个文件:
```
curl -u FTP_USERNAME:FTP_PASSWORD ftp://ftp.example.com/file.tar.gz
```
要将文件上传到 FTP 服务器，请使用-t 后跟要上传的文件的名称:
````
curl -T newfile.tar.gz -u FTP_USERNAME:FTP_PASSWORD ftp://ftp.example.com/
````
## 使用cookies

有时您可能需要使用特定的 cookie 发出 HTTP 请求以访问远程资源或调试问题。


默认情况下，当使用 curl 请求资源时，不会发送或存储 cookie。


若要将 cookie 发送到服务器，请使用-b 开关，后跟包含 cookie 或字符串的文件名。


例如，下载 oraclejavajdkrpm 文件 JDK-10.0.2 linux-x64 bin。 Rpm 你需要传递一个值为 a 的名为 oraclericense 的 cookie:
```
curl -L -b "oraclelicense=a" -O http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm
```

 ## 使用代理

支持不同类型的代理，包括 HTTP、 HTTPS 和 SOCKS。 若要通过代理服务器传输数据，请使用-x (-- proxy)选项，后跟代理 URL。


下面的命令使用代理在192.168.44.1 port 8888上下载指定的资源:
```
curl -x 192.168.44.1:8888 http://google.com/
```


如果代理服务器需要身份验证，请使用-u (-- proxy-user)选项，后跟用户名和密码，用冒号分隔(user: password) :
```
curl -U username:password -x 192.168.44.1:8888 http://google.com/
```
## 小结

`curl `是一个命令行工具，它允许您从远程主机或向远程主机传输数据。 它对于故障排除、下载文件等非常有用。
我只是做了一些简单的实例，但是演示了最常用的 `curl` 选项，这些示例旨在帮助您理解 curl 命令的工作原理。