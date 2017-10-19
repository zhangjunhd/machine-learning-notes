# 数据处理
## NumPy
1. [The NumPy ndarray: A Multidimensional Array Object][20]
    - The NumPy ndarray
        - data.shape
        - data.dtype
    - Creating ndarrays
        - np.array
        - np.zeros
        - np.ones
        - np.empty
        - np.arange
    - Data Types for ndarrays
        - data.astype
    - Operations between Arrays and Scalars
        - vectorization
        - broadcasting
1. [The NumPy Indexing and Slicing][21]
    - Basic Indexing and Slicing
        - Indexing with slices
    - Boolean Indexing
    - Fancy Indexing
        - np.ix_
1. [The NumPy Transposing and Swapping Axes][22]
    - data.T
    - np.dot
    - data.swapaxes
1. [Universal Functions: Fast Element-wise Array Functions][23]
    - np.sqrt(data)
    - np.exp(data)
1. [NumPy Loop-free programming with arrays][24]
    - np.meshgrid
    - Expressing Conditional Logic as Array Operations
        - np.where
    - Mathematical and Statistical Methods
        - np.mean()
        - data.mean(axis=1)
        - data.sum(0)
        - data.cumsum()
        - data.cumprod()
    - Methods for Boolean Arrays
        - bools.any
        - bools.all
    - Sorting
        - data.sort
        - data.sort(1)
    - Unique and Other Set Logic
        - np.unique

## Pandas
1. [数据结构Series][1]
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
1. [数据结构DataFrame][2]
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
1. [数据结构Index Object][3]
    - Index Objects
    - Immutable
    - Index methods and properties
1. [基本功能][4]
    - 重新索引
        - 重新索引并得到一个新的SeriesObj:SeriesOb2 = SeriesObj1.reindex(['index_value1', 'index_value2', 'index_value3'])
        - 基于索引创建新的SeriesObj并填充值(ffill/bfill)
        - 重新索引并得到一个新的DataFrameObj:DataFrameObj2 = DataFrameObj1.reindex(['index_value1', 'index_value2', 'index_value3'])
        - 基于columns关键字重索引：DataFrameObj.reindex(columns=['col_name1', 'col_name2', 'col_name3'])
        - 基于loc重索引：DataFrameObj.loc[['index_name1', 'index_name2', 'index_name3'], ['col_name1', 'col_name2', 'col_name3']]
    - 丢弃指定轴上的项
        - NewSeriesObj = SeriesObj.drop('index_name')
        - SeriesObj.drop(['index_name1', 'index_name2'])
        - DataFrameObj.drop(['index_name1', 'index_name2'])
        - DataFrameObj.drop('column_name', axis=1) 删除整列
        - DataFrameObj.drop(['column_name1', 'column_name2'], axis=1) 删除整列
        - DataFrameObj.drop(['index_name1', 'index_name2'], axis=0) 删除整行
        - SeriesObj.drop('index_name', inplace=True) 就地删除，不生成新对象
    - 索引、选取和过滤
        - Series获取内容值
            - 基于索引取内容
                - SeriesObj['index_name']
                - SeriesObj['index_name1','index_name2','index_name4']
                - SeriesObj['index_name1':'index_name4']
            - 基于位置序号取内容
                - SeriesObj[position]
                - SeriesObj[position1:position2]
                - SeriesObj[[position1,position2]]
            - 基于布尔数组取内容
                - SeriesObj[SeriesObj < 2]
        - DataFrame获取内容值
            - 基于column取内容
                - DataFrameObj['column_name']
                - DataFrameObj[['column_name3', 'column_name1']]
            - DataFrameObj[:2] 显示从第2行起的所有行数据
            - 基于布尔表达式取内容
                - DataFrameObj[DataFrameObj['column_name'] > 5] 显示满足某布尔表达式的数据
                - DataFrameObj < 5 得到一个全局布尔dataframe
                - DataFrameObj[DataFrameObj < 5] = 0 赋值满足某布尔表达式的数据
            - 基于loc/iloc取内容(loc基于value定位column/index，iloc基于position定位column/index)
                - DataFrameObj.loc['index_name', ['column_name2', 'column_name3']]
                - DataFrameObj.iloc[[1, 2], [3, 0, 1]] 显示指定行列上的数据，这里指定了二维数据
                - DataFrameObj.iloc[2] 显示第2行数据
                - DataFrameObj.loc[:'Utah', 'two']
                - DataFrameObj.iloc[:, :3][data.three > 5] 这里第二个维度使用了布尔表达式
    - 算术运算和数据对齐
        - 两个Series/DataFrame的算数运算是把对应行列上数据运算，不存在的补足NaN
    - 在算数方法中填充值
        - DataFrameObj1.add(DataFrameObj2, fill_value=0) 本来是Nan，现在以0填充
        - DataFrameObj1.reindex(columns=DataFrameObj2.columns, fill_value=0) 这样写就不会扩充更多的来自DataFrameObj2的行（对比上面第一种情况）
    - DataFrame和Series之间的运算
        - 从DataFrame中取某行成为Series：SeriesObj = DataFrameObj.iloc[0]
        - DataFrame与Series算术运算
            - DataFrameObj - SeriesObj
            - DataFrameObj.sub(SeriesObj, axis=0) 这里是按行计算然后broadcast
    - 函数应用与映射
        - Numpy.abs(DataFrameObj) NumPy ufuncs是(element-wise array methods)
        - DataFrameObj.apply(func) 
            - 默认func会按列作用到所有元素
            - axis=1，func会按行作用到所有元素
        - DataFrameObj.applymap(func) 这是element-wise
    - 排序和排名
        - sort_index
            - SeriesObj.sort_index()
            - DataFrameObj.sort_index() 基于index名排序
            - DataFrameObj.sort_index(axis=1) 基于列名排序
            - DataFrameObj.sort_index(axis=1, ascending=False) 基于列名排序，降序
        - sort_values
            - SeriesObj.sort_values()
            - DataFrameObj.sort_values(by='column_name')
            - DataFrameObj.sort_values(by=['column_name2', 'column_name1'])
        - rank
            - SeriesObj.rank()
            - SeriesObj.rank(method='first')
            - SeriesObj.rank(ascending=False, method='max')
            - DataFrameObj.rank(axis=1) 按行
            - DataFrameObj.rank(method='first') 按列
    - 带有重复值的轴索引
        - SeriesObj.index.is_unique
        - 若果index value是重复的
            - 对于Series：SeriesObj['index_name'] 得到另一个Series(多entry的)
            - 对于DataFrame：DataFrame['index_name'] 得到另一个对于DataFrame：DataFrame(多entry的)
