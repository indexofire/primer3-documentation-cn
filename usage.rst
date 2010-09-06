3. Primer3 使用
--------------------

如果你已经按照上一节的方法将 Primer3 安装到电脑，那么通过本节的简单介绍 Primer3 ，希望你能够掌握基本的使用方法。

3.1 调用 primer3_core
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

默认编译的可执行程序叫做 primer3_core，由这个 C 程序承担复杂的引物挑选任务。

.. note:: 个别Linux发行版会将 primer3_core 链接为 primer3。

终端下 primer3 的命令行格式如下:

.. code-block:: bash

   primer3_core [ -format_output ] [ -io_version=3|-io_version=4 ] [ -p3_settings_file=<file_path> ] [ -echo_settings_file ] [ -strict_tags ] [ -output=<file_path> ] [ -error=<file_path> ] [ input_file ] 

+ **-about** 这个参数显示 Primer3 的版本号信息。
+ **-format_output** 这个参数表示让 primer3_core 输出用户可读的输出。
+ **-strict_tags** 这个参数表示如果某个标签无法识别，则让 primer3_core 输出错误信息。
+ **-p3_settings_file=<file_path>** 这个参数指定一个设置文件， Primer3 读取该文件中的全局参数 （“PRIMER ...") 。标签中的设置出现文件覆盖默认 primer3 设置。在该设置文件中的值也被输入文件的标签覆盖。查看 primer3 文档以了解该文件格式的详细信息。
+ **-echo_settings_file** 这个参数表明 primer3_core 在配置文件中指定的应打印的输入标签。如果没有设置配置文件，或如果有 -format_output 选项，则这个参数将没有任何效果。
+ **-output=<file_path>** 这个参数指定输出结果为文件的保存路径。如果省略，则在终端打印结果信息。
+ **-io_version=n** 这个参数引导 primer3 使用的IO版本的输入和输出方式。 n可以是3（主要是向后兼容的行为）或4。 -io_version=4 是默认值，使用2.0.0及更高版本的新功能时需要使用这个值。
+ **-error=<file_path>** 这个参数指定输出错误信息的文件位置。如果省略，则在终端打印错误信息。

.. Note:: 如果没有 input 文件，则程序接受终端输入作为参数。

3.2 输入和输出
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

默认状态下，primer3 接受以 Boulder-io, pre-XML, pre-RDF, text-based 格式的输入/输出支持。 primer3 的输出默认也和输入采用一样的格式。 当在命令行添加 -format_output 参数时, primer3 会在终端打印出更加用户可读的报告格式。 Primer3 正常退出的话返回状态值 0。 (参见 EXIT STATUS CODES)。

**Boulder-io 输入的格式要求如下：**

- 输入的内容是一些含有序列记录的字符串

- 单条记录由一系列标签和其值组成，用换行区分。每一条记录的结尾用一个新行的 '=' 来标志其结束

- 每一个标签和其值符合以下规范:
  
  + 标签必须连接'='，中间不能有空格
  + 标签和其值用换行符间隔

一个合乎规范的标签写法：

::

  PRIMER_SEQUENCE_ID=my_marker

一个简单的 BOULDER-IO 记录例子：

::

  PRIMER_SEQUENCE_ID=test1
  SEQUENCE=GACTGATCGATGCTAGCTACGATCGATCGATGCATGCTAGCTAGCTAGCTGCTAGC
  =

同时可以按顺序发送多条记录，每条记录之间用'='分隔。下面是一个3条不同记录的例子：

::

  PRIMER_SEQUENCE_ID=test1
  SEQUENCE=GACTGATCGATGCTAGCTACGATCGATCGATGCATGCTAGCTAGCTAGCTGCTAGC
  =
  PRIMER_SEQUENCE_ID=test2
  SEQUENCE=CATCATCATCATCGATGCTAGCATCNNACGTACGANCANATGCATCGATCGT
  =
  PRIMER_SEQUENCE_ID=test3
  SEQUENCE=NACGTAGCTAGCATGCACNACTCGACNACGATGCACNACAGCTGCATCGATGC
  =

Primer3 在 stdin 输入 boulder-io 格式的字符后会打印一便，并在stdout返回 boulder-io 格式结果。

