# 统计思维
![cover](https://img3.doubanio.com/lpic/s28278025.jpg)

    作者: [美] Allen B. Downey 
    出版社: 人民邮电出版社
    副标题: 程序员数学之概率统计（第2版）
    译者: 金迎 
    出版年: 2015-9
    页数: 204
    定价: 49.00元
    装帧: 平装
    ISBN: 9787115401083

- [豆瓣链接](https://book.douban.com/subject/26593825/)

## 目录
1. [探索性数据分析][70]
1. [分布][71]
    - 分布(distribution):样本中出现的值以及每个值出现的频数。
    - 直方图(histogram):从值到频数的映射，或者展示这一映射的图形。
    - 离群值(outlier):远离集中趋势的值。
    - 展布(spread):对值在分布中扩展规模的度量。
    - 汇总统计量(summary statistic):对分布的某些方面（如集中趋势或展布）进行量化的统计量。
    - 效应量(effect size):一种汇总统计量，用于量化一个效应的大小，如群组之间的差异。
1. [概率质量函数][72]
    - 概率质量函数(probability mass function,PMF):将概率分布表示为从值到概率的映射。
    - 概率(probability):将频数表示为样本量的分数。
    - 正态化(normalization):将频数除以样本量获得概率值的过程。
1. [累积分布函数][73]
    - 百分位秩(percentile rank):一个分布中小于或等于给定值的百分比。
    - 百分位数(percentile):与给定百分位秩相关联的值。
    - 累积分布函数(cumulative distribution function,CDF):将值映射到累积概率的函数。CDF(x)是样本中大于或等于x的值所占的比例。
    - CDF反函数(inverse CDF):从累积概率p映射到对应值的函数。
    - 中位数(median):位于第50分位的值。
    - 四分位距(interquartile range):第75分位和第25分位之间的差异，用于度量展布。
    - 分位数(quantile):对应于等距百分位秩的一列值。例如，一个分布的四分位数为第25百分位数、第50百分位数和第75百分位数。
    - 放回(replacement):采用过程的一种属性。“放回”是指同一个值可以选择多次；“不放回”是指一个值一旦被选择，就从总体中移除。
1. [分布建模][74]
    - 指数分布
    - 正态分布
    - 正态概率图
    - 对数正态分布
    - Pareto分布
1. 概率密度函数
    - 概率密度函数(probability density function,PDF):连续CDF的导数。这个函数将值映射到其概率密度。
    - 概率密度(probability density):一个数值，可以在一个取值范围上进行积分得到一个概率。如果值的单位为厘米，那么概率密度的单位为每厘米的概率。
    - 核密度估计(kernel density estimation,KDE):基于一个样本对PDF进行估计的算法。
    - 离散化(discretize):用离散函数对一个连续函数或分布进行模拟。离散化的逆操作是平滑化。
    - 原始矩(raw moment):一个统计量，基于数据乘方之和。
    - 中心矩(cntral monent):一个统计量，基于数据与均值差的乘方之和。
    - 标准化矩(standardized moment):矩的一个比率，没有单位。
    - 偏度(skewness):度量分布的对称性。
    - 样本偏度(sample skewness):一个基于矩的统计量，用于量化分布的偏度。
    - Pearson中位数偏度系数(Pearson's median skewness coefficient):用于量化分布偏度的一个统计量，基于中位数、均值和标准差。
    - 稳健(robust):如果一个统计量受离群值的影响相对较小，那么这个统计量就是稳健的。
1. 变量之间的关系
    - 抖动(jitter):为了可视化而在数据中加入的随机噪音。
    - 饱和(saturation):多个点叠加而导致的信息丢失。
    - 相关性(correlation):衡量两个变量之间关系强弱的统计量。
    - 标准化(standardize):将一组值进行转换，使其均值为0，方差为1。
    - 标准分数(standard score):一个标准化的值，表示为距离均值的标准差数。
    - 协方差(covariance):对两个变量共同趋势的度量。
    - 秩(rank):一个元素出现在排序列表中的索引。
    - Pearson相关性(Pearson's correlation)
    - Spearman秩相关(Spearman's rank correlation)
1. 估计
    - 估计(estimation):从样本推导出分布参数的过程。
    - 估计量(estimator):用于估计一个参数的统计量。
    - 均方误差(mean suqared error,MSE):对估计误差的度量。
    - 均方根误差(root mean suqared error,RMSE):均方误差的平方根，能够更有意义地表示典型误差的量级。
    - 最大似然估计(maximum likelihood estimator,MLE):一种估计量，计算最有可能正确的点估计。
    - 估计量偏倚(bias of an estimator):在重复实验中，一个估计量高于或低于参数实际值的平均趋势。
    - 抽样误差(sampling error):因为样本规模有限以及随机变化而导致的估计错误。
    - 抽样偏倚(samoling bias):因为抽样过程产生的样本不能代表总体而导致的估计误差。
    - 测量误差(measurement error):因为收集或记录数据不准确而导致的估计误差。
    - 抽样分布(sampling distribution):实验重复多次得到的统计量分布。
    - 标准误差(standard error):一个估计的均方根误差，对抽样误差（而非其他误差源）导致的波动进行量化。
    - 置信区间(confidence interval):一个区间，代表实验重复多次时预期的估计量范围。
1. 假设检验
    - 假设检验(hypothesis testing):判断一个直观效应是否统计显著的过程。
    - 检验统计量(test statistic):用于量化效应规模的统计量。
    - 原假设(null hypothesis):一个系统模型，假设一个直观效应是偶然发生的。
    - p值(p-value):一个效应可能偶然产生的概率。
    - 统计显著(statistically significant):如果一个效应不太可能偶然产生，则是统计显著的。
    - 置换检验(permutation test):通过重新排列观测数据集计算p值。
    - 重抽样检验(resampling test):通过对观测数据集进行放回抽样计算p值。
    - 双侧检验(two-side test):此检验回答的是，如果不考虑差异的方向性，实际效应与直观效应规模相同的概率是多少？
    - 单侧检验(one-side test):此检验回答的是，实际效应与直观效应相同并且差异的方向也一致的概率是多少？
    - 卡方检验(chi-squared test):使用卡方统计量作为检验统计量的检验。
    - 误报(false positive):当效应为假时，作出效应为真的结论。
    - 漏报(false negative):当效应非偶然产生时，作出效应是偶然产生的结论。
    - 功效(power):当原假设为假时，正检验的概率。
1. 线性最小二乘法
    - 线性拟合(linear fit):对变量关系进行建模的线。
    - 最小二乘法拟合(least squares fit):使残差平方和最小的数据集模型。
    - 残差(residual):实际值与模型的偏差。
    - 拟合优度(goodness of fit):度量模型与数据相符合的程度。
    - 决定系数$R^2$(coefficient of determination):量化拟合优度的统计量。
    - 抽样权重(sampling weight):与样本中一个观测相关的值，说明该观测代表总体的哪一部分。
1. 回归
    - 回归(regression):估计模型参数以拟合数据的几个相关过程之一。
    - 因变量(dependent variable):回归模型中，希望进行预测的变量，也称为内生变量。
    - 解释变量(explanatory variable):用于预测或解释因变量的变量，也称为自变量或外生变量。
    - 简单回归(simple regression):只有一个因变量和一个解释变量的回归。
    - 多重回归(multiple regression):有多个解释变量，但只有一个因变量的回归。
    - 线性回归(linear regression):基于线性模型的回归。
    - 普通最小二乘法(ordinary least square):通过最小化残差的均方误差，进行参数估计的线性回归。
    - 伪关系(spurious releationship):两个变量间的关系，由统计结果造成，或由模型之外但与这两个变量都相关的因素导致。
    - 控制变量(control variable):回归中的变量，用于消除或“控制”伪关系。
    - 代理变量(proxy variable):因与其他因素相关，成为该因素的代理，从而对回归模型产生间接影响的变量。
    - 分类变量(category variable):一种变量，取值为一组无序的离散值之一。
    - Logistic回归(logistic regression):因变量为布尔型时使用的一种回归。
    - Poisson回归(poisson regression):因变量为非负整数（通常为一个计算值）时使用的一种回归。
    - 优势(odds):概率p的另一种表示方法，即概率与其补值的比率，p/(1-p)。
1. 时间序列分析
    - 时间序列(time series):一个数据集，其中每个值都与一个时间戳相关，通常为一系列测量值及其收集时间。
    - 窗口(window):时间序列中的一列连续值，经常用于计算移动平均值。
    - 移动平均值(moving average):用于估计时间序列的潜在趋势的统计量之一，通过计算一些列重叠窗口的（某种）平均值得到。
    - 滚动均值(rolling mean):基于每个窗口均值的一种移动平均值。
    - 指数加权移动平均(exponentially-weighted moving average,EWMA):一种基于加权均值的移动平均值，最近的值具有最高的权重，早期值的权重按指数级降低。
    - 序列相关(serial correlation):一个时间序列和它自身的一个移动或滞后版本间的相关。
    - 滞后(lag):序列相关或自相关中数据移动的大小。
    - 自相关(autocorrelation):一个更为通用的术语，描述使用任意滞后值的序列相关。
    - 自相关函数(autocorrelation function):将滞后映射到序列相关的函数。
    - 平稳(stationary):如果一个模型的参数和残差分布不随时间变化，那么这个模型就是平稳的。
1. 生存分析
    - 生存分析(survival analysis):描述和预测生存期（或直到某事件发生的时间）的一组方法。
    - 生存曲线(survival curve):一个函数，将时间t映射到在t之后仍然存活的概率。
    - 危险函数(hazard function):一个函数，将时间t映射到t之前存活的人中在t时刻死亡的比例。
    - Kaplan-Meier估计(Kaplan-Meier estimation):估计危险函数和生存函数的一种算法。
    - 群组(cohort):在特定的事件间隔内，由某个事件（如出生日期）决定的一组对象。
    - 群组效应(cohort effect):群组之间的差异。
    - NBUE:预期剩余生存期的一个属性，“新比旧好”(New better than used in expectation)。
    - UBNE:预期剩余生存期的一个属性，“旧比新好”(Used better than new in expectation)。
1. 分析方法


[70]: explode.ipynb
[71]: distribution.ipynb
[72]: pmf.ipynb
[73]: cdf.ipynb
[74]: distribution-modeling.ipynb