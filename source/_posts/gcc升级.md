---
title: gcc升级
id: 652
categories:
  - Linux
  - 工具
date: 2013-07-06 15:43:40
tags:
---

http://gcc.gnu.org/install/这个是官方安装指导，比较全面，在网上看到一些方法大都不用这么复杂。下面记录一下前两天在我的CentOS 6.2上面的升级记录：

1下载安装包，包括gmp-4.3.2.tar.bz2，mpfr-2.4.2.tar.bz2，mpc-0.8.1.tar.gz，gcc-4.7.2.tar.bz2

2由于安装包之间有依赖关系，必须依次安装，步骤为

*   tar jxf gmp-4.3.2.tar.bz2
cd gmp-4.3.2
./configure --prefix=/usr/local/gmp
make &amp;&amp; make install
*   cd ../
tar jxf mpfr-2.4.2.tar.bz2
cd mpfr-2.4.2
./configure --prefix=/usr/local/mpfr --with-gmp=/usr/local/gmp
make &amp;&amp; make install
*   cd ../
tar xzf mpc-0.8.1.tar.gz
cd mpc-0.8.1
./configure --prefix=/usr/local/mpc --with-mpfr=/usr/local/mpfr --with-gmp=/usr/local/gmp
make &amp;&amp; make install
*   cd ../
tar jxf gcc-4.7.2.tar.bz2
cd gcc-4.7.2
./configure --prefix=/usr/local/gcc --enable-threads=posix --disable-checking --disable-multilib --enable-languages=c,c++ --with-gmp=/usr/local/gmp --with-mpfr=/usr/local/mpfr/ --with-mpc=/usr/local/mpc/
*   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/mpc/lib:/usr/local/gmp/lib:/usr/local/mpfr/lib/
*   make &amp;&amp; make install
在我的虚拟机上面2个小时才搞定，一切顺利的话剩下就是设置新的gcc，主要有两种方式：

1.  <span style="line-height: 14px;"><span style="line-height: 14px;">通过alias</span></span>alias g++='/usr/local/gcc/bin/g++'
alias gcc='/usr/local/gcc/bin/gcc'
2.  通过ln建立软链接
mv /usr/bin{gcc,g++} /home/backup（记得一定要备份一下）

ln -s /usr/local/gcc/bin/gcc /usr/bin/gcc

ln -s /usr/local/gcc/bin/g++ /usr/bin/g++

另外如果可以上网的话可以按照这篇文字的介绍安装（没有实测过，应该是可以的）

http://www.cnblogs.com/linbc/archive/2012/08/03/2621169.html

2013.10.17更新：

遇到/usr/lib/libstdc++.so.6: version `GLIBCXX_3.4.15' not found问题，主要是没有将相应的库更新，在gcc源码目录里面找到最新的库，gcc-4.7.2里面最新的是libstdc++.so.6.0.17，将其拷贝到/usr/lib/目录下面，删除原有链接，新建链接即可：
<div>`rm` `-rf ``/usr/lib/libstdc``++.so.6`</div>
<div>`ln` `-s ``/usr/lib/libstdc``++.so.6.0.17  ``/usr/lib64/libstdc``++.so.6`</div>
<div></div>
<div>2013.10.20更新：</div>
<div></div>
<div>今天在使用scons时发现提示"/usr/local/gcc/libexec/gcc/i686-pc-linux-gnu/4.7.2/cc1plus: error while loading shared libraries: libmpc.so.2: cannot open shared object file: No such file or directory"</div>
<span style="line-height: 24px;">主要是由于在scons中定义Environment()时没加ENV = os.environ，导致在~/.bash_profile中导出的路径无效了，加上这个就可以了。</span>

<span style="line-height: 24px;">export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/mpc/lib:/usr/local/gmp/lib:/usr/local/mpfr/lib/
</span>