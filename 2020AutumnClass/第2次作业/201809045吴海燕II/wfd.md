# 第二步 线性回归

## 摘要

用线性回归作为学习神经网络的起点，相对比较容易理解，在它的基础上，逐步学习一些新的知识点。

单层神经网络，可以完成一些线性工作，如拟合一条直线。

当这个神经元只接收一个输入时，为单变量线性回归。  

当接收多个变量输入时，为多变量线性回归，但可视化理解较难，一般我们用变量两两组对的方式来表现。

当变量多于一个时，两个变量的量纲和数值有可能差别很大，这种情况下，我们通常需要对样本特征数据做归一化，然后将数据给神经网络进行训练。

# 第4章 单入单出的单层神经网络-单变量线性回归

## 单变量线性回归问题

### 1. 提出问题
  在互联网建设初期，各大运营商需要解决的问题就是保证服务器所在的机房的温度常年保持在23摄氏度左右。在一个新建的机房里，如果计划部署346台服务器，我们如何配置空调的最大功率？
  > 最大功率越大，电源所能负载的设备就越多。功率越大转速越高，汽车的最高速度也越高。

  将自变量X称为样本特征值，因变量Y称为样本标签值。通过一些统计(样本数据)可以得出二维图像：

  <img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/Data.png"/>
图4-1 样本数据可视化  

该题为一元线性回归问题

可得思路：利用已有值，预测未知值。

### 2. 一元线性回归模型

回归分析是一种数学模型。当因变量和自变量为线性关系时，它是一种特殊的线性模型。

最简单的情形是一元线性回归，由大体上有线性关系的一些自变量和一个因变量组成，模型是：
 $$Y = a + bX +\varepsilon   \tag{1} $$

$X$是自变量，Y是因变量，$\varepsilon$是随机误差，a和b是参数，在线性回归模型中，a,b是我们要通过算法学习出来的。

 对于线性回归模型，有如下一些概念需要了解：

- 通常假定随机误差 $\varepsilon$ 的均值为 $0$，方差为$σ^2$（$σ^2>0$，$σ^2$ 与 $X$ 的值无关）
- 若进一步假定随机误差遵从正态分布，就叫做正态线性模型
- 一般地，若有 $k$ 个自变量和 $1$ 个因变量（即公式1中的 $Y$），则因变量的值分为两部分：一部分由自变量影响，即表示为它的函数，函数形式已知且含有未知参数；另一部分由其他的未考虑因素和随机性影响，即随机误差
- 当函数为参数未知的线性函数时，称为线性回归分析模型
- 当函数为参数未知的非线性函数时，称为非线性回归分析模型
- 当自变量个数大于 $1$ 时称为多元回归
- 当因变量个数大于 $1$ 时称为多重回归

**在本章中我们会用几种方法来解决线性回归问题**
1. 最小二乘法；
2. 梯度下降法；
3. 简单的神经网络法；
4. 更通用的神经网络算法。
   
### 3. 公式形态
   $$Y = X \cdot W + B \tag{3}$$

   一般$X$为行向量，$W$为列变量比较符合阅读习惯；  

   对$B$来说，它永远是1行，列数与$W$的列数相等。  

   比如 $W$ 是 $3\times 1$ 的矩阵，则 $B$ 是 $1\times 1$ 的矩阵。如果 $W$ 是 $3\times 2$ 的矩阵，意味着3个特征输入到2个神经元上，则 $B$ 是 $1\times 2$ 的矩阵，每个神经元分配1个bias


## 4.1 最小二乘法

### 4.1.1 数学原理
线性回归试图学得：
   $$z_i = w \cdot x_i + b \tag{1}$$

使得：
   $$z_i \simeq y_i \tag{2}$$
其中，$x_i$是样本特征值，$y_i$是样本标签值，$z_i$是模型预测值。

如何学得$w$和$b$呢？均方差(MSE - mean squared error)是回归任务中常用的手段：
$$ J = \frac{1}{2m} \sum_{i=1}^m (z_i-y_i)^2 = \frac{1}{2m} \sum_{i=1}^m (y_i-wx_i-b)^2  \tag{3}$$

J称为损失函数。实际上就是试图找到一条直线，使所有样本到直线上的残差的平方和最小。

如果想让误差的值最小，通过对w和b求导，再令导数为0（到达最小极值），就是w和b的最优解。

推导过程如下：

$$
\begin{aligned}
\frac{\partial{J}}{\partial{w}} &=\frac{\partial{(\frac{1}{2m}\sum_{i=1}^m(y_i-wx_i-b)^2)}}{\partial{w}} \\\\
&= \frac{1}{m}\sum_{i=1}^m(y_i-wx_i-b)(-x_i) 
\end{aligned}
\tag{4}
$$

令公式4为 $0$：

$$
\sum_{i=1}^m(y_i-wx_i-b)x_i=0 \tag{5}
$$

$$
\begin{aligned}
\frac{\partial{J}}{\partial{b}} &=\frac{\partial{(\frac{1}{2m}\sum_{i=1}^m(y_i-wx_i-b)^2)}}{\partial{b}} \\\\
&=\frac{1}{m}\sum_{i=1}^m(y_i-wx_i-b)(-1) 
\end{aligned}
\tag{6}
$$

令公式6为 $0$：

$$
\sum_{i=1}^m(y_i-wx_i-b)=0 \tag{7}
$$

由式7得到（假设有 $m$ 个样本）：

$$
\sum_{i=1}^m b = m \cdot b = \sum_{i=1}^m{y_i} - w\sum_{i=1}^m{x_i} \tag{8}
$$

两边除以 $m$：

$$
b = \frac{1}{m}\left(\sum_{i=1}^m{y_i} - w\sum_{i=1}^m{x_i}\right)=\bar y-w \bar x \tag{9}
$$

其中：

$$
\bar y = \frac{1}{m}\sum_{i=1}^m y_i, \bar x=\frac{1}{m}\sum_{i=1}^m x_i \tag{10}
$$

将公式10代入公式5：

$$
\sum_{i=1}^m(y_i-wx_i-\bar y + w \bar x)x_i=0
$$

$$
\sum_{i=1}^m(x_i y_i-wx^2_i-x_i \bar y + w \bar x x_i)=0
$$

$$
\sum_{i=1}^m(x_iy_i-x_i \bar y)-w\sum_{i=1}^m(x^2_i - \bar x x_i) = 0
$$

$$
w = \frac{\sum_{i=1}^m(x_iy_i-x_i \bar y)}{\sum_{i=1}^m(x^2_i - \bar x x_i)} \tag{11}
$$

将公式10代入公式11：

$$
w = \frac{\sum_{i=1}^m (x_i \cdot y_i) - \sum_{i=1}^m x_i \cdot \frac{1}{m} \sum_{i=1}^m y_i}{\sum_{i=1}^m x^2_i - \sum_{i=1}^m x_i \cdot \frac{1}{m}\sum_{i=1}^m x_i} \tag{12}
$$

分子分母都乘以 $m$：

$$
w = \frac{m\sum_{i=1}^m x_i y_i - \sum_{i=1}^m x_i \sum_{i=1}^m y_i}{m\sum_{i=1}^m x^2_i - (\sum_{i=1}^m x_i)^2} \tag{13}
$$

$$
b= \frac{1}{m} \sum_{i=1}^m(y_i-wx_i) \tag{14}
$$

而事实上，式13有很多个变种，大家会在不同的文章里看到不同版本，往往感到困惑，比如下面两个公式也是正确的解：

$$
w = \frac{\sum_{i=1}^m y_i(x_i-\bar x)}{\sum_{i=1}^m x^2_i - (\sum_{i=1}^m x_i)^2/m} \tag{15}
$$

$$
w = \frac{\sum_{i=1}^m x_i(y_i-\bar y)}{\sum_{i=1}^m x^2_i - \bar x \sum_{i=1}^m x_i} \tag{16}
$$

以上两个公式，如果把公式10代入，也应该可以得到和式13相同的答案，只不过需要一些运算技巧。比如，很多人不知道这个神奇的公式：

$$
\begin{aligned}
\sum_{i=1}^m (x_i \bar y) &= \bar y \sum_{i=1}^m x_i =\frac{1}{m}(\sum_{i=1}^m y_i) (\sum_{i=1}^m x_i) \\\\
&=\frac{1}{m}(\sum_{i=1}^m x_i) (\sum_{i=1}^m y_i)= \bar x \sum_{i=1}^m y_i \\\\
&=\sum_{i=1}^m (y_i \bar x) 
\end{aligned}
\tag{17}
$$


### 4.1.2 代码实现
我们下面用python代码来实现一下以上的计算过程：  

**计算w值**
```
# 根据公式15
def method1(X,Y,m):
    x_mean = X.mean()
    p = sum(Y*(X-x_mean))
    q = sum(X*X) - sum(X)*sum(X)/m
    w = p/q
    return w

# 根据公式16
def method2(X,Y,m):
    x_mean = X.mean()
    y_mean = Y.mean()
    p = sum(X*(Y-y_mean))
    q = sum(X*X) - x_mean*sum(X)
    w = p/q
    return w

# 根据公式13
def method3(X,Y,m):
    p = m*sum(X*Y) - sum(X)*sum(Y)
    q = m*sum(X*X) - sum(X)*sum(X)
    w = p/q
    return w
```

**计算b值**
```
# 根据公式14
def calculate_b_1(X,Y,w,m):
    b = sum(Y-w*X)/m
    return b

# 根据公式9
def calculate_b_2(X,Y,w):
    b = Y.mean() - w * X.mean()
    return b
```
**运算结果**
```
w1=2.056827, b1=2.965434
w2=2.056827, b2=2.965434
w3=2.056827, b3=2.965434
```

## 4.2 梯度下降法
使用梯度下降法求解w和b
### 4.2.1 数学原理
在下面的公式中，我们规定x是样本特征值(单特征)，y是样本标签值，z是预测值，下标i表示其中一个样本。
#### 预设函数(Hypothesis Function)
线性函数：
   $$z_i = x_i \cdot w + b \tag{1}$$
损失函数(Loss Function)
均方误差：
   $$loss_i(w,b) = \frac{1}{2}(z_i-y_i)^2 \tag{2} $$

> 与最小二乘法比较可以看到，梯度下降法和最小二乘法的模型和损失函数是相同的，都是一个线性模型加均方差损失函数，模型用于拟合，损失函数用于评估效果。  

