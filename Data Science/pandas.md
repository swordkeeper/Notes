# Pandas 库

### 安装
```python
python3 -m pip install --upgrade pandas
## or 
conda install pandas
```
### 简介
```pandas```是python中用来处理数据于表格的一个库，包括处理csv，json，excel等文件。
pandas 中包括两种主要的数据结构，即```一维的Series和二维的DataFrame```，pandas库是建立在```numpy```库之上的。

### 基本应用
#### 导入，载入
```python
import numpy as np
import pandas as pd 
```
#### 建立一个Series，即建立一个序列（一列值）
```python
s = pd.Series([1,2,3,np.nan,6,8])
# 建立了一个包含一列数字1, 2, 3,无穷,6,8的序列
```
#### 创建一个DataFrame，即建立一个表格数据框架
1. 用函数np.random.rand创建DateFrame
    ```python
    dates = pd.date_range('20130101', periods=6)
    # 先建立一个时间的range，范围从20130101开始的6天
    # 结果为
    # DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
    #                '2013-01-05', '2013-01-06'],
    #               dtype='datetime64[ns]', freq='D')
    # 生成的dates为索引类型，即该dates可以作为索引序号
    df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list('ABCD'))
    # 生成dateframe，第一个参数为用np.random.randn生成一个6x4的随机正态分布矩阵，并且该dataframe以之前生产的日期为索引，列名为'A', 'B', 'C', 'D'
    # 结果如下
    #                    A         B         C         D
    # 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
    # 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
    # 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
    # 2013-01-04  0.721555 -0.706771 -1.039575  0.271860
    # 2013-01-05 -0.424972  0.567020  0.276232 -1.087401
    # 2013-01-06 -0.673690  0.113648 -1.478427  0.524988
    ```
2. 用字典创建
    ```python
    df2 = pd.DataFrame({'A': 1.,
       ...:                     'B': pd.Timestamp('20130102'),
       ...:                     'C': pd.Series(1, index=list(range(4)), dtype='float32'),
       ...:                     'D': np.array([3] * 4, dtype='int32'),
       ...:                     'E': pd.Categorical(["test", "train", "test", "train"]),
       ...:                     'F': 'foo'})
    ```
3. 查看数值类型

   ```python
   df.dtypes
   # 可以查看所有列的数据类型
   ```

4. 查看数据前几列和后几列

   ```python
   df.head(3)
   df.tail(3)
   ```

5. 显示索引和列名

   ```python
   df.index
   df.columns
   ```

6. 快速统计

   ```python
   df.describe()
   # 快速的的对每一列的数据进行统计
   #              A         B         C         D
   #count  6.000000  6.000000  6.000000  6.000000
   #mean  -0.022437 -0.733837  0.468751  0.387390
   #std    1.266900  1.293374  0.932082  0.703868
   #min   -1.374054 -2.779502 -0.973502 -0.435468
   #25%   -0.993327 -1.244238 -0.007789 -0.023001
   #50%   -0.273047 -0.667091  0.593460  0.155149
   #75%    0.834714 -0.033753  1.066978  0.947458
   #max    1.814981  0.969925  1.582474  1.320333
   ```

   