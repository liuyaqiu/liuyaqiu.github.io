---
layout: post
title: "pandas Tutorial"
categories: tutorial
---

pandas是非常好用的Python统计学扩展库，内置了多种非常便利的数据结构和函数，可以很容易地进行数据统计操作，是机器学习的强大工具。

# **Numpy**

Numpy是一个应用更加广泛的数值计算库，pandas的部分数据结构基于Numpy，pandas可以直接支持对Numpy数据结构的转换。Numpy中最重要的数据结构是array，经常被使用。

**基本操作**

```
import numpy as np
a1 = np.array([1, 2, 3, 4, 5]) --> array([1, 2, 3, 4, 5])
type(a1) --> numpy.ndarray
np.size(a1) --> 5
a2 = np.array([1, 2, 3, 4.0, 5.0]) --> array([1., 2., 3., 4., 5.])
a2.dtype --> dtype('float64')
np.zeros(5) --> array([0., 0., 0., 0., 0.])
np.arange(5) --> array([0, 1, 2, 3, 4])
np.linspace(0, 5, 6) --> array([0., 1., 2., 3., 4., 5.])
a1 * 2 = array([2, 4, 6, 8, 10])
a1 + a2 = array([2.0, 4.0, 6.0, 8.0, 10.0])
np.array([[1, 2], [3, 4]]) --> array([[1, 2],
                                      [3, 4]])
m = np.arange(0, 6).reshape(2, 3) --> array([[0, 1, 2],
                                             [3, 4, 5]])
np.size(m) --> 6
m.shape --> (2, 3)
np.size(m, 0) --> 2
np.size(m, 1) --> 3
m[1] --> array([3, 4, 5])
m[1, ] --> array([3, 4, 5])
m[1, 2] --> 5
m[:, 1] --> array([1, 4])
m[::-1, ::-1] --> array([[5, 4, 3],
                         [2, 1, 0]])
a = np.arange(5)
a < 2 --> array([True, True, False, False, False], dtype=bool)
(a < 2) | (a > 3) --> array([True, True, False, False, True], dtype=bool)
np.sum(a < 2) --> 2
def func(x):
    return x < 2 or x > 3
np.vectorize(func)(a) --> array([True, True, False, False, True], dtype=bool)
r = a < 2
a[r] --> array([0, 1])

a1 = np.arange(1, 10) -- > array([1, 2, 3, 4, 5, 6, 7, 8, 9])
a1[3:8] --> array([4, 5, 6, 7, 8])
a1[8:3:-1] --> array([9, 8, 7, 6, 5])
a1[::-2] --> array([9, 7, 5, 3, 1])

a = np.arange(0, 9)
m = a.reshape(3, 3) --> array([[0, 1, 2],
                               [3, 4, 5],
                               [6, 7, 8]])
m.ravel() --> array([0, 1, 2, 3, 4, 5, 6, 7, 8]) (return a new array)
m.flatten() --> array([0, 1, 2, 3, 4, 5, 6, 7, 8]) (return a copy view)
m.flatten().shape --> (9, ) (1 dimension array)
m.transpose() --> array([[0, 3, 6],
                         [1, 4, 7],
                         [2, 5, 8]]) (identical: m.T)
m.resize(1, 9) --> array([[0, 1, 2, 3, 4, 5, 6, 7, 8]]) (1 * 9 array, inplace transform)
```

**数组拼接**
```
a = np.arange(0, 9).reshape(3, 3)
b = (a + 1) * 10
np.hstack([a, b]) (identical: np.concatenate([a, b], axis=1))
array([[0, 1, 2, 10, 20, 30],
       [3, 4, 5, 40, 50, 60],
       [6, 7, 8, 70, 80, 90]])
np.vstack([a, b]) (identical: np.concatenate([a, b], axis=0))
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8],
       [10, 20, 30],
       [40, 50, 60],
       [70, 80, 90]])
更复杂的拼接方式有:
np.row_stack()
np.column_stack()
np.dstack()
```

**数组分拆**

分拆是拼接的逆操作
```
np.hsplit()
np.vsplit()
np.split()
np.dsplit()
```

**数值操作**
```
ndarray.mean() --> 均值
ndarray.std() --> 标准差
ndarray.var() --> 方差
ndarray.max() --> 最大值
ndarray.min() --> 最小值
ndarray.sum() --> 和
ndarray.cumsum() --> 累计和
ndarray.prod() --> 乘积
ndarray.cumprod() --> 累计乘积
(ndarray bool selection).all() --> 任意均成立
(ndarray bool selection).any() --> 存在
```

# **Series**

Series可以认为是一种带有index的特殊array, array是index为0, 1, 2...的自然数序列的Series