1. [汇总和计算描述统计][5]
    - 描述统计
        - DataFrameObj.sum()
        - DataFrameObj.sum(axis=1)
        - DataFrameObj.mean(axis=1, skipna=False)
        - DataFrameObj.describe()
        - Descriptive and summary statistics列表
    - 相关系数与协方差
        - corr()
        - cov()
        - corrwith()
    - 唯一值、值计数以及成员资格
        - unique()
        - value_counts()
        - isin()：匹配一个指定的数组
        - pd.match(SeriesObj1, SeriesObj2)：匹配SeriesObj1中是否有SeriesObj2的元素，得到一个array，是对应元素在SeriesObj2中的position
        - apply(pd.value_counts).fillna(0)：例子，按列统计value，不存在的补零
1. [处理缺失数据][6]
    - 处理缺失数据
        - isnull() 返回布尔Series
    - 过滤缺失数据
        - SeriesObj.dropna()，可以有参数thresh=3
        - SeriesObj[SeriesObj.notnull()] 与上面的表达是等价的
        - 对于DataFrame，dropna(axis=1, how='all')
    - 填充缺失数据
        - fillna(0)：填充零
        - fillna({1: 0.5, 3: -1, 2: 0})：这里key是column name
        - fillna(0, inplace=True)
        - fillna(method='ffill', limit=2)
        - fillna(SeriesObj.mean())：填充均值
