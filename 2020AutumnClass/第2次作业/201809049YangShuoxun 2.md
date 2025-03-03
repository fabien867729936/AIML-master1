# Step 2 预习

## 线性回归

线性回归是利用数理统计中回归分析，来确定两种或两种以上变量间相互依赖的定量关系的一种统计分析方法，运用十分广泛。其表达形式为y = w'x+e，e为误差服从均值为0的正态分布。

回归分析中，只包括一个自变量和一个因变量，且二者的关系可用一条直线近似表示，这种回归分析称为一元线性回归分析。如果回归分析中包括两个或两个以上的自变量，且因变量和自变量之间是线性关系，则称为多元线性回归分析。

### 单变量线性回归

#### 一元线性回归
最简单的情形是一元线性回归，由大体上有线性关系的一个自变量和一个因变量组成，模型是：

$$Y=a+bX+\varepsilon$$

$X$ 是自变量，$Y$ 是因变量，$\varepsilon$ 是随机误差，$a$ 和 $b$ 是参数，在线性回归模型中，$a,b$ 是我们要通过算法学习出来的。

对于线性回归模型，有如下一些概念需要了解：

- 通常假定随机误差 $\varepsilon$ 的均值为 $0$，方差为$σ^2$（$σ^2>0$，$σ^2$ 与 $X$ 的值无关）
- 若进一步假定随机误差遵从正态分布，就叫做正态线性模型
- 一般地，若有 $k$ 个自变量和 $1$ 个因变量（即公式1中的 $Y$），则因变量的值分为两部分：一部分由自变量影响，即表示为它的函数，函数形式已知且含有未知参数；另一部分由其他的未考虑因素和随机性影响，即随机误差
- 当函数为参数未知的线性函数时，称为线性回归分析模型
- 当函数为参数未知的非线性函数时，称为非线性回归分析模型
- 当自变量个数大于 $1$ 时称为多元回归
- 当因变量个数大于 $1$ 时称为多重回归

我们通过对数据的观察，可以大致认为它符合线性回归模型的条件，于是列出了公式1，不考虑随机误差的话，我们的任务就是找到合适的 $a,b$，这就是线性回归的任务。

会用几种方法来解决这个问题：

1. 最小二乘法：
   最小二乘法，也叫做最小平方法（Least Square），它通过最小化误差的平方和寻找数据的最佳函数匹配。利用最小二乘法可以简便地求得未知的数据，并使得这些求得的数据与实际数据之间误差的平方和为最小。

线性回归试图学得：

$$z_i=w \cdot x_i+b \tag{1}$$

使得：

$$z_i \simeq y_i \tag{2}$$

其中，$x_i$ 是样本特征值，$y_i$ 是样本标签值，$z_i$ 是模型预测值。

如何学得 $w$ 和 $b$ 呢？均方差(MSE - mean squared error)是回归任务中常用的手段：
$$
J = \frac{1}{2m}\sum_{i=1}^m(z_i-y_i)^2 = \frac{1}{2m}\sum_{i=1}^m(y_i-wx_i-b)^2 \tag{3}
$$

$J$ 称为损失函数。实际上就是试图找到一条直线，使所有样本到直线上的残差的平方和最小。


2. 梯度下降法：
    在下面的公式中，我们规定 $x$ 是样本特征值（单特征），$y$ 是样本标签值，$z$ 是预测值，下标 $i$ 表示其中一个样本。

    预设函数（Hypothesis Function）

    线性函数：

    $$z_i = x_i \cdot w + b \tag{1}$$

    损失函数（Loss Function）

    均方误差：

    $$loss_i(w,b) = \frac{1}{2} (z_i-y_i)^2 \tag{2}$$


    与最小二乘法比较可以看到，梯度下降法和最小二乘法的模型及损失函数是相同的，都是一个线性模型加均方差损失函数，模型用于拟合，损失函数用于评估效果。

    区别在于，最小二乘法从损失函数求导，直接求得数学解析解，而梯度下降以及后面的神经网络，都是利用导数传递误差，再通过迭代方式一步一步（用近似解）逼近真实解。

   
3. 简单的神经网络法：
    神经网络做线性拟合的原理，即：

