2. Primer3 安装
--------------------

你可以在多个平台下安装 Primer3，已知可以使用的有 Windows, Mac OSX, Linux, FreeBSD 等。

2.1 Unix and Linux 编译安装
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

下载并解压缩软件包。 编译源代码，并测试。

.. code-block:: bash

   $ unzip primer3_1.1.4.tar.gz
   $ tar xvf primer3_1.1.4.tar
   $ cd primer3_1.1.4/src

   # 如果系统不带 gcc, 则修改 makefile, 设置所使用的 C 编译器。

   $ make all

   # pr_release being unused 的警告提示可以不用理会
   # 编译后应该可以看到 primer3_core, ntdpal, olgotm, long_seq_tm_test 这4个可执行程序

   $ make test

   # 没有显示 'FAILED' 则表明通过测试，可以安装
   
   $ sudo cp primer3_core ntdpal olgotm long_seq_tm_test /usr/sbin

如果你的 perl 程序名字不是 perl 而是其他名称比如 perl5，则需要 link 一个 perl 名称，或者修改 test/ 目录里的 Makefile 文件以通过测试。 ntdpal (NucleoTide Dynamic Programming ALignment) 是一个 stand-alone 程序，它给 primer3 提供比对功能，文档将在以后版本中添加。

.. note:: 一些主流发行版例如 Ubuntu 等，有二进制包可以直接 apt-get install primer3 来安装。详细内容参见各发行版的软件安装和管理方法。

2.2 FreeBSD 安装
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

FreeBSD的最常见的安装方式为 Ports，而且ports tree里已经收录了包括Primer3等一批生物学软件。

.. code-block:: bash

   root# cd /usr/ports/biology/primer3
   root# make install clean

.. note::

   用 FreeBSD 的 Ports 安装 Primer3，系统会将 primer3_core 重命名为 primer3，因此运行程序直接使用 prime3 即可。

当然你也可以直接安装 primer3 二进制包，软件安装系统会自动根据你 FreeBSD 版本下载相应的包安装。

.. code-block:: bash

   root# pkg_add -vr primer3

2.3 Mac OSX 编译安装
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

如果你只想编译一个 *processor-native*, *non-universal* 的 primer3, 只要按照 Unix/Linux 的方式编译即可。但如果你想编译一个兼容 PPC 和 Intel 的 CPU 的 Primer3，请先安装 OS X 的开发者工具(developer tools)，你可以到 http://developer.apple.com 下载。操作系统版本 OS X 必须大于 10.4，并且已安装了最新版的 XCode。Mac OSX 的预编译包可以在 http://sourceforge.net/projects/primer3/ 下载到。

编译方法如下：

.. code-block:: bash

   # Mac OSX 10.5
   $ make -f Makefile.OSX.Leopard
   
   # Mac OSX 10.4
   $ make -f Makefile.OSX.Tiger

.. note:: 

   没有 SnowLeopard 版本的 Mac OS，能否在 SnowLeopard 上编译通过目前无法测试。

README.OSX.txt 文件里有介绍如何编译安装。你可以在 CFLAGS 和 LDFLAGS 后加参数 `-arch ppc64` 来编译一个多版本通用的二进制包，这样除了兼容 PPC 和 Intel 外，还兼容 PPC64 版本。不过目前这种编译方式还没有被仔细测试。

2.4 Windows 下安装
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Primer3 也提供了 WindowsXP 的安装包，可以到 `这里`__ 下载。

__ http://nchc.dl.sourceforge.net/sourceforge/primer3/primer3-1.1.4-WINXP.zip
