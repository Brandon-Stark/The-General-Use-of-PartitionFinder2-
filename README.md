"[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的一般使用方法"

-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------- 
**[Rob Lanfear](https://github.com/roblanf)**
![](http://www.robertlanfear.com/people/assets/rob_square.jpeg)
-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------

-----------------------------------------------------------------------------------------------

## 引言

-----------------------------------------------------------------------------------------------

>
* ### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)是目前对大中型数据集（数据类型包括：核苷酸、氨基酸和形态比对文件）同时进行最合适的分区方案和每个分区方案所对应的最优进化模型检测的最理想的程序，它所推演的最优进化模型结果与使用 [jModelTest2](http://www.atgc-montpellier.fr/phyml)和[ProtTest3](https://code.google.com/p/prottest3/)所推演的最优进化模型结果是比较接近的（**此观点来自布兰登·史塔克，如引用请注明出处**）。

>
* ### 这次练习的目的仅是教你 [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)最重要的功能。您将学习如何对比对文件进行分区，以便对比对文件的不同分区进行最优进化模型的选择。

>
* ### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的作者 [Rob Lanfear](https://github.com/roblanf)在[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/) [2.1.x手册](http://www.robertlanfear.com/partitionfinder/assets/Manual_v2.1.x.pdf)（18页）里面提到： 

<table><tr><td bgcolor=#7FFF00> **我目前最喜欢的用于进化模型选择的度量是AICc。一般来说，用户不应该用AIC，因为AICc与AIC相比始终是最好的。然而，AIC包含在partitionfinder里面大多是因为历史原因。** </td></tr></table>

>
* ### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的作者 [Rob Lanfear](https://github.com/roblanf)在[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/) [2.1.x手册](http://www.robertlanfear.com/partitionfinder/assets/Manual_v2.1.x.pdf)（21页）里面还提到：

 <table><tr><td bgcolor=#7FFF00> **一般来说，我不建议在任何情况下使用`search = hcluster;`这种算法。最好用`rcluster`这种算法并将`--rcluster-max`设置一些非常低的数字（例如，设置`--rcluster-max`为10）来代替。`hcluster`算法几乎与`rcluster`这种算法并将`--rcluster-max`设置为1是相同的。用户可以使用`--weights`命令行选项来控制该算法。该算法仍然在PartitionFinderl里面纯粹是因为它使得我们的研究证明了它是不值得重复使用的。** </td></tr></table>

-----------------------------------------------------------------------------------------------

## 入门

-----------------------------------------------------------------------------------------------

### **比对文件**

>
* #### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)可以支持松弛型连续的PHYLIP格式的输入比对文件，松弛型表示1-100个字符长度的序列名都是可以默认支持的。

### **配置文件**

>
* #### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的配置文件为`partition_finder.cfg`，注意，该配置文件的文件名是固定的，请不要更改它。

>
* #### `partition_finder.cfg`配置文件里面除了注释和方括号所在行的末尾不用分号结束，其他行的末尾要以分号结束。

>
* #### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的核苷酸配置文件举例如下：


```R
# ALIGNMENT FILE ##
alignment = test.phy;

## BRANCHLENGTHS: linked | unlinked ##
branchlengths = linked;

## MODELS OF EVOLUTION: all | allx | mrbayes | beast | gamma | gammai | <list> ##
models = GTR, GTR+G, GTR+I+G;

# MODEL SELECCTION: AIC | AICc | BIC #
model_selection = AICc;

## DATA BLOCKS: see manual for how to define ##
[data_blocks]
Gene1_pos1 = 1-789\3;
Gene1_pos2 = 2-789\3;
Gene1_pos3 = 3-789\3;
Gene2_pos1 = 790-1449\3;
Gene2_pos2 = 791-1449\3;
Gene2_pos3 = 792-1449\3;
Gene3_pos1 = 1450-2208\3;
Gene3_pos2 = 1451-2208\3;
Gene3_pos3 = 1452-2208\3;


## SCHEMES, search: all | user | greedy | rcluster | rclusterf | kmeans ##
[schemes]
search = greedy;
```
-----------------------------------------------------------------------------------------------

### 备注:

>
* #### *alignment = test.phy;*: 输入比对文件名，本例中为`test.phy`。这个文件必须与`partition_finder.cfg`配置文件在同一个文件夹里面，并且必须是正确的phylip格式。`test.phy`示例文件phylip格式的信息如下（为了展示的效果，序列文件被截短了）：


```R
 4 949
AD00P055  SLMLLISSSIVENGAGTGWTVYPPLSSNIAHSGSSVDLAIFSLHLAGISSILGAINFITTIINMKVNNLFFDQMSLFIWAVGITALLLLLSLPVLAGAITMLLTDRNLNT......
RV03N585  SLMLLISSSIVENGAGTGWTVYPPLSSNIAHSGSSVDLAIFSLHLAGISSILGAINFITTIINMKVNNLFFDQMSLFIWAVGITALLLLLSLPVLAGAITMLLTDRNLNT......
TDA99Q996 SLMLLISSSIVENGAGTGWTVYPPLSSNIAHSGSSVDLAIFSLHLAGISSILGAINFITTIINMKVNNMSFDQMSLFIWAVGITALLLLLSLPVLAGAITMLLTDRNLNT......
ZD99S305  SLMLLISSSIVENGAGTGWTVYPPLSSNIAHSGSSVDLAIFSLHLAGISSILGAINFITTIINMKVNNLFFDQMSLFIWAVGITALLLLLSLPVLAGAITMLLTDRNLNT......
```

> 
* #### *branchlengths = linked;*: 这个选项决定在用户分析的过程中枝长是如何被估计的。用户如何设置这个选项在一定程度上取决于用户打算用哪一个程序进行最终的系统发育分析。几乎所有的系统发育程序支持相关联的枝长，但只有一些支持不相关联的枝长（例如MrBayes、BEAST和[[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)](https://sco.h-its.org/exelixis/web/software/[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)/)）。`branchlengths = linked`的总枝长参数是`(2N-3)+(S-1)`,其中`N`代表物种数，`S`代表子集的数目。例如，如果有50个物种被分成10个子集，那么总枝长的参数就是106。`branchlengths = unlinked`的总枝长参数是`2NS-3S`,其中`N`代表物种数，`S`代表子集的数目。例如，如果有50个物种被分成10个子集，那么总枝长的参数就是970。

>  
* #### *models = GTR, GTR+G, GTR+I+G;*: 这个选项设置模型选择过程中需要考虑的进化模型。

> 
* #### *model_selection = AICc;*: 设置用于模型选择的度量。如果用户使用`search=greedy`这个选项的话，它还定义了用于比较分区方案的度量。

> 
* #### *[data_blocks]*: 设置数据分区。
    1. ##### [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)可以处理三种类型数据：核苷酸、氨基酸和形态。其中核苷酸数据又可以分为编码区DNA序列和非编码区DNA序列（例如内含子）。编码区DNA序列根据密码子位置又进一步分为三个分区：第一位密码子、第二位密码子和第三位密码子。
    2. ##### 根据[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)软件作者的研究文章，他们推荐用户使用数据分区来告诉[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)尽可能多的关于用户序列的生物学信息。
    3. ##### 设想一个DNA序列比对文件有2208bp的蛋白质编码区，分为三个基因，第一个基因的长度是789，第二个基因的长度是659bp,第三个基因的长度是758bp。那么就可以按照例子中的那样设置。
    4. ##### 氨基酸序列和非编码区DNA序列（例如内含子）可以按照基因设置数据分区，提供基因的起始和终止位点即可。
    5. ##### 对于形态学数据集来说，通常没有直观的方法来定义数据分区。在这种情况下，用户可以尝试`k-means`算法。

-----------------------------------------------------------------------------------------------

## [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的一般使用方法


### **对于一个小的多基因座数据集（例如大约10基因座）**

#### 对于DNA序列，用PhyML进行贪婪搜索。对于氨基酸序列，用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)进行贪婪搜索。下面的这些选项分别给出了DNA和氨基酸序列的大多数模型。


>
##### 1. 根据基因和密码子位置定义数据分区

>
##### 2. 在以.cfg为后缀的配置文件里面设置以下选项：

```R
branchlengths = linked;

models = all;

model_selection = AICc;

search = greedy;
```

>
##### 3. 对于DNA数据集，从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinder.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\nucleotide
```

>
##### 4. 对于氨基酸数据集，从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinderProtein.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\aminoacid
```
-----------------------------------------------------------------------------------------------

### 对于一个大的数据集（例如大约100基因座）

#### 对于DNA和氨基酸序列，用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)进行贪婪搜索。这通常相当快。下面的这些选项分别给出了DNA和氨基酸序列的大多数模型。

>
##### 1. 根据基因和密码子位置定义数据分区

>
##### 2. 在以.cfg为后缀的配置文件里面设置以下选项：

```R
branchlengths = linked;

models = all;

model_selection = AICc;

search = greedy;
```
>
##### 3. 对于DNA数据集，使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinder.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\nucleotide --[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)
```

>
##### 4. 对于氨基酸数据集，使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinderProtein.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\aminoacid --[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)
```

>
##### 5. 如果对于蛋白质来说运行太慢的话，可能是因为有很多模型正在进行比较，因此尝试在以.cfg为后缀的配置文件里面中指定一个较小的模型列表，例如：

```R
models = LG, LG+G, LG+I+G, LG+I+G+F, LG4X;
```

-----------------------------------------------------------------------------------------------

### 对于一个真正大的数据集（例如大约1000基因座）

#### 对于这种规模的数据集，贪婪算法很可能太慢。因此，我们可以使用一些更快的算法来尝试和猜测最好的分区方案。

>
##### 1. 根据基因和密码子位置定义数据分区

>
##### 2. 在以.cfg为后缀的配置文件里面设置以下选项：

```R
branchlengths = linked;

models = all;

model_selection = AICc;

search = rcluster;
```
>
##### 3. 对于DNA数据集，使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinder.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\nucleotide --[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)
```

>
##### 4. 对于氨基酸数据集，使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)从命令行运行[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinderProtein.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\aminoacid --[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)
```

>
##### 5. 这执行的是松弛型聚类算法,要想了解更多关于松弛型聚类算法，用户可以浏览[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)作者等人发表的[文章](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4012149/)。如果默认的命令对用户的数据集来说仍然太慢，请从默认的至少`rcluster-max = 1000`减少到例如`rcluster-max = 100`。关于这个的解释举例如下：

```R
C:\Python27\python.exe C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\PartitionFinderProtein.py C:\Users\BrandonStark\PartitionFinder\partitionfinder-2.1.1\examples\aminoacid --[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)
```
>
##### 5. `rcluser`算法给用户在平衡速度（`--rcluster-max）和精度（--rcluster-percent）这两个参数之间很多的控制。

-----------------------------------------------------------------------------------------------

### 备注:

>
1. #### *user_tree_topology*

>
    * #####  这是一个附加选项，可以在`alignment`行之后添加到以.cfg为后缀的配置文件中。如果用户想给[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)提供一个固定的拓扑结构树，而不是依靠软件默认估计的邻接法拓扑结构树，可以使用这个选项。如果用户提前知道真正的树是什么，例如，在进行模拟时，这可能是有用的。要使用该选项，只需在以.cfg为后缀的配置文件中添加额外的一行即可，示例如下：


```R
ALIGNMENT FILE #
alignment = test.phy;
user_tree_topology = tree.phy;
```
>
    * #####  `tree.phy`是包含一个`newick`格式的拓扑学结构树（有或没有分支长度）的文件名（不是路径）。文件名可以是随意命名，它不必是`tree.phy`。树文件必须与比对文件（test.phy）和以.cfg为后缀的配置文件`partition_finder.cfg`在同一个文件夹中。当用户使用此选项时，用户在树文件中提供的拓扑结构将在整个分析是固定不变的。在标准分析中，使用GTR+I+G模型对整个数据集的枝长进行重新估计。如果用户不想使用这个选项，你可以从以.cfg为后缀的配置文件`partition_finder.cfg`中忽略带有`user_tree_topology`的这一行。

>
    * #####  如果用户真想要所有可能模型的一个模型列表，可以在以.cfg为后缀的配置文件里面设置以下选项：


>
2. #### *models = allx*

>
    * #####  这个模型参数在`models = all`的基础上还使用最大似然法（+X）而不是经验法（+F）对碱基或者氨基酸频率进行估计。

-----------------------------------------------------------------------------------------------

## 命令行选项

### 还有一些附加的命令用户可以从命令行将其传给[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)。这些可以用来微调用户的分析。

> 
* #### *--all-states*: 只影响k-means算法。具体来说，此选项限制k-means算法只生成包含所有可能状态的子集。

> 
* #### *--force-restart*: 这将在重新启动运行之前删除以前的所有工作（删除`analysis文件夹）。默认情况下是不会这样做的，所以[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)可以使用已经计算的结果。

> 
* #### *--min-subset-size*: 
1. #### 默认：100。
2. #### 只影响k-means算法。此选项限制k-means算法只生成至少与`min-subset-size`一样大小的子集。默认值是100个位点，所以默认情况下，k-means算法永远不会生成一个小于100个位点的子集。

> 
* #### *--no-ml-tree*: [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)默认从用户的数据中估计一个最大似然树作为分析起始树。如果用户加上`--no-ml-tree`这个命令，那么[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)将估计一个邻接法起始树（如果用户使用PhyML）或者一个最大简约法起始树（如果用户使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)）。

> 
* #### *--processors N, -p N*:
1. ##### 默认：使用所有可用的处理器。
2. ##### `N`是用户想要[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)使用处理器的数目。但是，如果用户不希望[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)使用所有的处理器，就可以控制这个选项。例如，设置`-p 5`将告诉[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)一次最多使用5个处理器。

> 
* #### *--quick, -q*: 此选项将阻止[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)在分析的过程中写入不必要的关于分区方案的总结。大多数人不需要使用这个选项，但是如果用户在进行真正的大数据分析候，特别是使用贪婪算法的时候，它可以稍微加快速度。

> 
* #### *--[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/): 这个命令告诉[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)使用[RAxML](https://sco.h-its.org/exelixis/web/software/raxml/)而不是PhyML。

> 
* #### *--rcluster-max N*: 
1. #### 默认：1000。
2. #### 如果用户想要这个选项的数值是无限大的，请将其设为`- 1`。

> 
* #### *--rcluster-percent N*: 
1. #### 默认：10。
2. #### `rcluster-max`和`rcluster-percent`一起控制松弛型聚类算法的彻底性。将其中任何一项的数值设置的更高一些往往会使搜索更彻底，但是会更慢。将他们的数值设置的更低一些往往会使搜索更快，但是会更不彻底。

> 
* #### *--save-phylofiles*: 这个选项将使[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)写入大量额外的文件到磁盘。一般来说，用户不需要使用这个命令。

> 
* #### *--weights “Wrate, Wbase, Wmodel, Walpha”*:

> 
* #### 默认：--weights `1,0,0,0`。

> 
* #### 在聚类算法中使用的权重列表。该列表允许用户为一个子集的总替换速率、碱基/氨基酸频率、模型参数和`alpha`参数（它描述了位点之间的伽玛分布率）分配不同的权重。

-----------------------------------------------------------------------------------------------

## Windows系统下[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)屏幕打印

``R
INFO     | 2018-03-27 18:28:24,769 | config     | Loading configuration at '.\partition_finder.cfg'

INFO     | 2018-03-27 18:28:24,778 | config     | Setting 'alignment' to 'test.phy'

INFO     | 2018-03-27 18:28:24,778 | config     | Setting 'branchlengths' to 'linked'

INFO     | 2018-03-27 18:28:24,779 | parser     | Setting 'models' to a user-specified list

INFO     | 2018-03-27 18:28:24,779 | parser     | The models included in this analysis are: GTR+I+G, GTR, GTR+G

INFO     | 2018-03-27 18:28:24,779 | parser     | Setting datatype to 'DNA'

INFO     | 2018-03-27 18:28:24,779 | config     | Setting 'model_selection' to 'aicc'

INFO     | 2018-03-27 18:28:24,785 | config     | Setting 'search' to 'greedy'

INFO     | 2018-03-27 18:28:24,785 | config     | ------------------------ BEGINNING NEW RUN -------------------------------

INFO     | 2018-03-27 18:28:24,846 | config     | Looking for alignment file '.\test.phy'...

INFO     | 2018-03-27 18:28:24,848 | analysis   | Beginning Analysis

INFO     | 2018-03-27 18:28:24,848 | analysis   | Removing Schemes in '.\analysis\schemes' (they will be recalculated from existing subset data)

INFO     | 2018-03-27 18:28:24,849 | config     | Checking previously run configuration data...

INFO     | 2018-03-27 18:28:24,861 | alignment  | Reading alignment file '.\test.phy'

INFO     | 2018-03-27 18:28:24,894 | analysis   | No starting tree file found.

INFO     | 2018-03-27 18:28:24,897 | phyml      | Making BioNJ tree for .\analysis\start_tree\filtered_source.phy

INFO     | 2018-03-27 18:28:25,210 | phyml      | Estimating GTR+I+G branch lengths on tree

INFO     | 2018-03-27 18:28:26,124 | phyml      | Branchlength estimation finished

INFO     | 2018-03-27 18:28:26,125 | analysis   | Starting tree with branch lengths is here: .\analysis\start_tree\filtered_source.phy_phyml_tree.txt

INFO     | 2018-03-27 18:28:26,125 | method     | Performing greedy analysis

INFO     | 2018-03-27 18:28:26,125 | progress   | PartitionFinder will have to analyse 73 subsets to complete this analysis

INFO     | 2018-03-27 18:28:26,125 | progress   | This will result in 121 schemes being created

INFO     | 2018-03-27 18:28:26,125 | method     | Analysing starting scheme (scheme start_scheme)

INFO     | 2018-03-27 18:28:26,203 | threadpool | Found 4 cpus

INFO     | 2018-03-27 18:28:26,599 | progress   | Finished subset 1/73, 1.37 percent done

INFO     | 2018-03-27 18:28:26,631 | progress   | Finished subset 2/73, 2.74 percent done

INFO     | 2018-03-27 18:28:26,776 | progress   | Finished subset 3/73, 4.11 percent done

INFO     | 2018-03-27 18:28:26,841 | progress   | Finished subset 4/73, 5.48 percent done

INFO     | 2018-03-27 18:28:27,115 | progress   | Finished subset 5/73, 6.85 percent done

INFO     | 2018-03-27 18:28:27,551 | progress   | Finished subset 6/73, 8.22 percent done

INFO     | 2018-03-27 18:28:27,867 | progress   | Finished subset 7/73, 9.59 percent done

INFO     | 2018-03-27 18:28:27,887 | progress   | Finished subset 8/73, 10.96 percent done

INFO     | 2018-03-27 18:28:27,917 | progress   | Finished subset 9/73, 12.33 percent done

INFO     | 2018-03-27 18:28:27,954 | method     | ***Greedy algorithm step 1***

INFO     | 2018-03-27 18:28:28,346 | progress   | Finished subset 10/73, 13.70 percent done

INFO     | 2018-03-27 18:28:29,096 | progress   | Finished subset 11/73, 15.07 percent done

INFO     | 2018-03-27 18:28:29,974 | progress   | Finished subset 12/73, 16.44 percent done

INFO     | 2018-03-27 18:28:30,288 | progress   | Finished subset 13/73, 17.81 percent done

INFO     | 2018-03-27 18:28:30,941 | progress   | Finished subset 14/73, 19.18 percent done

INFO     | 2018-03-27 18:28:31,548 | progress   | Finished subset 15/73, 20.55 percent done

INFO     | 2018-03-27 18:28:31,964 | progress   | Finished subset 16/73, 21.92 percent done

INFO     | 2018-03-27 18:28:32,760 | progress   | Finished subset 17/73, 23.29 percent done

INFO     | 2018-03-27 18:28:32,948 | progress   | Finished subset 18/73, 24.66 percent done

INFO     | 2018-03-27 18:28:33,280 | progress   | Finished subset 19/73, 26.03 percent done

INFO     | 2018-03-27 18:28:33,611 | progress   | Finished subset 20/73, 27.40 percent done

INFO     | 2018-03-27 18:28:33,887 | progress   | Finished subset 21/73, 28.77 percent done

INFO     | 2018-03-27 18:28:34,197 | progress   | Finished subset 22/73, 30.14 percent done

INFO     | 2018-03-27 18:28:34,400 | progress   | Finished subset 23/73, 31.51 percent done

INFO     | 2018-03-27 18:28:34,625 | progress   | Finished subset 24/73, 32.88 percent done

INFO     | 2018-03-27 18:28:35,141 | progress   | Finished subset 25/73, 34.25 percent done

INFO     | 2018-03-27 18:28:35,437 | progress   | Finished subset 26/73, 35.62 percent done

INFO     | 2018-03-27 18:28:35,684 | progress   | Finished subset 27/73, 36.99 percent done

INFO     | 2018-03-27 18:28:35,983 | progress   | Finished subset 28/73, 38.36 percent done

INFO     | 2018-03-27 18:28:36,180 | progress   | Finished subset 29/73, 39.73 percent done

INFO     | 2018-03-27 18:28:36,400 | progress   | Finished subset 30/73, 41.10 percent done

INFO     | 2018-03-27 18:28:36,736 | progress   | Finished subset 31/73, 42.47 percent done

INFO     | 2018-03-27 18:28:37,865 | progress   | Finished subset 32/73, 43.84 percent done

INFO     | 2018-03-27 18:28:38,278 | progress   | Finished subset 33/73, 45.21 percent done

INFO     | 2018-03-27 18:28:38,640 | progress   | Finished subset 34/73, 46.58 percent done

INFO     | 2018-03-27 18:28:39,214 | progress   | Finished subset 35/73, 47.95 percent done

INFO     | 2018-03-27 18:28:39,459 | progress   | Finished subset 36/73, 49.32 percent done

INFO     | 2018-03-27 18:28:39,742 | progress   | Finished subset 37/73, 50.68 percent done

INFO     | 2018-03-27 18:28:39,940 | progress   | Finished subset 38/73, 52.05 percent done

INFO     | 2018-03-27 18:28:40,118 | progress   | Finished subset 39/73, 53.42 percent done

INFO     | 2018-03-27 18:28:40,450 | progress   | Finished subset 40/73, 54.79 percent done

INFO     | 2018-03-27 18:28:41,061 | progress   | Finished subset 41/73, 56.16 percent done

INFO     | 2018-03-27 18:28:41,256 | progress   | Finished subset 42/73, 57.53 percent done

INFO     | 2018-03-27 18:28:41,654 | progress   | Finished subset 43/73, 58.90 percent done

INFO     | 2018-03-27 18:28:41,924 | progress   | Finished subset 44/73, 60.27 percent done

INFO     | 2018-03-27 18:28:42,134 | progress   | Finished subset 45/73, 61.64 percent done

INFO     | 2018-03-27 18:28:42,183 | method     | ***Greedy algorithm step 2***

INFO     | 2018-03-27 18:28:42,565 | progress   | Finished subset 46/73, 63.01 percent done

INFO     | 2018-03-27 18:28:42,842 | progress   | Finished subset 47/73, 64.38 percent done

INFO     | 2018-03-27 18:28:43,275 | progress   | Finished subset 48/73, 65.75 percent done

INFO     | 2018-03-27 18:28:43,585 | progress   | Finished subset 49/73, 67.12 percent done

INFO     | 2018-03-27 18:28:43,816 | progress   | Finished subset 50/73, 68.49 percent done

INFO     | 2018-03-27 18:28:44,132 | progress   | Finished subset 51/73, 69.86 percent done

INFO     | 2018-03-27 18:28:44,322 | progress   | Finished subset 52/73, 71.23 percent done

INFO     | 2018-03-27 18:28:44,339 | method     | ***Greedy algorithm step 3***

INFO     | 2018-03-27 18:28:44,967 | progress   | Finished subset 53/73, 72.60 percent done

INFO     | 2018-03-27 18:28:45,645 | progress   | Finished subset 54/73, 73.97 percent done

INFO     | 2018-03-27 18:28:46,799 | progress   | Finished subset 55/73, 75.34 percent done

INFO     | 2018-03-27 18:28:47,676 | progress   | Finished subset 56/73, 76.71 percent done

INFO     | 2018-03-27 18:28:48,267 | progress   | Finished subset 57/73, 78.08 percent done

INFO     | 2018-03-27 18:28:49,032 | progress   | Finished subset 58/73, 79.45 percent done

INFO     | 2018-03-27 18:28:49,144 | method     | ***Greedy algorithm step 4***

INFO     | 2018-03-27 18:28:49,759 | progress   | Finished subset 59/73, 80.82 percent done

INFO     | 2018-03-27 18:28:49,953 | progress   | Finished subset 60/73, 82.19 percent done

INFO     | 2018-03-27 18:28:50,190 | progress   | Finished subset 61/73, 83.56 percent done

INFO     | 2018-03-27 18:28:50,483 | progress   | Finished subset 62/73, 84.93 percent done

INFO     | 2018-03-27 18:28:50,801 | progress   | Finished subset 63/73, 86.30 percent done

INFO     | 2018-03-27 18:28:50,861 | method     | ***Greedy algorithm step 5***

INFO     | 2018-03-27 18:28:51,947 | progress   | Finished subset 64/73, 87.67 percent done

INFO     | 2018-03-27 18:28:52,180 | progress   | Finished subset 65/73, 89.04 percent done

INFO     | 2018-03-27 18:28:52,483 | progress   | Finished subset 66/73, 90.41 percent done

INFO     | 2018-03-27 18:28:52,688 | progress   | Finished subset 67/73, 91.78 percent done

INFO     | 2018-03-27 18:28:52,698 | method     | Greedy algorithm finished after 5 steps

INFO     | 2018-03-27 18:28:52,700 | method     | Highest scoring scheme is scheme step_4, with aicc score of 10935.186

INFO     | 2018-03-27 18:28:52,700 | reporter   | Information on best scheme is here: .\analysis\best_scheme.txt

INFO     | 2018-03-27 18:28:52,703 | main       | Total processing time: 0:00:28 (h:m:s)

INFO     | 2018-03-27 18:28:52,703 | main       | Processing complete.
```

-----------------------------------------------------------------------------------------------

## 输出文件:

### 根据以上指令，[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)运行完成后，所有的输出都在名叫`analysis`的一个文件夹里面，它与用户的比对文件在同一个文件夹里面。在`analysis`这个文件夹里面有很多的输出，但是，一般情况下，用户可能在对三个文件或者文件夹感兴趣，他们的顺序可能是：

>
1.  #### *best_schemes.txt*: 这个最佳分区方案文件有关于发现最佳分区方案的信息，以及用于找到最佳分区方案的设置。这包括该方案的详细描述，以及该方案中每个子集选择的分子进化模型。它还包含每个方案在和Nexus格式的一个描述。

>
2.  #### *subsets folder*：这个子集文件夹包含每个子集模型选择的分析结果。他们都是以`.txt`为扩展名的文件，其中用户包含在分析中的每个模型以AICc增序的方式列成表（即最好的模型在顶部）。

>
3.  #### *schemes folder*：这个方案文件夹包含分析方案的详细信息。每一个方案分别在一个以`.txt`为扩展名的独立文件里面，很像`best_scheme.txt`文件。

----------------------------------------------------------------------------------------------

## 英文网站

### 想了解更多关于 [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)有关的内容请浏览 [PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)的官方网站 <PartitionFinder2](http://www.robertlanfear.com/partitionfinder/>。

-----------------------------------------------------------------------------------------------

## 联系我们

### 如果您有任何关于[PartitionFinder2](http://www.robertlanfear.com/partitionfinder/)相关的问题，欢迎您到我们的计算进化交流群（QQ: 153258468）与我们一起探讨。

-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------

### **Jon Snow**

![](https://gss0.bdstatic.com/-4o3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=23437e3cd333c895a67e9f7de92814cd/b3b7d0a20cf431ade7884a924136acaf2fdd98c6.jpg)

### email: 376063966@qq.com

### *Reseatch Interests*:

#### I am broadly interested in bioinformatics, with particular focus on the influenza virus.

-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------

### **Brandon Stark**

![](https://gss2.bdstatic.com/9fo3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=1e963699880a19d8cb0383030bc1e5b6/503d269759ee3d6d87d2e8e34b166d224f4ade86.jpg)

### email: 2049968646@qq.com

### *Reseatch Interests*:

#### Main interests of my research include: better understanding the evolution of emerging human viral pathogens, in particular, fast evolving RNA viruses that have been sampled through time; Inferring evolutionary and population dynamic processes from molecular sequence data; Models of molecular evolution for phylogenetic reconstruction using maximum likelihood and Bayesian inference. I am also interested in empirical and theoretical approaches focusing on the use of experimental evolutionary, population genetic, phylogenetic, and computational methods.

-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------
-----------------------------------------------------------------------------------------------

