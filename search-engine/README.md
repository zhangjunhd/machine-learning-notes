# 搜索引擎
## 综述
1. [What are some uses of machine learning in search engines][63]
1. [What is faceted search][64]
1. 搜索引擎-信息检索实践
    - [cover][61]
    - [搜索引擎架构][1]
    - [信息采集与信息源][2]
    - [文本处理][3]
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
    - [基于索引的相关排序][4]
        - 抽象的相关排序模型
        - 倒排索引
        - 压缩
        - 辅助结构
        - 索引构建
        - 查询处理
    - [查询与界面][5]
        - 查询转换与提炼
            - 停用词去除和词干提取
            - 拼写检查和建议
            - 查询扩展
            - 相关反馈
            - 上下文和个性化
        - 搜索结果显示
        - 跨语言搜索
    - [检索模型][6]
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
    - [搜索引擎评价][7]
    - [分类与聚类][8]
        - 分类
            - 朴素贝叶斯
            - SVM
            - 评价
            - 分类器和特征选择
            - 垃圾、情感及在线广告
        - 聚类
            - 层次聚类和K均值聚类
            - K近邻聚类
    - [社会化搜索][9]
1. 计算广告
    - [cover][62]
    - [在线广告综述][1]
        - 在线广告创意类型
        - 在线广告简史
    - [计算广告基础][2]
        - 广告有效性原理
        - 计算广告的核心问题
            - 广告收入的分解
            - 结算方式与eCPM估计的关系
    - [在线产品广告概览][3]
    - [合约广告][4]
        - 广告位合约
        - 受众定向
    - [搜索与竞价广告][5]
        - 搜索广告
            - 搜索广告产品形态
            - 搜索广告产品新形式
            - 搜索广告产品策略
        - 位置拍卖与机制设计
            - 定价问题
            - 市场保留价（Market Reserve Price，MRP）
            - 价格挤压
        - 广告网络
        - 竞价广告需求方产品

## 论文



[61]: search-engine
[62]: computational_ad
[63]: WhatAreSomeUsesOfMachineLearningInSearchEngines.md
[64]: WhatIsFacetedSearch.md


[1]: search-engine/archtecture.ipynb
[2]: search-engine/crawl.md
[3]: search-engine/handle_document.ipynb
[4]: search-engine/sort.ipynb
[5]: search-engine/query.ipynb
[6]: search-engine/search-model.ipynb
[7]: search-engine/evaluate.ipynb
[8]: search-engine/classification_clustering.ipynb
[9]: search-engine/social_search.ipynb

[11]: computational_ad/chap1.md
[12]: computational_ad/chap2.ipynb
[13]: computational_ad/chap3.md
[14]: computational_ad/chap4.md
[15]: computational_ad/chap5.ipynb