> 区别在于，最小二乘法从损失函数求导，直接求得数学解析解，而梯度下降以及后面的神经网络，都是利用导数传递误差，再通过迭代方式一步一步(用近似解)逼近真实解。

### 4.2.2 梯度计算
#### 计算z的梯度
根据公式2:
$$\frac{\partial loss}{\partial z_i}=z_i - y_i \tag{3} $$

####计算$w$的梯度
我们用 $loss$ 的值作为误差衡量标准，通过求 $w$ 对它的影响，也就是 $loss$ 对 $w$ 的偏导数，来得到 $w$ 的梯度。由于 $loss$ 是通过公式2->公式1间接地联系到 $w$ 的，所以我们使用链式求导法则，通过单个样本来求导。

根据公式1和公式3：

$$
\frac{\partial{loss}}{\partial{w}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{w}}=(z_i-y_i)x_i \tag{4}
$$

#### 计算b的梯度
$$
\frac{\partial{loss}}{\partial{b}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{b}}=z_i-y_i \tag{5}
$$


### 4.2.3 代码实现
```
if __name__ == '__main__':

    reader = SimpleDataReader()
    reader.ReadData()
    X,Y = reader.GetWholeTrainSamples()

    eta = 0.1
    w, b = 0.0, 0.0
    for i in range(reader.num_train):
        # get x and y value for one sample
        xi = X[i]
        yi = Y[i]
        # 公式1
        zi = xi * w + b
        # 公式3
        dz = zi - yi
        # 公式4
        dw = dz * xi
        # 公式5
        db = dz
        # update w,b
        w = w - eta * dw
        b = b - eta * db

    print("w=", w)    
    print("b=", b)
```

### 4.2.4 运行结果
```
w= [1.71629006]
b= [3.19684087]
```
这个结果和最小二乘法的结果(w=2.056827,bb=2.965434)相差比较多。


## 4.3 神经网络法
在梯度下降法中，我们简单讲述了一下神经网络做线性拟合的原理，即：
1. 初始化权重值
2. 根据权重值放出一个解
3. 根据均方差函数求误差
4. 误差反向传播给线性计算部分以调整权重值
5. 是否满足终止条件？不满足的华跳回2
   
### 4.3.1 定义神经网络结构
我们是首次尝试建立神经网络，先拿一个最简单的单层单点神经元为例，下面我们用最简单的线性回归的例子，来说明神经网络中的最重要的反向传播和梯度下降的概念、过程以及代码实现。

#### 输入层
此神经元在输入层只接受一个输入特征，经过参数w，b的计算后，直接输出结果。
#### 权重w,b
因为为一元线性问题，w,b都是标量。
#### 输出层
输出层线性预测公式为：
 $$z_i = x_i \cdot w +b$$
$z$是模型的预测输出，$y$是实际的样本标签值，下标i为样本。

#### 损失函数
因为是线性回归问题，所以损失函数使用均方差函数。
 $$loss(w,b) = \frac{1}{2} (z_i - y_i)^2$$

### 4.3.2 反向传播
#### 计算$w$的梯度
$$
{\partial{loss} \over \partial{w}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{w}}=(z_i-y_i)x_i
$$

#### 计算 $b$ 的梯度

$$
\frac{\partial{loss}}{\partial{b}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{b}}=z_i-y_i
$$

### 4.3.3 代码实现

其实神经网络法和梯度下降法在本质上是一样的，只不过神经网络法使用一个崭新的编程模型，即以神经元为中心的代码结构设计，这样便于以后的功能扩充。

在`Python`中可以使用面向对象的技术，通过创建一个类来描述神经网络的属性和行为，下面我们将会创建一个叫做`NeuralNet`的`class`，然后通过逐步向此类中添加方法，来实现神经网络的训练和推理过程。

#### 定义类

```Python
class NeuralNet(object):
    def __init__(self, eta):
        self.eta = eta
        self.w = 0
        self.b = 0
```
`NeuralNet`类从`object`类派生，并具有初始化函数，其参数是`eta`，也就是学习率，需要调用者指定。另外两个成员变量是`w`和`b`，初始化为`0`。

#### 前向计算

```Python
    def __forward(self, x):
        z = x * self.w + self.b
        return z
```
这是一个私有方法，所以前面有两个下划线，只在`NeuralNet`类中被调用，不对外公开。

#### 反向传播

下面的代码是通过梯度下降法中的公式推导而得的，也设计成私有方法：

```Python
    def __backward(self, x,y,z):
        dz = z - y
        db = dz
        dw = x * dz
        return dw, db
```
`dz`是中间变量，避免重复计算。`dz`又可以写成`delta_Z`，是当前层神经网络的反向误差输入。

#### 梯度更新

```Python
    def __update(self, dw, db):
        self.w = self.w - self.eta * dw
        self.b = self.b - self.eta * db
```

每次更新好新的`w`和`b`的值以后，直接存储在成员变量中，方便下次迭代时直接使用，不需要在全局范围当作参数内传来传去的。

#### 训练过程

只训练一轮的算法是：

***

`for` 循环，直到所有样本数据使用完毕：

1. 读取一个样本数据
2. 前向计算
3. 反向传播
4. 更新梯度

***

```Python
    def train(self, dataReader):
        for i in range(dataReader.num_train):
            # get x and y value for one sample
            x,y = dataReader.GetSingleTrainSample(i)
            # get z from x,y
            z = self.__forward(x)
            # calculate gradient of w and b
            dw, db = self.__backward(x, y, z)
            # update w,b
            self.__update(dw, db)
        # end for
```

#### 推理预测

```Python
    def inference(self, x):
        return self.__forward(x)
```

推理过程，实际上就是一个前向计算过程，我们把它单独拿出来，方便对外接口的设计。

#### 主程序

```Python
if __name__ == '__main__':
    # read data
    sdr = SimpleDataReader()
    sdr.ReadData()
    # create net
    eta = 0.1
    net = NeuralNet(eta)
    net.train(sdr)
    # result
    print("w=%f,b=%f" %(net.w, net.b))
    # predication
    result = net.inference(0.346)
    print("result=", result)
    ShowResult(net, sdr)
```

### 4.3.4 运行结果可视化

打印输出结果：

```
w=1.716290,b=3.196841
result= [3.79067723]
```

在上面的函数中，先获得所有样本点数据，把它们绘制出来。然后在 $[0,1]$ 之间等距设定 $10$ 个点做为 $x$ 值，用 $x$ 值通过网络推理方法`net.inference()`获得每个点的 $y$ 值，最后把这些点连起来，就可以画相应的拟合直线。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/result.png" ch="500" />

图4-5 拟合效果


### 4.3.5 工作原理

线性回归的原理就是先假设样本点是呈线性分布的。

在梯度下降法中，首先假设这个问题是个线性问题，因而有了公式 $z=xw+b$，用梯度下降的方式求解最佳的 $w,b$ 的值。

在反向传播中，用神经元的编程模型把梯度下降法包装了一下，这样就进入了神经网络的世界，从而可以有成熟的方法论可以解决更复杂的问题，比如多个神经元协同工作、多层神经网络的协同工作等等。

如图4-5所示，神经网络的任务就是找到一根直线（注意我们首先假设这是线性问题），让该直线穿过样本点阵，并且所有样本点到该直线的距离的平方的和最小。

可以想象成每一个样本点都有一根橡皮筋连接到直线上，连接点距离该样本点最近，所有的橡皮筋形成一个合力，不断地调整该直线的位置。该合力具备两种调节方式：

1. 如果上方的拉力大一些，直线就会向上平移一些，这相当于调节 $b$ 值；
2. 如果侧方的拉力大一些，直线就会向侧方旋转一些，这相当于调节 $w$ 值。

直到该直线处于平衡位置时，也就是线性拟合的最佳位置了。

Q:如果样本点不是呈线性分布的，可以用直线拟合吗？

是可以的，只是最终的效果不太理想，误差可以做到在线性条件下的最小，但是误差值本身还是比较大的。比如一个半圆形的样本点阵，用直线拟合可以达到误差值最小为 $1.2$，如果用弧线去拟合，可以达到误差值最小为 $0.3$。

所以，当使用线性回归的效果不好时，即判断出一个问题不是线性问题时，我们会用后续的方法来解决。

## 4.4 多样本单特征值计算

在前面的代码中，我们一直使用单样本计算来实现神经网络的训练过程，但是单样本计算有一些缺点：

1. 前后两个相邻的样本很有可能会对反向传播产生相反的作用而互相抵消。
2. 在样本数据量大时，逐个计算会花费很长的时间。

如果使用多样本计算，就要涉及到矩阵运算了，而所有的深度学习框架，都对矩阵运算做了优化，会大幅提升运算速度。

下面我们来看看多样本运算会对代码实现有什么影响，假设我们一次用3个样本来参与计算，每个样本只有1个特征值。

### 4.4.1 前向计算

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

$z_1,z_2,z_3$ 是三个样本的计算结果。根据公式1和公式2，我们的前向计算`Python`代码可以写成：

```Python
    def __forwardBatch(self, batch_x):
        Z = np.dot(batch_x, self.w) + self.b
        return Z
```
`Python`中的矩阵乘法命名有些问题，`np.dot()`并不是矩阵点乘，而是矩阵叉乘。

### 4.4.2 损失函数

用传统的均方差函数，其中，$z$ 是每一次迭代的预测输出，$y$ 是样本标签数据。我们使用 $m$ 个样本参与计算，因此损失函数为：

$$J(w,b) = \frac{1}{2m}\sum_{i=1}^{m}(z_i - y_i)^2$$

其中的分母中有个2，实际上是想在求导数时把这个2约掉，没有什么原则上的区别。

我们假设每次有3个样本参与计算，即 $m=3$，则损失函数实例化后的情形是：

$$
\begin{aligned}
J(w,b) &= \frac{1}{2\times3}[(z_1-y_1)^2+(z_2-y_2)^2+(z_3-y_3)^2] \\\\
&=\frac{1}{2\times3}\sum_{i=1}^3[(z_i-y_i)^2]
\end{aligned} 
\tag{3}
$$

公式3中大写的 $Z$ 和 $Y$ 都是矩阵形式，用代码实现：