- 初始化权重值
- 根据权重值放出一个解
- 根据均方差函数求误差
- 误差反向传播给线性计算部分以调整权重值
- 是否满足终止条件？不满足的话跳回2
  
  结构：
- 输入层

  此神经元在输入层只接受一个输入特征，经过参数 $w,b$ 的计算后，直接输出结果。这样一个简单的“网络”，只能解决简单的一元线性回归问题，而且由于是线性的，我们不需要定义激活函数，这就大大简化了程序，而且便于大家循序渐进地理解各种知识点。

  严格来说输入层在神经网络中并不能称为一个层。

- 权重 $w,b$

  因为是一元线性问题，所以 $w,b$ 都是标量。

- 输出层

  输出层 $1$ 个神经元，线性预测公式是：

  $$z_i = x_i \cdot w + b$$

  $z$ 是模型的预测输出，$y$ 是实际的样本标签值，下标 $i$ 为样本。

- 损失函数

  因为是线性回归问题，所以损失函数使用均方差函数。

  $$loss(w,b) = \frac{1}{2} (z_i-y_i)^2$$
  
4. 更通用的神经网络算法：
- 前向计算

  由于有多个样本同时计算，所以我们使用 $x_i$ 表示第 $i$ 个样本，$X$ 是样本组成的矩阵，$Z$ 是计算结果矩阵，$w$ 和 $b$ 都是标量：

  $$
  Z = X \cdot w + b \tag{1}
  $$

  把它展开成3个样本（3行，每行代表一个样本）的形式：

$$
X=\begin{pmatrix}
    x_1 \\\\ 
    x_2 \\\\ 
    x_3
\end{pmatrix}
$$

$$
Z= 
\begin{pmatrix}
    x_1 \\\\ 
    x_2 \\\\ 
    x_3
\end{pmatrix} \cdot w + b 
=\begin{pmatrix}
    x_1 \cdot w + b \\\\ 
    x_2 \cdot w + b \\\\ 
    x_3 \cdot w + b
\end{pmatrix}
=\begin{pmatrix}
    z_1 \\\\ 
    z_2 \\\\ 
    z_3
\end{pmatrix} \tag{2}
$$

$z_1,z_2,z_3$ 是三个样本的计算结果。
- 损失函数

用传统的均方差函数，其中，$z$ 是每一次迭代的预测输出，$y$ 是样本标签数据。我们使用 $m$ 个样本参与计算，因此损失函数为：

$$J(w,b) = \frac{1}{2m}\sum_{i=1}^{m}(z_i - y_i)^2$$

其中的分母中有个2，实际上是想在求导数时把这个2约掉，没有什么原则上的区别。

我们假设每次有3个样本参与计算，即 $m=3$，则损失函数实例化后的情形是：

$$
\begin{aligned}
J(w,b) &= \frac{1}{2\times3}[(z_1-y_1)^2+(z_2-y_2)^2+(z_3-y_3)^2] \\\\
&=\frac{1}{2\times3}\sum_{i=1}^3[(z_i-y_i)^2]
\end{aligned} 
$$

#### 多变量线性回归
建立多元线性回归模型时，为了保证回归模型具有优良的解释能力和预测效果，应首先注意自变量的选择，其准则是：

1. 自变量对因变量必须有显著的影响，并呈密切的线性相关；
2. 自变量与因变量之间的线性相关必须是真实的，而不是形式上的；
3. 自变量之间应具有一定的互斥性，即自变量之间的相关程度不应高于自变量与因变量之因的相关程度；
4. 自变量应具有完整的统计数据，其预测值容易确定。
   
两种方法的比较

|方法|正规方程|梯度下降|
|---|-----|-----|
|原理|几次矩阵运算|多次迭代|
|特殊要求|$X^{\top}X$ 的逆矩阵存在|需要确定学习率|
|复杂度|$O(n^3)$|$O(n^2)$|
|适用样本数|$m \lt 10000$|$m \ge 10000$|

- 正规方程法

  英文名是 Normal Equations。

对于线性回归问题，除了前面提到的最小二乘法可以解决一元线性回归的问题外，也可以解决多元线性回归问题。

对于多元线性回归，可以用正规方程来解决，也就是得到一个数学上的解析解。它可以解决下面这个公式描述的问题：

$$y=a_0+a_1x_1+a_2x_2+\dots+a_kx_k $$

