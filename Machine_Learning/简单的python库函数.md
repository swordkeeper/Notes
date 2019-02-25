# Python 数学库和函数

#### 最基本的库
- numpy
- scipy
- pylab
- cython
- pypy
- matplotlib

### numpy
- random
```python
# 生成一个 4X4的数组，类型为array
np.random.rand(4,4) 
```
在numpy中数组类型```array```与```matrix```类型不同。前者就是一个多维数组，**不能**进行矩阵运算，后者就是矩阵类型，**可以**进行矩阵运算。
```python
# 转换方法为
randMat = np.mat(np.random.rand(4,4))
# 生成了一个4x4的矩阵
```
