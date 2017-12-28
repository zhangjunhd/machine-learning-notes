# 自然语言处理

## 词袋模型
1. [Python自然语言处理][200]
    - [NLTK入门][201]
        - 搜索文本
        - 计数词汇
        - 将文本当作词链表
        - 简单的统计
            - 频率分布
            - 词语搭配和双连词
            - 词长分布
    - [获取文本语料库][202]
        - 古腾堡语料库
        - 网络和聊天文本
        - 布朗语料库
        - 路透社语料库
        - 就职演说语料库
        - 其他语言的语料库
        - 文本语料库的结构
        - 载入你自己的语料库
            - txt文件载入
            - 载入宾州树库的副本
    - [条件频率分布][203]
        - 条件与事件
        - 按文体计数词汇
        - 绘制分布图和分布表
        - 使用Bigrams生成随机文本
    - [词典资源][204]
        - 词典资源
            - 词典(`Lexical`)或词典资源是一个词和/或短语及其相关信息的集合，例如：词性(`part-of-speech`)和词意(`sense`)定义等相关信息。
            - 词项(`lexical entry`)包括词目(`headword`)（也叫词条(`lemma`)）及其他附加信息，例如：词性和词意定义。
            - 词汇列表语料库
                - 停用词(`stopwords`)语料库
                - 名字语料库
            - 发音的词典
                - 美国英语的CMU发音词典
                    - 音素(`phones`)
            - 比较词表
                - Swadesh wordlists
            - 词汇工具：Toolbox
        - WordNet
            - 意义与同义词(`synonyms`)
            - WordNet的层次结构
                - `unique beginners`
                - `hyponyms`
                - `hypernyms`
            - 更多的词汇关系
                - `Hypernyms` 和 `hyponyms` 被称为词汇关系(`lexical relations`)。
                - 蕴含(`entails`)
                - 反义词(`antonymy`)
            - 语义相似度
                - 如果两个同义词集共用一个特定的`hypernym`——在hypernym层次结构中处于较低层——它们一定有密切的联系。
                - 可以通过查找每个同义词集的深度来量化这个普遍性概念。
                - path_similarity基于hypernym层次结构概念中相互关联的最短路径下，在0~1范围内的相似度（两者之间没有路径返回-1）。同义词集与自身比较将返回1。
    - [处理原始文本][205]
        - 从网络和硬盘访问文本
            - 电子书
            - 处理HTML
            - 处理RSS订阅
            - 读取本地文件
            - NLP流程
        - 使用正则表达式
            - 提取字符块
            - 查找词干(`stems`)
            - 搜索已分词文本
        - 规范化(`Normalizing`)文本
            - 词干提取器(`Stemmers`)
            - 词形归并(`Lemmatization`)
            - 分割(`Segmentation`)
                - `Tokenization` is an instance of a more general problem of `segmentation`.
                - 将分词转换成搜索问题
                    - 评估函数
                    - 模拟退火
    - [分类和标注词汇][206]
        - 使用词性标注器(`part-of-speech tagger` 或 `POS tagger`)
        - 标注语料库
            - 读取已标注的语料库
            - 简化的词性标记集
            - 名词
            - 动词
            - 未简化的标记
            - 探索已标注的语料库
        - 自动标注
            - 默认标注器
            - 正则表达式标注器
            - 查询标注器
        - N-gram标注
            - 一元标注器(Unigram Tagging)
            - 分离训练和测试数据
            - 一般的N-gram的标注
            - 组合标注器
            - 标注生词
            - 存储标注器
            - 性能限制
            - 跨句子边界标注
        - 基于转换的标注(Transformation-Based Tagging)
        - 如何确定一个词的分类
            - 形态学(Morphological)线索
            - 句法(Syntactic)线索
            - 语义(Semantic)线索
            - 新词
            - 词性标注集中的形态学(Morphology in Part-of-Speech Tagsets)
    - [分类文本性别鉴定][207]
        - naive Bayes classifier
        - 似然比(`likelihood ratios`)
    - [分类文本电影评论正负面评价][208]
        - NaiveBayesClassifier
    - [分类文本词性标注][209]
        - 词性标注(`Part-of-Speech Tagging`)
        - 基于后缀
        - 探索上下文语境
        - 序列分类
            - 为了获取相关分类任务之间的依赖关系，我们可以使用`joint classifier`模型，为一些相关的输入选择适当的标签。
            - 在词性标注的例子中，可以使用各种不同的`sequence classifier`模型为给定的句子中的所有词选择词性标签。
            - 一种称为`consecutive classification`或`greedy sequence classification`的序列分类器策略，为第一个输入找到最有可能的类标签，然后在此基础上找到下一个输入的最佳的标签。这个过程可以不断重复直到所有的输入都被贴上标签。
        - 其他序列分类方法
            - 这种方法的缺点是一旦做出决定便无法更改。例如：如果决定将一个词标注为名词，但后来发现应该是动词，那也没有办法修复我们的错误了。解决这个问题的方法是采取转型策略(`transformational strategy`)。转型联合分类(`Transformational joint classifiers`)的工作原理是为输入的标签创建一个初始值，然后反复提炼该值，尝试修复相关输入之间的不一致。`Brill标注器`，是使用这种策略的。
            - 隐马尔可夫模型(`Hidden Markov Models`)采取了这种方法。
                - 最大熵隐马尔可夫模型(`Maximum Entropy Markov Models`)和线性链条件随机场模型(`Linear-Chain Conditional Random Field Models`)
    - [分类文本句子分割][210]
    - [识别对话行为类型][211]
    - [识别文字蕴含][212]
    - [决策树][213]
        - 熵(Entropy)和信息增益(Information Gain)
    - [朴素贝叶斯][214]
    - [最大熵][215]
    - [从文本提取信息][216]
        - 信息提取
            - 信息提取结构
        - 分块(Chunking)
            - 名词短语分块
            - 用正则表达式分块
            - 探索文本语料库
            - 缝隙(Chinking)
            - 分块的表示：标记与树状图
        - 开发和评估分块器
            - 读取IOB格式与CoNLL2000分块语料库
            - 简单评估和基准
            - 训练基于分类器的分块器
        - 语言结构中的递归
            - 用级联分块器构建嵌套结构
            - 树状图
        - 命名实体识别(Named Entity Recognition)
        - 关系抽取(Relation Extraction)
    - [分析句子结构][217]
        - 文法(Syntax)的用途
        - 上下文无关文法
        - 上下文无关文法分析(Parsing with Context-Free Grammar)
            - 递归下降解析器(Recursive Descent Parsing)
            - 移进-规约分析(Shift-Reduce Parsing)
        - 依存关系(Dependencies)和依存文法(Dependency Grammar)