- 神经网路法

  与单特征值的线性回归问题类似，多变量（多特征值）的线性回归可以被看做是一种高维空间的线性拟合。以具有两个特征的情况为例，这种线性拟合不再是用直线去拟合点，而是用平面去拟合点。

- 数据标准化（Normalization），又可以叫做数据归一化。

  - Min-Max标准化（离差标准化），将数据映射到 $[0,1]$ 区间

  $$x_{new}=\frac{x-x_{min}}{x_{max} - x_{min}} \tag{1}$$

  - 平均值标准化，将数据映射到[-1,1]区间
   
  $$x_{new} = \frac{x - \bar{x}}{x_{max} - x_{min}} \tag{2}$$

  - 对数转换
  $$x_{new}=\ln(x_i) \tag{3}$$

  - 反正切转换
  $$x_{new}=\frac{2}{\pi}\arctan(x_i) \tag{4}$$

  - Z-Score法

  把每个特征值中的所有数据，变成平均值为0，标准差为1的数据，最后为正态分布。Z-Score规范化（标准差标准化 / 零均值标准化，其中std是标准差）：

  $$x_{new} = \frac{x_i - \bar{x}}{std} \tag{5}$$

  - 中心化，平均值为0，无标准差要求
  
  $$x_{new} = x_i - \bar{x} \tag{6}$$

  - 比例法，要求数据全是正值

  $$
  x_{new} = \frac{x_k}{\sum_{i=1}^m{x_i}} \tag{7}
  $$

- 标准化标签值
  1. 样本不做标准化的话，网络发散，训练无法进行；
  2. 训练样本标准化后，网络训练可以得到结果，但是预测结果有问题；
  3. 还原参数值后，预测结果正确，但是此还原方法并不能普遍适用；
  4. 标准化测试样本，而不需要还原参数值，可以保证普遍适用；
  5. 标准化标签值，可以使得网络训练收敛快，但是在预测时需要把结果反标准化，以便得到真实值。

# Step 3 预习

## 线性分类

神经网络的一个重要功能就是分类，现实世界中的分类任务复杂多样，但万变不离其宗，我们都可以用同一种模式的神经网络来处理。

### 线性二分类
对率函数Logistic Function，即可以做为激活函数使用，又可以当作二分类函数使用。而在很多不太正规的文字材料中，把这两个概念混用了，比如下面这个说法：“我们在最后使用Sigmoid激活函数来做二分类”，这是不恰当的。在本书中，我们会根据不同的任务区分激活函数和分类函数这两个概念，在二分类任务中，叫做Logistic函数，而在作为激活函数时，叫做Sigmoid函数。

- Logistic函数公式

$$Logistic(z) = \frac{1}{1 + e^{-z}}$$

以下记 $a=Logistic(z)$。

- 导数

$$Logistic'(z) = a(1 - a)$$

具体求导过程可以参考8.1节。

- 输入值域

$$(-\infty, \infty)$$

- 输出值域

$$(0,1)$$

- 函数图像

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/8/logistic.png" ch="500" />

Logistic函数图像

- 使用方式

此函数实际上是一个概率计算，它把 $(-\infty, \infty)$ 之间的任何数字都压缩到 $(0,1)$ 之间，返回一个概率值，这个概率值接近 $1$ 时，认为是正例，否则认为是负例。

#### 正向传播
1. 损失函数满足二分类的要求，无论是正例还是反例，都是单调的；
2. 损失函数可导，以便于使用反向传播算法；
3. 计算过程非常简单，一个减法就可以搞定。
   
#### 对数几率
概率到对数几率的对照表

|概率$a$|0|0.1|0.2|0.3|0.4|0.5|0.6|0.7|0.8|0.9|1|
|--|--|--|--|--|--|--|--|--|--|--|--|
|反概率$1-a$|1|0.9|0.8|0.7|0.6|0.5|0.4|0.3|0.2|0.1|0|
|几率 $odds$ |0|0.11|0.25|0.43|0.67|1|1.5|2.33|4|9|$\infty$|
|对数几率 $\ln(odds)$|N/A|-2.19|-1.38|-0.84|-0.4|0|0.4|0.84|1.38|2.19|N/A|

优点如下：

