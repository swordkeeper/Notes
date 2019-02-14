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

7. df的转置

   ```python
   df.T
   ```

8. df排序

   ```python
   df.sort_index(axis=1,ascending=False)
   # 根据索引排序，axis=1，表示根据第二纬度的索引进行降序排序 
   df.sort_value(by = "B") 
   # 根据列B的值进行排序
   ```

9. df索引

   ```python
   df['A']
   # or
   df.A
   # 返回索引是A的列
   ###################################################
   
   df[0:3]
   # df
   df['20130202':'20130206']
   # 返回前三行，或返回索引是'20130202'到'20130206'之间的行。
   
   ###################################################
   df.loc[dates[0]] 
   # 返回0号日期的行索引，即返回20130101该行
   df.loc[:,['A','B']]
   # 返回所有行，A和B列
   ###################################################
   
   df.iloc[3:5,0:2]
   # 返回行序号为3～5，列序号为0～2的元素
   # 该方法的区别是1.关键字为 iloc，2.括号内写数字序号
   
   ###################################################
   # 布尔索引
   df.loc[df.A > 0] 
   # loc里面 ，逗号之前都是筛选行，因而此处就是筛选行为df.A列大于零的所有行
   
   ```

10. df拷贝和添加数据

    ```python
    df1 = df.copy()
    # 拷贝df的数据到df1
    df1['E'] = ['one', 'one', 'two', 'three', 'four', 'three']
    # 添加df1一个新的列
    
    # isin函数
    df2[df2['E'].isin(['two','three'])]
    # 返回df2中列E的元素是'two'或'three'的行
    ```

11. df简单数据清洗，补全

    ```python
    df.dropna(how="any")
    # 删除所有包含'NaN'的所有行
    df.fillna(value = '5')
    # 所有NaN都补为5
    ```

12. df的补全技能 shift

    ```python
    df = pd.DataFrame({'Col1': [10, 20, 15, 30, 45],'Col2': [13, 23, 18, 33, 48],'Col3': [17, 27, 22, 37, 52]})
    # 用字典来创建包含Col1~3三列的df
    df.shift(periods=3) # shift periods 3 表示，扩展3行（逗号前面都表示行，所以是行扩展）
    # 结果为
    #     Col1  Col2  Col3
    # 0   NaN   NaN   NaN
    # 1   NaN   NaN   NaN
    # 2   NaN   NaN   NaN
    # 3  10.0  13.0  17.0
    # 4  20.0  23.0  27.0
    # 即扩展了前三行
    df.shift(periods=1, axis='columns') # 从列维度来扩展一个1列
    #   Col1  Col2  Col3
    #0   NaN  10.0  13.0
    #1   NaN  20.0  23.0
    #2   NaN  15.0  18.0
    #3   NaN  30.0  33.0
    #4   NaN  45.0  48.0
    df.shift(periods=3, fill_value=0) # 补0
    ```

13. df基本的函数

    ```python
    df.mean() # 每列数据的均值
    df.mean(1) # 求axis=1纬度的数据均值，即每行均值
    ```

14. df的string

    ```python
    s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
    s.str.lower()  # 大写转小写
    ```

    