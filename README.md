# parted 3.7 for arm64
## 1\. introduction 介绍
As we all know, a binary program running on Android should ideally have no dependencies at all. But in almost all Linux distribution, parted software package is built with dymatic link which mean  to require reliance on libraries. It makes inconvenience if you want to use it on Android. So I refrence some program to build the static compile program for arm64. It has readline optimizations, which can get history input and is very rare in other projects. There is my compile file. The compiled file can be found in `./parted-3.7/parted ` and ` release ` page.    
众所周知，在安卓上运行的二进制程序最好是不要有任何依赖。但是在几乎所有的 Linux 发行版上，parted 软件包都是动态编译，这意味着需要依赖。如果你要在安卓上使用的话会很不方便。所以我参考了一些项目构建了这个arm64静态编译项目。我给它集成了其他项目少有的 readline 功能优化，可以获取历史输入。这是我的编译文件。编译好的文件可以在 `./parted-3.7/parted `  和 ` release ` 界面获取。

## 2\. how to build 如何构建
The cross-compiler I use is `arm-gnu-toolchain-13.3.rel1-x86\_64-aarch64-none-linux-gnu` . Build cammand  is as follows. After executing the `make` command, there might be an error when linking into parted at the end. In such cases, we can copy the final linking command and replace the unrecognized options like `-luuid` with our shared library address.  
我使用的交叉编译器是 `arm-gnu-toolchain-13.3.rel1-x86\_64-aarch64-none-linux-gnu` 。编译命令如下所示。执行 ` make ` 命令后可能在最后链接成 parted 的时候会报错，这时候我们复制最后的链接命令，将报错未发现的 ` -luuid ` 等改为我们的共享库地址。
~~~
# ./configure --host=aarch64-linux --prefix=`pwd`/out --disable-device-mapper --without-readline --disable-nls --disable-shared --enable-static CC=aarch64-none-linux-gnu-gcc CFLAGS="-I/home/localhost/Downloads/share/include -O2 -pipe -static -fno-pie" LDFLAGS="-L/home/localhost/Downloads/share/lib -static -no-pie"
# make V=1 LDFLAGS="-all-static $LDFLAGS" 
# cd parted
# aarch64-none-linux-gnu-gcc -I/home/localhost/Downloads/share/include -O2 -pipe -fno-pie -Wl,--as-needed -static -o parted command.o parted.o strlist.o ui.o jsonwrt.o table.o  libver.a ../libparted/.libs/libparted.a /home/localhost/Downloads/share/lib/\*.a
# aarch64-none-linux-gnu-strip -s parted
~~~

## 3\. acknowledgments 致谢
* parted (v3.7 [https://ftp.gnu.org/gnu/parted/](https://ftp.gnu.org/gnu/parted/))
* libuuid (from e2fsprogs v1.47.4 [https://e2fsprogs.sourceforge.net/](https://e2fsprogs.sourceforge.net/))
* readline ( v8.3 [https://ftp.gnu.org/gnu/readline/](https://ftp.gnu.org/gnu/readline/))
* termcap (v1.3.1 [https://ftp.gnu.org/gnu/termcap/](https://ftp.gnu.org/gnu/termcap/))
* parted\_static ([https://github.com/pocketblue/parted-static/releases](https://github.com/pocketblue/parted-static/releases))
