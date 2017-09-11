# 数据处理
## Pandas
1. [Pandas数据结构Series][1]
    - Series创建与更新
        - Series.values
        - Series.index
        - 基于index更新value
        - 基于index显示多行
    - NumPy-like operations
        - filtering with a boolean array：Series[Series > 0]
        - scalar multiplication：Series * 2
        - applying math functions：Numpy.exp(Series)
    - Fixed-length ordered dict
        - value in Series
        - Pandas.isnull(Series)
        - Pandas.notnull(Series)
        - Series.isnull()
        - Series + Series 对应index的值相加
    - name属性
        - Series.name
        - Series.index.name
1. [Pandas数据结构DataFrame][2]
    - DataFrame的创建、显示、更新、删除
        - 基于dict（value为数组）创建
        - DataFrame.head()
        - 指定columns以及顺序，如果column不存在，值为NaN
        - 指定index
        - DataFrame.columns
        - DataFrame['column_name'] 显示指定列的数据
        - DataFrame.column_name 显示指定列的数据
        - DataFrame.loc['index_name'] 显示指定行的数据
        - DataFrame['column_name'] = 16.5 更新指定列的数据为某一常数
        - DataFrame['column_name'] = np.arange(5.) 更新指定列的数据为某一数组
        - DataFrame['column_name'] = Series 更新指定列的数据为某一Series
        - DataFrame['column_name1'] = DataFrame.column_name2 == 'Ohio' 更新指定列的数据为某布尔判断式
        - del DataFrame['column_name'] 删除某列数据
        - 基于嵌套dict创建
        - DataFrame.T 转置
        - 基于多个Series创建
    - name属性
        - DataFrame.index.name
        - DataFrame.columns.name
    - values属性
        - DataFrame.values
    - DataFrame constructor
1. [Pandas数据结构Index Object][3]
    - Index Objects
    - Immutable
    - Index methods and properties


[1]: pandas-series.ipynb
[2]: pandas-dataframe.ipynb
[3]: pandas-index.ipynb
---
- 基本功能
    - 重新索引
    - 丢弃指定轴上的项
    - 索引、选取和过滤
    - 算术运算和数据对齐
    - 在算数方法中填充值
    - DataFrame和Series之间的运算
    - 函数应用与映射
    - 排序和排名
    - 带有重复值的轴索引
- 汇总和计算描述统计
    - 相关系数与协方差
    - 唯一值、值计数以及成员资格
- 处理缺失数据
    - 过滤缺失数据
    - 填充缺失数据
- 层次化索引
    - 重排分级(levels)顺序
    - 根据级别(level)汇总数据
    - 使用DataFrame的列
- 其他有关pandas的话题
    - 整数索引
    - 面板数据