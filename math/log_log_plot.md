# log-log双对数曲线图
所谓[双对数曲线图][1]，就是图的两个坐标轴的刻度均为对数刻度，这样一来的话，形如y=ax^b的指数曲线，在双对数曲线图中就表现为一条直线，b就是这条直线的斜率。

![](https://upload.wikimedia.org/wikipedia/commons/thumb/9/92/LogLog_exponentials.svg/512px-LogLog_exponentials.svg.png)

可以这样来理解，将y=ax^b两边都取对数，得到：ln(y) = ln(a) + bln(x)，令y’ = ln(y), x’ = ln(x), 那么在对数曲线图中，得到的就是一条y’ = a’ + bx’ 的直线，数轴的长度单位用的就是y’ 和x’ 的单位，但是“对数曲线图”的“对数”指的是刻度取对数，所以数轴上的值标的还是x和y的值，所以相邻长度单位上标的数值随数轴的延伸相差越大，也就是说 x’ 每次增加1，但是x 增加的幅度却是按x’ = ln(x)越来越大的。



[1]: https://en.wikipedia.org/wiki/Log%E2%80%93log_plot