```Python
    def __checkLoss(self, dataReader):
        X,Y = dataReader.GetWholeTrainSamples()
        m = X.shape[0]
        Z = self.__forwardBatch(X)
        LOSS = (Z - Y)**2
        loss = LOSS.sum()/m/2
        return loss
```
`Python`中的矩阵减法运算，不需要对矩阵中的每个对应的元素单独做减法，而是整个矩阵相减即可。做求和运算时，也不需要自己写代码做遍历每个元素，而是简单地调用求和函数即可。

### 4.4.3 求 $w$ 的梯度

我们用 $J$ 的值作为基准，去求 $w$ 对它的影响，也就是 $J$ 对 $w$ 的偏导数，就可以得到 $w$ 的梯度了。从公式3看 $J$ 的计算过程，$z_1,z_2,z_3$都对它有贡献；再从公式2看 $z_1,z_2,z_3$ 的生成过程，都有 $w$ 的参与。所以，$J$ 对 $w$ 的偏导应该是这样的：

$$
\begin{aligned}
\frac{\partial{J}}{\partial{w}}&=\frac{\partial{J}}{\partial{z_1}}\frac{\partial{z_1}}{\partial{w}}+\frac{\partial{J}}{\partial{z_2}}\frac{\partial{z_2}}{\partial{w}}+\frac{\partial{J}}{\partial{z_3}}\frac{\partial{z_3}}{\partial{w}} \\\\
&=\frac{1}{3}[(z_1-y_1)x_1+(z_2-y_2)x_2+(z_3-y_3)x_3] \\\\
&=\frac{1}{3}
\begin{pmatrix}
    x_1 & x_2 & x_3
\end{pmatrix}
\begin{pmatrix}
    z_1-y_1 \\\\
    z_2-y_2 \\\\
    z_3-y_3 
\end{pmatrix} \\\\
&=\frac{1}{m} \sum^m_{i=1} (z_i-y_i)x_i \\\\ 
&=\frac{1}{m} X^{\top} \cdot (Z-Y) \\\\ 
\end{aligned} \tag{4}
$$

其中：
$$X = 
\begin{pmatrix}
    x_1 \\\\ 
    x_2 \\\\ 
    x_3
\end{pmatrix}, X^{\top} =
\begin{pmatrix}
    x_1 & x_2 & x_3
\end{pmatrix}
$$

公式4中最后两个等式其实是等价的，只不过倒数第二个公式用求和方式计算每个样本，最后一个公式用矩阵方式做一次性计算。

### 4.4.4 求 $b$ 的梯度

$$
\begin{aligned}    
\frac{\partial{J}}{\partial{b}}&=\frac{\partial{J}}{\partial{z_1}}\frac{\partial{z_1}}{\partial{b}}+\frac{\partial{J}}{\partial{z_2}}\frac{\partial{z_2}}{\partial{b}}+\frac{\partial{J}}{\partial{z_3}}\frac{\partial{z_3}}{\partial{b}} \\\\
&=\frac{1}{3}[(z_1-y_1)+(z_2-y_2)+(z_3-y_3)] \\\\
&=\frac{1}{m} \sum^m_{i=1} (z_i-y_i) \\\\ 
&=\frac{1}{m}(Z-Y)
\end{aligned} \tag{5}
$$

公式5中最后两个等式也是等价的，在`Python`中，可以直接用最后一个公式求矩阵的和，免去了一个个计算 $z_i-y_i$ 最后再求和的麻烦，速度还快。

```Python
    def __backwardBatch(self, batch_x, batch_y, batch_z):
        m = batch_x.shape[0]
        dZ = batch_z - batch_y
        dW = np.dot(batch_x.T, dZ)/m
        dB = dZ.sum(axis=0, keepdims=True)/m
        return dW, dB
```


## 4.5 梯度下降的三种形式

我们比较一下目前我们用三种方法得到的 $w$ 和 $b$ 的值，见表4-2。

表4-2 三种方法的结果比较

|方法|$w$|$b$|
|----|----|----|
|最小二乘法|2.056827|2.965434|
|梯度下降法|1.71629006|3.19684087|
|神经网络法|1.71629006|3.19684087|

这个问题的原始值是可能是 $w=2,b=3$，由于样本噪音的存在，使用最小二乘法得到了 $2.05,2.96$ 这样的非整数解，这是完全可以接受的。但是使用梯度下降和神经网络两种方式，都得到 $1.71,3.19$ 这样的值，准确程度很低。从图4-6的神经网络的训练结果来看，拟合直线是斜着穿过样本点区域的，并没有在正中央的骨架上。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/result.png" ch="500" />

图4-6 拟合效果

最小二乘法可以得到数学解析解，所以它的结果是可信的。梯度下降法和神经网络法实际是一回事儿，只是梯度下降没有使用神经元模型而已。所以，接下来我们研究一下如何调整神经网络的训练过程，先从最简单的梯度下降的三种形式说起。

在下面的说明中，我们使用如下假设，以便简化问题易于理解：

1. 使用可以解决本章的问题的线性回归模型，即 $z=x \cdot w+b$；
2. 样本特征值数量为1，即 $x,w,b$ 都是标量；
3. 使用均方差损失函数。

计算 $w$ 的梯度：

$$
\frac{\partial{loss}}{\partial{w}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{w}}=(z_i-y_i)x_i
$$

计算 $b$ 的梯度：

$$
\frac{\partial{loss}}{\partial{b}} = \frac{\partial{loss}}{\partial{z_i}}\frac{\partial{z_i}}{\partial{b}}=z_i-y_i
$$

### 4.5.1 单样本随机梯度下降

SGD(Stochastic Gradient Descent)

样本访问示意图如图4-7所示。
  
<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/SingleSample-example.png" />

图4-7 单样本访问方式

#### 计算过程

假设一共100个样本，每次使用1个样本：

***