1. [结巴中文分词][300]
    - 分词
    - 添加自定义词典
    - 关键词提取
        - 基于 TF-IDF 算法的关键词抽取
        - 基于 TextRank 算法的关键词抽取
    - 词性标注
    - Tokenize
1. [文本聚类][301]
    - 词袋模型与文本向量化
    - 相似度：向量间距离
    - 聚类KMeans
1. [主题模型][302]
    - 构建主题模型
    - 在主题空间比较相似度

## 深度学习

### Neural Probabilistic Language Model

1. [A Neural Probabilistic Language Model][1],Yoshua Bengio,Réjean Ducharme,Pascal Vincent,Christian Jauvin,[Journal of Machine Learning Research 3 (2003)][101]
1. Semantic Hashing
1. [Natural Language Processing (almost) from Scratch][15],Ronan Collobert,Jason Weston,[2 Mar 2011][115]

### Continuous Space Representations

1. [Recurrent neural network based language model][7],Tomas Mikolov,[INTERSPEECH 2010][102]
1. [Linguistic Regularities in Continuous Space Word Representations][2],Tomas Mikolov,Wen-tau Yih,Geoffrey Zweig,[NAACL-HLT 2013][103] 
1. [Efficient Estimation of Word Representations in Vector Space][3],Tomas Mikolov,Kai Chen,Greg Corrado,Jeffrey Dean,[7 Sep 2013][104]
1. [Distributed Representations of Words and Phrases and their Compositionality][4],Tomas Mikolov,Ilya Sutskever,Kai Chen,Greg Corrado,Jeffrey Dean,[16 OCT 2013][105]
1. [Distributed Representations of Sentences and Documents][5],Quoc Le,Tomas Mikolov,[22 May 2014][106]
1. [Document Embedding with Paragraph Vectors][6],Andrew M. Dai,Christopher Olah,Quoc V. Le,[29 Jul 2015][107]
1. [Skip-Thought Vectors][8],Ryan Kiros,Yukun Zhu,Ruslan Salakhutdinov,Richard S. Zemel,Antonio Torralba, Raquel Urtasun, Sanja Fidler,[2015][108]

