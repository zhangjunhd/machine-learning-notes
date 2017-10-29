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


[201]: nltk-introduction.ipynb
[202]: corpus.ipynb
[203]: conditional-frequency-distribution.ipynb
[204]: lexical.ipynb