Primer3 有一套错误信息提示系统。用户在 PRIMER_ERROR 标签定义的值，可以显示需要纠正的错误信息（见以下），以及系统的其他错误，比如配置错误，资源的错误（例如出内存不足的错误），并通过stderr错误信息和非零退出消息表明程序错误。

Primer3 会打印出所输入错误的标记并将其忽略，除非命令行设置了 -strict_tags 标签。在这种情况下 primer3 在打印输出的 PRIMER_ERROR 标签错误（见下文），并 stdout 中打印其他信息；此选项在调试时非常有用。

除了标注为“interval list” 的标签，每个标签只允许出现一次。但这种限制，并非由系统来检查，因此使用标签是请尽量小心避免重复。

| 输入标签有2个类别：
| “Sequence” 输入标签描述一个提供给 primer3 的特定输入序列，并在遇到每一条新的 boulder-io 记录时复位并重新载入。
| “Global” 输入标签描述一些 primer3 搜索引物需要的参数，这些参数在切换 boulder-io 记录时仍旧保留，除非你显性标注让其复位。

错误的 “Sequence” 输入标签出错造成无效时，primer3 会继续处理的其他记录。“Global” 输入标签出错是致命的，因为一旦标签失效，则挑选引物的基本条件就出错导致无法选择到正确的引物。

3.3 一个简单的例子
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

某人想做 PCR引物设计，CA 在序列中部重复，想定义引物进行特定选择，使用标签来设计引物。我们首先通过 stdin 将 boulder-io 格式的信息发送给 Primer3。有很多种方法来操作，最简单的方法是可以将内容保存成一个 example 文件，然后运行程序，在终端输入：

.. code-block:: bash

   $ primer3_core < example

   PRIMER_SEQUENCE_ID=example
   SEQUENCE=GTAGTCAGTAGACNATGACNACTGACGATGCAGACNACACACACACACACAGCACACAGGTATTAGTGGGCCATTCGATCCCGACCCAAATCGATAGCTACGATGACG
   TARGET=37,21
   PRIMER_OPT_SIZE=18
   PRIMER_MIN_SIZE=15
   PRIMER_MAX_SIZE=21
   PRIMER_NUM_NS_ACCEPTED=1
   PRIMER_PRODUCT_SIZE_RANGE=75-100
   PRIMER_FILE_FLAG=1
   PRIMER_PICK_INTERNAL_OLIGO=1
   PRIMER_INTERNAL_OLIGO_EXCLUDED_REGION=37,21
   PRIMER_EXPLAIN_FLAG=1
   =

下面依次解释下用到的各个标签：

**PRIMER_SEQUENCE_ID=example**

这个标签主要是给序列建立一个id，这样针对多个序列设计引物时就不会搞混。这个标签不是必须标签，可以省略。但是如果 P3_FILE_FLAG 标签值不等于0的话，则必须使用该标签。因为程序需要用此标签来个输出的结果文件来命名。

**SEQUENCE=GTAGTCAGTAGACNATGACNA...TACGATGACG**

这个标签不用过多介绍，一看便知，这是用来存放模板序列的。本例子的模板有108个碱基，要注意这个标签的值不能回车换行。

**TARGET=37,21**

模板的中部从37的位置开始有21个长度的CA重复序列，我们想让 primer3 选择这个位点作为重点扩增目的片段。这样设计的引物会尽可能的针对从37开始的21各碱基。

**PRIMER_OPT_SIZE=18**

因为模板长度较小，所以引物也需要优化，将其设置为18，那么程序会尽可能选择该长度的引物片段。

**PRIMER_MIN_SIZE=15**
**PRIMER_MAX_SIZE=21**

引物的最小和最大长度。

**PRIMER_NUM_NS_ACCEPTED=1**

该标签设置为1的话，可以让程序挑选的引物中含有1各未知碱基。

**PRIMER_PRODUCT_SIZE_RANGE=75-100**

这是表示需要的扩增产物长度的标签。该标签的默认值是100-300，由于模板短所以将其修改成75-100，否则程序可能很难找到需要的引物。

