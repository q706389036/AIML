# 第1次作业
本次课学习了神经网络的基本工作原理，并在学习神经网络中知道了一些用到的基本数学导数公式，然后还学习到了线性和非线性传播。
## 一.神经网络
### 基本概念
人工神经网络（Artificial Neural Networks，简写为ANNs）也简称为神经网络（NNs）或称作连接模型（Connection Model），它是一种模仿动物神经网络行为特征，进行分布式并行信息处理的算法数学模型。这种网络依靠系统的复杂程度，通过调整内部大量节点之间相互连接的关系，从而达到处理信息的目的。
### 基本工作原理
#### 神经网络模型
#### 神经元细胞的数学模型
![](picture/NeuranCell.PNG)

神经网络由基本的神经元组成。
#### 输入 input
$(x_1,x_2,...x_n)$ 是外界输入信号
#### 权重 weights
$(w_1,w_2,...w_n)$ 是每个输入信号的权重值
#### 偏移 bias
$y=wx+b$，$b$ 是偏移值，使得直线能够沿 $Y$ 轴上下移动。
#### 求和计算 sum
Z=w1*x1+w2*x*+...wn*xn+b
#### 激活函数 activation
求和之后，神经细胞已经处于兴奋状态了，已经决定要向下一个神经元传递信号了，但是要传递多强烈的信号，要由激活函数来确定

#### 小结

- 一个神经元可以有多个输入。
- 一个神经元只能有一个输出，这个输出可以同时输入给多个神经元。
- 一个神经元的权重的数量和输入的数量一致。
- 一个神经元只有一个偏移。
- 权重和偏移有人为的初始值，在训练过程中被不断修改。
- 激活函数可以等于求和计算，即激活函数不是必须有的。
- 一层神经网络中的所有神经元的激活函数必须一致。

#### 特点

大规模并行处理，分布式存储，弹性拓扑，高度冗余和非线性运算。因而具有很髙的运算速度，很强的联想能力，很强的适应性，很强的容错能力和自组织能力。

## 二.反向传播

### 神经网络中的三大概念
这三大概念是：反向传播，梯度下降，损失函数。

#### 反向传播与梯度下降的基本工作原理
1. 初始化；
2. 正向计算；
3. 损失函数为我们提供了计算损失的方法；
4. 梯度下降是在损失函数基础上向着损失最小的点靠近而指引了网络权重调整的方向；
5. 反向传播把损失值反向传给神经网络的每一层，让每一层都根据损失值反向调整权重；
6. Go to 2，直到精度足够好

#### 1.基本函数及其导数

|公式序号|函数|导数|备注|
|---|---|---|---|
|1|$y=c$|$y'=0$|
|2|$y=x^a$|$y'=ax^{a-1}$|
|3|$y=log_ax$|$y'=\frac{1}{x}log_ae=\frac{1}{xlna}$|
|4|$y=lnx$|$y'=\frac{1}{x}$|
|5|$y=a^x$|$y'=a^xlna$|
|6|$y=e^x$|$y'=e^x$|
|7|$y=e^{-x}$|$y'=-e^{-x}$|
|8|$y=sin(x)$|$y'=cos(x)$|正弦函数|
|9|$y=cos(x)$|$y'=-sin(x)$|余弦函数|
|10|$y=arcsin(x)$|$y'=\frac{1}{\sqrt{1-x^2}}$| 反正弦函数 |
|11|$y=arccos(x)$|$y'=-\frac{1}{\sqrt{1-x^2}}$| 反余弦函数 |
|12|$y=arctan(x)$|$y'=\frac{1}{1+x^2}$| 反正切函数 |
|13|$y=sinh(x)=(e^x-e^{-x})/2$|$y'=cosh(x)$|双曲正弦函数 |
|14|$y=cosh(x)=(e^x+e^{-x})/2$|$y'=sinh(x)$|双曲余弦函数 |
|15|$y=tanh(x)=(e^x-e^{-x})/(e^x+e^{-x})$|$y'=sech^2(x)=1-tanh^2(x)$|双曲正切函数|

链式法则：若函数 u=ψ(t)，v=ϕ(t)在点t可导，z=f(u,v),有
$$\frac{\partial{z}}{\partial{t}}=\frac{\partial{z}}{\partial{u}} \cdot \frac{\partial{u}}{\partial{t}}+\frac{\partial{z}}{\partial{v}} \cdot \frac{\partial{v}}{\partial{t}}$$

### 梯度下降

梯度下降法的计算过程就是沿梯度下降的方向求解极小值（也可以沿梯度上升方向求解极大值）。

