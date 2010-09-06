4. Primer3 参考手册
-----------------------------

4.1 输出标签
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

对于每一个通过 stdin 传递给 primer3 的 boulder-io 数据记录，会有一个 boulder-io 输出到 stdout 。这些输出包含所有输入值所含有的信息，加上一组跟随 '标签/值' 的子集。除用 ‘星号’ 标记的标签外，以下所列标签都固定返回给一对得到的引物。输出的结果中，第一对引物的标签模式如下：

.. code-block:: none

   PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO,PAIR}_<tag_name>

第二对引物的标签模式添加一个<j>值，其中 <j> 是一个大于0。小于n的整数(其中n是PRIMER_NUM_RETURN的值)，比如第二对引物的 j=1，第三对引物的 j=2，意思类推：

.. code-block:: none

   PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO,PAIR}_<j>_<tag_name>.

.. note::

   下面的描述中i表示起始点，n表示长度，s表示字符串，x表示任意整数，f表示小数。

**PRIMER_ERROR=s** (*)

这个标签描述了当用户输入时，可修正的错误信息。如果没有错误则不显示。

**PRIMER_LEFT=i,n**

上游引物(5'端)，i是引物在模板上的起始位点，n是引物长度。

**PRIMER_RIGHT=i,n**

下游引物(3'端)，i是引物在模板上的结束位点，n是引物长度。

**PRIMER_INTERNAL_OLIGO=i,n**

所选的杂交探针。如果PRIMER_PICK_INTERNAL_OLIGO不等于0，程序输出该值。如果程序挑选中间oligo失败，将不会显示该值。i是oligo在模板的起始位点，n是它的长度。

**PRIMER_PRODUCT_SIZE=x**

PCR产物的长度。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_EXPLAIN=s** (*)

此标签值为一个选择引物时，得到单 oligo 的可能性统计结果的字符串。

例如：

.. code-block:: none

   PRIMER_LEFT_EXPLAIN=considered 62, too many Ns 53, ok 9
   PRIMER_RIGHT_EXPLAIN=considered 62, too many Ns 53, ok 9
   PRIMER_INTERNAL_OLIGO_EXPLAIN=considered 87, too many Ns 39, overlap excluded region 40, ok 8

除了 'considered' 外，所有的类别均有互斥性。

**PRIMER_PAIR_EXPLAIN=s** (*)

s 是一组包含挑选引物对统计信息的字符串。比如：

.. code-block:: none

   PRIMER_PAIR_EXPLAIN=considered 81, unacceptable product size 49, no internal oligo 32, ok 0

除了 'considered' 外，所有的类别均有互斥性。在某些情况下，程序会在某条引物违反限制条件之前就检测到。 即使 *PRIMER_LEFT_EXPLAIN*, *PRIMER_RIGHT_EXPLAIN* 或者 *PRIMER_INTERNAL_OLIGO_EXPLAIN* 为 'ok 0'， *PRIMER_PAIR_EXPLAIN* 也可能为非considered 0。

**PRIMER_PAIR_PENALTY=f**

这个标签值表明引物对扩增的不利效果，这个值越小表明引物扩增效果越好。一般输出根据这个值来从小到大排序。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_PENALTY=f**

引物或者 oligo 对已扩增的抑制作用评分。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_SEQUENCE=s**

所得到的引物和 oligo 的序列。所有序列都是 5'-> 3' 的。所以下游引物的反义互补序列才与模板匹配。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_TM=f**

所选引物或 oligo 的 Tm 值。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_GC_PERCENT=f**

所选引物或 oligo 的GC含量。

| **PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_SELF_ANY=f**
| **PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_SELF_END=f**

所选引物或 oligo 的自我互补错配。

| **PRIMER_PAIR_COMPL_ANY=f**
| **PRIMER_PAIR_COMPL_END=f**

所选引物之间的互补错配。

**PRIMER_WARNING=s** (*)

程序提供的警告信息。如果没有警告信息则不显示该标签。

**PRIMER_{LEFT,RIGHT,PAIR}_MISPRIMING_SCORE=f, s**

f 是筛选引物时，针对 *PRIMER_MISPRIMING_LIBRARY* 库的最大错选分；s 是库文件里的 id 序列号。*PRIMER_PAIR_MISPRIMING_SCORE* 是上下游引物在单个库序列中的错选分数总和

**PRIMER_{LEFT,RIGHT,PAIR}_TEMPLATE_MISPRIMING=f**

与标签 *PRIMER_{LEFT,RIGHT,PAIR}_MISPRIMING_SCORE* 类似，但是这几个输出标签针对模板序列的错选。比如基因中的重复外显子。为了程序向下兼容，这些标签只有当相应的输入标签被定义后才被打印显示。

**PRIMER_PRODUCT_TM=f**

f 是扩增产物的 Tm 值。根据文献[2]计算获得。只有当输入标签 *PRIMER_MAX_PRODUCT_TM* 或者 *PRIMER_MIN_PRODUCT_TM* 被定义后，此标签才会打印输出。

.. note::
	
   2. `Rychlik W, Spencer WJ and Rhoads RE (1990) "Optimization of the annealing temperature for DNA amplification in vitro", Nucleic Acids Res 18:6409-12`__
   
__ http://www.pubmedcentral.nih.gov/articlerender.fcgi?tool=pubmed&pubmedid=2243783

**PRIMER_PRODUCT_TM_OLIGO_TM_DIFF=f**

f 是产物 Tm 值和更不稳定的引物 Tm 值的差异。只有当定义了标签 *PRIMER_MAX_PRODUCT_TM* 或 *PRIMER_MIN_PRODUCT_TM* 后才会被打印到终端。

**PRIMER_PAIR_T_OPT_A=f**


f 是来自方程式[3]的 'T sub a super OPT'。 只有当输入标签 *PRIMER_MAX_PRODUCT_TM* 或 *PRIMER_MIN_PRODUCT_TM* 被定义后，此标签才会被打印输出。

.. note::

   3. `Rychlik W, Spencer WJ and Rhoads RE (1990) "Optimization of the annealing temperature for DNA amplification in vitro", Nucleic Acids Res 18:6409-12`__

__ http://www.pubmedcentral.nih.gov/articlerender.fcgi?tool=pubmed&pubmedid=2243783

**PRIMER_INTERNAL_OLIGO_MISHYB_SCORE=f, s**

f 是筛选引物时，针对 PRIMER_INTERNAL_OLIGO_MISHYB_LIBRARY 的最大错误杂交分；s 是库文件里对应的序列 id。

**PRIMER_{LEFT,RIGHT,INTERNAL_OLIGO}_MIN_SEQ_QUALITY=i**

i 是引物或探针序列最小的质量。不要和输出标签 PRIMER_PAIR_QUALITY 搞混。

**PRIMER_{LEFT,RIGHT}_END_STABILITY=f**

f 是引物5个 3' 端碱基的 delta G 值。

**PRIMER_STOP_CODON_POSITION=i**

i 是 primer3 找到的结束密码子的第一个碱基位置，如果该标签值为 -1 则表示没有找到。只有当输入标签 PRIMER_START_CODON_POSITION 值被自定义后，此标签才会在结果输出中打印。

4.2 输入标签
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

4.2.1 Sequence 引物输入标签
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

**SEQUENCE_ID** (string, default:empty)

用来在输出时显示的标记符以区别引物所对应的模板。 如果 PRIMER_FILE_FLAG 不等于 0，则必须使用此标签。

.. note:: (2.0版本中的MARKER_NAME是同义标签，现在已经废除)

**SEQUENCE_TEMPLATE** (nucleotide sequence, default:empty)

用来设计引物的模板序列。从 5' -> 3'，碱基大写或小写都可，所有碱基写在一行，不能用回车换行。

**SEQUENCE_INCLUDED_REGION** (interval list, default:empty)

挑选序列中一段子区域作为设计引物的模板。标签值的格式：

.. note::

   <start>,<length>

<start> 是定义的起始位点，<length> 是从该位点起的序列长度。 

**SEQUENCE_TARGET** (interval list, default:empty)

如果多个区域被定义作为重点扩增位点，则至少其中一个区域作为引物筛选区。多个排除区段可用空格区分。

.. note::

   <start>,<length>

<start> 是定义的起始位点，<length> 是从该位点起的序列长度。为了向下兼容，primer3 可以接受（但忽略）每个元素的拖尾。

**SEQUENCE_EXCLUDED_REGION** (interval list, default:empty)

定义引物和探针不能在的模板序列位点。多个排除区段可用空格区分。标签值的格式：

.. note::

   <start>,<length>

<start> 是定义的起始位点，<length> 是从该位点起的序列长度。 这个标签可以用来排除一些不能用来设计引物的区段。

**SEQUENCE_PRIMER_PAIR_OK_REGION_LIST** (semicolon separated list of integer "quadruples"; default:empty)

这个标签允许你详细的指定引物所在位置的详细信息。

标签值必须由分号分割。

<left_start>, <left_length>, <right_start>, <right_length> 结构标志位置与搜索范围。上游引物必须在 <left_start>, <left_length> 指定的范围内,下游引物必须在 <right_start>, <right_length> 指定的范围内。 <left_start> 和 <right_start> 分别定义上下游引物的起始搜索位置， <left_length> 和 <right_length> 分别定义上下游引物从搜索起始位置开始往下游的搜索距离。如果没有指定数值，则不对这个条件做限制。

比如：

SEQUENCE_PRIMER_PAIR_OK_REGION_LIST=100,50,300,50 ; 900,60,, ; ,,930,100

指定引物的条件有3个：

上游引物从第100个碱基开始往后的50个碱基内查找，并且下游引物从第300个碱基位置开始并往后50个碱基内查找。

或者

上游引物在第900个碱基开始往后60个碱基内查找，下游引物随意。

或者

下游引物从第930个碱基开始往后100个碱基内查找，上游引物随意。

**SEQUENCE_OVERLAP_JUNCTION_LIST** (space separated integers; default:empty)

If this list is not empty, then the forward OR the reverse primer must overlap one of these junction points by at least PRIMER_MIN_3_PRIME_OVERLAP_OF_JUNCTION nucleotides at the 3' end and at least PRIMER_MIN_5_PRIME_OVERLAP_OF_JUNCTION nucleotides at the 5' end. 
如果列表非空值，
In more detail: The junction associated with a given position is the space immediately to the right of that position in the template sequence on the strand given as input. 

比如：

SEQUENCE_OVERLAP_JUNCTION_LIST=20
# 1-based indexes
PRIMER_MIN_3_PRIME_OVERLAP_OF_JUNCTION=4

template: atcataggccatgcctgagc^gctacgact

          ok           ...gagc^gcta-3'  (left primer)
          not ok       ...gagc^gct-3'   (left primer)
          ok           3'-ctcg^cgat...  (right pimer)
          not ok        3'-tcg^cgat...  (right primer)

PRIMER_MIN_5_PRIME_OVERLAP_OF_JUNCTION=5

         ok           5'-tgagc^gccg...  (left primer)
         not ok        5'-gagc^gccg...  (left primer)
         ok           ...tgagc^gctac-5' (right primer)
         not ok       ...tgagc^gcta-5'  (right primer)

**SEQUENCE_QUALITY** (space separated integers; default:empty)

该标签的值是由一串空格分割的整数。例如对于序列 ANNTTCA...， *SEQUENCE_QUALITY* 可能等于 45 10 0 50 30 34 50 67... 数字越大表示该位点可信度越高，反之亦然。这个参数只有在你使用碱基调用程序（比如 phred__ ）来提供质量信息时才有用。

__ http://www.phrap.org/phredphrapconsed.html#block_phred



**PRIMER_START_CODON_POSITION** (int, default:1,000,000)

该标签目前还处于实验阶段。如果要使用此标签，请仔细查看输出，某些错误的输入可能会导致程序出错。

这个标签是起始密码子的索引号，可以允许程序选择相应的 ORF 来挑选扩增引物对。例如创建一个 fusion 蛋白的模板。Primer3  会尝试从起始密码子上游或下游寻找一个框内的上游引物。该值可以为负数，表明真实启动子在序列左侧。如果为正值，并且根据参数指定的密码子不是 ATG 则程序返回一个错误信息。当值小于等于 -10^6 表明 Primer3 应忽略这个标签参数。

Primer3 根据上游引物搜索终止密码子的结果来挑选下游引物。理想条件下，下游引物会在终止密码子上或者会在它下游的位置上。

**SEQUENCE_FORCE_LEFT_START** (int; default:-1)

强制上游引物的5'端为指定位置来筛选引物，即使违反其他一些设置，也会优先考虑此选项，因此符合此设置的引物也会被挑选出来。

**SEQUENCE_FORCE_LEFT_END** (int; default:-1)

强制上游引物的3'端为指定位置来筛选引物，即使违反其他一些设置，也会优先考虑此选项，因此符合此设置的引物也会被挑选出来。

**SEQUENCE_FORCE_RIGHT_START** (int; default:-1)

强制下游引物的5'端为指定位置来筛选引物，即使违反其他一些设置，也会优先考虑此选项，因此符合此设置的引物也会被挑选出来。

**SEQUENCE_FORCE_RIGHT_END** (int; default:-1)

强制下游引物的3'端为指定位置来筛选引物，即使违反其他一些设置，也会优先考虑此选项，因此符合此设置的引物也会被挑选出来。


4.2.2 Global 引物输入标签
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

**PRIMER_LEFT_INPUT** (nucleotide sequence, default empty)

定义一条上游引物来设计针对该引物的下游引物和探针。

**PRIMER_RIGHT_INPUT** (nucleotide sequence, default empty)

定义一条下游引物来设计针对该引物的下游引物和探针。


**PRIMER_COMMENT** (string, optional)

相当于注释，标签的值不会被用于计算挑选引物。

**COMMENT** (string, optional)

已被废除，功能由 PRIMER_COMMENT 代替。


**PRIMER_PICK_ANYWAY** (boolean, default 0)

如果设置为True，保证有引物结果输出，不受以下3个参数限制：

::

   PRIMER_LEFT_INPUT
   PRIMER_RIGHT_INPUT 
   PRIMER_INTERNAL_OLIGO_INPUT

**PRIMER_MISPRIMING_LIBRARY** (string, optional)

载入一个核苷酸序列库的文件，以缩小引物挑选范围(如避免重复序列，或可能在一个基因家族，不应扩增基因的序列)，该文件必须是FASTA格式。 如果该指定该参数，则primer3将每一个候选引物与库里的序列进行比对，然后剔除评分低于 PRIMER_MAX_MISPRIMING 设置的引物。 该FASTA格式的文件中，每条序列必须从一个“>“开始标注id。 因为id行的内容是松散限制的，所以primer3解析星号('*')之后的所有可选的浮点数字。如果ID行不包含星那么 weight 值默认为1.0。 该比对评分系统采用与计算寡核苷酸之间的互补性（如PRIMER_SELF_ANY）相同的算法，除IUB处理/ IUPAC模糊代码。 序列信息从第二行开始直到下一条以'>'开始的序列信息之前，或者直到文件结束行。所有的空格和换行会被忽略。字符 'A', 'T', 'G', 'C', 'a', 't', 'g', 'c' 和IUB/IUPAC的'ambiguity' codes ('R', 'Y', 'K', 'M', 'S', 'W', 'N', 'r', 'y', 'k', 'm', 's', 'w', 'n') 被保留。对序列的长度是没有限制的，但是长度必须大于3。当然，小于10的序列显然也是无意义的，但是程序也会接受该序列并且不报错。

.. warning::

   + 如果序列中含有'N'的未知碱基的话，一定要设置：PRIMER_LIB_AMBIGUITY_CODES_CONSENSUS=0

此参数为空值的话表明没有重复库被应用，并且关闭之前指定的库。 Repbase_ (J. Jurka, A.F.A. Smit, C. Pethiyagoda, and others, 1995-1996) 是一个很好的重复序列库文件。使用前需要现将其转换为FASTA格式的文件。

.. _Repbase: ftp://ncbi.nlm.nih.gov/repository/repbase

**PRIMER_LIB_AMBIGUITY_CODES_CONSENSUS** (boolean, default 1)

If set to 1, treat ambiguity codes as if they were consensus
codes when matching oligos to mispriming or mishyb
libraries. For example, if this flag is set, then a C in an
oligo will be scored as a perfect match to an S in a library
sequence, as will a G in the oligo. More importantly,
though, any base in an oligo will be scored as a perfect
match to an N in the library.  This is very bad if the
library contains strings of Ns, as no oligo will be legal
(and it will take a long time to find this out). So unless
you know for sure that your library does not have runs of Ns
(or Xs), then set this flag to 0.

**PRIMER_MAX_MISPRIMING** (decimal,9999.99, default 12.00)

比对 PRIMER_MISPRIMING_LIBRARY 的序列库文件后，允许的最大相似性评分。 

**PRIMER_MAX_TEMPLATE_MISPRIMING** (decimal,9999.99, default -1.00)

模板上最大异常位点相似性评分。这套评分系统除了模板上的模糊碱基不被当作同一种东西外，其他与 *PRIMER_MAX_MISPRIMING* 一致。

.. seealso:: PRIMER_LIB_AMBIGUITY_CODES_CONSENSUS

**PRIMER_PAIR_MAX_MISPRIMING** (decimal,9999.99, default 24.00)

引物对与 PRIMER_MISPRIMING_LIBRARY 库里任何序列的错配最大允许的分数。库里序列的 weights 不被用来进行这种相似性计算。

**PRIMER_PAIR_MAX_TEMPLATE_MISPRIMING** (decimal,9999.99, default -1.00)

The maximum allowed summed similarity of both primers to
ectopic sites in the template. A negative value means do not
check.  The scoring system is the same as used for
PRIMER_PAIR_MAX_MISPRIMING, except that an ambiguity code in
the template is never treated as a consensus (see
PRIMER_LIB_AMBIGUITY_CODES_CONSENSUS).  Primer3 does not
check the similarity of hybridization oligos (internal
oligos) to locations outside of the amplicon.

**PRIMER_PRODUCT_MAX_TM** (float, default 1000000.0)

扩增产物的最大 Tm值。

Primer3 计算产物的 Tm 值的方法[4]:

.. note::

   4. `Bolton and McCarthy, PNAS 84:1390 (1962) as presented in Sambrook, Fritsch and Maniatis, Molecular Cloning, p 11.46 (1989, CSHL Press)`

::

   Tm = 81.5 + 16.6(log10([Na+])) + .41*(%GC) - 600/length

   # [Na+] 钠离子浓度， (%GC) 序列的 GC 含量， length 序列长度。

GCG_ 里的引物设计软件也使用了类似的公式，只是最后一个值采用 675.0/length [5]。 这2个公式都采用 Na+ 而不是 K+ 来计算。 根据文献[6] K+ 相当于 0.2 M Na+。 Primer3 用了相同的盐浓度值作为计算引物 Tm 和 oligo Tm 值。如果你打算使用PCR产物做杂交，这里不能提供杂交的 Tm 值。

.. note::

   5. `F. Baldino, Jr, M.-F. Chesselet, and M.E. Lewis, Methods in Enzymology 168:766 (1989) eqn (1) on page 766`

   6. `J.G. Wetmur, Critical Reviews in BioChem. and Mol. Bio. 26:227 (1991) 50 mM`
 
.. _GCG: http://www.gcg.com

**PRIMER_PRODUCT_MIN_TM** (float, default -1000000.0)

扩增产物的最小 Tm值。

**PRIMER_EXPLAIN_FLAG** (boolean, default 0)

如果 flag 不等于0，则产生以下输出标签：

|  PRIMER_LEFT_EXPLAIN
|  PRIMER_RIGHT_EXPLAIN
|  PRIMER_INTERNAL_OLIGO_EXPLAIN

并且输出记录引物信息的相应文件，并统计各种原因造成的丢弃引物的数量。如果有 -format_output 参数，则信息更为可读。

**PRIMER_PRODUCT_SIZE_RANGE** (size range list, default 100-300)

扩增产物的大小区间

可以指定相关值来定义所需要扩增片段的产度，格式如下：

.. note:: 
   
   PRIMER_PRODUCT_SIZE_RANGE=<x>-<y>

<x>-<y> 表示长度区间。 比如让产物大小在 100-150 bp之间，可以写成 '100-150'

可以同时写多组产物大小的区间，程序会按照顺序优先选择第一个产物长度区间所能设计的引物，只有当这个区间无法找到足够的引物时，才会去下一个区间搜索。注意，这里定义的区间是产物片段长度，也就是包括引物的片段长度。

**PRIMER_PICK_INTERNAL_OLIGO** (boolean, default 0)

如果此标签值为非0，primer3 会尝试挑选一个杂交探针。如果为了向下兼容，则可以使用 PRIMER_TASK。

**PRIMER_GC_CLAMP** (int, default 0)

上下游引物在 3' 端的连续 GC 数量。

**PRIMER_OPT_SIZE** (int, default 20)

最适引物长度，默认值为20。程序会尽可能选择接近该参数的引物。

**PRIMER_DEFAULT_SIZE** (int, default 20)

作为与v2版本兼容的标签已经废除，与 *PRIMER_OPT_SIZE* 同意，

**PRIMER_MIN_SIZE** (int, default 18)

引物最小长度，默认值为18。该值必须大于0且小于等于PRIMER_MAX_SIZE。

**PRIMER_MAX_SIZE** (int, default 27)

引物长度最大可接受值。目前该参数不能大于35。

**PRIMER_OPT_TM** (float, default 60.0C)

引物tm的优化参数。程序会选择尽可能接近该值的引物。tm规则可以由用户自定义。

.. seealso:: PRIMER_TM_SANTALUCIA

**PRIMER_MIN_TM** (float, default 57.0C)

引物tm最小可接受值。

**PRIMER_MAX_TM** (float, default 63.0C)

引物tm最大可接受值。

**PRIMER_MAX_DIFF_TM** (float, default 100.0C)

上游和下游引物tm值差异的最大可接受值。

**PRIMER_TM_SANTALUCIA** (int, default 0)

自定义 Tm 计算方式。(New in v1.1.0, added by Maido Remm and Triinu Koressaar.)

推荐该值为1，这表明 primer3使用文献[7]的热力学计算值来作为 Tm 计算方式。

.. note::

   7. `SantaLucia JR (1998) "A unified view of polymer, dumbbell and oligonucleotide DNA nearest-neighborthermodynamics", Proc Natl Acad Sci 95:1460-65`__

__ http://dx.doi.org/10.1073/pnas.95.4.1460

如果该值为0，则 primer3 采用以前的计算方式[8],[9]

.. note::

   8. `Breslauer KJ, Frank R, Blöcker H and Marky LA (1986) "Predicting DNA duplex stability from the base sequence" Proc Natl Acad Sci 83:4746-50`__

   9. `Rychlik W, Spencer WJ and Rhoads RE (1990) "Optimization of the annealing temperature for DNA amplification in vitro", Nucleic Acids Res 18:6409-12`__

__ http://dx.doi.org/10.1073/pnas.83.11.3746
 
__ http://www.pubmedcentral.nih.gov/articlerender.fcgi?tool=pubmed&pubmedid=2243783


可以用 *PRIMER_SALT_CORRECTIONS* 来定义 Tm 计算的 salt 纠正方法。例如：

| PRIMER_TM_SANTALUCIA=1
| PRIMER_SALT_CORRECTIONS=1

引物为： **primer=CGTGACGTGACGGACT**

用默认的 salt 和 DNA 浓度：

.. code-block:: none

   Tm = deltaH/(deltaS + R*ln(C/4)), 

R是气体常数(1.987 cal/K mol)，C 是 DNA 浓度

.. code-block:: none

   deltaH(predicted)

     = dH(CG) + dH(GT) + dH(TG) + .. + dH(CT) + dH(init.w.term.GC) + dH(init.w.term.AT)

     = -10.6 + (-8.4) + (-8.5) + .. + (-7.8) + 0.1 + 2.3

     = -128.8 kcal/mol

'init.w.term GC' 和 'init.w.term AT' 是初始参数 'initiation with terminal GC' 和 'initiation with terminal AT'

.. code-block:: none

   deltaS(predicted) 
   
     = dS(CG) + dS(GT) + dS(TG) + .. + dS(CT) + dS(init.w.term.GC) + dS(init.w.term.AT) 

     = -27.2 + (-22.4) + (-22.7) + .. + (-21.0) + (-2.8) + 4.1
 
     = -345.2 cal/k*mol

   deltaS(salt corrected) 
   
     = deltaS(predicted) + 0.368*15(NN pairs)*ln(0.05M monovalent cations)
     
     = -361.736

   Tm = -128.800/(-361.736+1.987*ln((5*10^(-8))/4)) 
   
      = 323.704 K

   Tm(C) = 323.704 - 273.15 = 50.554 C


**PRIMER_SALT_CONC** (float, default 50.0 mM)

PCR 反应中毫摩尔单位的单价盐浓度（通常为 KCl）。Primer3 用这个参数来计算引物的 Tm 值。用标签 *PRIMER_DIVALENT_CONC* 定义双价盐浓度。（在这个例子中你还应该使用标签 PRIMER_DNTP_CONC）

**PRIMER_DIVALENT_CONC** (float, default 0.0 mM)

PCR 反应中毫摩尔单位的双价盐浓度（通常为 MgCl）。(New in v. 1.1.0, added by Maido Remm and Triinu Koressaar) 

Primer3 转换浓度的公式来自与文献[10].

.. note::

   10. `Ahsen von N, Wittwer CT, Schutz E (2001) "Oligonucleotide Melting Temperatures under PCR Conditions: Nearest-Neighbor Corrections for Mg^(2+), Deoxynucleotide Triphosphate, and Dimethyl Sulfoxide Concentrations with Comparision to Alternative Empirical Formulas", Clinical Chemistry 47:1956-61`__

__ http://www.clinchem.org/cgi/content/full/47/11/1956

.. code-block:: none

   [Monovalent cations] = [Monovalent cations] + 120*(([divalent cations] - [dNTP])^0.5)

根据该公示，dNTP的浓度必须小于双价盐浓度。否则，双价盐浓度的作用效果将不被计算在内。dNTP 浓度要被减掉是因为一些 Mg 离子会与 dNTP 绑定，避免重复计算效果。

**PRIMER_DNTP_CONC** (float, default 0.0 mM)

PCR 反应中毫摩尔单位的 dNTP 浓度。这个参数只有在 *PRIMER_DIVALENT_CONC* 标签被使用的情况下才有意义。

**PRIMER_SALT_CORRECTIONS** (int, default 0)

修正盐浓度公式以计算 Tm 值。(New in v. 1.1.0, added by Maido Remm and Triinu Koressaar)

建议将其值设置为 1，这样 Primer3会根据文献[11]来修正公式。

.. note::

   11. `SantaLucia JR (1998) "A unified view of polymer, dumbbell and oligonucleotide DNA nearest-neighbor thermodynamics", Proc Natl Acad Sci 95:1460-65`__

__ http://dx.doi.org/10.1073/pnas.95.4.1460

如果值为 0，则 Primer3 使用文献[12]的公式，此公式在一直使用在之前版本的 Primer3。

.. note::

   12. `Schildkraut, C, and Lifson, S (1965) "Dependence of the melting temperature of DNA on salt concentration", Biopolymers 3:195-208`

如果值为 2，则使用文献[13]的公式。

.. note::

   13. `Owczarzy R, You Y, Moreira BG, Manthey JA, Huang L, Behlke MA and Walder JA (2004) "Effects of sodium ions on DNA duplex oligomers: Improved predictions of melting temperatures", Biochemistry 43:3537-54`__
   
__ http://dx.doi.org/10.1021/bi034621r

**PRIMER_LOWERCASE_MASKING** (int, default 0)

该标签允许程序智能化设计 masked regions 的引物，并以小写子母表示。(New in v. 1.1.0, added by Maido Remm and Triinu Koressaar)

如果该值为 1，则拒绝引物 3' 端小写的重叠结构。

这个工具依赖于假设 masked 位点(比如重复)能重复引物，但是无法重复引物的 3' 端。换句话说，引物其他位置上小写的碱基可以接受。

**PRIMER_MIN_GC** (float, default 20.0%)

GC含量最小值。

**PRIMER_OPT_GC_PERCENT** (float, default 50.0%)

GC含量优化值。只有在 PRIMER_WT_GC_PERCENT_GT 或者 PRIMER_WT_GC_PERCENT_LT 不等于0时，该参数才会影响引物的选择。

**PRIMER_MAX_GC** (float, default 80.0%)

GC含量最大值。

**PRIMER_DNA_CONC** (float, default 50.0 nM)

The nanomolar concentration of annealing oligos in the PCR.
Primer3 uses this argument to calculate oligo melting
temperatures.  The default (50nM) works well with the standard
protocol used at the Whitehead/MIT Center for Genome
Research--0.5 microliters of 20 micromolar concentration for each
primer oligo in a 20 microliter reaction with 10 nanograms
template, 0.025 units/microliter Taq polymerase in 0.1 mM each
dNTP, 1.5mM MgCl2, 50mM KCl, 10mM Tris-HCL (pH 9.3) using 35
cycles with an annealing temperature of 56 degrees Celsius.  This
parameter corresponds to 'c' in equation (ii) of the paper
[14],
where a suitable value (for a
lower initial concentration of template) is "empirically
determined".  The value of this parameter is less than the actual
concentration of oligos in the reaction because it is the
concentration of annealing oligos, which in turn depends on the
amount of template (including PCR product) in a given cycle.
This concentration increases a great deal during a PCR;
fortunately PCR seems quite robust for a variety of oligo melting
temperatures.

.. note::

   14. `Rychlik W, Spencer WJ and Rhoads RE (1990) "Optimization of the annealing temperature for DNA amplification in vitro", Nucleic Acids Res 18:6409-12`__

__ http://www.pubmedcentral.nih.gov/articlerender.fcgi?tool=pubmed&pubmedid=2243783

See ADVICE FOR PICKING PRIMERS.

**PRIMER_NUM_NS_ACCEPTED** (int, default 0)

Maximum number of unknown bases (N) allowable in any primer.

**PRIMER_SELF_ANY** (decimal,9999.99, default 8.00)

The maximum allowable local alignment score when testing a single
primer for (local) self-complementarity and the maximum allowable
local alignment score when testing for complementarity between
left and right primers.  Local self-complementarity is taken to
predict the tendency of primers to anneal to each other without
necessarily causing self-priming in the PCR.  The scoring system
gives 1.00 for complementary bases, -0.25 for a match of any base
(or N) with an N, -1.00 for a mismatch, and -2.00 for a gap.
Only single-base-pair gaps are allowed.  For example, the
alignment

.. code-block:: none

   5' ATCGNA 3'
      || | |
   3' TA-CGT 5'

is allowed (and yields a score of 1.75), but the alignment

.. code-block:: none

   5' ATCCGNA 3'
      ||  | |
   3' TA--CGT 5'

is not considered.  Scores are non-negative, and a score of 0.00
indicates that there is no reasonable local alignment between two
oligos.

**PRIMER_SELF_END** (decimal 9999.99, default 3.00)

The maximum allowable 3'-anchored global alignment score when
testing a single primer for self-complementarity, and the maximum
allowable 3'-anchored global alignment score when testing for
complementarity between left and right primers.  The 3'-anchored
global alignment score is taken to predict the likelihood of
PCR-priming primer-dimers, for example

.. code-block:: none

   5' ATGCCCTAGCTTCCGGATG 3'
                ||| |||||
             3' AAGTCCTACATTTAGCCTAGT 5'

或者

.. code-block:: none

	5' AGGCTATGGGCCTCGCGA 3'
	               ||||||
	            3' AGCGCTCCGGGTATCGGA 5'

The scoring system is as for the Maximum Complementarity
argument.  In the examples above the scores are 7.00 and 6.00
respectively.  Scores are non-negative, and a score of 0.00
indicates that there is no reasonable 3'-anchored global
alignment between two oligos.  In order to estimate 3'-anchored
global alignments for candidate primers and primer pairs, Primer
assumes that the sequence from which to choose primers is
presented 5'->3'.  It is nonsensical to provide a larger value
for this parameter than for the Maximum (local) Complementarity
parameter because the score of a local alignment will always be at
least as great as the score of a global alignment.

**PRIMER_DEFAULT_PRODUCT** (size range list, default 100-300)

已被废除，与 *PRIMER_PRODUCT_SIZE_RANGE* 同义。

**PRIMER_FILE_FLAG** (boolean, default 0)

If the associated value is non-0, then primer3 creates two output
files for each input SEQUENCE.  File <sequence_id>.for lists all
acceptable left primers for <sequence_id>, and <sequence_id>.rev
lists all acceptable right primers for <sequence_id>, where
<sequence_id> is the value of the PRIMER_SEQUENCE_ID tag (which
must be supplied).  In addition, if the input tag
PRIMER_PICK_INTERNAL_OLIGO is non-0, primer3 produces a file
<sequence_id>.int, which lists all acceptable internal oligos.

**PRIMER_MAX_POLY_X** (int, default 5)

引物允许的最大单核苷酸重复长度，比如 AAAAAA

**PRIMER_LIBERAL_BASE** (boolean, default 0)

This parameter provides a quick-and-dirty way to get primer3 to
accept IUB / IUPAC codes for ambiguous bases (i.e. by changing
all unrecognized bases to N).  If you wish to include an
ambiguous
base in an oligo, you must set PRIMER_NUM_NS_ACCEPTED to a
non-0 value.

Perhaps '-' and '*' should be squeezed out rather than changed
to 'N', but currently they simply get converted to N's.  The authors
invite user comments.

**PRIMER_NUM_RETURN** (int, default 5)

The maximum number of primer pairs to return.  Primer pairs
returned are sorted by their "quality", in other words by the
value of the objective function (where a lower number indicates a
better primer pair).  Caution: setting this parameter to a large
value will increase running time.

**PRIMER_FIRST_BASE_INDEX** (int, default 0)

This parameter is the index of the first base in the input
sequence.  For input and output using 1-based indexing (such as
that used in GenBank and to which many users are accustomed) set
this parameter to 1.  For input and output using 0-based indexing
set this parameter to 0.  (This parameter also affects the
indexes in the contents of the files produced when the primer
file flag is set.)

**PRIMER_MIN_QUALITY** (int, default 0)

引物最小序列质量(由 *PRIMER_SEQUENCE_QUALITY* 定义)

**PRIMER_MIN_END_QUALITY** (int, default 0)

The minimum sequence quality (as specified by
PRIMER_SEQUENCE_QUALITY) allowed within the 5' pentamer of a
primer.

**PRIMER_QUALITY_RANGE_MIN** (int, default 0)

The minimum legal sequence quality (used for error checking
of PRIMER_MIN_QUALITY and PRIMER_MIN_END_QUALITY).

**PRIMER_QUALITY_RANGE_MAX** (int, default 100)

The maximum legal sequence quality (used for error checking
of PRIMER_MIN_QUALITY and PRIMER_MIN_END_QUALITY).

**PRIMER_INSIDE_PENALTY** (float, default -1.0)

Non-default values are valid only for sequences with 0 or 1
target regions.  If the primer is part of a pair that spans a
target and overlaps the target, then multiply this value times
the number of nucleotide positions by which the primer overlaps
the (unique) target to get the 'position penalty'.  The effect of
this parameter is to allow primer3 to include overlap with the
target as a term in the objective function.

**PRIMER_OUTSIDE_PENALTY** (float, default 0.0)

Non-default values are valid only for sequences with 0 or 1
target regions.  If the primer is part of a pair that spans a
target and does not overlap the target, then multiply this value
times the number of nucleotide positions from the 3' end to the
(unique) target to get the 'position penalty'.  The effect of
this parameter is to allow primer3 to include nearness to the
target as a term in the objective function.

**PRIMER_MAX_END_STABILITY** (float 999.9999, default 100.0)

引物 3' 端最后5个碱基的最大稳定度。数值越大表示要求引物的 3' 端更稳定。这个值是 delta G (kcal/mol) 最大值。由 *PRIMER_TM_SANTALUCIA* 以邻近法计算出的5个 3' 端碱基的值。

如果 *PRIMER_TM_SANTALUCIA=1*, 那么最稳定的 (GCGCG) delta G 是 6.86 kcal/mol, 最不稳定的 (TATAT) delta G 是 0.86 kcal/mol.

如果 *PRIMER_TM_SANTALUCIA=0*, 那么最稳定的 (GCGCG) delta G 是 13.4 kcal/mol, 最不稳定的 (TATAC) delta G 是 4.6 kcal/mol.

**PRIMER_PRODUCT_OPT_TM** (float, default 0.0)

PCR 产物优化的 Tm 值。0 表示没有专门优化温度。

**PRIMER_PRODUCT_OPT_SIZE** (int, default 0)

PCR 产物优化的长度。0 表示没有专门优化长度。这个参数值只有当 *PRIMER_PAIR_WT_PRODUCT_SIZE_GT* 或者 *PRIMER_PAIR_WT_PRODUCT_SIZE_LT* 不等于0时才有效果。

**PRIMER_TASK** (string, default pick_pcr_primers)

告诉 primer3 执行那种任务。有以下几个值可选：

::

 pick_pcr_primers
 pick_pcr_primers_and_hyb_probe
 pick_left_only
 pick_right_only
 pick_hyb_probe_only

如果设置任务为 pick_pcr_primers_and_hyb_probe，会等同于设置 PRIMER_PICK_INTERNAL_OLIGO 为一个非0值并将 PRIMER_TASK 设置为 pick_pcr_primers 的任务。

**PRIMER_WT_TM_GT** (float, default 1.0)

设置引物 Tm 值超过 PRIMER_OPT_TM 时的惩罚值。

**PRIMER_WT_TM_LT** (float, default 1.0)

设置引物 Tm 值低于 PRIMER_OPT_TM 时的惩罚值。

**PRIMER_WT_SIZE_LT** (float, default 1.0)

设置引物长度小于 PRIMER_OPT_SIZE 的惩罚值。

**PRIMER_WT_SIZE_GT** (float, default 1.0)

设置引物长度大于 PRIMER_OPT_SIZE 的惩罚值。

**PRIMER_WT_GC_PERCENT_LT** (float, default 1.0)

设置引物 GC 含量低于 PRIMER_OPT_GC_PERCENT 的惩罚值。

**PRIMER_WT_GC_PERCENT_GT** (float, default 1.0)

设置引物 GC 含量高于 PRIMER_OPT_GC_PERCENT 的惩罚值。

| **PRIMER_WT_COMPL_ANY** (float, default 0.0)
| **PRIMER_WT_COMPL_END** (float, default 0.0)
| **PRIMER_WT_NUM_NS** (float, default 0.0)
| **PRIMER_WT_REP_SIM** (float, default 0.0)
| **PRIMER_WT_SEQ_QUAL** (float, default 0.0)
| **PRIMER_WT_END_QUAL** (float, default 0.0)
| **PRIMER_WT_POS_PENALTY** (float, default 0.0)
| **PRIMER_WT_END_STABILITY** (float, default 0.0)
| **PRIMER_WT_TEMPLATE_MISPRIMING** (float, default 0.0)
| **PRIMER_PAIR_WT_PR_PENALTY** (float, default 1.0)
| **PRIMER_PAIR_WT_IO_PENALTY** (float, default 0.0)
| **PRIMER_PAIR_WT_DIFF_TM** (float, default 0.0)
| **PRIMER_PAIR_WT_COMPL_ANY** (float, default 0.0)
| **PRIMER_PAIR_WT_COMPL_END** (float, default 0.0)
| **PRIMER_PAIR_WT_PRODUCT_TM_LT** (float, default 0.0)
| **PRIMER_PAIR_WT_PRODUCT_TM_GT** (float, default 0.0)
| **PRIMER_PAIR_WT_PRODUCT_SIZE_GT** (float, default 0.0)
| **PRIMER_PAIR_WT_PRODUCT_SIZE_LT** (float, default 0.0)
| **PRIMER_PAIR_WT_REP_SIM** (float, default 0.0)
| **PRIMER_PAIR_WT_TEMPLATE_MISPRIMING** (float, default 0.0)

就像参数控制着 PCR 引物的选取，这些控制着探针选择的输入标签分成为几类： Sequence 输入标签，Global 输入标签。
Like the arguments governing PCR primer selection, the input tags
governing internal oligo selection are divided into sequence
input tags and global input tags, with for former being
automatically reset after each input record, and the latter
persisting until explicitly reset.

Because the laboratory detection step using internal oligos
is independent of the PCR amplification procedure,
internal oligo tags have defaults that are independent
of the parameters that govern the selection of PCR primers.
For example, the melting temperature of an oligo
used for hybridization might be considerably lower
than that used as a PCR primer.

4.2.3 "Sequence" 探针输入标签
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

**PRIMER_INTERNAL_OLIGO_EXCLUDED_REGION** (interval list, default empty)

引物之间的探针可能无法与指定区域匹配，该标签值用空格分割

.. note::

   <start>,<length>

<start> 是定义的起始位点，<length> 是从该位点起的序列长度。

**PRIMER_INTERNAL_OLIGO_INPUT** (nucleotide sequence, default empty)

检测探针序列是符合引物。必须是模板的一段序列。

4.2.4 "Global" 探针输入标签
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

这些标签与之前讨论的 Global 输入标签类似，区别是：

| **PRIMER_INTERNAL_OLIGO_SELF_END** 当挑选用于杂交的探针时，因为不会产生引物二聚体，所以是无意义的。
| **PRIMER_INTERNAL_OLIGO_SELF_END** 建议至少设置成与 PRIMER_INTERNAL_OLIGO_SELF_ANY 一样大的值。

| **PRIMER_INTERNAL_OLIGO_OPT_SIZE** (int, default 20)
| **PRIMER_INTERNAL_OLIGO_MIN_SIZE** (int, default 18)
| **PRIMER_INTERNAL_OLIGO_MAX_SIZE** (int, default 27)
| **PRIMER_INTERNAL_OLIGO_OPT_TM** (float, default 60.0 degrees C)
| **PRIMER_INTERNAL_OLIGO_OPT_GC_PERCENT** (float, default 50.0%)
| **PRIMER_INTERNAL_OLIGO_MIN_TM** (float, default 57.0 degrees C)
| **PRIMER_INTERNAL_OLIGO_MAX_TM** (float, default 63.0 degrees C)
| **PRIMER_INTERNAL_OLIGO_MIN_GC** (float, default 20.0%)
| **PRIMER_INTERNAL_OLIGO_MAX_GC** (float, default 80.0%)
| **PRIMER_INTERNAL_OLIGO_SALT_CONC** (float, default 50.0 mM)
| **PRIMER_INTERNAL_OLIGO_DIVALENT_CONC** (float, default 0.0 mM)
| **PRIMER_INTERNAL_OLIGO_DNTP_CONC** (float, default 0.0 mM)
| **PRIMER_INTERNAL_OLIGO_DNA_CONC** (float, default 50.0 nM)
| **PRIMER_INTERNAL_OLIGO_SELF_ANY** (decimal 9999.99, default 12.00)
| **PRIMER_INTERNAL_OLIGO_MAX_POLY_X** (int, default 5)
| **PRIMER_INTERNAL_OLIGO_SELF_END** (decimal 9999.99, default 12.00)

**PRIMER_INTERNAL_OLIGO_MISHYB_LIBRARY** (string, optional)

与 PRIMER_MISPRIMING_LIBRARY 类似， 但是这是为了避免与库里的序列和探针产生杂交。

**PRIMER_INTERNAL_OLIGO_MAX_MISHYB** (decimal,9999.99, default 12.00)

与 PRIMER_MAX_MISPRIMING 类似，但是这个参数作用于 PRIMER_INTERNAL_OLIGO_MISHYB_LIBRARY。

**PRIMER_INTERNAL_OLIGO_MAX_TEMPLATE_MISHYB** (decimal,9999.99, default 12.00)

未被使用。

**PRIMER_INTERNAL_OLIGO_MIN_QUALITY** (int, default 0)

请注意没有 PRIMER_INTERNAL_OLIGO_MIN_END_QUALITY 标签

| **PRIMER_IO_WT_TM_GT** (float, default 1.0)
| **PRIMER_IO_WT_TM_LT** (float, default 1.0)
| **PRIMER_IO_WT_GC_PERCENT_GT** (float, default 1.0)
| **PRIMER_IO_WT_GC_PERCENT_LT** (float, default 1.0)
| **PRIMER_IO_WT_SIZE_LT** (float, default 1.0)
| **PRIMER_IO_WT_SIZE_GT** (float, default 1.0)
| **PRIMER_IO_WT_COMPL_ANY** (float, default 0.0)
| **PRIMER_IO_WT_COMPL_END** (float, default 0.0)
| **PRIMER_IO_WT_NUM_NS** (float, default 0.0)
| **PRIMER_IO_WT_REP_SIM** (float, default 0.0)
| **PRIMER_IO_WT_SEQ_QUAL** (float, default 0.0)
| **PRIMER_IO_WT_END_QUAL** (float, default 0.0)