1. [数据转换][7]
    - 去重
        - DataFrameObj.duplicated() 得到一个布尔Series
        - DataFrameObj.drop_duplicates()
        - DataFrameObj.drop_duplicates(['column_name']) 基于某一列算重复值
        - DataFrameObj.drop_duplicates(['column_name1', 'column_name2'], keep='last') 基于某两列算重复值
    - 使用Function 或 Mapping做转换
        - 使用map()
            - data['column_name'].str.lower().map(func)
            - DataFrameObj['column_name'].map(lambda x: func[x.lower()])
    - 替换值
        - SeriesObj.replace(-999, np.nan) 一对一
        - SeriesObj.replace([-999, -1000], np.nan) 多对1
        - SeriesObj.replace([-999, -1000], [np.nan, 0]) 本质还是一对一
        - SeriesObj.replace({-999: np.nan, -1000: 0}) 多组一对一
    - 重命名 Axis Indexes
        - DataFrameObj.index.map(transformFunc)
        - DataFrameObj.rename(index=str.title, columns=str.upper)
        - DataFrameObj.rename(index={'origIndexName': 'newIndexName'},columns={'origColumnsName': 'newColumnName'})
        - rename(inplace=True)
    - Discretization 与 Binning
        - cats = pd.cut(数组数据, bins)
            - cats：分箱完Obj
            - cats.categories 每个箱子的name
            - cats.codes 每个值属于的箱子编号
            - pd.value_counts(cats) 得到每个箱子内数量
            - right=False：每个箱子默认是左闭右开，设置False后，左开右闭
            - pd.cut(数组数据, bins, labels=group_names)，设置category名字
            - pd.cut(数组数据, 4, precision=2) 分成4组，保留小数点2位
        - cats = pd.qcut(data, 4)：Cut into quartiles
    - 发现和过滤 Outliers
        - 基于某规则过滤一列，转化成一个布尔数组：
            - col = DataFrameObj[column_name];col[np.abs(col) > 3]
        - 找出任一符合某规则的列
            - DataFrameObj[(np.abs(DataFrameObj) > 3).any(1)]
        - 排除Outliers
            - DataFrameObj[np.abs(DataFrameObj) > 3] = np.sign(DataFrameObj) * 3
    - Permutation 和 Random Sampling
        - np.random.permutation(5) 排列0-4
        - df.take(一个数字数组) ，index是数字，则按这个数字数组作为index取数据
        - np.random.randint(0, 100, size=10) 在0-100中随机取10个整数
    - 计算 Indicator/Dummy 变量
        - pd.get_dummies()
        - 示例了如何将movielens/movies.dat的genres做成Indicator
        - 示例了将bins和get_dummies结合
1. [String操作][8]
    - String对象方法
        - split()
        - strip()
        - join()
        - index()
        - find()
        - count()
        - replace()
    - 正则表达式
        - \s+:one or more whitespace characters
        - findall()
        - search():returns only the first match.
        - match():only matches at the beginning of the string.
        - sub()
            - using special symbols like \1 and \2
        - groups()
            - findall returns a list of tuples when the pattern has groups
        - 列表：Regular expression methods
    - Pandas中向量化string函数
        - SeriesObj.str.contains('keyword')
        - SeriesObj.str.findall(pattern, flags=re.IGNORECASE) 结合正则表达式
        - 列表：Vectorized string methods
1. [数据合并与重塑][9]
    - 合并数据集
        - 数据库风格的DataFrame合并
            - pd.merge(DataFrameObj1, DataFrameObj2)
            - 基于两个dataframe中同名column_name作为merge key，类似inner join
            - pd.merge(DataFrameObj1, DataFrameObj2, on='column_name')
                - 显式地指定column name
            - pd.merge(DataFrameObj1, DataFrameObj2, left_on='column_name1', right_on='column_name2')
            - pd.merge(DataFrameObj1, DataFrameObj2, how='outer'):outer join
            - pd.merge(DataFrameObj1, DataFrameObj2, on='column_name', how='left'):left outer join
            - pd.merge(DataFrameObj1, DataFrameObj2, how='inner')
            - 列表：Different join types with how argument
            - 列表：merge function arguments
        - 索引上的合并
            - 指定column name和index进行merge
                - pd.merge(DataFrameObj1, DataFrameObj2, left_on='column_name', right_index=True)
                - pd.merge(DataFrameObj1, DataFrameObj2, left_on='column_name', right_index=True, how='outer')
                - pd.merge(DataFrameObj1, DataFrameObj2, left_on=['column_name1', 'column_name2'], right_index=True)
            - 指定index和index进行merge
                - pd.merge(DataFrameObj1, DataFrameObj2, how='outer', left_index=True, right_index=True)
                - 等价于： DataFrameObj1.join(DataFrameObj2, how='outer')
            - 多个dataframe间的join
                - DataFrameObj1.join([DataFrameObj2, DataFrameObj3])
        - axis连接
            - concat
                - pd.concat([Series1, Series2, Series3]):合并到1列
                - pd.concat([Series1, Series2, Series3], axis=1):合并到1行
                - pd.concat([Series1, Series2], axis=1, join='inner'):默认是全外连接，这里显式地指定内连接
                - pd.concat([Series1, Series2], axis=1, join_axes=[['index1', 'index2', 'index3', 'index4']]):指定`join_axes`
                - result = pd.concat([Series1, Series1, Series2], keys=['key1', 'key2', 'key3']):三个level并不存在，所以组成了层次结构
                    - result.unstack():摊平
                - pd.concat([DataFrameObj1, DataFrameObj2], ignore_index=True)
        - 合并重叠数据
            - np.where(pd.isnull(Series1), Series2, Series1)
            - Series2[:-2].combine_first(Series1[2:])
    - 层次化索引
        - 层次化索引介绍
            - 设置MultiIndex(`hierarchically-indexed` object, so-called `partial indexing`)
            - unstack():把多level index转成单level index
            - unstack().stack():再转成多level index
        - 重排分级(levels)顺序
            - swaplevel('index_name1', 'index_name2')
            - sortlevel(index_position)
            - swaplevel(index_position0, index_position1).sortlevel(index_position0)
        - 根据级别(level)汇总数据
            - sum(level='index_name')
            - sum(level='index_name', axis=1)
        - 使用DataFrame的列进行索引
            - set_index()
            - reset_index()
        - 整型位置索引
            - loc (for labels) 
            - iloc (for integers)
    - 重塑和轴向旋转
        - 重塑层次化索引
            - SeriesObj.DataFrameObj.stack():把一个dataframe 压成一个series，即变成层次结构(level)
                - SeriesObj.unstack()
                - SeriesObj.unstack(0):level number
                - SeriesObj.unstack('level_name')
            - stack(dropna=False):stack()默认会过滤缺失值
        - 将『长格式』旋转(pivot)为『宽格式』
            - 旋转：stack() 和 pivot()