- 把线性回归的成功经验引入到分类问题中，相当于对“分界线”的预测进行建模，而“分界线”在二维空间上是一条直线，这就不需要考虑具体样本的分布（比如在二维空间上的坐标位置），避免了假设分布不准确所带来的问题；
- 不仅预测出类别（0/1），而且得到了近似的概率值（比如0.31或0.86），这对许多需要利用概率辅助决策的任务很有用；
- 对率函数是任意阶可导的凸函数，有很好的数学性，许多数值优化算法都可以直接用于求取最优解。

#### 实现
- 输入层

  输入经度 $x_1$ 和纬度 $x_2$ 两个特征：

  $$
  X=\begin{pmatrix}
  x_{1} & x_{2}
  \end{pmatrix}
  $$

- 权重矩阵

  输入是2个特征，输出一个数，则 $W$ 的尺寸就是   $2\times 1$：

  $$
  W=\begin{pmatrix}
  w_{1} \\\\ w_{2}
  \end{pmatrix}
 $$

  $B$ 的尺寸是 $1\times 1$，行数永远是1，列数永远和   $W$ 一样。

  $$
  B=\begin{pmatrix}
  b
  \end{pmatrix}
  $$

- 输出层

  $$
  \begin{aligned}    
  z &= X \cdot W + B
  =\begin{pmatrix}
     x_1 & x_2
  \end{pmatrix}
  \begin{pmatrix}
    w_1 \\\\ w_2
  \end{pmatrix} + b \\\\
  &=x_1 \cdot w_1 + x_2 \cdot w_2 + b 
  \end{aligned}
  \tag{1}
  $$
  $$a = Logistic(z) \tag{2}$$

- 损失函数

  二分类交叉熵损失函数：

  $$
  loss(W,B) = -[y\ln a+(1-y)\ln(1-a)] \tag{3}
  $$

#### 工作原理
线性回归和线性分类的比较

||线性回归|线性分类|
|---|---|---|
|相同点|需要在样本群中找到一条直线|需要在样本群中找到一条直线|
|不同点|用直线来拟合所有样本，使得各个样本到这条直线的距离尽可能最短|用直线来分割所有样本，使得正例样本和负例样本尽可能分布在直线两侧|

可以看到线性回归中的目标--“距离最短”，还是很容易理解的，但是线性分类的目标--“分布在两侧”，用数学方式如何描述呢？我们可以有代数和几何两种方式来描述。

代数方式：通过一个分类函数计算所有样本点在经过线性变换后的概率值，使得正例样本的概率大于0.5，而负例样本的概率小于0.5。

用代数方式来解释其工作原理。

1. 正向计算

$$
z = x_1 w_1+ x_2 w_2 + b  \tag{1}
$$

2. 分类计算

$$
a={1 \over 1 + e^{-z}} \tag{2}
$$

3. 损失函数计算

$$
loss = -[y \ln (a)+(1-y) \ln (1-a)] \tag{3}
$$

#### 可视化
关键是对框架系统的理解，对运行机制和工作原理的理解。

### 线性多分类
多分类问题一共有三种解法：

1. 一对一方式
   
每次先只保留两个类别的数据，训练一个分类器。

2. 一对多方式
   
处理一个类别时，暂时把其它所有类别看作是一类，这样对于三分类问题，可以得到三个分类器。

3. 多对多方式

假设有4个类别ABCD，我们可以把AB算作一类，CD算作一类，训练一个分类器1；再把AC算作一类，BD算作一类，训练一个分类器2。

#### 多分类函数
- 取max值

  z值是 $[3,1,-3]$，如果取max操作会变成 $[1,0,0]$，这符合我们的分类需要，即三者相加为1，并且认为该样本属于第一类。但是有两个不足：

  1. 分类结果是 $[1,0,0]$，只保留的非0即1的信息，没有各元素之间相差多少的信息，可以理解是“Hard Max”；
  2. max操作本身不可导，无法用在反向传播中。

