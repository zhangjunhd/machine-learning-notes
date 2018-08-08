# 搜索引擎
## 综述
1. [What is faceted search][64]
1. [Lucene in Action](LuceneInAction)
    - [Meet Lucene](LuceneInAction/MeetLucene.md)
        - WHAT IS LUCENE?
        - LUCENE AND THE COMPONENTS OF A SEARCH APPLICATION
            - Components for indexing
            - Components for searching
            - The rest of the search application
        - LUCENE IN ACTION: A SAMPLE APPLICATION
            - Creating an index
            - Searching an index
        - UNDERSTANDING THE CORE INDEXING CLASSES
            - IndexWriter
            - Directory
            - Analyzer
            - Document
            - Field
        - UNDERSTANDING THE CORE SEARCHING CLASSES
            - IndexSearcher
            - Term
            - Query
            - TermQuery
            - TopDocs
    - [Building a search index](LuceneInAction/BuildingASearchIndex.md)
        - HOW LUCENE MODELS CONTENT
            - Documents and fields
            - Flexible schema
            - Denormalization
        - UNDERSTANDING THE INDEXING PROCESS
            - BASIC INDEX OPERATIONS
            - Adding documents to an index
            - Deleting documents from an index
            - Updating documents in the index
        - FIELD OPTIONS
            - Field options for indexing
            - Field options for storing fields
            - Field options for term vectors
            - Reader, TokenStream, and byte[] field values
            - Field option combinations
            - Field options for sorting
            - Multivalued fields
        - BOOSTING DOCUMENTS AND FIELDS
            - Boosting documents
            - Boosting fields
            - Norms
        - INDEXING NUMBERS, DATES, AND TIMES
            - Indexing numbers
            - Indexing dates and times
        - FIELD TRUNCATION
        - NEAR-REAL-TIME SEARCH
        - OPTIMIZING AN INDEX
        - OTHER DIRECTORY IMPLEMENTATIONS
        - CONCURRENCY, THREAD SAFETY, AND LOCKING ISSUES
            - Thread and multi-JVM safety
            - Accessing an index over a remote file system
            - Index locking
    - [Index File Formats](LuceneInAction/IndexFileFormats.md)
        - Definitions
        - Types of Fields
        - Segments
        - Document Numbers
        - Overview
        - File Naming
    - [Adding search to your application](LuceneInAction/AddingSearchToYourApplication.md)
        - IMPLEMENTING A SIMPLE SEARCH FEATURE
            - Searching for a specific term
            - Parsing a user-entered query expression: QueryParser
        - USING INDEXSEARCHER
            - Creating an IndexSearcher
            - Performing searches
            - Working with TopDocs
            - Paging through results
            - Near-real-time search
        - UNDERSTANDING LUCENE SCORING
            - How Lucene scores
            - Using explain() to understand hit scoring
        - LUCENE’S DIVERSE QUERIES
            - Searching by term: TermQuery
            - Searching within a term range: TermRangeQuery
            - Searching within a numeric range: NumericRangeQuery
            - Searching on a string: PrefixQuery
            - Combining queries: BooleanQuery
            - Searching by phrase: PhraseQuery
            - Searching by wildcard: WildcardQuery
            - Searching for similar terms: FuzzyQuery
            - Matching all documents: MatchAllDocsQuery
        - PARSING QUERY EXPRESSIONS: QUERYPARSER
            - Query.toString
            - TermQuery
            - Term range searches
            - Numeric and date range searches
            - Prefix and wildcard queries
            - Boolean operators
            - Phrase queries
            - Fuzzy queries
            - MatchAllDocsQuery
            - Grouping
            - Setting the boost for a subquery
    - [Lucene’s analysis process](LuceneInAction/LuceneAnalysisProcess.md)
        - USING ANALYZERS
            - Indexing analysis
            - QueryParser analysis
            - Parsing vs. analysis: when an analyzer isn’t appropriate
        - WHAT’S INSIDE AN ANALYZER?
            - What’s in a token?
            - TokenStream uncensored
            - Visualizing analyzers
        - USING THE BUILT-IN ANALYZERS
        - SOUNDS-LIKE QUERYING
        - SYNONYMS, ALIASES, AND WORDS THAT MEAN THE SAME
            - Creating SynonymAnalyzer
            - Visualizing token positions
        - STEMMING ANALYSIS
            - StopFilter leaves holes
            - Combining stemming and stop-word removal
1. [Elasticsearch Basic Concepts](ElasticsearchBasicConcepts.md)
1. [搜索引擎-信息检索实践](search-engine)
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


