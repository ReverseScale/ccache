ccache
======

[![Build Status](https://travis-ci.org/ccache/ccache.svg?branch=master)](https://travis-ci.org/ccache/ccache)

### 中文文档

一、什么是ccache
     什么是ccache？简单地讲，这个工具，可以在项目初次编译时缓存部分编译结果，在下一次编译时直接读取该部分缓存，从而提高整体的编译速度。详情可至官网了解（http://ccache.samba.org/）

二、使用方法：

1.安装

安装非常简单，从官网下载好安装包并进行解压。然后在目录下依次运行以下命令即可

``` ./configure
   make
   make install
```

2.配置

> 假设项目在xcode中设置了以llvm-gcc作为编译器，通过查看编译时log

注意上图红框中的内容，我们可知项目是调用了Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/下的llvm-gcc进行编译

接着cd到Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/,再输入

```
ls -all llvm-gcc*得到llvm-gcc文件的信息
```

可以发现lvm-gcc-4.2实际是软链到../llvm-gcc-4.2/bin/llvm-gcc-4.2,../llvm-gcc-4.2/bin/llvm-gcc-4.2才是真正的llvm-gcc-4.2“所在地”

现在需要对文件进行修改：

首先，将Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/的llvm-gcc-4.2软链到ccache(ccache安装在/usr/local/bin下，因此可以执行 

```
ls -s /usr/local/bin/ccache /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/llvm-gcc-4.2）
```

接着，输入

```
which llvm-gcc-4.2
```
得到的输出是“/usr/bin/llvm-gcc-4.2"，ccache会调用/usr/bin/llvm-gcc-4.2来进行编译，因此我们需要将这里的/usr/bin/llvm-gcc-4.2替换成上文提到的”../llvm-gcc-4.2/bin/llvm-gcc-4.2“（软链或复制粘贴均可）
现在再进行编译，ccache就会进行缓存了（可以通过ccahce -s命令查看缓存命中情况）

三、项目实践

选取了三个项目进行实际的测试

![](http://og1yl0w9z.bkt.clouddn.com/17-8-22/13073301.jpg)

存在问题：如果项目是采用clang进行编译，编译会失败，目前还没找到办法解决

======

About
-----

ccache is a compiler cache. It speeds up recompilation by caching the result of
previous compilations and detecting when the same compilation is being done
again. Supported languages are C, C++, Objective-C and Objective-C++.


Documentation
-------------

See the https://ccache.samba.org.


Installation
------------

See [INSTALL.md](INSTALL.md).


Web site
--------

The main ccache web site is here:

https://ccache.samba.org


Mailing list
------------

There is a mailing list for discussing usage and development of ccache:

https://lists.samba.org/mailman/listinfo/ccache/

Anyone is welcome to join.


Bug reports
-----------

To submit a bug report or to search for existing reports, please visit this web
page:

https://ccache.samba.org/bugs.html


History
-------

ccache was originally written by Andrew Tridgell and is currently developed and
maintained by Joel Rosdahl. ccache started out as a reimplementation of Erik
Thiele's "compilercache" (see http://www.erikyyy.de/compilercache/) in C.

See also https://ccache.samba.org/news.html.


License and copyright
---------------------

See https://ccache.samba.org/license.html.


