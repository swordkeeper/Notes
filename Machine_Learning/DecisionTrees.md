# 决策树
#### 决策树概述
- 决策树是**树型**分类算法
- 基本原理是根据不同的条件，逐渐将一个数据划分到某一类别之中
- <img src="./images/Decision-Trees-modified-1.png"></img>
- 决策树的关键是：
    - 选择划分方法
    - 减少树形结构（防止树结构过大或者过拟合）

### 决策树的算法的分类
决策树算法有很多变种，包括：
1. ID3 基于信息熵和信息增量（information gain）的算法
2. C4.5 基于基尼不纯度 （Gini impurity）的算法
3. CART 基于分类回归树 的算法

#### ID3 信息增量决策树算法
```信息熵```或```熵```（entropy）是信息学中的一个概念，已经被广泛利用到现代物理和信息学领域。香浓提出了该理论。<br>
**熵**是描述一个系统内元素的混乱程度的概念，或者是描述一个信息系统内信息的杂乱程度的概念。一个信息系统内元素越混乱，信息越杂乱无章，**熵**就越大；反之，一个系统内的元素越有条理，**熵**就越小。<br>