**基本操作**
```
import pandas as pd
s1 = pd.Series(np.arange(10))
s1.values # 输出s的值序列
s1.index # 输出s的index序列，默认为0开始的自然数序列
s2 = pd.Series([1, 2, 3], index=['a', 'b', 'c'])
s3 = pd.Series(2, index=s1.index)
s4 = pd.Series({'a': 1, 'b': 2, 'c': 3})
s5 = pd.Series([np.nan] * 3) #np.nan表示Not a Number
len(s1) --> 10 (identical: s1.size)
s1.shape --> (10, )
s1.count() --> 10 # s1中所有非NaN数值的数量
s1.unique() # 返回s1所有的不同数值(不包括NaN)
s1.value_count() # 返回s1中各个数值的数目统计
```

**读取数据**
```
s1.head() # 返回s1的前5个数
s1.tail() # 返回s1的后5个数
s1.take([0, 3, 9]) # 返回s1对应行数的数据
s2['a'] # 读取index为'a'的数
s2[1] # 读取行数为1的数
s2[['a', 'c']] # 读取相应index的数据
s2.loc['a'] # 通过label index读取
s2.loc[['a', 'c']]
s2.iloc[1] # 通过positoin index读取
s2.iloc[[1, 2]]
s2.ix[['a', 'c']]
s2.ix[[1, 2]] #ix既可以通过label又可以通过position读取，不推荐
```
数据读取还支持slice操作，即start:stop:step形式，冒号可以进行后向省略.例如
```
:9 --> 读取序列的前8个元素
::2 --> 以2为步长读取序列的元素
4::-1 --> 逆序读取第4, 3, 2, 1, 0等5个元素
```

**NaN**

在Numpy中，NaN数据也会被代入计算，例如array中包含NaN时，mean()将会返回NaN.
在pandas中，NaN数据在计算时会被默认忽略, 可以通过函数的skipna=False参数将NaN代入运算。
`s1.mean(skipna=False)`

**Boolean Selection**

与Numpy相似，s1 < 5 返回一个boolean Series, s1[s1 < 5]选中值小于5的数据
```
or --> |
and --> &
not --> ~
```
不能使用Python中的逻辑词来连接多个选则语句，要使用对应的位运算符

**Reindex**

Series的index是可以修改的
```
s1.index = ... # inplace修改
s1.reindex(...) # return a new Series
```

**删除数据**
```
del(s2['a']) # 删除s2的'a'行
```

# **DataFrame**

DataFrame可以认为是一种数据库，是多个Series的集合，通过index进行索引。

**基本操作**
```
df = pd.DataFrame([np.arange(5), np.arange(5, 10)])
df.shape --> (2, 5)
df1 = pd.DataFrame([np.array([[10, 11], [12, 13]])], columns=['a', 'b'])
df1.columns # 输出列标签
df1.columns = ['c', 'd'] # 更改列标签
df1.index # 输出行标签
df1.index = [2, 3] # 更改行标签
df.head() # 输出前5行数据
df.tail() # 输出末尾5行数据
len(df1) # df1的行数
np.size(df1, 0) # df1的行数
np.size(df1, 1) # df1的列数
df1.size # df1的元素数目(row * column)
```

**选择数据**

DataFrame可以通过slice的方式选中数据
```
df = pd.DataFrame(np.random.randn(10, 4), columns=list('abcd'), index=list('ABCDEFGHIJ'))
df[:3] # 选取前3行
df[:'C'] # 选取前3行
df[['c']] # 选取'c'列
df[['a', 'c']] # 选取'a', 'c'两列
df['a'] # 选取'a'列数据，返回Series (identical: df.a)
df.loc['A'] # 通过label选取行数据, 返回Series
df.loc[:'C'] # slice selection
df.loc[:, 'a'] # 选取'a'列数据，返回Series
df.iloc[1] # 通过position选取行数据，返回Series
df.iloc[:3] # slice selection
df.iloc[:, 1] # 选取1列数据
df.ix['A']
df.ix[1] # ix既可以支持label, 又可以支持position，不推荐使用
df.at['A', 'b'] # 返回'A'行，'b'列的数据
df.iat[1, 2] # 返回1行，2列的数据
df.index.get_loc('A') # 获取'A'行的position
df.columns.get_loc('a') # 获取'a'列的position
```

**Boolean Selection**

DataFrame也可以通过boolean selection方式选取数据
```
df[df.c > 0] # 选取'c'列值大于0的各行
逻辑连接词如下
or --> |
and --> &
not --> ~
```

**Modify**
```
df.rename(columns={...}, index={})# 修改列标签
df['e'] = ... # 添加'e'列，注意Series的index需要与df一致
df.insert(...) # 在特定的列号位置插入列
df.loc['X'] = ... # 在特定行插入数据，若该行已经存在，则修改
del df['a'] # 删除'a'列数据
df.pop('b') # 删除'b'列数据，并返回该列的Series
df.drop('c', axis=1, inplace=True) # 删除'c'列数据
df.drop('A', axis=0, inplace=True) # 删除'A'行数据
df.append(...) # 将其他DataFrame拼接到df后. 注意DataFrame合并时可能会发生数据列扩张和行扩张，会进行自动index对齐 
pd.concat([df1, df2]) # 对df1, df2进行行拼接
pd.concat([df1, df2], axis=1) # 对df1, df2进行列拼接
```