1. [数据聚合与分组运算][10]
    - GroupBy技术
        - 对分组进行迭代
        - 选取一个或一组列
        - 通过字典或Series进行分组
        - 通过函数进行分组
        - 根据索引级别分组
    - 数据聚合
        - 面向列的多函数应用
        - 以『无索引』的形式返回聚合数据
    - 分组级运算和转换
        - Apply: 一般性的 split-apply-combine
        - 示例：用特定于分组的值填充缺失值
        - 示例：随机采样和排列
        - 示例：分组加权平均数和相关系数
        - 示例：面向分组的线性回归
    - 透视表和交叉表
        - 交叉表: crosstab
    - 示例：2012联邦选举委员会数据库
        - 根据职业和雇主统计赞助信息
        - 对出资额分组
        - 根据州统计赞助信息
1. [数据导入存储与格式][11]
    - 文本格式
        - Parsing functions in pandas
        - pd.read_csv('filepath')
        - pd.read_table('filepath', sep=',')
        - pd.read_csv('filepath', header=None):不带文件头
        - pd.read_csv('filepath', names=['col1', 'col2', 'col3']):指定文件头（列名）
        - 指定names和index
            - names = ['col1', 'col2', 'col3']
            - pd.read_csv('filepath', names=names, index_col='col3')
        - 指定层次化index
            - pd.read_csv('filepath', index_col=['col1', 'col2'])
        - 使用正则表达式作为分隔符
            - pd.read_table('filepath', sep='\s+')
        - 忽略某些行：skiprows
            - pd.read_csv('filepath', skiprows=[0, 2, 3]):忽略第0，2，3行
        - 缺失值:NA, -1.#IND, and NULL
            - 检验：pd.isnull(dataframeObj)
            - 指定特定形式的缺失值
                - sentinels = {'col1': ['foo', 'NA'], 'col3': ['two']}
                - pd.read_csv('filepath', na_values=sentinels)
                - 即NA,foo,two都被定义为缺失值
        - read_csv / read_table function arguments
    - 读入部分数据
        - pd.options.display.max_rows
        - 设置浏览的行数
            - pd.read_csv('filepath', nrows=5)
        - iterate方式

    ```py
    chunker = pd.read_csv('data/dataload/ex6.csv', chunksize=1000)

    tot = pd.Series([])
    for piece in chunker:
        tot = tot.add(piece['key'].value_counts(), fill_value=0)
    ```

    - 导出数据到文本
        - data.to_csv('filepath')
        - 指定分割符
            - import sys
            - data.to_csv(sys.stdout, sep='|')
        - 缺失值显式地“展现”
            - data.to_csv(sys.stdout, na_rep='NULL')
        - 关闭index和header
            - data.to_csv(sys.stdout, index=False, header=False)
        - 指定列
            - data.to_csv(sys.stdout, index=False, columns=['col1', 'col2', 'col3'])
    - 手动操作数据

    ```py
    import csv
    f = open('data/dataload/ex7.csv')

    reader = csv.reader(f)
    for line in reader:
        print(line)
    ```

        - csv.Dialect:设置读取csv的配置信息
        - CSV dialect options
    - JSON数据
        - jsonStr = json.loads(jsonObj)
        - jsonObj = json.dumps(jsonStr)
        - 转成dataframe
            - pd.DataFrame(jsonStr['json_key'], columns=['json_key_1', 'json_key_2'])
        - data = pd.read_json('jsonfile')
            - data.to_json()
            - data.to_json(orient='records') 
    - XML 与 HTML
        - pd.read_html('htmlfilepath')
        - objectify.parse(open('xmlfilepath'))
    - 二进制文件格式
        - dataframeObj.to_pickle('picklepath')
        - pd.read_pickle('picklepath')
    - 使用HDF5格式
        - store = pd.HDFStore('h5filepath')
        - store['key1'] = dataframeObj
        - store['key1_1'] = dataframeObj['col1']
        - store.put('key2', frame, format='table')
        - dataframeObj.to_hdf('h5filepath', 'key3', format='table')
        - pd.read_hdf('h5filepath', 'key3', where=['index < 5'])
    - 读取Excel文件
        - 读
            - xlsx = pd.ExcelFile('xlsxfilepath')
            - pd.read_excel(xlsx, 'Sheet1')
        - 写
            - writer = pd.ExcelWriter('xlsxfilepath')
            - frame.to_excel(writer, 'Sheet1')
            - writer.save()
    - 操作Web API
        - import requests
        - url = 'https://api.github.com/repos/pandas-dev/pandas/issues'
        - resp = requests.get(url)
        - data = resp.json()
        - pd.DataFrame(data, columns=['number', 'title', 'labels', 'state'])
    - 数据库操作
