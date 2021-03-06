# 第二次上课总结
## 通俗地理解三大概念
### 反向传播与梯度下降的工作原理
1. 初始化
2. 正向计算
3. 损失函数为我们提供了计算损失的方法
4. 梯度下降是在损失函数基础上向着损失最小的点靠近而指引了网络权重调整的方向
5. 反向传播把损失值反向传给神经网络的每一层，让每一层都根据损失值反向调整权重
6. goto 2，直到精度足够好（比如损失函数值小于0.001）
#### 直接用z的表达式计算对w的偏导数
根据公式1、2、3，我们有：

$$z=x \cdot y=(2w+3b)(2b+1)=4wb+2w+6b^2+3b \tag{5}$$

对上式求w的偏导：

$$
{\partial z \over \partial w}=4b+2=4 \cdot 4 + 2=18 \tag{6}
$$
通过直接用Z的表达式
### 非线性反向传播
在上面的线性例子中，我们可以发现，误差一次性地传递给了初始值w和b，即，只经过一步，直接修改w和b的值，就能做到误差校正。因为从它的计算图看，无论中间计算过程有多么复杂，它都是线性的，所以可以一次传到底。缺点是这种线性的组合最多只能解决线性问题，不能解决更复杂的问题。这个我们在神经网络基本原理中已经阐述过了，需要有激活函数连接两个线性单元。
### 梯度下降
#### 从自然现象中理解梯度下降
在自然界中，梯度下降的最好例子，就是泉水下山的过程：
1. 水受重力影响，会在当前位置，沿着最陡峭的方向流动，有时会形成瀑布（梯度下降）
2. 水流下山的路径不是唯一的，在同一个地点，有可能有多个位置具有同样的陡峭程度，而造成了分流（可以得到多个解）
3. 遇到坑洼地区，有可能形成湖泊，而终止下山过程（不能得到全局最优解，而是局部最优解）
### 梯度下降的数学理解
先抛开神经网络，损失函数，反向传播等内容，用数学概念理解一下梯度下降。

梯度下降的数学公式：

$$\theta_{n+1} = \theta_{n} - \eta \cdot \nabla J(\theta) \tag{1}$$

其中：
- $\theta_{n+1}$：下一个值
- $\theta_n$：当前值
- $-$：梯度的反向
- $\eta$：学习率或步长，控制每一步走的距离，不要太快以免错过了最佳景点，不要太慢以免时间太长
- $\nabla$：梯度，函数当前位置的最快上升点
- $J(\theta)$：函数

#### 梯度下降的三要素

1. 当前点
2. 方向
3. 步长
### 损失函数
#### 概念
在各种材料中经常看到的中英文词汇有：误差，偏差，Error，Cost，Loss，损失，代价......意思都差不多，在本系列文章中，使用损失函数和Loss Function这两个词汇，具体的损失函数符号用J()来表示，误差值用loss表示。

**损失**就是所有样本的**误差**的总和，亦即：
$$损失 = \sum^m_{i=1}误差_i$$
$$J = \sum_{i=1}^m loss$$


在黑盒子的例子中，我们如果说“某个样本的损失”是不对的，只能说“某个样本的误差”，如果我们把神经网络的参数调整到完全满足一个样本的输出误差为0，通常会令其它样本的误差变得更大，这样作为误差之和的损失函数值，就会变得更大。所以，我们通常会在根据某个样本的误差调整权重后，计算一下整体样本的损失函数值，来判定网络是不是已经训练到了可接受的状态。

#### 损失函数的作用

损失函数的作用，就是计算神经网络每次迭代的前向计算结果与真实值的差距，从而指导下一步的训练向正确的方向进行。

如何使用损失函数呢？具体步骤：

1. 用随机值初始化前向计算公式的参数
2. 代入样本，计算输出的预测值
3. 用损失函数计算预测值和标签值（真实值）的误差
4. 根据损失函数的导数，沿梯度最小方向将误差回传，修正前向计算公式中的各个权重值
5. goto 2, 直到损失函数值达到一个满意的值就停止迭代
### 均方差函数
MSE - Mean Square Error。

该函数就是最直观的一个损失函数了，计算预测值和真实值之间的欧式距离。预测值和真实值越接近，两者的均方差就越小。

均方差函数常用于线性回归(linear regression)，即函数拟合(function fitting)。
### 交叉熵损失函数
交叉熵（Cross Entropy）是Shannon信息论中一个重要概念，主要用于度量两个概率分布间的差异性信息。在信息论中，交叉熵是表示两个概率分布p,q的差异，其中p表示真实分布，q表示非真实分布，那么H(p,q)就称为交叉熵：

$$H(p,q)=\sum_i p_i \cdot log {1 \over q_i} = - \sum_i p_i \log q_i$$

交叉熵可在神经网络中作为损失函数，p表示真实标记的分布，q则为训练后的模型的预测标记分布，交叉熵损失函数可以衡量p与q的相似性。

**交叉熵函数常用于逻辑回归(logistic regression)，也就是分类(classification)。**
### 3.2.4 为什么不能使用均方差做为分类问题的损失函数？

1. 回归问题通常用均方差损失函数，可以保证损失函数是个凸函数，即可以得到最优解。而分类问题如果用均方差的话，损失函数的表现不是凸函数，就很难得到最优解。而交叉熵函数可以保证区间内单调。

2. 分类问题的最后一层网络，需要分类函数，Sigmoid或者Softmax，如果再接均方差函数的话，其求导结果很复杂，运算量比较大。用交叉熵函数的话，可以得到比较简单的计算结果，一个简单的减法就可以得到反向误差。
## 总结与心得体会
通过本节课的学习，我知道了什么是反向传播和梯度下降，反向传播就是前向传递输入信号直至输出产生误差，反向传播误差信息更新权重矩阵。其根本就是求偏导以及高数中的链式法则。梯度下降是找损失函数极小值的一种方法，反向传播是求解梯度的一种方法。关于损失函数：在训练阶段，深度神经网络经过前向传播之后，得到的预测值与先前给出真实值之间存在差距。我们可以使用损失函数来体现这种差距。损失函数的作用可以理解为：当前向传播得到的预测值与真实值接近时，取较小值。反之取值增大。并且，损失函数应是以参数（w 权重, b 偏置）为自变量的函数。
![](\01.png)
### 反向传播的根本就是求偏导以及高数中的链式法则
![](\02.png)
还学习了训练神经网络，其中训练神经网络中的训练就是指通过输入大量训练数据，使得神经网络中的各种参数不断调整学习到一个合适的数值，训练神经网络采用梯度下降的方式，一点点的调整参数，找损失函数的极小值。本节课的学习，又加深了我对神经网络还有一系列算法的学习，让我对人工智能的行为学习产生了浓厚的兴趣，也为之后神经网络更加深层次的学习打下了坚实的基础。