**运算**

DataFrame对于数值的运算是对每一个元素进行计算，当两个DataFrame进行运算时，会先进行index对齐

**变更index**
```
df.reset_index() # 重置index为自然数序列，原来的index变成新的一列
df.set_index('index') # 将'index'列的数据作为index
df.reindex(index=[...], columns=[...]) # 更改df的行标签，列标签
df.set_index(['index', 'a']) # 使用'index', 'a'两列作为多级索引
```

**数值操作**
```
df.mean() # 均值, axis=0为每列均值，axis=1为每行均值
df.std() # 标准差
df.var() # 方差
df.min() # 最小值
df.max() # 最大值
df.median() # 中值
df.idxmax() # 获取最大行坐标
df.idxmin()
df.sum()
df.cumsum()
df.prod()
df.cumprod()
df.describe() # 输出数据汇总
```

**数据读取**
```
pd.read_csv(...) # 读取csv文件
df.to_csv(...) # 存储为csv文件
pd.read_table(path, sep=',') # 读取以','为分隔符的table文件
df.to_csv(..., sep='|') # 以','为分隔符存储
pd.read_excel(path, sheetname=)# 读取excel文件中特定的sheet
df.to_excel(..., sheetname=) # 存储为excel文件中的特定sheet
pd.read_json(...) # 读取json文件
df.to_json(...) # 存储为json文件
```

# **数据清理**
```
df.isnull() # 判断元素是否为NaN值
df.isnull().sum() # 每列NaN值的数量
df.isnull().sum().sum() # 所有NaN值元素的数量
df.notnull() # 判断元素是否为NaN值
df.dropna(axis=0) # 删去包含NaN值的行
df.dropna(axis=1) # 删去包含NaN值的列, how和thresh参数能够控制删除条件
df.fillna(0) # 对NaN值用0填充
df.fillna(method='ffill') # 对NaN值前向填充
df.fillna(method='bfill') # 对NaN值后向填充
df.interpolate() # 插值填充, method参数可以控制插值方式
df.duplicated() # 判定行是否重复
df.drop_duplicates() # 删除重复行
df.replace(...) # 替换数值
df.map(df1) # 从df映射到df1
df.apply(...) # 结合lambda, map, reduce函数实现特殊操作
```
# **数据合并**

DataFrame可以像数据库那样将多个数据集进行整合
```
df1.merge(df2) # 将df2合并到df1, 默认方式为inner join
df1.merge(df2, how=...) # how参数可以设置为inner, outer, left, right四种方式，执行不同形式的join操作
df1.join(df2) # 使用index进行join，merge使用columns进行join
df.pivot(index=, columns=, values=) # 将数据库变成二维列表
df.stack() # 将一个列标签移到行标签中
df.unstack() # 将一个行标签移到列标签中
pd.melt(data, id_vars=[...], value_vars=[...]) # 进行melt操作
```

# **数据汇总**
```
grouped = df.groupby(...) # 进行group操作，返回GroupBy对象
grouped.size() # 各个group分组的条目数
grouped.count() # 统计各个group各列的数值数量(不包含NaN)
grouped.get_group(...)  # 读取对应group分组
grouped.nth(10) # 获取各个group分组的前10个条目
df.groupby(level=0) # 通过第0级index进行group
grouped.mean() # 输出group分组的均值
grouped.std()
grouped.var()
grouped.sum()
grouped.prod()
grouped.aggregate(func) # 使用func函数做汇总
grouped.transform(func) # 对grouped的每个分组做func变换
grouped.filter(func) # 通过func函数做筛选
pd.cut(array, 5) # 将array中的数据按照大小分为5个等长区间
pd.qcut(array, 5) # 将array中的数据按照大小分为5个区间，各个区间中的元素数量相同
```

# **采样**

为了使数据平滑，常常使用滑窗(对某时间开始后的一段时间窗口做均值或者求和处理)
为了使数据按照时间段汇总，常常使用下采样，为了获得更多的数据，常常使用上采样(采取前向、后向或者插值填充)
```
df.rolling(...) # 返回一个滑窗, 返回Rolling对象
df.rolling(...).mean() # 滑窗取均值
df.resample(...) # 进行上下采样，返回Resampler对象
df.resample(...).sum() # 下采样取求和
df.resample(...).interpolate() # 上采样进行插值填充
```

# **其他**
pandas原生支持对时间序列的处理，时间序列处理是pandas的优势特色功能。由于时间序列的处理独立于基本数据操作，故在此不加赘述。

pandas默认支持可视化，可以通过Series, DataFrame的plot()函数绘制简单的图像可。视化相关的技术与数据处理是相对独立的，而且广泛使用的实际上是matplotlib和seaborn两个扩展库。关于数据可视化，以后可能会根据需要单独写一篇博客