**PRIMER_FILE_FLAG=1**

通过这个标签，可以把结果输出到以 PRIMER_SEQUENCE_ID 值定义名字的文件。

**PRIMER_PICK_INTERNAL_OLIGO=1**

我们想了解程序能否挑选一个杂交探针，所以将这个值设为1。

**PRIMER_INTERNAL_OLIGO_EXCLUDED_REGION=37,21**

一般来说很难获得CA重复序列的探针。所以我们把这个位点从探针合成的位点设计中给排除出去。

**PRIMER_EXPLAIN_FLAG=1**

我们想要获得一些统计结果，可以用这个标签定义

**=**

'=' 用来结束一个 boulder-io 数据。

结尾用 '=' 标记后，程序就开始搜索，挑选出最佳引物，最后输出到终端。由于有 PRIMER_FILE_FLAG=1，因此当前目录下会有输出 example.* 文件保存引物和探针信息。

3.4 如何移植标签为 IO VERSION 4
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

With primer3 release 2.0, many Boulder-IO tags were modified and new tags were introduced. The new primer3 tags are designed with the idea in mind that computer scripts and other programs use primer3_core. The modifications make it easier for programs to read and write primer3 input and output.

For temporary backward compatibility, a command line argument (-io_version=3) allows the use of primer3 tags the are nearly identical to the previous version. However, even with -io_version=3, the text of some warning and error messages has changed, as have some details of the **PRIMER_..._EXPLAIN** output tags. New functionality is not available with the -io_version=3 tags. Furthermore the primer3 default values and the use of PRIMER_WT_TEMPLATE_MISPRIMING and PRIMER_PAIR_WT_TEMPLATE_MISPRIMING have changed in version 2.0. The backward compatibility might be dropped in the next release of primer3.

There are three classes of input: "Sequence" input tags describe a particular input sequence to primer3, and are reset after every Boulder record (now starting with **SEQUENCE_**). "Global" input tags describe the general parameters that primer3 should use in its searches, and the values of these tags persist between input Boulder records until or unless they are explicitly reset (now starting with **PRIMER_**). "Program" parameters that deal with the behavior of the primer3 program itself (now starting with **P3_**). See below for a list of the modified tags.

The handling of PRIMER_TASK changed completely. In the past we used it to tell primer3 what task to perform. Now the task is complemented with **PRIMER_PICK_RIGHT_PRIMER**, **PRIMER_PICK_INTERNAL_OLIGO** and **PRIMER_PICK_LEFT_PRIMER**, which specify which primers are to be picked.

These Tags are modified:

3.4.1 The "per sequence" tags:
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

+----------------------------------+----------------------------------------------------+
|NEW VERSION                       | OLD VERSION                                        |
+==================================+====================================================+                      
|SEQUENCE_ID                       | PRIMER_SEQUENCE_ID                                 |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_TEMPLATE                 | SEQUENCE                                           |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_QUALITY                  | PRIMER_SEQUENCE_QUALITY                            |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_INCLUDED_REGION          | INCLUDED_REGION                                    |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_TARGET                   | TARGET                                             |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_EXCLUDED_REGION          | EXCLUDED_REGION                                    |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_START_CODON_POSITION     | PRIMER_START_CODON_POSITION                        |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_PRIMER                   | PRIMER_LEFT_INPUT                                  |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_PRIMER_REVCOMP           | PRIMER_RIGHT_INPUT                                 |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_INTERNAL_OLIGO           | PRIMER_INTERNAL_OLIGO_INPUT                        |
+----------------------------------+----------------------------------------------------+
|SEQUENCE_INTERNAL_EXCLUDED_REGION | PRIMER_INTERNAL_OLIGO_EXCLUDED_REGION              |
+----------------------------------+----------------------------------------------------+



3.4.2 The "global" tags:
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