$repeat \\{ \\\\ $
$\quad for \quad i=1,2,3,...,100\{ \\\\ $
$\quad \quad z_i = x_i \cdot w + b\\\\ $
$\quad \quad dw= x_i \cdot (z_i - y_i)\\\\ $
$\quad \quad db= z_i - y_i \\\\ $
$\quad \quad w=w-\eta \cdot dw \\\\ $
$\quad \quad db=b-\eta \cdot db \\\\ $
$\\}$

***

#### 特点
  
  - 训练样本：每次使用一个样本数据进行一次训练，更新一次梯度，重复以上过程。
  - 优点：训练开始时损失值下降很快，随机性大，找到最优解的可能性大。
  - 缺点：受单个样本的影响最大，损失函数值波动大，到后期徘徊不前，在最优解附近震荡。不能并行计算。

#### 运行结果

设置`batch_size=1`，即单样本方式：

```Python
if __name__ == '__main__':
    sdr = SimpleDataReader()
    sdr.ReadData()
    params = HyperParameters(1, 1, eta=0.1, max_epoch=100, batch_size=1, eps = 0.02)
    net = NeuralNet(params)
    net.train(sdr)
```    

表4-3 单样本方式的训练情况

|损失函数值|梯度下降过程|
|---|---|
|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/SingleSample-Loss.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/SingleSample-Trace.png"/>|

表4-3的左图，由于我们使用了限定的停止条件，即当损失函数值小于等于 $0.02$ 时停止训练，所以，单样本方式迭代了300次后达到了精度要求。

右图是 $w$ 和 $b$ 共同构成的损失函数等高线图。梯度下降时，开始收敛较快，稍微有些弯曲地向中央地带靠近。到后期波动较大，找不到准确的前进方向，曲折地达到中心附近。

### 4.5.2 小批量样本梯度下降

Mini-Batch Gradient Descent

样本访问示意图如图4-8所示。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/MiniBatch-example.png" />

图4-8 小批量样本访问方式

#### 计算过程

假设一共100个样本，每个小批量5个样本：

***

$repeat \\{ \\\\ $
$\quad for \quad i=1,6,11,...,96\{\\\\ $
$\quad \quad z_i = x_i \cdot w + b \\\\ $
$\quad \quad z_{i+1} = x_{i+1} \cdot w + b \\\\ $
$\quad \quad \dots \\\\ $
$\quad \quad z_{i+4} = x_{i+4} \cdot w + b \\\\ $
$\quad \quad dw= {1 \over 5}\sum_{k=i}^{i+4} x_k \cdot (z_k - y_k) \\\\ $
$\quad \quad db= {1 \over 5}\sum_{k=i}^{i+4} (z_k - y_k) \\\\ $
$\quad \quad w=w-\eta \cdot dw \\\\ $
$\quad \quad db=b-\eta \cdot db \\\\ $
$\\}$

***

上述算法中，循环体中的前5行分别计算了 $z_i, z_{i+1}, ..., z_{i+4}$，可以换成一次性的矩阵运算。

#### 特点

  - 训练样本：选择一小部分样本进行训练，更新一次梯度，然后再选取另外一小部分样本进行训练，再更新一次梯度。
  - 优点：不受单样本噪声影响，训练速度较快。
  - 缺点：batch size的数值选择很关键，会影响训练结果。


#### 运行结果

设置`batch_size=10`：

```Python
if __name__ == '__main__':
    sdr = SimpleDataReader()
    sdr.ReadData()
    params = HyperParameters(1, 1, eta=0.3, max_epoch=100, batch_size=10, eps = 0.02)
    net = NeuralNet(params)
    net.train(sdr)   
```

表4-4 小批量样本方式的训练情况

|损失函数值|梯度下降过程|
|---|---|
|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/MiniBatch-Loss.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/MiniBatch-Trace.png"/>|

表4-4的右图，梯度下降时，在接近中心时有小波动。图太小看不清楚，可以用matplot工具放大局部来观察。和单样本方式比较，在中心区的波动已经缓解了很多。

小批量的大小通常由以下几个因素决定：

- 更大的批量会计算更精确的梯度，但是回报却是小于线性的。
- 极小批量通常难以充分利用多核架构。这决定了最小批量的数值，低于这个值的小批量处理不会减少计算时间。
- 如果批量处理中的所有样本可以并行地处理，那么内存消耗和批量大小成正比。对于多硬件设施，这是批量大小的限制因素。
- 某些硬件上使用特定大小的数组时，运行时间会更少，尤其是GPU，通常使用2的幂数作为批量大小可以更快，如`32,64,128,256`，大模型时尝试用`16`。
- 可能是由于小批量在学习过程中加入了噪声，会带来一些正则化的效果。泛化误差通常在批量大小为1时最好。因为梯度估计的高方差，小批量使用较小的学习率，以保持稳定性，但是降低学习率会使迭代次数增加。

在实际工程中，我们通常使用小批量梯度下降形式。

### 4.5.3 全批量样本梯度下降 

Full Batch Gradient Descent

样本访问示意图如图4-9所示。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/FullBatch-example.png" />

图4-9  全批量样本访问方式

#### 计算过程

假设一共100个样本，每次使用全部样本：

***

$repeat \\{ \\\\ $
$\quad z_1 = x_1 \cdot w + b \\\\ $
$\quad z_2 = x_2 \cdot w + b \\\\ $
$\quad \dots \\\\ $
$\quad z_{100} = x_{100} \cdot w + b \\\\ $
$\quad dw= {1 \over 100}\sum_{i=1}^{100} x_i \cdot (z_i - y_i) \\\\ $
$\quad db= {1 \over 100}\sum_{i=1}^{100} (z_i - y_i) \\\\ $
$\quad w=w-\eta \cdot dw \\\\ $
$\quad db=b-\eta \cdot db \\\\ $
$\\}$

***

上述算法中，循环体中的前100行分别计算了 $z_1, z_2, ..., z_{100}$，可以换成一次性的矩阵运算。

#### 特点

  - 训练样本：每次使用全部数据集进行一次训练，更新一次梯度，重复以上过程。
  - 优点：受单个样本的影响最小，一次计算全体样本速度快，损失函数值没有波动，到达最优点平稳。方便并行计算。
  - 缺点：数据量较大时不能实现（内存限制），训练过程变慢。初始值不同，可能导致获得局部最优解，并非全局最优解。

#### 运行结果

```Python
if __name__ == '__main__':
    sdr = SimpleDataReader()
    sdr.ReadData()
    params = HyperParameters(1, 1, eta=0.5, max_epoch=1000, batch_size=-1, eps = 0.02)
    net = NeuralNet(params)
    net.train(sdr)
```

设置`batch_size=-1`，即是全批量的意思。

表4-5 全批量样本方式的训练情况

|损失函数值|梯度下降过程|
|---|---|
|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/FullBatch-Loss.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/FullBatch-Trace.png"/>|

表4-5中的右图，梯度下降时，在整个过程中只拐了一个弯儿，就直接到达了中心点。

### 4.5.4 三种方式的比较

表4-6 三种方式的比较

||单样本|小批量|全批量|
|---|---|---|---|
|梯度下降过程图解|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/SingleSample-Trace.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/MiniBatch-Trace.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/FullBatch-Trace.png"/>|
|批大小|1|10|100|
|学习率|0.1|0.3|0.5|
|迭代次数|304|110|60|
|epoch|3|10|60|
|结果|w=2.003, b=2.990|w=2.006, b=2.997|w=1.993, b=2.998|

表4-6比较了三种方式的结果，从结果看，都接近于 $w=2,b=3$ 的原始解。最后的可视化结果图如图4-10，可以看到直线已经处于样本点比较中间的位置。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/4/mbgd-result.png" ch="500" />

图4-10 较理想的拟合效果图

相关的概念：

- Batch Size：批大小，一次训练的样本数量。
- Iteration：迭代，一次正向 + 一次反向。
- Epoch：所有样本被使用了一次，叫做一个Epoch，中文的翻译比较杂乱，所以干脆就用原文比较清楚。

假设一共有样本1000个，batch size=20，则一个Epoch中，需要1000/20=50次Iteration才能训练完所有样本。


# 第5章 多入单出的单层神经网络 - 多变量线性回归

## 5.0 多变量线性回归问题

若用两个特征值作为 $x$ 和 $y$，用标签值作为 $z$，在 $xyz$ 坐标空间中展示如表5-2。

表5-2 样本在三维空间的可视化

|正向|侧向|
|---|---|
|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/data1.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/data2.png"/>|

从正向看，很像一块草坪，似乎是一个平面。再从侧向看，和第4章中的直线拟合数据很像。所以，对于这种三维的线性拟合，我们可以把它想象成为拟合一个平面，这个平面会位于这块“草坪”的中位，把“草坪”分割成上下两块更薄的“草坪”，最终使得所有样本点到这个平面的距离的平方和最小。

### 5.0.2 多元线性回归模型


通过图示，我们基本可以确定这个问题是个线性回归问题，而且是典型的多元线性回归，即包括两个或两个以上自变量的回归。多元线性回归的函数模型如下：

$$y=a_0+a_1x_1+a_2x_2+\dots+a_kx_k$$

具体化到房价预测问题，上面的公式可以简化成：

$$ 
z = x_1 \cdot w_1 + x_2 \cdot w_2 + b
$$

对于一般的应用问题，建立多元线性回归模型时，为了保证回归模型具有优良的解释能力和预测效果，应首先注意自变量的选择，其准则是：

1. 自变量对因变量必须有显著的影响，并呈密切的线性相关；
2. 自变量与因变量之间的线性相关必须是真实的，而不是形式上的；
3. 自变量之间应具有一定的互斥性，即自变量之间的相关程度不应高于自变量与因变量之因的相关程度；
4. 自变量应具有完整的统计数据，其预测值容易确定。

### 5.0.3 解决方案

如果用传统的数学方法解决这个问题，我们可以使用正规方程，从而可以得到数学解析解，然后再使用神经网络方式来求得近似解，从而比较两者的精度，再进一步调试神经网络的参数，达到学习的目的。

我们不妨先把两种方式在这里做一个对比，运行得到结果后，再回到这里来仔细体会表5-3中的比较项。

表5-3 两种方法的比较

|方法|正规方程|梯度下降|
|---|-----|-----|
|原理|几次矩阵运算|多次迭代|
|特殊要求|$X^{\top}X$ 的逆矩阵存在|需要确定学习率|
|复杂度|$O(n^3)$|$O(n^2)$|
|适用样本数|$m \lt 10000$|$m \ge 10000$|


## 5.1 正规方程解法

英文名是 Normal Equations。

对于线性回归问题，除了前面提到的最小二乘法可以解决一元线性回归的问题外，也可以解决多元线性回归问题。

对于多元线性回归，可以用正规方程来解决，也就是得到一个数学上的解析解。它可以解决下面这个公式描述的问题：

$$y=a_0+a_1x_1+a_2x_2+\dots+a_kx_k \tag{1}$$

### 5.1.1 简单的推导方法

在做函数拟合（回归）时，我们假设函数 $H$ 为：

$$H(w,b) = b + x_1 w_1+x_2 w_2+ \dots +x_n w_n \tag{2}$$

令 $b=w_0$，则：

$$H(W) = w_0 + x_1 \cdot w_1 + x_2 \cdot w_2 + \dots + x_n \cdot w_n\tag{3}$$

公式3中的 $x$ 是一个样本的 $n$ 个特征值，如果我们把 $m$ 个样本一起计算，将会得到下面这个矩阵：

$$H(W) = X \cdot W \tag{4}$$

公式5中的 $X$ 和 $W$ 的矩阵形状如下：

$$
X = 
\begin{pmatrix} 
1 & x_{1,1} & x_{1,2} & \dots & x_{1,n} \\\\
1 & x_{2,1} & x_{2,2} & \dots & x_{2,n} \\\\
\vdots & \vdots & \vdots & \ddots & \vdots \\\\
1 & x_{m,1} & x_{m,2} & \dots & x_{m,n}
\end{pmatrix} \tag{5}
$$

$$
W= \begin{pmatrix}
w_0 \\\\
w_1 \\\\
\vdots \\\\
 w_n
\end{pmatrix}  \tag{6}
$$

然后我们期望假设函数的输出与真实值一致，则有：

$$H(W) = X \cdot W = Y \tag{7}$$

其中，Y的形状如下：

$$
Y= \begin{pmatrix}
y_1 \\\\
y_2 \\\\
\vdots \\\\
y_m
\end{pmatrix}  \tag{8}
$$


直观上看，$W = Y/X$，但是这里三个值都是矩阵，而矩阵没有除法，所以需要得到 $X$ 的逆矩阵，用 $Y$ 乘以 $X$ 的逆矩阵即可。但是又会遇到一个问题，只有方阵才有逆矩阵，而 $X$ 不一定是方阵，所以要先把左侧变成方阵，就可能会有逆矩阵存在了。所以，先把等式两边同时乘以 $X$ 的转置矩阵，以便得到 $X$ 的方阵：

$$X^{\top} X W = X^{\top} Y \tag{9}$$

其中，$X^{\top}$ 是 $X$ 的转置矩阵，$ X^{\top}X$ 一定是个方阵，并且假设其存在逆矩阵，把它移到等式右侧来：

$$W = (X^{\top} X)^{-1}{X^{\top} Y} \tag{10}$$

至此可以求出 $W$ 的正规方程。

### 5.1.2 复杂的推导方法

我们仍然使用均方差损失函数（略去了系数$\frac{1}{2m}$）：

$$J(w,b) = \sum_{i=1}^m (z_i - y_i)^2 \tag{11}$$

把 $b$ 看作是一个恒等于 $1$ 的feature，并把 $Z=XW$ 计算公式带入，并变成矩阵形式：

$$J(W) = \sum_{i=1}^m \left(\sum_{j=0}^nx_{ij}w_j -y_i\right)^2=(XW - Y)^{\top} \cdot (XW - Y) \tag{12}$$

对 $W$ 求导，令导数为 $0$，可得到 $W$ 的最小值解：

$$
\begin{aligned}
\frac{\partial J(W)}{\partial W} &= \frac{\partial}{\partial W}[(XW - Y)^{\top} \cdot (XW - Y)] \\\\
&=\frac{\partial}{\partial W}[(W^{\top}X^{\top} - Y^{\top}) \cdot (XW - Y)] \\\\
&=\frac{\partial}{\partial W}[(W^{\top}X^{\top}XW -W^{\top}X^{\top}Y - Y^{\top}XW + Y^{\top}Y)] 
\end{aligned}
\tag{13}
$$

求导后（请参考矩阵/向量求导公式）：

第一项的结果是：$2X^{\top}XW$（分母布局，denominator layout）

第二项的结果是：$X^{\top}Y$（分母布局方式，denominator layout）

第三项的结果是：$X^{\top}Y$（分子布局方式，numerator layout，需要转置$Y^{\top}X$）

第四项的结果是：$0$

再令导数为 $0$：

$$
\frac{\partial J}{\partial W}=2X^{\top}XW - 2X^{\top}Y=0 \tag{14}
$$
$$
X^{\top}XW = X^{\top}Y \tag{15}
$$
$$
W=(X^{\top}X)^{-1}X^{\top}Y \tag{16}
$$

结论和公式10一样。

以上推导的基本公式可以参考第0章的公式60-69。

逆矩阵 $(X^{\top}X)^{-1}$ 可能不存在的原因是：

1. 特征值冗余，比如 $x_2=x^2_1$，即正方形的边长与面积的关系，不能作为两个特征同时存在；
2. 特征数量过多，比如特征数 $n$ 比样本数 $m$ 还要大。

以上两点在我们这个具体的例子中都不存在。

### 5.1.3 代码实现

我们把表5-1的样本数据带入方程内。根据公式(5)，我们应该建立如下的 $X,Y$ 矩阵：

$$
X = \begin{pmatrix} 
1 & 10.06 & 60 \\\\
1 & 15.47 & 74 \\\\
1 & 18.66 & 46 \\\\
1 & 5.20 & 77 \\\\
\vdots & \vdots & \vdots \\\\
\end{pmatrix} \tag{17}
$$

$$
Y= \begin{pmatrix}
302.86 \\\\
393.04 \\\\
270.67 \\\\
450.59 \\\\
\vdots \\\\
\end{pmatrix}  \tag{18}
$$

根据公式(10)：

$$W = (X^{\top} X)^{-1}{X^{\top} Y} \tag{19}$$

1. $X$ 是 $1000\times 3$ 的矩阵，$X$ 的转置是 $3\times 1000$，$X^{\top}X$ 生成 $3\times 3$ 的矩阵；
2. $(X^{\top}X)^{-1}$ 也是 $3\times 3$ 的矩阵；
3. 再乘以 $X^{\top}$，即 $3\times 3$ 的矩阵乘以 $3\times 1000$ 的矩阵，变成 $3\times 1000$ 的矩阵；
4. 再乘以 $Y$，$Y$是 $1000\times 1$ 的矩阵，所以最后变成 $3\times 1$ 的矩阵，成为 $W$ 的解，其中包括一个偏移值 $b$ 和两个权重值 $w$，3个值在一个向量里。

```Python
if __name__ == '__main__':
    reader = SimpleDataReader()
    reader.ReadData()
    X,Y = reader.GetWholeTrainSamples()
    num_example = X.shape[0]
    one = np.ones((num_example,1))
    x = np.column_stack((one, (X[0:num_example,:])))
    a = np.dot(x.T, x)
    # need to convert to matrix, because np.linalg.inv only works on matrix instead of array
    b = np.asmatrix(a)
    c = np.linalg.inv(b)
    d = np.dot(c, x.T)
    e = np.dot(d, Y)
    #print(e)
    b=e[0,0]
    w1=e[1,0]
    w2=e[2,0]
    print("w1=", w1)
    print("w2=", w2)
    print("b=", b)
    # inference
    z = w1 * 15 + w2 * 93 + b
    print("z=",z)
```

### 5.1.4 运行结果

```
w1= -2.0184092853092226
w2= 5.055333475112755
b= 46.235258613837644
z= 486.1051325196855
```

我们得到了两个权重值和一个偏移值，然后得到房价预测值 $z=486$ 万元。

至此，我们得到了解析解。我们可以用这个做为标准答案，去验证我们的神经网络的训练结果。


## 5.2 神经网络解法

与单特征值的线性回归问题类似，多变量（多特征值）的线性回归可以被看做是一种高维空间的线性拟合。以具有两个特征的情况为例，这种线性拟合不再是用直线去拟合点，而是用平面去拟合点。

### 5.2.1 定义神经网络结构

我们定义一个如图5-1所示的一层的神经网络，输入层为2或者更多，反正大于2了就没区别。这个一层的神经网络的特点是：

1. 没有中间层，只有输入项和输出层（输入项不算做一层）；
2. 输出层只有一个神经元；
3. 神经元有一个线性输出，不经过激活函数处理，即在下图中，经过 $\Sigma$ 求和得到 $Z$ 值之后，直接把 $Z$ 值输出。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/setup.png" ch="500" />

图5-1 多入单出的单层神经元结构

与之前的单个单层神经元相比，这次仅仅是多了一个输入，但却是质的变化，即，一个神经元可以同时接收多个输入，这是神经网络能够处理复杂逻辑的根本。

#### 输入层

单独看第一个样本是这样的：

$$
x_1 =
\begin{pmatrix}
x_{11} & x_{12}
\end{pmatrix} = 
\begin{pmatrix}
10.06 & 60
\end{pmatrix} 
$$

$$
y_1 = \begin{pmatrix} 302.86 \end{pmatrix}
$$

一共有1000个样本，每个样本2个特征值，X就是一个$1000 \times 2$的矩阵：

$$
X = 
\begin{pmatrix} 
x_1 \\\\ x_2 \\\\ \vdots \\\\ x_{1000}
\end{pmatrix} =
\begin{pmatrix} 
x_{1,1} & x_{1,2} \\\\
x_{2,1} & x_{2,2} \\\\
\vdots & \vdots \\\\
x_{1000,1} & x_{1000,2}
\end{pmatrix}
$$

$$
Y =
\begin{pmatrix}
y_1 \\\\ y_2 \\\\ \vdots \\\\ y_{1000}
\end{pmatrix}=
\begin{pmatrix}
302.86 \\\\ 393.04 \\\\ \vdots \\\\ 450.59
\end{pmatrix}
$$


$x_1$ 表示第一个样本，$x_{1,1}$ 表示第一个样本的一个特征值，$y_1$ 是第一个样本的标签值。

#### 权重 $W$ 和 $B$

由于输入层是两个特征，输出层是一个变量，所以 $W$ 的形状是 $2\times 1$，而 $B$ 的形状是 $1\times 1$。

$$
W=
\begin{pmatrix}
w_1 \\\\ w_2
\end{pmatrix}
$$

$$B=(b)$$

$B$ 是个单值，因为输出层只有一个神经元，所以只有一个bias，每个神经元对应一个bias，如果有多个神经元，它们都会有各自的b值。

#### 输出层

由于我们只想完成一个回归（拟合）任务，所以输出层只有一个神经元。由于是线性的，所以没有用激活函数。
$$
\begin{aligned}
Z&=
\begin{pmatrix}
  x_{11} & x_{12}
\end{pmatrix}
\begin{pmatrix}
  w_1 \\\\ w_2
\end{pmatrix}
+(b) \\\\
&=x_{11}w_1+x_{12}w_2+b
\end{aligned}
$$

写成矩阵形式：

$$Z = X\cdot W + B$$

#### 损失函数

因为是线性回归问题，所以损失函数使用均方差函数。

$$loss_i(W,B) = \frac{1}{2} (z_i-y_i)^2 \tag{1}$$

其中，$z_i$ 是样本预测值，$y_i$ 是样本的标签值。

### 5.2.2 反向传播

#### 单样本多特征计算

与上一章不同，本章中的前向计算是多特征值的公式：

$$\begin{aligned}
z_i &= x_{i1} \cdot w_1 + x_{i2} \cdot w_2 + b \\\\
&=\begin{pmatrix}
  x_{i1} & x_{i2}
\end{pmatrix}
\begin{pmatrix}
  w_1 \\\\
  w_2
\end{pmatrix}+b
\end{aligned} \tag{2}
$$

因为 $x$ 有两个特征值，对应的 $W$ 也有两个权重值。$x_{i1}$ 表示第 $i$ 个样本的第 $1$ 个特征值，所以无论是 $x$ 还是 $W$ 都是一个向量或者矩阵了，那么我们在反向传播方法中的梯度计算公式还有效吗？答案是肯定的，我们来一起做个简单推导。

由于 $W$ 被分成了 $w_1$ 和 $w_2$ 两部分，根据公式1和公式2，我们单独对它们求导：

$$
\frac{\partial loss_i}{\partial w_1}=\frac{\partial loss_i}{\partial z_i}\frac{\partial z_i}{\partial w_1}=(z_i-y_i) \cdot x_{i1} \tag{3}
$$
$$
\frac{\partial loss_i}{\partial w_2}=\frac{\partial loss_i}{\partial z_i}\frac{\partial z_i}{\partial w_2}=(z_i-y_i) \cdot x_{i2} \tag{4}
$$

求损失函数对 $W$ 矩阵的偏导是无法直接求的，所以要变成求各个 $W$ 的分量的偏导。由于 $W$ 的形状是：

$$
W=
\begin{pmatrix}
w_1 \\\\ w_2
\end{pmatrix}
$$

所以求 $loss_i$ 对 $W$ 的偏导，应该这样写：

$$
\begin{aligned}  
\frac{\partial loss_i}{\partial W}&=
\begin{pmatrix}
  \frac{\partial loss_i}{\partial w_1} \\\\
  \frac{\partial loss_i}{\partial w_2}
\end{pmatrix} 
=\begin{pmatrix}
  (z_i-y_i) \cdot x_{i1} \\\\
  (z_i-y_i) \cdot x_{i2}
\end{pmatrix}  \\\\
&=\begin{pmatrix}
  x_{i1} \\\\
  x_{i2}
\end{pmatrix}
(z_i-y_i) 
=\begin{pmatrix}
  x_{i1} & x_{i2}
\end{pmatrix}^{\top}(z_i-y_i) \\\\
&=x_i^{\top}(z_i-y_i)
\end{aligned} \tag{5}
$$

$$
\frac{\partial loss_i}{\partial B}=z_i-y_i \tag{6}
$$

#### 多样本多特征计算

当进行多样本计算时，我们用 $m=3$ 个样本做一个实例化推导：

$$
z_1 = x_{11}w_1+x_{12}w_2+b
$$

$$
z_2= x_{21}w_1+x_{22}w_2+b
$$

$$
z_3 = x_{31}w_1+x_{32}w_2+b
$$

$$
J(W,B) = \frac{1}{2 \times 3}[(z_1-y_1)^2+(z_2-y_2)^2+(z_3-y_3)^2]
$$

$$
\begin{aligned}  
\frac{\partial J}{\partial W}&=
\begin{pmatrix}
  \frac{\partial J}{\partial w_1} \\\\
  \frac{\partial J}{\partial w_2}
\end{pmatrix}
=\begin{pmatrix}
  \frac{\partial J}{\partial z_1}\frac{\partial z_1}{\partial w_1}+\frac{\partial J}{\partial z_2}\frac{\partial z_2}{\partial w_1}+\frac{\partial J}{\partial z_3}\frac{\partial z_3}{\partial w_1} \\\\
  \frac{\partial J}{\partial z_1}\frac{\partial z_1}{\partial w_2}+\frac{\partial J}{\partial z_2}\frac{\partial z_2}{\partial w_2}+\frac{\partial J}{\partial z_3}\frac{\partial z_3}{\partial w_2}  
\end{pmatrix}
\\\\
&=\begin{pmatrix}
  \frac{1}{3}(z_1-y_1)x_{11}+\frac{1}{3}(z_2-y_2)x_{21}+\frac{1}{3}(z_3-y_3)x_{31} \\\\
  \frac{1}{3}(z_1-y_1)x_{12}+\frac{1}{3}(z_2-y_2)x_{22}+\frac{1}{3}(z_3-y_3)x_{32}
\end{pmatrix}
\\\\
&=\frac{1}{3}
\begin{pmatrix}
  x_{11} & x_{21} & x_{31} \\\\
  x_{12} & x_{22} & x_{32}
\end{pmatrix}
\begin{pmatrix}
  z_1-y_1 \\\\
  z_2-y_2 \\\\
  z_3-y_3
\end{pmatrix}
\\\\
&=\frac{1}{3}
\begin{pmatrix}
  x_{11} & x_{12} \\\\
  x_{21} & x_{22} \\\\
  x_{31} & x_{32} 
\end{pmatrix}^{\top}
\begin{pmatrix}
  z_1-y_1 \\\\
  z_2-y_2 \\\\
  z_3-y_3
\end{pmatrix}
\\\\
&=\frac{1}{m}X^{\top}(Z-Y) 
\end{aligned}
\tag{7}
$$
注：3泛化为m。
$$
\frac{\partial J}{\partial B}=\frac{1}{m}(Z-Y) \tag{8}
$$

### 5.2.3 代码实现

公式6和第4.4节中的公式5一样，所以我们依然采用第四章中已经写好的`HelperClass`目录中的那些类，来表示我们的神经网络。虽然此次神经元多了一个输入，但是不用改代码就可以适应这种变化，因为在前向计算代码中，使用的是矩阵乘的方式，可以自动适应`x`的多个列的输入，只要对应的`w`的矩阵形状是正确的即可。

但是在初始化时，我们必须手动指定`x`和`W`的形状，如下面的代码所示：

```Python
if __name__ == '__main__':
    # net
    params = HyperParameters(2, 1, eta=0.1, max_epoch=100, batch_size=1, eps = 1e-5)
    net = NeuralNet(params)
    net.train(reader)
    # inference
    x1 = 15
    x2 = 93
    x = np.array([x1,x2]).reshape(1,2)
    print(net.inference(x))
```

在参数中，指定了学习率`0.1`，最大循环次数`100`轮，批大小`1`个样本，以及停止条件损失函数值`1e-5`。

在神经网络初始化时，指定了`input_size=2`，且`output_size=1`，即一个神经元可以接收两个输入，最后是一个输出。

最后的`inference`部分，是把两个条件（15公里，93平方米）代入，查看输出结果。

在下面的神经网络的初始化代码中，`W`的初始化是根据`input_size`和`output_size`的值进行的。

```Python
class NeuralNet(object):
    def __init__(self, params):
        self.params = params
        self.W = np.zeros((self.params.input_size, self.params.output_size))
        self.B = np.zeros((1, self.params.output_size))
```

#### 正向计算的代码

```Python
class NeuralNet(object):
    def __forwardBatch(self, batch_x):
        Z = np.dot(batch_x, self.W) + self.B
        return Z
```

#### 误差反向传播的代码

```Python
class NeuralNet(object):
    def __backwardBatch(self, batch_x, batch_y, batch_z):
        m = batch_x.shape[0]
        dZ = batch_z - batch_y
        dB = dZ.sum(axis=0, keepdims=True)/m
        dW = np.dot(batch_x.T, dZ)/m
        return dW, dB
```

### 5.2.4 运行结果

在Visual Studio 2017中，可以使用Ctrl+F5运行Level2的代码，但是，会遇到一个令人沮丧的打印输出：

```
epoch=0
NeuralNet.py:32: RuntimeWarning: invalid value encountered in subtract
  self.W = self.W - self.params.eta * dW
0 500 nan
epoch=1
1 500 nan
epoch=2
2 500 nan
epoch=3
3 500 nan
......
```

减法怎么会出问题？什么是`nan`？

`nan`的意思是数值异常，导致计算溢出了，出现了没有意义的数值。现在是每500个迭代监控一次，我们把监控频率调小一些，再试试看：

```
epoch=0
0 10 6.838664338516814e+66
0 20 2.665505502247752e+123
0 30 1.4244204612680962e+179
0 40 1.393993758296751e+237
0 50 2.997958629609441e+290
NeuralNet.py:76: RuntimeWarning: overflow encountered in square
  LOSS = (Z - Y)**2
0 60 inf
...
0 110 inf
NeuralNet.py:32: RuntimeWarning: invalid value encountered in subtract
  self.W = self.W - self.params.eta * dW
0 120 nan
0 130 nan
```

前10次迭代，损失函数值已经达到了`6.83e+66`，而且越往后运行值越大，最后终于溢出了。下面的损失函数历史记录也表明了这一过程。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/wrong_loss.png" ch="500" />

图5-2 训练过程中损失函数值的变化


## 5.3 样本特征数据标准化

数据标准化（Normalization），又可以叫做数据归一化。

### 5.3.1 发现问题的根源

数据标准化是深度学习的必要步骤之一，已经是大师们的必杀技能，也因此它很少被各种博客/文章所提及，以至于初学者们经常被坑。

根据5.0.1中对数据的初步统计，我们是不是也可以把公里数都除以100，而平米数都除以1000呢？这样可能也会得到 $[0,1]$ 之间的数字？公里数的取值范围是 $[2,22]$，除以100后变成了 $[0.02,0.22]$。平米数的取值范围是 $[40,120]$，除以1000后变成了 $[0.04,0.12]$。

对本例来说这样做肯定是可以正常工作的，但是下面我们要介绍一种更科学合理的做法。

### 5.3.2 为什么要做标准化

理论层面上，神经网络是以样本在事件中的统计分布概率为基础进行训练和预测的，所以它对样本数据的要求比较苛刻。具体说明如下：

1. 样本的各个特征的取值要符合概率分布，即 $[0,1]$。
2. 样本的度量单位要相同。我们并没有办法去比较1米和1公斤的区别，但是，如果我们知道了1米在整个样本中的大小比例，以及1公斤在整个样本中的大小比例，比如一个处于0.2的比例位置，另一个处于0.3的比例位置，就可以说这个样本的1米比1公斤要小。
3. 神经网络假设所有的输入输出数据都是标准差为1，均值为0，包括权重值的初始化，激活函数的选择，以及优化算法的设计。

4. 数值问题

    标准化可以避免一些不必要的数值问题。因为激活函数sigmoid/tanh的非线性区间大约在 $[-1.7，1.7]$。意味着要使神经元有效，线性计算输出的值的数量级应该在1（1.7所在的数量级）左右。这时如果输入较大，就意味着权值必须较小，一个较大，一个较小，两者相乘，就引起数值问题了。
    
5. 梯度更新
    
    若果输出层的数量级很大，会引起损失函数的数量级很大，这样做反向传播时的梯度也就很大，这时会给梯度的更新带来数值问题。
    
6. 学习率
   
    如果梯度非常大，学习率就必须非常小，因此，学习率（学习率初始值）的选择需要参考输入的范围，不如直接将数据标准化，这样学习率就不必再根据数据范围作调整。对 $w_1$ 适合的学习率，可能相对于 $w_2$ 来说会太小，若果使用适合 $w_1$ 的学习率，会导致在 $w_2$ 方向上步进非常慢，从而消耗非常多的时间；而使用适合 $w_2$ 的学习率，对 $w_1$ 来说又太大，搜索不到适合 $w_1$ 的解。
    
### 5.3.3 从损失函数等高线图分析标准化的必要性

在房价数据中，地理位置的取值范围是 $[2,20]$，而房屋面积的取值范围为 $[40,120]$，二者相差太远，放在一起计算会怎么样？

根据公式$z = x_1 w_1+x_2 w_2 + b$，神经网络想学习 $w_1$ 和 $w_2$，但是数值范围问题导致神经网络来说很难“理解”。图5-5展示了标准化前后的情况损失函数值的等高图，意思是地理位置和房屋面积取不同的值时，作为组合来计算损失函数值时，形成的类似地图的等高图，见图5-5，左侧为标准化前，右侧为标准化后。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/normalize.jpg" />

图5-5 标准化前后的损失函数等高线图的对比

房屋面积的取值范围是 $[40,120]$，而地理位置的取值范围是 $[2,20]$，二者会形成一个很扁的椭圆，如左侧。这样在寻找最优解的时候，过程会非常曲折。运气不好的话，根本就没法训练。

### 5.3.4 标准化的常用方法

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


### 5.3.5 如何做数据标准化

我们再看看样本的数据，表5-5。

表5-5 房价原始样本数据抽样

|样本序号|地理位置|居住面积|价格（万元）|
|---|---|---|---|
|1|10.06|60|302.86|
|2|15.47|74|393.04|
|3|18.66|46|270.67|
|4|5.20|77|450.59|
|...|...|...|...|

按照标准化的定义，我们只要把地理位置列和居住面积列分别做标准化就达到要求了，结果如表5-6。

表5-6 标准化后的样本数据

|样本序号|地理位置|居住面积|价格（万元）|
|---|---|---|---|
|1|0.4033|0.2531|302.86|
|2|0.6744|0.4303|393.04|
|3|0.8341|0.0759|270.67|
|4|0.1592|0.4683|450.59|
|...|...|...|...|

注意：

1. 我们并没有标准化样本的标签Y数据，所以最后一行的价格还是保持不变
2. 我们是对两列特征值分别做标准化处理的

### 5.3.6 代码实现

在`HelperClass`目录的`SimpleDataReader.py`文件中，给该类增加一个方法：

```Python
    def NormalizeX(self):
        ......
```

返回值`X_new`是标准化后的样本，和原始数据的形状一样。

再把主程序修改一下，在`ReadData()`方法后，紧接着调用`NormalizeX()`方法：

```Python
if __name__ == '__main__':
    # data
    reader = SimpleDataReader()
    reader.ReadData()
    reader.NormalizeX()
    ......
```

### 5.3.7 运行结果

运行上述代码，看打印结果：

```
epoch=9
9 0 391.75978721600353
9 100 387.79811202735783
......
9 800 380.78054509441193
9 900 575.5617634691969
W= [[-41.71417524]
 [395.84701164]]
B= [[242.15205099]]
z= [[37366.53336103]]
```

虽然损失函数值没有像我们想象的那样趋近于`0`，但是却稳定在了`400`左右震荡，这也算是收敛！看一下损失函数图像。

<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/correct_loss.png" ch="500" />

图5-6 训练过程中损失函数值的变化

再看看W和B的输出值和z的预测值：

```
w1 = -41.71417524
w2 = 395.84701164
b = 242.15205099
z = 37366.53336103
```

回忆一下正规方程的输出值：

```
w1= -2.0184092853092226
w2= 5.055333475112755
b= 46.235258613837644
z= 486.1051325196855
```

正规方程预测房价结果：

$$
\begin{aligned}
Z&= -2.018 \times 15 + 5.055 \times 93 + 46.235 \\\\
&= 486.105(万元)
\end{aligned}
$$

神经网络预测房价结果：

$$
\begin{aligned}
Z&= -14.714 \times 15 + 395.847 \times 93 + 242.152 \\\\
&= 37366(万元)
\end{aligned}
$$

好吧，我们遇到了天价房！这是怎么回事儿？难道和我们做数据标准化有关系？

### 5.3.8 工作原理

在5.0.1中，我们想象神经网络会寻找一个平面，来拟合这些空间中的样本点，是不是这样呢？我们通过下面的函数来实现这个可视化：

```Python
def ShowResult(net, reader):
    ......
``` 

前半部分代码先是把所有的点显示在三维空间中，我们曾经描述它们像一块厚厚的草坪。后半部分的代码在 $[0,1]$ 空间内形成了一个50乘50的网格，亦即有2500个点，这些点都是有横纵坐标的。然后把这些点送入神经网络中做预测，得到了2500个Z值，相当于第三维的坐标值。最后把这2500个三维空间的点，以网格状显示在空间中，就形成了表5-7的可视化的结果。

细心的读者可能会问两个问题：

1. 为什么要在[0,1]空间中形成50乘50的网格呢？
2. 50这个数字从哪里来的？

`NumPy`库的`np.linspace(0,1)`的含义，就是在 $[0,1]$ 空间中生成50个等距的点，第三个参数不指定时，缺省是50。因为我们前面对样本数据做过标准化，统一到了 $[0,1]$ 空间中，这就方便了我们对问题的分析，不用考虑每个特征值的实际范围是多大了。

表5-7 三维空间线性拟合的可视化

|正向|侧向|
|---|---|
|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/level3_result_1.png"/>|<img src="https://aiedugithub4a2.blob.core.windows.net/a2-images/Images/5/level3_result_2.png"/>|

从表5-7中的正向图可以看到，真的形成了一个平面；从侧向图可以看到，这个平面也确实穿过了那些样本点，并且把它们分成了上下两个部分。只不过由于训练精度的问题，没有做到平分，而是斜着穿过了点的区域，就好像第4.3节中的那个精度不够的线性回归的结果。

所以，这就印证了我们在4.3.5中的关于高维空间拟合的说法：在二维平面上是一条拟合直线，在三维空间上是一个拟合平面。每个样本点到这个平面上都有一个无形的作用力（不妨想象为引力），当作用力达到平衡时，也就是平面拟合的最佳位置。

这下子我们可以大致放心了，神经网络的训练结果并没有错，一定是别的地方出了什么问题。在下一节中我们来一起看看问题出在哪里！


## 5.4 还原参数值

### 5.4.1 对比结果

在上一节中，我们使用了如下超参进行神经网络的训练：

```Python
params = HyperParameters(eta=0.1, max_epoch=10, batch_size=1, eps = 1e-5)
```

我们再把每次checkpoint的`W`和`B`的值打印出来：

```
9 0 437.5399553941636 [[-35.46926435] [399.01136072]] [[252.69305588]]
9 100 420.78580862641473 [[-36.93198181] [400.03047293]] [[251.26503706]]
......
9 900 413.7210407763991 [[-36.67601742] [406.55322285]] [[246.8067483]]
```
打印结果中每列的含义按顺序是：`epoch`,`iteration`,`loss`,`w1`,`w2`,`b`。

可以看到`loss`,`w1`,`w2`,`b`的值，每次跳跃都很大，怀疑是学习率过高导致的梯度下降在最优解附近徘徊。所以，我们先把超参修改一下：

```Python
params = HyperParameters(eta=0.01, max_epoch=500, batch_size=10, eps=1e-5)
```

做了三处修改：

- 学习率缩小10倍，变成`0.01`
- `max_epoch`扩大50倍，从`10`变成`500`，让网络得到充分训练（但实际上不需要这么多次的循环，大家可以自己试验）
- `batch_size=10`，使用mini-batch批量样本训练，提高精度，减缓个别样本引起的跳跃程度

运行结果：

```
......
499 89 380.62299460835936 [[-40.2782923 ] [399.34224968]] [[244.14309928]]
499 99 380.5935045560184 [[-40.26440193] [399.39472352]] [[244.3928586]]
```

可以看到达到了我们的目的，`loss`,`w1`,`w2`,`b`的值都很稳定。我们使用这批结果做为分析基础。首先列出W和B的训练结果如表5-8，可以看到神经网络得出的值（绝对值）比正规方程的结果大不少。

表5-8 神经网络的训练结果与正规方程法的结果比较

|结果|w1|w2|b|
|---|---|---|---|
|正规方程|-2.018|5.055|46.235|
|神经网络|-40.2|399.3|244.5|
|比值|19.92|78.99|5.288|

再列出标准化后的数据，如表5-9。

表5-9 标准化后的特征值

|特征|地理位置|居住面积|
|----|----|---|
|最小值|2.02|40|
|最大值|21.96|119|
|差值|19.94|79|

通过对比我发现，关于`W`的结果，表5-8最后一行的数据（19.92，78.99），和表5-9最后一行的数据（19.94，79），有惊人的相似之处！这是为什么呢？

### 5.4.2 还原真实的 $W,B$ 值

我们唯一修改的地方，就是样本数据特征值的标准化，并没有修改标签值。可以大概猜到W的值和样本特征值的缩放有关系，而且缩放倍数非常相似，甚至可以说一致。下面推导一下这种现象的数学基础。

假设在标准化之前，真实的样本值是 $X$，真实的权重值是 $W$；在标准化之后，样本值变成了 $X'$，训练出来的权重值是 $W'$：

$$
y = x_1 w_1 + x_2 w_2 + b \tag{y是标签值}
$$

$$
z = x_1' w_1' + x_2' w_2' + b' \tag{z是预测值}
$$

由于训练时标签值（房价）并没有做标准化，意味着我们是用真实的房价做的训练，所以预测值和标签值应该相等，所以：
$$
y = z $$
$$
x_1 w_1 + x_2 w_2 + b = x_1' w_1' + x_2' w_2' + b' \tag{1}
$$

标准化的公式是：
$$
x' = \frac{x - x_{min}}{x_{max}-x_{min}} \tag{2}
$$

为了简化书写，我们令$xm=x_{max}-x_{min}$，把公式2代入公式1：

$$
x_1 w_1 + x_2 w_2 + b = \frac{x_1 - x_{1min}}{xm_1} w_1' + \frac{x_2 - x_{2min}}{xm_2} w_2' + b' \\
=x_1 \frac{w_1'}{xm_1} + x_2 \frac{w_2'}{xm_2}+b'-\frac{w_1'x_{1min}}{xm_1}-\frac{w_2'x_{2min}}{xm_2} 
\tag{3}
$$

公式3中，$x1,x2$ 是变量，其它都是常量，如果想让公式3等式成立，则变量项和常数项分别相等，即：

$$
w_1 = \frac{w_1'}{xm_1} \tag{4}
$$
$$
w_2 = \frac{w_2'}{xm_2} \tag{5}
$$
$$ 
b = b'-\frac{w_1'x_{1min}}{xm_1}-\frac{w_2'x_{2min}}{xm_2} \tag{6}
$$

下面我们用实际数值代入公式4,5,6：

$$
w_1 = \frac{w_1'}{x_{1max}-x_{1min}} = \frac{-40.2}{21.96-2.02} = -2.016 \tag{7}
$$
$$
w_2 = \frac{w_2'}{x_{2max}-x_{2min}} = \frac{399.3}{119-40} = 5.054 \tag{8}
$$
$$
b=244.5-(-2.016) \cdot 2.02 - 5.054 \cdot 40=46.412 \tag{9}
$$

可以看到公式7、8、9的计算结果（神经网络的训练结果的变换值）与正规方程的计算结果(-2.018, 5.055, 46.235)基本相同，基于神经网络是一种近似解的考虑，可以认为这种推导是有理论基础和试验证明的。

### 5.4.3 代码实现

下面的代码实现了公式4，5，6：

```Python
# get real weights and bias
def DeNormalizeWeightsBias(net, dataReader):
    W_real = np.zeros_like(net.W)
    X_Norm = dataReader.GetDataRange()
    for i in range(W_real.shape[0]):
        W_real[i,0] = net.W[i,0] / X_Norm[i,1]
        B_real = net.B - W_real[0,0]*X_Norm[0,0] - W_real[1,0]*X_Norm[1,0]
    return W_real, B_real
```

`X_Norm`是我们在做标准化时保留下来的样本的两个特征向量的最小值和数值范围（最大值减去最小值）。

修改主程序如下：

```Python
if __name__ == '__main__':
    ......
    net.train(reader, checkpoint=0.1)
    # inference
    W_real, B_real = DeNormalizeWeightsBias(net, reader)
    print("W_real=", W_real)
    print("B_real=", B_real)

    x1 = 15
    x2 = 93
    x = np.array([x1,x2]).reshape(1,2)
    z = np.dot(x, W_real) + B_real
    print("Z=", z)

    ShowResult(net, reader)
```
在`net.train()`方法返回之后，训练好的`W`和`B`的值就保存在`NeuralNet`类的属性里了。然后通过调用`DeNormalizeWeightsBias()`函数，把它们转换成真实的`W_real`和`B_real`值，就好比我们不做标准化而能训练出来的权重值一样。

最后在推理预测时，我们直接使用了`np.dot()`公式，而没有使用`net.inference()`方法，是因为在`net`实例中的`W`和`B`是还原前的值，做前向计算时还是会有问题，所以我们直接把前向计算公式拿出来，代入`W_real`和`B_real`，就可以得到真实的预测值了。

### 5.4.4 运行结果

运行上述代码，观察最后部分的打印输出：

```
......
499 99 380.5934686827507 
[[-40.23261123] [399.36389489]] [[244.39118797]]
W= [[-40.23261123]
 [399.36389489]]
B= [[244.39118797]]
W_real= [[-2.01737219]
 [ 5.05523918]]
B_real= [[46.26647363]]
Z= [[486.14313417]]
```

把结果与正规方程的结果对比一下，如表5-10。

表5-10 还原后的值与正规方程结果的比较

||w1|w2|b|
|---|---|---|---|
|正规方程|-2.018|5.055|46.235|
|神经网络|-2.017|5.055|46.266|


## 5.5 正确的推理预测方法

### 5.5.1 预测数据的标准化

在上一节中，我们在用训练出来的模型预测房屋价格之前，还需要先还原 $W$ 和 $B$ 的值，这看上去比较麻烦，下面我们来介绍一种正确的推理方法。

既然我们在训练时可以把样本数据标准化，那么在预测时，把预测数据也做相同方式的标准化，不是就可以和训练数据一样进行预测了吗？且慢！这里有一个问题，训练时的样本数据是批量的，至少是成百成千的数量级。但是预测时，一般只有一个或几个数据，如何做标准化？

我们在针对训练数据做标准化时，得到的最重要的数据是训练数据的最小值和最大值，我们只需要把这两个值记录下来，在预测时使用它们对预测数据做标准化，这就相当于把预测数据“混入”训练数据。前提是预测数据的特征值不能超出训练数据的特征值范围，否则有可能影响准确程度。

### 5.5.2 代码实现

基于这种想法，我们先给`SimpleDataReader`类增加一个方法`NormalizePredicateData()`，如下述代码：

```Python
class SimpleDataReader(object):
    # normalize data by self range and min_value
    def NormalizePredicateData(self, X_raw):
        X_new = np.zeros(X_raw.shape)
        n = X_raw.shape[1]
        for i in range(n):
            col_i = X_raw[:,i]
            X_new[:,i] = (col_i - self.X_norm[i,0]) / self.X_norm[i,1]
        return X_new
```

`X_norm`数组中的数据，是在训练时从样本数据中得到的最大值最小值，比如表5-11所示的样例。

表5-11 各个特征值的特征保存

||最小值|数值范围（最大值减最小值）|
|---|---|---|
|特征值1|2.02|21.96-2.02=19.94|
|特征值2|40|119-40=79|

所以，最后`X_new`就是按照训练样本的规格标准化好的预测标准化数据，然后我们把这个预测标准化数据放入网络中进行预测：

```Python
import numpy as np
from HelperClass.NeuralNet import *

if __name__ == '__main__':
    # data
    reader = SimpleDataReader()
    reader.ReadData()
    reader.NormalizeX()
    # net
    params = HyperParameters(eta=0.01, max_epoch=100, batch_size=10, eps = 1e-5)
    net = NeuralNet(params, 2, 1)
    net.train(reader, checkpoint=0.1)
    # inference
    x1 = 15
    x2 = 93
    x = np.array([x1,x2]).reshape(1,2)
    x_new = reader.NormalizePredicateData(x)
    z = net.inference(x_new)
    print("Z=", z)
```
### 5.5.3 运行结果

```
......
199 99 380.5942402877278 
[[-40.23494571] [399.40443921]] [[244.388824]]
W= [[-40.23494571]
 [399.40443921]]
B= [[244.388824]]
Z= [[486.16645199]]
```
比较一下正规方程的结果：
```
z= 486.1051325196855
```
二者非常接近，可以说这种方法的确很方便，把预测数据看作训练数据的一个记录，先做标准化，再做预测，这样就不需要把权重矩阵还原了。

看上去我们已经完美地解决了这个问题，但是且慢，仔细看看`loss`值，还有`W`和`B`的值，都是几十几百的数量级，这和神经网络的概率计算的优点并不吻合，实际上它们的值都应该在 $[0,1]$ 之间的。

大数量级的数据有另外一个问题，就是它的波动有可能很大。目前我们还没有使用激活函数，一旦网络复杂了，开始使用激活函数时，像486.166这种数据，一旦经过激活函数就会发生梯度饱和的现象，输出值总为1，这样对于后面的网络就没什么意义了，因为输入值都是1。

好吧，看起来问题解决得并不完美，我们看看还能有什么更好的解决方案！

## 5.6 对标签值标准化

### 5.6.1 发现问题

这一节里我们重点解决在训练过程中的数值的数量级的问题。

我们既然已经对样本数据特征值做了标准化，那么如此大数值的损失函数值是怎么来的呢？看一看损失函数定义：

$$
J(w,b)=\frac{1}{2m} \sum_{i=1}^m (z_i-y_i)^2 \tag{1}
$$

其中，$z_i$ 是预测值，$y_i$ 是标签值。初始状态时，$W$ 和 $B$ 都是 $0$，所以，经过前向计算函数 $Z=X \cdot W+B$ 的结果是 $0$，但是 $Y$ 值很大，处于 $[181.38, 674.37]$ 之间，再经过平方计算后，一下子就成为至少5位数的数值了。

再看反向传播时的过程：

```Python
    def __backwardBatch(self, batch_x, batch_y, batch_z):
        m = batch_x.shape[0]
        dZ = batch_z - batch_y
        dB = dZ.sum(axis=0, keepdims=True)/m
        dW = np.dot(batch_x.T, dZ)/m
        return dW, dB
```
第二行代码求得的`dZ`，与房价是同一数量级的，这样经过反向传播后，`dW`和`dB`的值也会很大，导致整个反向传播链的数值都很大。我们可以debug一下，得到第一反向传播时的数值是：
```
dW
array([[-142.59982906],
       [-283.62409678]])
dB
array([[-443.04543906]])
```
上述数值又可能在读者的机器上是不一样的，因为样本做了shuffle，但是不影响我们对问题的分析。

这么大的数值，需要把学习率设置得很小，比如`0.001`，才可以落到 $[0,1]$ 区间，但是损失函数值还是不能变得很小。

如果我们像对特征值做标准化一样，把标签值也标准化到 $[0,1]$ 之间，是不是有帮助呢？

### 5.6.2 代码实现

参照X的标准化方法，对Y的标准化公式如下：

$$y_{new} = \frac{y-y_{min}}{y_{max}-y_{min}} \tag{2}$$

在`SimpleDataReader`类中增加新方法如下：

```Python
class SimpleDataReader(object):
    def NormalizeY(self):
        self.Y_norm = np.zeros((1,2))
        max_value = np.max(self.YRaw)
        min_value = np.min(self.YRaw)
        # min value
        self.Y_norm[0, 0] = min_value 
        # range value
        self.Y_norm[0, 1] = max_value - min_value 
        y_new = (self.YRaw - min_value) / self.Y_norm[0, 1]
        self.YTrain = y_new
```

原始数据中，$Y$ 的数值范围是：

  - 最大值：674.37
  - 最小值：181.38
  - 平均值：420.64

标准化后，$Y$ 的数值范围是：
  - 最大值：1.0
  - 最小值：0.0
  - 平均值：0.485

注意，我们同样记住了`Y_norm`的值便于以后使用。

修改主程序代码，增加对 $Y$ 标准化的方法调用`NormalizeY()`：

```Python
# main
if __name__ == '__main__':
    # data
    reader = SimpleDataReader()
    reader.ReadData()
    reader.NormalizeX()
    reader.NormalizeY()
    ......
```

### 5.6.3 运行结果

运行上述代码得到的结果其实并不令人满意：

```
......
199 99 0.0015663978030319194 
[[-0.08194777] [ 0.80973365]] [[0.12714971]]
W= [[-0.08194777]
 [ 0.80973365]]
B= [[0.12714971]]
z= [[0.61707273]]
```

虽然W和B的值都已经处于 $[-1,1]$ 之间了，但是 $z$ 的值也在 $[0,1]$ 之间，一套房子不可能卖0.61万元！

聪明的读者可能会想到：既然对标签值做了标准化，那么我们在得到预测结果后，需要对这个结果应该做反标准化。

根据公式2，反标准化的公式应该是：

$$y = y_{new}(y_{max}-y_{min})+y_{min} \tag{3}$$

代码如下：

```Python
if __name__ == '__main__':
    # data
    reader = SimpleDataReader()
    reader.ReadData()
    reader.NormalizeX()
    reader.NormalizeY()
    # net
    params = HyperParameters(eta=0.01, max_epoch=200, batch_size=10, eps=1e-5)
    net = NeuralNet(params, 2, 1)
    net.train(reader, checkpoint=0.1)
    # inference
    x1 = 15
    x2 = 93
    x = np.array([x1,x2]).reshape(1,2)
    x_new = reader.NormalizePredicateData(x)
    z = net.inference(x_new)
    print("z=", z)
    Z_real = z * reader.Y_norm[0,1] + reader.Y_norm[0,0]
    print("Z_real=", Z_real)
```

倒数第二行代码，就是公式3。运行结果如下：

```
W= [[-0.08149004]
 [ 0.81022449]]
B= [[0.12801985]]
z= [[0.61856996]]
Z_real= [[486.33591769]]
```

看`Z_real`的值，完全满足要求！

总结一下从本章中学到的正确的方法：

1. $X$ 必须标准化，否则无法训练；
2. $Y$ 值不在 $[0,1]$ 之间时，要做标准化，好处是迭代次数少；
3. 如果 $Y$ 做了标准化，对得出来的预测结果做关于 $Y$ 的反标准化

至此，我们完美地解决了北京通州地区的房价预测问题！

### 5.6.4 总结

归纳总结一下前面遇到的困难及解决办法：

1. 样本不做标准化的话，网络发散，训练无法进行；
2. 训练样本标准化后，网络训练可以得到结果，但是预测结果有问题；
3. 还原参数值后，预测结果正确，但是此还原方法并不能普遍适用；
4. 标准化测试样本，而不需要还原参数值，可以保证普遍适用；
5. 标准化标签值，可以使得网络训练收敛快，但是在预测时需要把结果反标准化，以便得到真实值。