1. [使用Pandas和Seaborn绘图][12]
    - Line
    - Bar
        - cross-tabulation
        - seaborn
    - Histograms 和 Density Plots
    - Scatter 或 Point Plots
    - Facet grids 和 categorical data
1. [时间序列][13]
    - Date和Time
        - datetime
        - 在String和datetime之间转换
    - 时间序列基础
        - Indexing, selection, subsetting
        - Time series with duplicate indices
    - Date ranges,Frequencies,Shifting
        - 生成date ranges
        - Frequencies 和 Date Offsets
            - Week of month dates
        - Shifting (leading and lagging) data
            - Shifting dates with offsets
    - 处理Time Zone
        - Localization 与 Conversion
        - 对time zone-aware Timestamp objects的操作
        - 不同time zones之间的操作
    - Periods 与 Period Arithmetic
        - Period Frequency转换
        - Quarterly period frequencies
    - Timestamps 与 Periods之间的转换
        - 基于arrays创建PeriodIndex
    - Resampling 与 Frequency Conversion
        - Downsampling
            - Open-High-Low-Close (OHLC) resampling
            - Resampling with GroupBy
        - Upsampling 与 interpolation
        - Resampling with periods
    - Time series绘图
    - Moving window functions
        - Exponentially-weighted functions
        - Binary moving window functions
        - User-defined moving window functions
    - Performance and Memory Usage Notes

[1]: pandas-series.ipynb
[2]: pandas-dataframe.ipynb
[3]: pandas-index.ipynb
[4]: pandas-basic.ipynb
[5]: pandas-summarize-statistics.ipynb
[6]: pandas-data-cleaning-preparation.ipynb
[7]: pandas-data-transformation.ipynb
[8]: pandas-string-manipulation.ipynb
[9]: pandas-data-merge-reshape.ipynb
[10]:pandas-data-aggregation-and-group.ipynb
[11]:pandas-dataloading-storage-fileformats.ipynb
[12]:plotting-with-pandas-and-seaborn.ipynb
[13]:pandas-time-series.ipynb

[20]:numpy-ndarray.ipynb
[21]:numpy-indexing-and-slicing.ipynb
[22]:numpy-transposing-and-swapping-axes.ipynb
[23]:numpy-universal-functions.ipynb
[24]:numpy-loop-free-programming-with-arrays.ipynb