+-----------------------------------+----------------------------------------------------+
|**NEW VERSION**                    | **OLD VERSION**                                    |
+===================================+====================================================+ 
|PRIMER_TASK                        | PRIMER_TASK (modified use)                         |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PICK_RIGHT_PRIMER           | did not exist                                      |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PICK_INTERNAL_OLIGO         | PRIMER_PICK_INTERNAL_OLIGO (modified use)          |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PICK_LEFT_PRIMER            | did not exist                                      |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_WT_TEMPLATE_MISPRIMING | PRIMER_PAIR_WT_TEMPLATE_MISPRIMING (modified use)  |
+-----------------------------------+----------------------------------------------------+
|PRIMER_WT_TEMPLATE_MISPRIMING      | PRIMER_WT_TEMPLATE_MISPRIMING (modified use)       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_MAX_LIBRARY_MISPRIMING      | PRIMER_MAX_MISPRIMING                              |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_LIBRARY_MISHYB | PRIMER_INTERNAL_OLIGO_MAX_MISHYB                   |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_MAX_LIBRARY_MISPRIMING | PRIMER_PAIR_MAX_MISPRIMING                         |
+-----------------------------------+----------------------------------------------------+
|PRIMER_WT_LIBRARY_MISPRIMING       | PRIMER_WT_REP_SIM                                  |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_LIBRARY_MISHYB  | PRIMER_INTERNAL_WT_REP_SIM                         |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_WT_LIBRARY_MISPRIMING  | PRIMER_PAIR_WT_REP_SIM                             |
+-----------------------------------+----------------------------------------------------+
|PRIMER_MAX_NS_ACCEPTED             | PRIMER_NUM_NS_ACCEPTED                             |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_MAX_DIFF_TM            | PRIMER_MAX_DIFF_TM                                 |
+-----------------------------------+----------------------------------------------------+
|PRIMER_SALT_MONOVALENT             | PRIMER_SALT_CONC                                   |
+-----------------------------------+----------------------------------------------------+
|PRIMER_SALT_DIVALENT               | PRIMER_DIVALENT_CONC                               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_TM_FORMULA                  | PRIMER_TM_SANTALUCIA                               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_MAX_SELF_ANY                | PRIMER_SELF_ANY                                    |
+-----------------------------------+----------------------------------------------------+
|PRIMER_MAX_SELF_END                | PRIMER_SELF_END                                    |
+-----------------------------------+----------------------------------------------------+
|PRIMER_WT_SELF_ANY                 | PRIMER_WT_COMPL_ANY                                |
+-----------------------------------+----------------------------------------------------+
|PRIMER_WT_SELF_END                 | PRIMER_WT_COMPL_END                                |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_MAX_COMPL_ANY          | PRIMER_PAIR_ANY                                    |
+-----------------------------------+----------------------------------------------------+
|PRIMER_PAIR_MAX_COMPL_END          | PRIMER_PAIR_END                                    |
+-----------------------------------+----------------------------------------------------+
|P3_FILE_FLAG                       | PRIMER_FILE_FLAG                                   |
+-----------------------------------+----------------------------------------------------+
|P3_COMMENT                         | PRIMER_COMMENT                                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_SALT_MONOVALENT    | PRIMER_INTERNAL_OLIGO_SALT_CONC                    |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_SALT_DIVALENT      | PRIMER_INTERNAL_OLIGO_DIVALENT_CONC                |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_SELF_ANY        | PRIMER_IO_WT_COMPL_ANY                             |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_SELF_END        | PRIMER_IO_WT_COMPL_END                             |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_NS_ACCEPTED    | PRIMER_INTERNAL_OLIGO_NUM_NS                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_SELF_ANY       | PRIMER_INTERNAL_OLIGO_SELF_ANY                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_SELF_END       | PRIMER_INTERNAL_OLIGO_SELF_END                     |
+-----------------------------------+----------------------------------------------------+

