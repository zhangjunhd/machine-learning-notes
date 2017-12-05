# 百科知识图谱构建
https://mp.weixin.qq.com/s/ugriWVL6OoIHdRuvOvF1mQ

- [什么是百科知识图谱？](#什么是百科知识图谱)
- [知识获取](#知识获取)
    - [知识抽取](#知识抽取)
    - [数据清洗](#数据清洗)
        - [一、单数据源的属性融合](#一单数据源的属性融合)
        - [二、数值属性值归一化](#二数值属性值归一化)
- [知识填充](#知识填充)
    - [实体属性-值关系填充（infobox completion）](#实体属性-值关系填充infobox-completion)
    - [实体分类(Entity Typing)](#实体分类entity-typing)
        - [CN-DBpedia中的实体分类](#cn-dbpedia中的实体分类)
            - [利用语义关系特征来进行分类](#利用语义关系特征来进行分类)
            - [利用文本特征来对实体进行分类](#利用文本特征来对实体进行分类)
    - [概念表示](#概念表示)
- [知识更新](#知识更新)

## 什么是百科知识图谱？
我们认为百科知识图谱是一类专门从百科类网站中抽取知识构建而成的知识图谱。典型的百科类网站包括百科网站，例如维基百科和百度百科。

百科类网站具有三个特点：

1. 一个实体一个页面，每个页面都是围绕一个实体进行全方面介绍的。从它们网页的命名方式也能看出来，都是以网站的前缀+实体名构成的。
1. 这类网站包含了格式统一的半结构化文本。这样的半结构化文本让我们能够写统一的抽取模板来对网页进行抽取。
1. 内容质量高，百科类网站都是通过众包或者专业人员编辑的，错误相对要少一些。

百科类网站的这三个特点刚好对应了百科知识图谱的三大特点：**获取容易、抽取简单、质量高。**

百科知识图谱构建的方法从数据源来说，可以分为两类。

1. 对单百科数据源进行深度抽取，典型代表有Dbpedia,Yago和CN-DBpedia。Dbpedia和Yago是以维基百科作为数据源，而CN-Dbpedia是以百度百科作为数据源。
1. 对多百科数据源进行融合。典型代表有BabelNet，zhishi.me和XLORE。BabelNet是融合了非常多的知识图谱。Zhishi.me融合了百度百科、互动百科以及中文维基百科。XLORE融合了百度百科、互动百科以及英文维基百科。

总的来说，百科知识图谱构建分为三个部分，

1. **知识获取**，就是通过自动的方法将网页数据转化为高质量的结构化数据。
1. **知识填充**，即在现有数据的基础上加以完善，得到更多结构化的知识。
1. **知识更新**，就是保证你所获得的知识永远是最新的。

## 知识获取
知识获取主要分为两个部分，一个是**抽取知识**，另一个是对抽取来的知识**进行清洗**。

### 知识抽取
知识抽取大致能分为三类，

1. 结构化数据知识抽取，比如说从数据库里面抽取知识。
1. 半结构化知识数据抽取，比如说从表格里面抽取知识。
1. 非结构化知识数据抽取，比如说从文本里面抽取知识。

一般来说，百科知识可以分为这么几大类，首先是标题，用来表示实体名称。然后是它的同义词和多义词以及摘要关系。

![1](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26a2QrlicfMFkea7Sj1RLeHt9W1hGk5CblbNDszVtticD3ptXLRYTDNcLeg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

接下来就是infobox信息，又叫基本信息框，是一组（属性，属性值）对，是对实体的结构化总结。很多情况下，你可以不看内容，只看infobox信息就能了解这个实体了。Infobox是百科知识图谱最重要的知识来源之一，从关系数量来说，它也能提供最多知识的一类关系。

![2](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aevk81IF7libfDWP87sOWANjWIq9S5dnn2ct0zHLtpuicyias3kkmUoLCA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

还有就是相关实体关系， 如下图中蓝色字体部分，代表着一个与当前实体有联系的其他实体。另外就是标签了。

![3](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aIm5RFeArsTRxlmVy5bCtic59AJ8XxuzUwFBrA4c7qMX1KXA4QqlpFiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

知识体系分为三类。

1. ontology，不同层节点之间是严格的IsA关系，可以适用于知识的推理。
1. Taxonomy，上下位节点之间是非严格的IsA关系，可以表示概念的二义性。
1. Folksonomy，能够涵盖更多的概念，但难以进行标签管理。百度百科中的标签就属于第三类，开放分类。概念与概念之间无直接联系。

统一的自动抽取框架就是为每类关系建立一个单独的抽取器。如下图所示，这个是DBpedia的抽取框架，输入可以是Dump文件，也可以是API形式。一个网页进来后，首先通过解析器对其进行解析，然后通过不同的抽取器获得不同的知识，再存储到数据库中，供外部应用使用。

![4](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26asJS0e3jAm38FUGaWAjnINC1namBjMKebegQnxkZrG1PEbSic17YQ4rA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 数据清洗
数据清洗模块我们分为了三个部分，

1. 属性融合，将所有等价属性合并，比如英文名和英文名称。
1. 对日期属性值进行归一化，转化为统一的日期格式。
1. 再通过一个值分割算法，将所有多值的情况分开。

#### 一、单数据源的属性融合
一般来讲，分为两步，

1. 找到候选属性对。可以通过属性相似性来找候选属性对，这样英文名和英文名称就能找到了。但是有一些属性对还是找不到，比如妻子和老婆。这个就需要利用外部的同义词知识库来做了。大家可以从同义词字典、百度汉语等地方来找同义词。如果还有些找不到的话，就要通过人工的方式录入进去。
1. 删除错误的候选属性对。那么如何删除错误属性对呢？一般来说，是基于启发式规则来做的，包括等价属性不能同时出现在一个实体中。例如复旦大学这个实体中不可能同时出现“英文名”和“英文名称”这两个属性。以及等价属性的domain和range要相同等等。最后，再通过人工的方式删掉一些。

![5](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26atpUzqyAUmFAcT0Eviab7WIGlvMxP6aqRVG7icbqxnr0rbfdpw723vjrg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

#### 二、数值属性值归一化
之所以要对数值属性归一化，是因为数值属性的属性值往往是由数字+ 单位构成。所以可以分为数值抽取和单位统一两个步骤，比如通过正则表达式抽取出年、月、日等数值信息，通过转化公式来统一单位。

![6](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26ax0XAEs8apy7G84iaI0yZqJicicb68fkStqH024yPMeSoD7Sn0yjUuiaWkA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

针对对象属性值，我们首先判断这个字符串是否存在分隔符。如果不存在任何一种分隔符，那就不需要分割。如果存在，计算字符串按照出现的每一种分隔符进行分割后的得分，得分如图所示。再判断该得分是否大于未分割的得分，最终，取得分最大的方案作为最终的分割方案。

![7](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aIB9yCMgu7ouzPfNPPZcegXeBApX0pkPfCiclV2U7Fk6wRupv64Rwj7Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

三类数据清洗的例子如图所示。根据属性融合方法，我们将“英文名”改成了“英文名称”。根据数值属性归一化方法，我们将“1905年（乙巳年）9月14日”统一成“1905年09月14日”。根据对象属性值分割方法，我们将复旦大学的知名校友给分割开来。

![8](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aoYHKVgZv0hzV8aRjfxwhyF3IrjtDJCnx6aqfL47SNhNDwBGG1LGfGg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

通过知识抽取和数据清洗两个步骤，我们就能将网页数据转换为高质量的结构化数据了。

## 知识填充
所有百科知识图谱都会面临的一个不可避免的问题就是知识缺失。因为我们的数据源是百科类网站，而它们很难做到完整。总体来说，包括三类知识缺失：

1. 单个实体的知识缺失
1. 所有实体都缺少的分类知识
1. 概念缺失表示知识

第一类是**单个实体的知识缺失**。比如缺失infobox，标签，摘要等信息。下图左边是周杰伦的信息，右边的刘德华的信息，刘德华的infobox信息就要比周杰伦多5个。这两个实体还都是最popular的实体，都有这么明显的差距。其他实体就更不用说了。

![9](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aK5xJfwjHOs9CTP770x8mMXIIicU0ZBvgxYnS0RSbMvBR4E9vp0lLjnA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

第二个是实体的分类信息缺失。我们知道，百度百科中的标签属于folksonomy开放标签，无法进行推理，因此，我们需要把实体分类到能进行推理的ontology上去。

![10](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aicyetAOk3wRLRgTdxBb0rlpmKPvSt0Cky2cnTXzdsCGPsLpdlhEF6Sw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

第三个是**概念表示知识缺失**。相对于实体而言，我们对概念的认识还不够，目前只知道一个概念的父概念是哪个，子概念是哪个，以及它包含哪些实体，但我们无法直接从知识图谱中获知这个概念是怎么形成的？

![11](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26anbWnQVttobEYgvKOiacTvJFVNBaOQavm3HJM4QPuLt7Mwr2jOvONpvA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

针对这三类知识的缺失，我们将其归类到三个具体的研究任务上。

- 针对单个实体的知识缺失问题，我们把它看做是实体的属性-值，也叫infobox关系填充问题来解决。
- 针对实体分类知识缺失问题，我们就把它当做实体分类问题来解决。
- 针对概念表示知识缺失问题，我们把它当做概念的符号表示问题来解决。

### 实体属性-值关系填充（infobox completion）
Infobox completion方法很多，主要包括：

1. 利用其它知识图谱进行填充。
    - 主要使用的是知识图谱融合的思想，不同知识图谱的实体之间可以通过等价关系“SameAS”链接在一起，不同知识图谱由于构建方式不同，知识也不尽相同，因此，可利用其它知识图谱来对自身知识图谱Infobox进行填充。此类方法需要解决的难点包括：实体匹配、属性匹配、属性值融合等。如果要做跨语言的融合，还需要解决跨语言的问题。
1. 利用百科网站实体标签进行填充。
    - 例如已知“刘德华”的一个标签信息为“香港男演员”，那么就可以推出（刘德华，出生地，香港），（刘德华，性别，男）和（刘德华，职业，演员）这三组infobox。目前主流的方法倾向于使用人工建立规则的方法来进行抽取，主要包括YAGO和Catriple。
    - YAGO是使用了一种基于正则表达式的方法来从标签信息中抽取关系。例如，通过正则表达式([0-9]{3,4}) births 来抽取BornOnDate关系，或者通过Mountains|Rivers in (.*)来获取LocatedIn关系。这种方法的优点是准确率高，每类关系的准确率都超过90%。但缺点是代价太大了，需要为每个关系定制一套正则表达式。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aBxMmkibqdmYpoROr856xtmiaD432dLIiaDsY8G5AxLUHDRkicO7tYhskiaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
    - Catriple利用了Wikipedia的上下位概念来获取实体的infobox信息。如下图所示，根据（Songs by artist, The Beatles songs）能推断出任何属于概念TheBeatles songs的实体都包含（artist,The Beatles）这一对属性-值对。根据（Rock songs ，British rock songs ）能推断出任何属于概念Britishrock songs的实体都包含（Country,British）这一对属性-值对。因此，实体Hey Jude能得到两组infobox，分别是（Hey Jude, artist, The Beatles）和（ HeyJude, Country, British）。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26azibcX0jITHpJqUw8SYTuWRcKfNEZSMsDKjJLvx5VyHgFpWotdKOVj3w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
    - 具体方法为构建了四种有效的上下位概念模式：
        - 第一种为by-prep的上下位概念组合。其中上位概念需要满足“by +属性”这一条件，例如songs by theme。下位概念需要满足“是介词从句且包含属性值”这一条件，例如Songs about divorce。如果满足这两个条件，那么就可以从上位概念中抽取属性，从下位概念中抽取属性值。这一属性-值对将传递给下位概念的所有实体。
        - 第二种为by-noun的上下位概念。和第一种的区别就是下位概念需要满足“是名词从句且包含属性值”这一条件。抽取方式和第一种相似。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26a7bcp17mF0U9P8fQxN1fuDo9YFeJ07UicibaHibjicbiaBm0eEibuZypWwpZA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
        - 第三种为非by-prep形式的*-prep上下位概念组合。其中上位概念不包含属性，如“songs”。下位概念满足 “是介词从句且包含属性值”这一条件，如“songs from films”。这种情况下，可以从下位概念中抽取属性值，再根据投票的方法确认属性值对应的属性。投票的方法包括全局投票和局部投票两种。全局投票是统计整个数据集中，当属性值为某一个具体值（如films）的时候，最常出现的属性是哪个。局部投票是统计在某个下位概念包含的实体infobox数据集中，当属性值为某一个具体值（如films）的时候，最常出现的属性是哪个。
        - 第四种模式和第三种类似，只是把介词从句改成了名词从句。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26a4YVplWQXB1Fd38AezO7moMA7TicvfMcpiaQH7hMFp4CN5uLDtHX9lIzQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
1. 利用百科网站文本进行填充。
    - 对实体正文进行属性值抽取和之前的网页抽取器类似，可以看作是为每个属性分别构建一个抽取器，例如“刘德华（AndyLau），1961年9月27日出生于中国香港。”这个句子，通过“英文名称”抽取器可以得到AndyLau这个属性值，出生日期抽取器可以得到1961年9月27日，而出生地抽取器可以得到中国香港这个属性值。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aZzof8BMkQrMwWViaz7cqolhzCqmqNdtMaOleWRKWmR6Kt8MQGJhccUg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
    - 序列数据标记问题
        - 文本属性值抽取被认为是一个序列数据标记问题。它是把整个句子当做是一个序列数据，每个属性值抽取器的抽取过程当做是序列数据的标记过程。1表示是属性的值，0表示不是属性的值。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26ajyRywhGrKC1YDSbFSTHJRicPI7fabX8XpsUiahUkXHeRAWzzYntQB8Rw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
        - 条件随机场
            - 它同时考虑了每个单词的特征以及句子的顺序性，但缺点是需要专家人为设计一套特征，如下图右边所示，特征选择的好坏直接影响分类的结果。同时，也不具有通用性，英文的需要设计一套特征，换成中文的话，又要设计另外一套特征了。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aPicxoNwRe91Xrz9sF8HGjhncQsnSOneoenJ9ZtJrSiaccQSrfCBl0NxQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
        - 深度学习
            - 典型方法为基于LSTM的方法，输入为一个句子，输出为句子中每个单词的标记，其中，句子中的每个单词用两类向量来表示，一个是word embedding，另一个是character embedding。同时，整个句子再通过一个双向的lstm模型来自动抽取出特征，最后对每个单词进行分类，0表示不是属性值，1表示是属性值。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26a4zTQMZicJU8uiccIicsH3e1IxWkCuosYDuDAGBFk0lcpjmrsjYadYiaE8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
            - 目前更好的深度学习方法，是使用lstm自动抽取特征，再通过条件随机场来进行序列数据标记，也就是在lstm的输出层中在加一个crf层。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aKqIpGRbqTFZq3YdicMDTriaIF2Jv1zbIYky3me8ib27bvh7gJ6rCruUpQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 实体分类(Entity Typing)
实体分类是指将知识图谱中的实体分类到一组预定义的概念集合中。这与传统的命名实体识别问题有一些区别，传统命名实体识别问题研究的是一个句子中的实体指称项（也称mention），考虑的也仅仅是上下文的文本特征，实体指称项的分类结果可能只是现实世界中实体分类结果的一部分。例如从句子“刘德华出生于1961年9月”中，只能推断出“刘德华”属于“人物”这个概念，其他的“演员”、“歌手”等概念都无法得到。知识图谱中的实体分类研究的对象是实体本身，考虑的是实体在知识图谱中的全部知识，包括文本特征和语义关系特征等，与真实世界中实体的分类结果理论上一致。

1. 人工方法
    - 人工方法的典型代表就是DBpedia，它通过众包的方式构建了一个本体ontology，然后又通过众包的方式建立起了Wikipedia infobox模板名称和ontology中概念的等价关系。这样的话，一个实体就能根据它的模板名称来判断它属于哪些概念了。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26amhxOia6yWVNk14quvR2JfhibN24gIxQ5nSJuVYOLFpb82a7IrbZb5GPw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
1. 自动规则
    - 基于等价概念、等价实体以及继承关系来进行推理。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aibd0I5EEJSwwYaXyWWLPZurfC3B9YDicViblibCnr6R2cFib65uziaXlw70w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
1. 有监督实体分类
    - 如果你有充足的训练集的话，那么不管你用哪个模型，比如支持向量机、逻辑回归、决策树等等，都能取得非常好的效果。
1. 弱监督/远程监督
    - 更多情况下，我们遇到的问题是没有训练集，例如，我们要将百度百科的实体分类到英文DBpedia的本体上去。所以我们可以通过一种被称为弱监督或者远程监督的方法来构建训练集，即首先建立起中文百度百科实体和英文DBpedia实体的等价关系，再把英文实体的分类关系传递给等价的中文实体，通过这种方式就能为一些中文百度百科实体获得标记数据了。![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26a211tXL5znQcouYqiaHL0fMxiaWaLIoHMaQIwvDicwlBmAVCfetTVZnbqQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
    - 远程监督方法构建的训练集会存在更多的噪声问题。主要是由三方面原因构成：
        1. 目标知识图谱本身存在噪声，这点无法避免；
        1. 实体链接错误，这也很难避免，因为实体链接本身就是一个很难的问题，准确率不能达到100%；
        1. 实体特征缺失，导致很多正确的分类错误变成了错误标记。举个极端点的例子就是，一个实体，它的所有特征都缺失了，但是根据等价关系知道它有很多分类结果，这就意味着，如果你不包含任何特征，你就能属于这些分类。这显然是错误的。

#### CN-DBpedia中的实体分类
因为中文并没有一个很好的本体ontology，但英文有一些很好的本体，比如DBpedia,所以我们将百度百科中的实体分类到英文DBpedia的本体中去。具体用到的特征包括语义关系特征(属性，属性-值以及标签等)以及文本特征(主要来自于摘要以及正文信息)。处理这两种特征的方法不同，我们针对每类特征分别提出了一种分类方法。

##### 利用语义关系特征来进行分类
![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26at37zNC9BibYrm6EIxD9qLDrCWrvg1jpUL8fFueSYfrr8LibbheoZHKibw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

一个实体通过一组语义关系特征来表示,根据这一组语义关系特征来判断实体的分类。核心点之前已经说过了，就是构建一个高质量的训练集，也就是我们框架图的中间层部分。

我们首先通过远程监督的方式构建了一个训练集，包括使用了跨语言实体链接技术构建了中文实体和英文实体的等价关系，然后通过跨语言分类传递方法，将英文实体的分类信息传递给等价的中文实体,接着，通过之前提到的训练集过滤算法，得到一个去噪后的训练集。当然，为了使这个结果更好，我们进行了额外的两步优化:第一步，针对DBpedia实体不完整的情况，对DBpedia的分类关系进行了填充，利用到了DBpedia实体的标签信息；第二步针对Dbpedia本体的树状结构，提出了一个层次化的分类算法。

##### 利用文本特征来对实体进行分类
![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aFXSlCJ4ib7bCCMD2zXibR3M7nsKIWc9e7q229rz2h3NOajXUPg0xFpUg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

从文本中得到实体的分类结果和利用语义关系特征得到分类结果是不一样的。因为，实体在句子中可能只能表现出实体的部分分类结果，你不能只通过实体在一个句子中的信息来判断它的分类结果，这样是不全面的。比如第一句“刘德华出生于1961年9月”，这里mention刘德华只能推断出它是一个人物。类似的，第二个句子“刘德华出演《长城》”，只能推断出刘德华不仅是一个人物，还是一个演员，但歌手这个身份是无法推断出来的。因此，我们的基本思路是分别对每个mention进行分类，然后合并所有的分类结果。

第一个难点在于人工标记代价大。

针对这个难点我们都采用远程监督的方法，把知识图谱中实体的分类结果赋给句子中的mention，这里需要注意的是，远程监督方法能够实现的原因是我们通过上一步基于语义关系特征已经得到了大量实体的分类。没有上一步的话，这一步也做不出来。同样的，基于远程监督的方法会产生很多的噪声，因此我们又需要通过多分类器过滤的方法对训练集进行过滤。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aOtwiaA1jl9VVrs9oSpwwgWrKE2tr2glzdgYtI5UIP8lclYiafibBQ2WzA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

第二个难点也是之前提过的，人工设计代价大。

针对这个难点，我们采用了神经网络模型。这里需要注意的是，我们是要对mention进行typing，而不是对句子进行typing，这就需要在神经网络模型里面明确的告知mention在哪里。因此，我们将句子拆分为了三部分，mention部分，和它的左右context部分，这三部分都是固定长度的，缺了就补0，多了就删掉。最后，将这三个模型分别通过lstm来抽取特征再合并起来。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aD61Z8xJwUyNGrcAD5mwNpn7t5Kg9Dn2HhxQl29eXLqfmH8oHXImdUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

第三个难点在于简单的融合效果不好。

例如表中的前三种融合策略，分别是取并集、取交集、多数投票，效果都不尽如人意。 所以我们提出了一个整数线性规划的问题，在合并的同时，考虑概念与概念之间的关系才进行约束，包括互斥概念不能同时存在，比如人物和建筑，以及概念层次化约束，父概念不为1的情况下，所有子概念均不能为1。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26abAlxfibWWq0xoVt8qhqPOOPJ5yZ5696MIGdiaQ9kM9O2XqIoqZSdxuXQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 概念表示
概念是怎么形成的这一问题，很早就有人研究了。最早可以追溯到亚里士多德时代，他认为，概念是由一组包含相同特征集合的实体构成的。这组特征后来在Allan Collins的论文中被称为概念的固有特征集合，英文叫Defining Features.

概念的固有特征集合满足两个性质。

1. 如果一个实体包括某个概念的固有特征集合，那么它一定属于这个概念。这个性质可以帮助我们做实体分类。
1. 如果一个实体属于某个概念，那么它也一定包含这个概念的固有特征集合。这个性质可以用来帮助我们做infobox completion.

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26adicvPeYlY2REFiaVNu4UqHCGERLmpHib8rWkh1wMDQoCTIwgiauM7icxZMw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

在这之前，我们需要回答两个问题。

**第一个是哪些概念适合用知识图谱中的关系来表示？** 概念包括粗粒度概念和细粒度概念。粗粒度概念有“花”，“鸟”，“鱼”，“虫”等，细粒度概念有“周杰伦歌曲”，“香港男演员”等。

很明显我们能发现，知识图谱只能对细粒度概念进行表示，粗粒度概念的特征很难出现在知识图谱中。比如说鸟的固有特征集合包括有羽毛、能飞、有翅膀等。这些在百科知识图谱中都很难出现。

**第二个问题是知识图谱中的哪些知识能够用来表示概念？** 我们根据Allan Collins人工定义的概念的固有特征集合可以发现，可以通过实体的infobox和type信息来表示。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aNPgJZs3sCHx49bS02sl0daXibibUExhs5eDYzJTQlaes5cRCdZsv5yrQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

由于知识图谱是不完整的，我们无法一次性获得所有概念的固有特征集合。因此，我们提出了一种迭代的方法：先基于统计方法来得到一些概念的固有特征集合作为种子结点，根据这些种子结点生成规则，再利用这些规则来扩展得到更多概念的固有特征集合，有了很多概念的固有特征集合之后，反过来对知识图谱进行填充，新增的知识又能发现更多概念的固有特征集合。依此迭代，直至没有新的知识产生为止。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26asIgJQyyvX8pZ5a8ar7ibnMh0icV0hUOqtbCabZS8L1sgAEZw24KV3ujA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

首先，我们介绍**基于统计的方法**。每个概念都会包含许多实体，而这些实体的特征集合中肯定都包含了这个概念的固有特征集合，然而，一个实体不仅包含概念的固有特征集合，也包含更多的非固有特征集合的特征。比如实体《Inception》，也就是《盗梦空间》，是概念Films directed by Christopher Nolan的一个实例，它包含了很多特征，其中只有红色标记的才是概念的固有特征集合。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aic6yj5rCM6kCnVUzABaFkQhF4oicX4o4XQVDf4icibjBzYBImNXmibFiaeFA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

一个naïve的方法就是穷举概念的所有可能的特征集合，并计算每个特征集合的得分。打分函数如图所示，主要是两个概率的乘积。第一个概率表示，如果一个实体属于某个概念，那么它将包含这个概念的固有特征集合的概率。第二个概念表示，如果一个实体包含了某个概念的固有特征集合，那么这个实体属于这个概念的概率。当两边概率都为1时，表示特征集合为这个概念的固有特征集合。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aRxQT8CF2WEGdjhq3OKz2c0NutyNO1fYAB3BfKjRbzF9DM2tK5C3X9w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

这种方法是有缺陷的:

1. 由于知识图谱的不完整性，很多概念的固有特征集合的得分都小于1，这些就会漏掉很多概念的固有特征集合。
1. 穷举所有特征集合的代价太大了。

针对这两个缺陷，我们分别提出了两种解决方案。

1. 针对缺陷一，我们降低了打分函数的取值。不变要求它的值必须为1，只要是得分最大的那个特征集合就好了。当然，并不是每个概念都能按照这种方法找到它的固有特征集合的，所以我们对得分设置了一个阈值，必须大于这个阈值才行。
1. 针对缺陷二，我们利用了频繁项集发掘的方法来进行剪枝。只计算哪些频繁的特征集合。我们在论文中证明了，只要频繁项集的支持度阈值和打分函数的阈值相同，就能保证非频繁特征集合一定不是概念的固有特征集合。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26ajpW08wfh5hnXohGEyW2MhlSgNxTCtSB9ibcLvKexKjCLVsnlhVk7kSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

由于知识图谱的不完整性，很多概念的固有特征集合可能是不频繁的，导致其无法通过基于统计的方法得到结果，因此，我们又提出了**基于规则的方法**。

每个生成的规则我们都需要进行评估，评估指标包括支持度和置信度。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aUkCNwYfNG8qXQA1YA7V6a3YTRr26ndeSpM7l28SoBewg0UWB8ZSMbw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

有了这些好的规则，我们就可以扩展得到更多概念的固有特征集合了。

有了一些固有特征集合，我们就能对知识图谱进行填充了。例如，根据固有特征集合的第二个性质，我们能做实体的infobox填充和type填充。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aY0d9XVxfZFIX6pUzhefkrZUgrompwn2icUW3icQ5uhEspQEv2ib9RfePQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

根据固有特征集合的第一个性质，我们能做实体的细粒度概念填充。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26ayfxnK6jHe3vYkqbMgIu5xERXgOsdmxTdutK0yq11Jfv2POS0PokPxw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

## 知识更新
传统的更新方法有两种：

1. 基于更新日志的更新，这也是DBpedia的更新策略，因为wikipedia会把每天更新的内容以文件的形式发布出来的，那么知识图谱只要更新这些文件就好了。这是最好的更新方式，但是百度百科没有，你只能去猜到底哪些实体更新过了。
1. 周期性更新，即每隔一段时间，比如说半年重新爬取一遍数据，并进行解析。且不说这种方法代价巨大，时效性同样不能保证。有些知识可能在刚爬取完就更新了，那么它也只能等待下一个更新周期了再更新了。

另外两种是我们提出来的，**基于语义搜索引擎的更新。**

1. 之前提到的我们构建了一个语义搜索平台，用户可以通过输入关键字来得到结构化的结果。如果用户觉得这条知识过时了，百度百科上有新的，那么点击更新按钮，我们就会从百度百科上去拿新的内容过来。
1. 基于搜索日志的新词发现，如果用户搜索一个词时，没有在我们的知识图谱中找到，那么就认为它是一个新词，我们也会去百度百科上看看有没有这个词条。

我们又提出了一个主动的自动更新方法。基本思路是监控互联网上的热词。热词分为两种情况，一种是新词，另一种是旧词，但信息发生了变化，这刚好满足我们的更新旧词以及增加新词的更新策略，然后，更新热词以及与之相关的词条。

之所以要做实体扩展是因为牵一发而动全身。一个关系发生了变化，会牵扯到很多实体，它们的关系都会发生变化。比如王宝强离婚，不仅王宝强的婚姻关系变了，马蓉的婚姻关系同样也变了。

整个更新框架就是这样，先发现热词作为种子结点，更新这个热词，再从热词的页面中，扩展得到更新的实体。重点是，到了这一步并不是马上更新这些实体，因为一扩展实体的量就上来了，我们根本更新不了这么多的实体，并且并不是每个实体都发生了变化，所以我们对每个扩展实体设置了一个**更新优先级**，然后根据这个优先级来进行更新。

优先级设置的原则是这样的，如果是一个新词，那么优先级设置为最高，如果是一个旧词，估计其上一次更新结束到当前时间内可能更新的次数，将这个次数作为优先级的指标。

指标为更新频率乘以更新间隔。一个实体期望的更新频率通过随机森林回归预测器得到。

最后，总结下来。针对单数据源百科知识图谱构建的方法就是分为这样三个步骤，先通过知识获取，将网页数据转化为高质量的结构化数据，再通过知识填充，得到更多结构化的知识，最后通过知识更新，保证你的知识永远是最新的。

![](https://mmbiz.qpic.cn/mmbiz_png/GylibWq3RaRyKG1etmTeBZDLyqaoJy26aLhpSrAE8YSiaEVbZY08yeSiclHOoBDaOgf2CJoia0qpTSzYdqedtUUpjA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)
