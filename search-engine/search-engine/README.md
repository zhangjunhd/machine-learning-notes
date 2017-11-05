# 搜索引擎
![cover](https://img3.doubanio.com/lpic/s6176453.jpg)

[豆瓣链接](https://book.douban.com/subject/4861766/)

    作者: W.Bruce Croft / Donald Metzler / Trevor Strohman
    出版社: 机械工业出版社
    副标题: 信息检索实践
    原作名: Search Engines : Information Retrieval in Practice
    译者: 刘挺 / 秦兵 / 张宇 / 车万翔
    出版年: 2010-6-1
    页数: 309
    定价: 56.00元
    装帧: 平装
    丛书: 计算机科学丛书
    ISBN: 9787111288084

1. [搜索引擎架构][1]
1. [信息采集与信息源][2]
1. [文本处理][3]
    - 从词到词项
    - 文本统计
        - Zipf's Law
        - 语料规模与词表大小关系Heaps(1978)
        - 估计数据集和结果集大小
    - 文档解析
    - 文档结构和标记
    - 链接分析
    - 信息抽取
    - 国际化
1. [基于索引的相关排序][4]
    - 抽象的相关排序模型
    - 倒排索引
    - 压缩
    - 辅助结构
    - 索引构建
    - 查询处理
1. [查询与界面][5]
    - 查询转换与提炼
        - 停用词去除和词干提取
        - 拼写检查和建议
        - 查询扩展
        - 相关反馈
        - 上下文和个性化
    - 搜索结果显示
    - 跨语言搜索
1. [检索模型][6]
    - 检索模型概述
        - 布尔检索模型
        - 向量空间模型
    - 概率模型
        - 将信息检索作为分类问题
        - BM25排序算法
    - 基于排序的语言模型
        - 查询项似然排序
        - 相关性模型和伪相关反馈
    - 复杂查询与证据整合
    - 机器学习和信息检索
        - 排序学习
        - 主题模型和词汇不匹配
1. [搜索引擎评价][7]
1. [分类与聚类][8]
    - 分类
        - 朴素贝叶斯
        - SVM
        - 评价
        - 分类器和特征选择
        - 垃圾、情感及在线广告
    - 聚类
        - 层次聚类和K均值聚类
        - K近邻聚类
1. [社会化搜索][9]

[1]: archtecture.ipynb
[2]: crawl.md
[3]: handle_document.ipynb
[4]: sort.ipynb
[5]: query.ipynb
[6]: search-model.ipynb
[7]: evaluate.ipynb
[8]: classification_clustering.ipynb
[9]: social_search.ipynb