### Deep Structured Semantic Models(DSSM)

1. [Learning Deep Structured Semantic Models for Web Search using Clickthrough Data][9],Po-Sen Huang, Xiaodong He, Jianfeng Gao, Li Deng,Alex Acero, Larry Heck,[CIKM’13, Oct. 27 2013][109]
1. [Learning Semantic Representations Using Convolutional Neural Networks for Web Search][10],Yelong Shen,Xiaodong He,Jianfeng Gao,Li Deng,Grégoire Mesnil,[WWW’14 Companion, April 7–11, 2014][110]
1. [Semantic Parsing for Single-Relation Question Answering][11],Wen-tau Yih,Xiaodong He,Christopher Meek,[2014][111]
1. [Modeling Interestingness with Deep Neural Networks][12],Jianfeng Gao, Patrick Pantel, Michael Gamon, Xiaodong He, Li Deng,[2014][112]
1. [A Latent Semantic Model with Convolutional-Pooling Structure for Information Retrieval][13],Yelong Shen,Xiaodong He,Jianfeng Gao,Li Deng,Grégoire Mesnil,[CIKM’14, November 03 2014][113]
1. [Semantic Parsing via Staged Query Graph Generation: Question Answering with Knowledge Base][14],Wen-tau Yih,Ming-Wei Chang,Xiaodong He,Jianfeng Gao,[2015][114]

### Translation