- 引入Softmax

  Softmax加了个"soft"来模拟max的行为，但同时又保留了相对大小的信息。

  $$
  a_j = \frac{e^{z_j}}{\sum\limits_{i=1}^m e^{z_i}}=\frac{e^{z_j}}{e^{z_1}+e^{z_2}+\dots+e^{z_m}}
  $$

  上式中:

  - $z_j$ 是对第 $j$ 项的分类原始值，即矩阵运算的结果
  - $z_i$ 是参与分类计算的每个类别的原始值
  - $m$ 是总分类数
  - $a_j$ 是对第 $j$ 项的计算结果

  假设 $j=1,m=3$，上式为：
  
  $$a_1=\frac{e^{z_1}}{e^{z_1}+e^{z_2}+e^{z_3}}$$

  用图来形象地说明这个过程。

  <img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/7/softmax.png" />

  图 Softmax工作过程

  当输入的数据$[z_1,z_2,z_3]$是$[3,1,-3]$时，按照图示过程进行计算，可以得出输出的概率分布是$[0.879,0.119,0.002]$。

  对比MAX运算和Softmax的不同，如表7-2所示。

  表 MAX运算和Softmax的不同

  |输入原始值|MAX计算|Softmax计算|
  |:---:|:---:|:---:|
  |$[3, 1, -3]$|$[1, 0, 0]$|$[0.879, 0.119, 0.002]$|

  也就是说，在（至少）有三个类别时，通过使用Softmax公式计算它们的输出，比较相对大小后，得出该样本属于第一类，因为第一类的值为0.879，在三者中最大。注意这是对一个样本的计算得出的数值，而不是三个样本，亦即Softmax给出了某个样本分别属于三个类别的概率。

  它有两个特点：

  1. 三个类别的概率相加为1
  2. 每个类别的概率都大于0
   
- 正向传播
  
  - 矩阵运算

  $$
  z=x \cdot w + b \tag{1}
  $$

  - 分类计算

  $$
  a_j = \frac{e^{z_j}}{\sum\limits_{i=1}^m e^{z_i}}=\frac{e^{z_j}}{e^{z_1}+e^{z_2}+\dots+e^{z_m}} \tag{2}
  $$

  - 损失函数计算

  计算单样本时，m是分类数：
  $$
  loss(w,b)=-\sum_{i=1}^m y_i \ln a_i \tag{3}
  $$

  计算多样本时，m是分类数，n是样本数：
  $$J(w,b) =- \sum_{j=1}^n \sum_{i=1}^m y_{ij} \log a_{ij} \tag{4}$$

- 反向传播
  - 一般性推导
  1. Softmax函数自身的求导
  2. 结合损失函数的整体反向传播公式
   
#### 多分类函数

- 取max值

  z值是 $[3,1,-3]$，如果取max操作会变成 $[1,0,0]$，这符合我们的分类需要，即三者相加为1，并且认为该样本属于第一类。但是有两个不足：

  1. 分类结果是 $[1,0,0]$，只保留的非0即1的信息，没有各元素之间相差多少的信息，可以理解是“Hard Max”；
  2. max操作本身不可导，无法用在反向传播中。

- 引入Softmax

  Softmax加了个"soft"来模拟max的行为，但同时又保留了相对大小的信息。

  $$
  a_j = \frac{e^{z_j}}{\sum\limits_{i=1}^m e^{z_i}}=\frac{e^{z_j}}{e^{z_1}+e^{z_2}+\dots+e^{z_m}}
  $$

  上式中:

  - $z_j$ 是对第 $j$ 项的分类原始值，即矩阵运算的结果
  - $z_i$ 是参与分类计算的每个类别的原始值
  - $m$ 是总分类数
  - $a_j$ 是对第 $j$ 项的计算结果

  假设 $j=1,m=3$，上式为：
  
  $$a_1=\frac{e^{z_1}}{e^{z_1}+e^{z_2}+e^{z_3}}$$

  用图来形象地说明这个过程。

  <img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/7/softmax.png" />

  图 Softmax工作过程

  当输入的数据$[z_1,z_2,z_3]$是$[3,1,-3]$时，按照图示过程进行计算，可以得出输出的概率分布是$[0.879,0.119,0.002]$。

  对比MAX运算和Softmax的不同，如表7-2所示。

  表 MAX运算和Softmax的不同

  |输入原始值|MAX计算|Softmax计算|
  |:---:|:---:|:---:|
  |$[3, 1, -3]$|$[1, 0, 0]$|$[0.879, 0.119, 0.002]$|

  也就是说，在（至少）有三个类别时，通过使用Softmax公式计算它们的输出，比较相对大小后，得出该样本属于第一类，因为第一类的值为0.879，在三者中最大。注意这是对一个样本的计算得出的数值，而不是三个样本，亦即Softmax给出了某个样本分别属于三个类别的概率。

  它有两个特点：

  1. 三个类别的概率相加为1
  2. 每个类别的概率都大于0

