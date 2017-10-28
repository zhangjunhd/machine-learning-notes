# 自然语言处理

## 词袋模型

1. [Python自然语言处理][200]
    - [NLTK入门][201]
    - [获取文本语料库][202]
    - [条件频率分布][203]
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


[300]:jieba.ipynb
[301]:text_clustering.ipynb
[302]:topic_model.ipynb