1. [Learning Continuous Phrase Representations for Translation Modeling][16],Jianfeng Gao,Xiaodong He,Wen-tau Yih,Li Deng,[2013][116]
1. [Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation][17],Kyunghyun Cho,Yoshua Bengio,
[3 Sep 2014][117]
1. [Sequence to Sequence Learning with Neural Networks][18],Ilya Sutskever,Oriol Vinyals,Quoc V. Le,[14 Dec 2014][118]
1. [MULTI-TASK SEQUENCE TO SEQUENCE LEARNING][21],Minh-Thang Luong, Quoc V. Le, Ilya Sutskever, Oriol Vinyals, Lukasz Kaiser,[ICLR 2016][121]
1. [Google's Neural Machine Translation System: Bridging the Gap between Human and Machine Translation][20],Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V. Le, Jeffrey Dean,[8 Oct 2016][120]

### Natural Language Generation(NLG)

[1]: A-Neural-Probabilistic-Language-Model.ipynb
[2]: Linguistic-Regularities-in-Continuous-Space-Word-Representations.ipynb
[3]: Efficient-Estimation-of-Word-Representations-in-Vector-Space.ipynb
[4]: Distributed-Representations-of-Words-and-Phrases-and-their-Compositionality.ipynb
[5]: Distributed-Representations-of-Sentences-and-Documents.ipynb
[6]: Document-Embedding-with-Paragraph-Vectors.ipynb
[7]: Recurrent-neural-network-based-language-model.ipynb
[8]: Skip-Thought-Vectors.ipynb
[9]: Learning-Deep-Structured-Semantic-Models-for-Web-Search-using-Clickthrough-Data.ipynb
[10]:Learning-Semantic-Representations-Using-Convolutional-Neural-Networks-for-Web-Search.ipynb
[11]:Semantic-Parsing-for-Single-Relation-Question-Answering.ipynb
[12]:Modeling-Interestingness-with-Deep-Neural-Networks.ipynb
[13]:A-Latent-Semantic-Model-with-Convolutional-Pooling-Structure-for-Information-Retrieval.ipynb
[14]:Semantic-Parsing-via-Staged-Query-Graph-Generation-Question-Answering-with-Knowledge-Base.ipynb
[15]:Natural-Language-Processing-almost-from-Scratch.ipynb
[16]:Learning-Continuous-Phrase-Representations-for-Translation-Modeling.ipynb
[17]:Learning-Phrase-Representations-using-RNN-Encoder–Decoder-for-Statistical-Machine-Translation.ipynb
[18]:Sequence-to-Sequence-Learning-with-Neural-Networks.ipynb

[20]:Googles-Neural-Machine-Translation-System-Bridging-the-Gap-between-Human-and-Machine-Translation.ipynb
[21]:MULTI-TASK-SEQUENCE-TO-SEQUENCE-LEARNING.ipynb

[101]:http://www.jmlr.org/papers/volume3/bengio03a/bengio03a.pdf
[102]:http://www.fit.vutbr.cz/research/groups/speech/publi/2010/mikolov_interspeech2010_IS100722.pdf
[103]:http://www.aclweb.org/anthology/N13-1090
[104]:https://arxiv.org/pdf/1301.3781.pdf
[105]:https://arxiv.org/pdf/1310.4546.pdf
[106]:https://arxiv.org/pdf/1405.4053.pdf
[107]:https://arxiv.org/pdf/1507.07998.pdf
[108]:http://papers.nips.cc/paper/5950-skip-thought-vectors.pdf
[109]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/cikm2013_DSSM_fullversion.pdf
[110]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/www2014_cdssm_p07.pdf
[111]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/SingleRelationQA-YihHeMeek-ACL14.pdf
[112]:https://www.microsoft.com/en-us/research/wp-content/uploads/2014/10/604_Paper.pdf
[113]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/cikm2014_cdssm_final.pdf
[114]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ACL15-STAGG.pdf
[115]:https://arxiv.org/pdf/1103.0398.pdf
[116]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/nn4smt.acl_.v9.pdf
[117]:https://arxiv.org/pdf/1406.1078.pdf
[118]:https://arxiv.org/pdf/1409.3215.pdf

[120]:https://arxiv.org/pdf/1609.08144.pdf
[121]:https://nlp.stanford.edu/pubs/luong2016iclr_multi.pdf

[200]: NaturalLanguageProcessingWithPython
[201]: NaturalLanguageProcessingWithPython/nltk-introduction.ipynb
[202]: NaturalLanguageProcessingWithPython/corpus.ipynb
[203]: NaturalLanguageProcessingWithPython/conditional-frequency-distribution.ipynb
[204]: NaturalLanguageProcessingWithPython/lexical.ipynb
[205]: NaturalLanguageProcessingWithPython/handle-with-raw-text.ipynb
[206]: NaturalLanguageProcessingWithPython/classify-and-pos-tagging.ipynb
[207]: NaturalLanguageProcessingWithPython/classify_gender.ipynb
[208]: NaturalLanguageProcessingWithPython/classify_movie_reviews.ipynb
[209]: NaturalLanguageProcessingWithPython/classify_pos_tagging.ipynb
[210]: NaturalLanguageProcessingWithPython/classify_sentence_segment.ipynb
[211]: NaturalLanguageProcessingWithPython/recognizing-dialog-behaviour.ipynb
[212]: NaturalLanguageProcessingWithPython/recognizing-textual-entailment.ipynb
[213]: NaturalLanguageProcessingWithPython/decision-tree.ipynb
[214]: NaturalLanguageProcessingWithPython/naive-bayes.ipynb
[215]: NaturalLanguageProcessingWithPython/maximum-entropy.ipynb
[216]: NaturalLanguageProcessingWithPython/extract-from-text.ipynb
[217]: NaturalLanguageProcessingWithPython/sentence-structure.ipynb

[300]:jieba.ipynb
[301]:text_clustering.ipynb
[302]:topic_model.ipynb
