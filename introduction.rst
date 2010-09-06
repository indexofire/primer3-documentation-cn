1. Primer3 简介
--------------------

1.1 Primer3 背景介绍
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Primer3_ 是一款优秀的批量设计PCR引物、杂交探针、测序引物的工具，支持多种系统，也有各种开发语言的版本，如C/Perl等。 Primer3_ 由 `Whitehead Institute`__ 和 `Howard Hughes Medical Institute`__ 的 `Steve Rozen`__ 与 Helen Skaletsky 开发。 Primer3_ 程序的大部分代码基于 `New BSD License`__ ，只有其中的 oligtm 库和 tests 测试是基于 `GPL v2`__  发布的，它的软件源代码可在 SourceForge_ 下载。目前稳定版本为 1.1.4，开发版本为 2.2.2 Beta。

__ http://whitehead.mit.edu/

__ http://www.hhmi.org/

__ http://jura.wi.mit.edu/rozen/

__ http://creativecommons.org/licenses/BSD/

__ http://creativecommons.org/licenses/GPL/2.0/

.. _SourceForge: http://primer3.sourceforge.net/

Primer3_ 可以通过设定各种标签来指定引物设计参数，从而筛选 PCR 目的引物，返回引物的相关信息。 

.. note:: 常见的设置参数

   - 寡核苷酸tm(溶解温度)值，引物长度，GC含量，引物二聚体的可能性
   - PCR 产物大小
   - 模板的位点限制
   - 错配的可能性
   - 其他限制条件

所有的设置都可以由用户自行定义，在扩展了引物设计能力的同时，可以方便的为用户找到所需要的最佳 PCR 引物结果。

1.2 基于 Web 的 Primer3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

下载源代码，根据用户及其编译安装 Primer3 客户端， 以 Standalone 程序在 Unix 命令行下运行，是最基本的 Primer3 引物设计的用法。

但是由于生物学家未必是计算机专家，为了更方便使用，有许多机构提供了在线的 Primer3_ 应用程序：

1. `Primer3-Web`__

__ http://fokker.wi.mit.edu/primer3/

2. `Primer3 Plus`__

__ http://www.bioinformatics.nl/cgi-bin/primer3plus/primer3plus.cgi

3. `NCBI Primer-Blast`__

__ http://www.ncbi.nlm.nih.gov/tools/primer-blast/

4. EMBOSS's eprimer3

Primer3-Web的用法可以 `参见这里`__

__ http://tong.dxy.cn/experiment/430/457/459/15701.htm

1.3 如何引用 Primer3
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Primer3_ 的开发者没有要求使用 Primer3_ 合成引物发表的文章中必须进行引用。 如果在文章中要引用 Primer3 的话，可以参照[1]进行引用。 文章的PDF版本 参见_

.. Note:: 

   1. `Steve Rozen and Helen J. Skaletsky (2000) Primer3 on the WWW for general users and for biologist programmers. In: Krawetz S, Misener S (eds) Bioinformatics Methods and Protocols: Methods in Molecular Biology.  Humana Press, Totowa, NJ, pp 365-386`

.. _参见: http://jura.wi.mit.edu/rozen/papers/rozen-and-skaletsky-2000-primer3.pdf

.. _Primer3: http://primer3.sourceforge.net/