#### 梯度下降的数学公式：

$$\theta_{n+1} = \theta_{n} - \eta \cdot \nabla J(\theta) \tag{1}$$

#### 梯度下降的三要素

1. 当前点；
2. 方向；
3. 步长。

#### “梯度下降”包含了两层含义：

1. 梯度：函数当前位置的最快上升点；
2. 下降：与导数相反的方向，用数学语言描述就是那个减号。

### 单变量函数的梯度下降
单变量函数：

$$J(x) = x ^2$$
假设终止条件为 $J(x)<0.01$，迭代过程是：
```
x=0.480000, y=0.230400
x=0.192000, y=0.036864
x=0.076800, y=0.005898
x=0.030720, y=0.000944
```
上面的过程如图所示。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/2/gd_single_variable.png" ch="500" />

### 双变量的梯度下降

双变量函数：

$$J(x,y) = x^2 + \sin^2(y)$$
双变量梯度下降的迭代过程

|迭代次数|x|y|J(x,y)|
|---|---|---|---|
|1|3|1|9.708073|
|2|2.4|0.909070|6.382415|
|...|...|...|...|
|15|0.105553|0.063481|0.015166|
|16|0.084442|0.050819|0.009711|
图 在三维空间内的梯度下降过程

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images\Images\2\gd_double_variable2.png">|

### 学习率η的选择
表 不同学习率对迭代情况的影响

|学习率|迭代路线图|说明|
|---|---|---|
|1.0|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/2/gd100.png" width="500" height="150"/>|学习率太大，迭代的情况很糟糕，在一条水平线上跳来跳去，永远也不能下降。|
|0.8|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/2/gd080.png" width="400"/>|学习率大，会有这种左右跳跃的情况发生，这不利于神经网络的训练。|
|0.4|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/2/gd040.png" width="400"/>|学习率合适，损失值会从单侧下降，4步以后基本接近了理想值。|
|0.1|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/2/gd010.png" width="400"/>|学习率较小，损失值会从单侧下降，但下降速度非常慢，10步了还没有到达理想状态。|

## 损失函数
损失函数（loss function）是用来估量模型的预测值f(x)与真实值Y的不一致程度，它是一个非负实值函数,通常使用L(Y, f(x))来表示，损失函数越小，模型的鲁棒性就越好。损失函数是经验风险函数的核心部分，也是结构风险函数重要组成部分。

### 损失函数的作用
计算神经网络每次迭代的前向计算结果与真实值的差距，从而指导下一步的训练向正确的方向进行。

### 两种损失函数

#### 均方差函数
均方差函数常用于线性回归(linear regression)，即函数拟合(function fitting)。公式如下：

$$
loss = {1 \over 2}(z-y)^2 \tag{单样本}
$$

$$
J=\frac{1}{2m} \sum_{i=1}^m (z_i-y_i)^2 \tag{多样本}
$$

#### 交叉熵损失函数
用于度量两个概率分布间的差异性信息。在信息论中，交叉熵是表示两个概率分布 $p,q$ 的差异，其中 $p$ 表示真实分布，$q$ 表示预测分布，那么 $H(p,q)$ 就称为交叉熵：

$$H(p,q)=\sum_i p_i \cdot \ln {1 \over q_i} = - \sum_i p_i \ln q_i $$

## 慕课 商务数据分析 学习过程
第一单元主要学的是神经网络基础

### 第一部分：神经网路简介
简要介绍了神经网路的一些基础知识，并且讲了一下有关的历史。
### 第二部分：BP神经网路
BP神经网络具有任意复杂的模式分类能力和优良的多维函数映射能力，解决了简单感知器不能解决的异或(Exclusive OR，XOR)和一些其他问题。从结构上讲，BP网络具有输入层、隐藏层和输出层；从本质上讲，BP算法就是以网络误差平方为目标函数、采用梯度下降法来计算目标函数的最小值。
### 第三部分：银行客户流失预测
详细介绍了和银行客户流失预测有关的东西，以及相关的神经网络。

## 总结
通过这一部分学习对神经网路有了一定的了解，在反向传播部分又通过以前学习的相关函数知识有了一定的联系，通过导数来完成程序和设计。而在梯度下降部分，通过几个给出的程序，然后运行产生图片，形象的的展示了相关的函数图像。而损失函数感觉是更加复杂的一种应用函数，只能稍微的了解一点。总体来说，对什么是神经网路？神经网路有什么？以及神经网路的作用等知识有了一定的掌握。