# Python自然语言处理

![cover](https://img3.doubanio.com/lpic/s27313176.jpg)

    作者: (美)Steven Bird Ewan Klein Edward Loper 
    出版社: 人民邮电出版社
    原作名: Natural Language Processing With Python
    译者: 张旭 / 崔阳 / 刘海平 
    出版年: 2014-6-25
    页数: 508
    定价: 89.00
    装帧: 平装
    ISBN: 9787115333681

[豆瓣链接](https://book.douban.com/subject/25916599/)

1. [NLTK入门][201]
    - 搜索文本
    - 计数词汇
    - 将文本当作词链表
    - 简单的统计
        - 频率分布
        - 词语搭配和双连词
        - 词长分布
1. [获取文本语料库][202]
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
1. [条件频率分布][203]
    - 条件与事件
    - 按文体计数词汇
    - 绘制分布图和分布表
    - 使用Bigrams生成随机文本
1. [词典资源][204]
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
1. [处理原始文本][205]
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
1. [分类和标注词汇][206]
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
1. [分类文本性别鉴定][207]
    - naive Bayes classifier
    - 似然比(`likelihood ratios`)
1. [分类文本电影评论正负面评价][208]
    - NaiveBayesClassifier




[201]: nltk-introduction.ipynb
[202]: corpus.ipynb
[203]: conditional-frequency-distribution.ipynb
[204]: lexical.ipynb
[205]: handle-with-raw-text.ipynb
[206]: classify-and-pos-tagging.ipynb
[207]: classify_gender.ipynb
[208]: classify_movie_reviews.ipynb