3.4.3 The following tags INTERNAL_OLIGO is replaced by INTERNAL:
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_OPT_SIZE           | PRIMER_INTERNAL_OLIGO_OPT_SIZE                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MIN_SIZE           | PRIMER_INTERNAL_OLIGO_MIN_SIZE                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_SIZE           | PRIMER_INTERNAL_OLIGO_MAX_SIZE                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_OPT_TM             | PRIMER_INTERNAL_OLIGO_OPT_TM                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MIN_TM             | PRIMER_INTERNAL_OLIGO_MIN_TM                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_TM             | PRIMER_INTERNAL_OLIGO_MAX_TM                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MIN_GC             | PRIMER_INTERNAL_OLIGO_MIN_GC                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_OPT_GC_PERCENT     | PRIMER_INTERNAL_OLIGO_OPT_GC_PERCENT               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_GC             | PRIMER_INTERNAL_OLIGO_MAX_GC                       |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_DNTP_CONC          | PRIMER_INTERNAL_OLIGO_DNTP_CONC                    |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_DNA_CONC           | PRIMER_INTERNAL_OLIGO_DNA_CONC                     |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MAX_POLY_X         | PRIMER_INTERNAL_OLIGO_MAX_POLY_X                   |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MISHYB_LIBRARY     | PRIMER_INTERNAL_OLIGO_MISHYB_LIBRARY               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_MIN_QUALITY        | PRIMER_INTERNAL_OLIGO_MIN_QUALITY                  |
+-----------------------------------+----------------------------------------------------+

3.4.4 The following tags IO is replaced by INTERNAL:
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_TM_GT           | PRIMER_IO_WT_TM_GT                                 |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_TM_LT           | PRIMER_IO_WT_TM_LT                                 |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_SIZE_LT         | PRIMER_IO_WT_SIZE_LT                               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_SIZE_GT         | PRIMER_IO_WT_SIZE_GT                               |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_GC_PERCENT_LT   | PRIMER_IO_WT_GC_PERCENT_LT                         |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_GC_PERCENT_GT   | PRIMER_IO_WT_GC_PERCENT_GT                         |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_NUM_NS          | PRIMER_IO_WT_NUM_NS                                |
+-----------------------------------+----------------------------------------------------+
|PRIMER_INTERNAL_WT_SEQ_QUAL        | PRIMER_IO_WT_SEQ_QUAL                              |
+-----------------------------------+----------------------------------------------------+


3.4.5 OUTPUT TAGS:
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

There are three big changes on the output:
- INTERNAL_OLIGO is now replaced by INTERNAL.
- The first version is numbered 0.
- The "PRODUCT" tags are renamed
- The errors are modified
- Errors caused by a specific primer are given as PRIMER_LEFT_4_PROBLEMS

Now all primer related output follows the rule: PRIMER_{LEFT,RIGHT,INTERNAL,PAIR}_<j>_<tag_name>. where <j> is an integer from 0 to n, where n is at most the value of PRIMER_NUM_RETURN - 1.

This allows easy scripting by using the underscores _ to split the name. The first part is PRIMER, the second the type of oligo or pair parameters, the third is always a number, starting at 0 and the rest is used by the tags.

That affects also (shown for output number 4):

+---------------------------------------+--------------------------------------------------------+
|**NEW VERSION**                        | **OLD VERSION**                                        |
+=======================================+========================================================+
|PRIMER_PAIR_4_PENALTY                  | PRIMER_PAIR_PENALTY_4 (number moved behind PAIR)       |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_PAIR_4_PRODUCT_SIZE             | PRIMER_PRODUCT_SIZE_4 (grouped with PAIR)              |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_PAIR_4_PRODUCT_TM               | PRIMER_PRODUCT_TM_4 (grouped with PAIR)                |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_PAIR_4_PRODUCT_TM_OLIGO_TM_DIFF | PRIMER_PRODUCT_TM_OLIGO_TM_DIFF_4 (grouped with PAIR)  |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_INTERNAL_EXPLAIN                | PRIMER_INTERNAL_OLIGO_EXPLAIN                          |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_LEFT_4_LIBRARY_MISPRIMING       | PRIMER_LEFT_4_MISPRIMING_SCORE                         |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_INTERNAL_4_LIBRARY_MISHYB       | PRIMER_INTERNAL_OLIGO_4_MISHYB_SCORE                   |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_RIGHT_4_LIBRARY_MISPRIMING      | PRIMER_RIGHT_4_MISPRIMING_SCORE                        |
+---------------------------------------+--------------------------------------------------------+
|PRIMER_PAIR_4_LIBRARY_MISPRIMING       | PRIMER_PAIR_4_MISPRIMING_SCORE                         |
+---------------------------------------+--------------------------------------------------------+
