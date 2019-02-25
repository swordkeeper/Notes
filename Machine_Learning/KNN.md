# KNN (K-nearest neighbours) K近邻算法
#### 概述
- KNN 算法是一个离散型分类算法，属于监督学习范畴
- 优点：算法简单易于实现，对于数据不怎么变化的问题精度高
- 缺点：计算复杂度特别高，尤其是对于特征纬度高的数据集。
#### 工作原理：
1. 输入**测试样本**的每条数据，将它们的特征与训练样本的特征进行一一比对，产生一个距离值。
2. 根据距离的由近到远，会产生一个排列，距离近的说明该条数据与训练样本集中的该类更接近（更相似），因而就更有有可能是该类别。
3. 列出前K个类别的集合，比如说比较后的结果为      <<<<是桃子，是桃子，是苹果，是香蕉，是桃子>>>> (K为5，此处)。          得到这样一个比较序列以后，找出这个序列里出现次数最多的类别，则最终做出的预测就是该类别。
#### 基本步骤
1. 收集数据
2. 计算距离值，一定要归一化，或者慎重考虑计算距离时，不同列的距离权重。
3. 分析数据
4. 训练算法，不需要训练模型
5. 测试算法
#### 简易代码
```python
import numpy as np
import operator as op
def createDataSet():   # 创建训练样本集合
    group = np.array([[1.0,1.1],[1.0,1.0],[0,0],[0,0.1]])
    labels = ['A','A','B','B']
    return group,labels
group,labels = createDataSet() # 获取样本
def classify0(inX, dataSet, labels, K):  # inX 为需要被分类的输入向量
    dataSetSize = dataSet.shape[0]
    diffMat = np.tile(inX, (dataSetSize,1)) - dataSet
    sqDiffMat = diffMat ** 2
    sqDistances = sqDiffMat.sum(axis=1)
    distances = sqDistances**0.5
    sortedDistIndicies = distances.argsort()
    classCount = {}
    for i in range(K):
        voteIlabel = labels[sortedDistIndicies[i]]
        classCount[voteIlabel] = classCount.get(voteIlabel,0) + 1
    sortedClassCount = sorted(classCount.items(),key = op.itemgetter(1),reverse = True)
    # 此句的意思是：用python内嵌的sorted函数对classCount字典的内容进行排序，
    # classCount.items()返回出一个list，list中的每个元素都是key-value对
    # 用operator库中的itemgetter获取每个键值对元组中的第二个值，并以此值大小排列，该list
    return sortedClassCount[0][0]
    # 返回该list中的最小的一个元素的key

    classify0([0,0],group,labels,3)
    # 结果为B
```
