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
- 矩阵的一些运算
```python
# randMat 为 matrix类型
invRandMat = randMat.I #逆矩阵
ret = invRandMat * randMat # 矩阵乘法
# 相乘结果非常接近单位矩阵，因而
zeroMat = ret - np.eye(4) #eye为单位矩阵
# 相减之后非常接近零矩阵，因而可以用，
np.isallclose(zeroMat, np.zeros(4)) #返回true，或者
np.isclose(zeroMat, np.zeros(4)) # 返回true矩阵

traRandMat = randMat.T #转至矩阵transpose