#### 实现
与前面的单层网络不同的是，输出层还多出来一个Softmax分类函数，这是多分类任务中的标准配置，可以看作是输出层的激活函数，并不单独成为一层，与二分类中的Logistic函数一样。

- 输入层

  输入经度 $x_1$ 和纬度 $x_2$ 两个特征：

$$
x=\begin{pmatrix}
x_1 & x_2
\end{pmatrix}
$$

- 权重矩阵

  $W$权重矩阵的尺寸，可以从前往后看，比如：输入层是2个特征，输出层是3个神经元，则$W$的尺寸就是 $2\times 3$。

  $$
  W=\begin{pmatrix}
  w_{11} & w_{12} & w_{13}\\\\
  w_{21} & w_{22} & w_{23} 
  \end{pmatrix}
  $$

  $B$的尺寸是1x3，列数永远和神经元的数量一样，行数永远是1。

  $$
  B=\begin{pmatrix}
  b_1 & b_2 & b_3 
  \end{pmatrix}
  $$

- 输出层

  输出层三个神经元，再加上一个Softmax计算，最后有$A1,A2,A3$三个输出，写作：

  $$
  Z = \begin{pmatrix}z_1 & z_2 & z_3 \end{pmatrix}
  $$
  $$
  A = \begin{pmatrix}a_1 & a_2 & a_3 \end{pmatrix}
  $$

  其中，$Z=X \cdot W+B，A = Softmax(Z)$

- 工作原理
  - 过程
    我们在此以具有两个特征值的三分类举例。可以扩展到更多的分类或任意特征值，比如在ImageNet的图像分类任务中，最后一层全连接层输出给分类器的特征值有成千上万个，分类有1000个。

  1. 线性计算

  $$z_1 = x_1 w_{11} + x_2 w_{21} + b_1 \tag{1}$$
  $$z_2 = x_1 w_{12} + x_2 w_{22} + b_2 \tag{2}$$
  $$z_3 = x_1 w_{13} + x_2 w_{23} + b_3 \tag{3}$$

  2. 分类计算

  $$
  a_1=\frac{e^{z_1}}{\sum_i e^{z_i}}=\frac{e^{z_1}}{e^{z_1}+e^{z_2}+e^{z_3}}  \tag{4}
  $$
  $$
  a_2=\frac{e^{z_2}}{\sum_i e^{z_i}}=\frac{e^{z_2}}{e^{z_1}+e^{z_2}+e^{z_3}}  \tag{5}
  $$
  $$
  a_3=\frac{e^{z_3}}{\sum_i e^{z_i}}=\frac{e^{z_3}}{e^{z_1}+e^{z_2}+e^{z_3}}  \tag{6}
  $$

  3. 损失函数计算

  单样本时，$n$表示类别数，$j$表示类别序号：

  $$
  \begin{aligned}
  loss(w,b)&=-(y_1 \ln a_1 + y_2 \ln a_2 + y_3 \ln a_3) \\\\
  &=-\sum_{j=1}^{n} y_j \ln a_j 
  \end{aligned}
  \tag{7}
  $$

  批量样本时，$m$ 表示样本数，$i$ 表示样本序号：

  $$
  \begin{aligned}
  J(w,b) &=- \sum_{i=1}^m (y_{i1} \ln a_{i1} + y_{i2} \ln a_{i2} + y_{i3} \ln a_{i3}) \\\\
  &=- \sum_{i=1}^m \sum_{j=1}^n y_{ij} \ln a_{ij}
  \end{aligned}
   \tag{8}
  $$

  损失函数计算在交叉熵函数一节有详细介绍。

- 结果可视化
  训练一对多分类器时，是把蓝色样本当作一类，把红色和绿色样本混在一起当作另外一类。训练一对一分类器时，是把绿色样本扔掉，只考虑蓝色样本和红色样本。而我们在此并没有这样做，三类样本是同时参与训练的。所以我们只能说神经网络从结果上看，是一种一对多的方式。
