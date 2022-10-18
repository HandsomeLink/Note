\> 本文由 \[简悦 SimpRead\](http://ksria.com/simpread/) 转码， 原文地址 \[www.kuxai.com\](https://www.kuxai.com/post/165)

**前言**
------

想了解 Numpy 的人基本上都是要和数据打交道的，Numpy 对数据操作的方法多，底层也是使用 C 实现的，也就是说 Numpy 处理数据的速度是比较快的，这也体现了 Python 胶水语言的特性。Numpy 也被称为机器学习三剑客之一，另外的就是 Pandas 和 Matplotlib 了，虽然当前有诸如 scikit-learn 机器学习包以及 Pytorch、TensorFlow 深度学习框架，这些包和框架都少不了对数据的操作，当然也少不了对数据进行预处理，这些包和框架也支持与 Numpy 中的数据格式（ndarray）进行交互，所以我认为学好 Numpy 的操作也有利于更深入地了解一些高级的包和框架的使用。

对于 Numpy 来说，官方文档内容相当多，并且是英文的，难道我们需要全部学习一遍吗？我想，如果经常在数据处理领域中摸爬滚打的话，是需要的，但是我也相信二八定理，我们经常使用的也就是 Numpy 中的 20% 左右，至于剩下的内容，需要我们在业余时间补回来，在需要的时候能够快速想起来，不需要做的特别熟练，如果特别熟练就更好了。下面的内容是自己总结的，方便自己看，也希望方便大家看。

程序运行环境：window10 Python3.7 (Anaconda) Numpy 1.18.1

文档参考：**官方文档 \[1\]**，**中文文档 \[2\]**

**1 安装与导入**
-----------

如果安装了 Anaconda，就不需要再安装 Numpy 了，在安装 Anaconda 的时候就安装了 Numpy 及其相关的包。当然你可以使用如下命令安装：

```python
conda install numpy
```

或者：

```python
pip install numpy
```

在 Python 编程社区中，大家会对经常使用的包设置一个都比较认可的别名，Numpy 的别名是 np，使用别名编程也更加简洁，实际编程导入如下：

```python
import numpy as np
```

当然，你也可以自定义别名，但是为了使得代码更具可读性可交流性，最好还是使用大家比较认可的别名。

先了解一下 Numpy 中的数据类型：

```python
array = np.array(\[1, 2, 3, 4\])   # 从一个list中创建Numpy类型的数据
print(type(array))   #
```

即使 Numpy 中只有一个元素，其也是 ndarray 的数据类型。其实也可理解普通的 number(int, float) 类型可看为是标量，而 ndarray 数据类型是数组、向量或矩阵。

**2 创建 ndarray 类型数据及相关信息**
--------------------------

通常，可以从 list 类型的数据进行创建，也可以从 pandas 中 dataframe 类型中获取，生成一个 array，注：一个 ndarray 数据类型都是相同的 (底层 c 语言处理，速度快)，否则会按照 int->float（np.float）->str（object）进行类型转换。

```python
array1 = np.array(\[\[1,2,3\],
                 \[4,5,6\],
                 \[7,8,9\]\])  # 使用一个二维的list生成一个二维的ndarray
array2 = np.array(\[10, 11, 12, 13\])   # 创建一个1维的ndarray
```

**2.1 常用属性**
------------

查看数组的形状，在矩阵计算时，数据的维度要满足矩阵计算要求。

```python
print(array1.shape, array2.shape)  # 返回元组类型 (3, 3) (4,)
```

查看数组的维度，

```python
print(array1.ndim, array2.ndim)  # 2 1
```

查看元素的数据类型，需要满足一定的精度

```python
print(array1.dtype)   # 默认为int32类型(根据机器决定)
```

查看数组中共有多少个数据，

```python
print(array1.size)  # 9
```

**补充：** 在创建 ndarray 的时候，可以选择使用什么样的数据类型，如使用 float 类型：

```python
array3 = np.array(\[1, 3, 5\], dtype=np.float)
print(array3.dtype)  # float64
```

**2.2 创建特殊类型的 ndarray**
-----------------------

在进行矩阵计算时，可以快速创建如全 0 矩阵，单位矩阵，全 1 矩阵，并指定对应的维度等。

**在一定范围内创建等间隔的 ndarray，该方法类型与 Python 中的 range 函数相似，但更强大。**

```python
array = np.arange(10, 15, 0.5, dtype=np.float)
print(array) # \[10.  10.5 11.  11.5 12.  12.5 13.  13.5 14.  14.5\]
```

**拓展：** 创建特殊函数以 10 为底的 log 对数

```python
array = np.logspace(0, 1, 5)  # \[0, 1\]，10^0, 10^0.25 .. 10^1
print(array)  # \[ 1.   1.77827941  3.16227766  5.62341325 10.  \]
```

**快速创建行向量 (不凸显矩阵特性，如果表型矩阵的话 shape 应为 (1,4))**

```python
array = np.r\_\[0:2:0.5\]
print(array)        # \[0.  0.5 1.  1.5\]
print(array.shape)  # (4,)
```

**快速创建列向量 (凸显矩阵特性)**

```
array = np.c\_\[0:2:0.5\]
print(array)
print(array.shape)
"""
\[\[0. \]
 \[0.5\]
 \[1. \]
 \[1.5\]\]
(4, 1)
"""
```

**创建全 0 矩阵，只有一个参数默认为行向量，如果有两个参数需用则使用元组的方式传递参数**

```
array1 = np.zeros(3, dtype=np.float)
array2 = np.zeros((2, 3), dtype=np.float)
print(array1)
print(array2)
"""
\[0. 0. 0.\]
\[\[0. 0. 0.\]
 \[0. 0. 0.\]\]
"""
```

**创建全 1 矩阵，类似全 0 矩阵**

```python
array1 = np.ones(3, dtype=np.float)
array2 = np.ones((2, 3), dtype=np.float)
print(array1)
print(array2)
"""
\[1. 1. 1.\]
\[\[1. 1. 1.\]
 \[1. 1. 1.\]\]
"""
```

**创建一个在 2，10，之间有 6 个元素的 ndarray，并将其形状改为 (2,3)**

```python
array = np.linspace(2, 10, 6)
print(array)
array = array.reshape(2, -1)  # -1为占位，numpy可以推测后面的值
print(array)
"""
\[ 2.   3.6  5.2  6.8  8.4 10. \]
\[\[ 2.   3.6  5.2\]
 \[ 6.8  8.4 10. \]\]
"""
```

**创建一个单位矩阵**

```python
array = np.eye(5)  # 等同于np.identity(5)
print(array)
"""
\[\[1. 0. 0. 0. 0.\]
 \[0. 1. 0. 0. 0.\]
 \[0. 0. 1. 0. 0.\]
 \[0. 0. 0. 1. 0.\]
 \[0. 0. 0. 0. 1.\]\]
"""
```

**3 矩阵计算**
----------

矩阵计算涉及到矩阵对应位置元素操作，矩阵乘法操作。

**3.1 单个矩阵操作**
--------------

**为矩阵所有元素加减乘除，取对数，取平方，取平方根等，还有正余弦，指数等。**

```python
array = np.arange(2, 5, 0.5, dtype=np.float).reshape(3, -1)
print(array)
print(2 + array)
print(array - 2)
print(array/2)
print(2\*array)
print(np.log(array))
print(np.power(array, 2))
print(np.sqrt(array))
```

**矩阵转置**

```python
array = np.arange(2, 5, 0.5, dtype=np.float).reshape(3, -1)
print(array.T)
print(np.transpose(array))
"""
\[\[2.  3.  4. \]
 \[2.5 3.5 4.5\]\]
\[\[2.  3.  4. \]
 \[2.5 3.5 4.5\]\]
"""
```

**矩阵的数值运算**

**随机生成一个 shape 为 (3,2)，元素范围在 0-10 之间的矩阵，并计算整个矩阵各个元素的和、找出最大值、最小值，找到每行 \\ 列最大值和最小值以及各行 \\ 列的和。**

```python
np.random.seed(1)   # 设置随机数种子，保证每次生成的随机数相同
array = 10\*np.random.random((3, 2))
print(array)
print(array.sum())  # 整个矩阵各个元素的和
print(array.sum(axis=1))  # 各行的和
print(array.sum(axis=0))  # 各列的和
print(array.max()) # 整个矩阵中的最大值
print(array.max(axis=1))  # 各行的最大值
print(array.max(axis=0))  # 各列的最大值
# 最小值使用.min即可，操作如上
"""
\[\[4.17022005e+00 7.20324493e+00\]
 \[1.14374817e-03 3.02332573e+00\]
 \[1.46755891e+00 9.23385948e-01\]\]
16.788879311798276
\[11.37346498  3.02446947  2.39094486\]
\[ 5.6389227  11.14995661\]
7.203244934421581
\[7.20324493 3.02332573 1.46755891\]
\[4.17022005 7.20324493\]
"""
```

**说明** ：如果不设置 axis 维度参数的话，则都为整个 array 的元素来说，但一般运用都只是算某个维度的 sum,max,min。

**矩阵中各个元素累乘，矩阵各元素的平均值，矩阵中值，矩阵标准差，矩阵方差，矩阵当前元素减去前面元素的差，每个元素变成当前元素＋前面所有元素的和**

```python
array = np.arange(1, 13).reshape(3, -1)
print(array)
print(array.prod())      # 元素累乘
print(array.mean())      # 矩阵各元素平均值
print(np.median(array))  #  矩阵中值
print(array.std())       #  各元素标准差
print(array.var())       #  各元素方差
print(np.diff(array))    #  当前元素减去前面元素的差
print(np.cumsum(array))  #  每个元素变成当前元素＋前面所有元素的和
"""
\[\[ 1  2  3  4\]
 \[ 5  6  7  8\]
 \[ 9 10 11 12\]\]
6.5
6.5
3.452052529534663
11.916666666666666
\[\[1 1 1\]
 \[1 1 1\]
 \[1 1 1\]\]
\[ 1  3  6 10 15 21 28 36 45 55 66 78\]
"""
```

**根据条件修改矩阵数值**：如比 4 小的全部为 4，比 8 大的全部为 8，四舍五入 (可指定精度)：

```
array = np.linspace(1, 10, 9, dtype=np.float).reshape(3, -1)
print(array)
print(np.clip(array, 4, 8))
 # 对第一位小数点进行四舍五入，默认四舍五入到整数
print(array.round(decimals=1))
"""
\[\[ 1.     2.125  3.25 \]
 \[ 4.375  5.5    6.625\]
 \[ 7.75   8.875 10.   \]\]
\[\[4.    4.    4.   \]
 \[4.375 5.5   6.625\]
 \[7.75  8.    8.   \]\]
\[\[ 1.   2.1  3.2\]
 \[ 4.4  5.5  6.6\]
 \[ 7.8  8.9 10. \]\]
"""
```

**矩阵求逆**

```
a = np.arange(1, 10).reshape(3, 3)
print(a)
print(np.linalg.inv(a))  #
"""
\[\[1 2 3\]
 \[4 5 6\]
 \[7 8 9\]\]
\[\[ 3.15251974e+15 -6.30503948e+15  3.15251974e+15\]
 \[-6.30503948e+15  1.26100790e+16 -6.30503948e+15\]
 \[ 3.15251974e+15 -6.30503948e+15  3.15251974e+15\]\]
"""
```

**3.2 两个矩阵操作**
--------------

**两个形状相同的矩阵对应元素操作，比较元素大小**

```
array = np.arange(2, 5, 0.5, dtype=np.float).reshape(3, -1)
array1 = np.arange(3, 6, 0.5, dtype=np.float).reshape(3, -1)
print(array)
print(array1)
print(array1 + array)
print(array - array1)
print(array/array1)
print(array1\*array)  # 等同于 np.multiply(array1, array)
print(np.log(array))
print(np.power(array, array1))
print(array > 2.5)
print(array > array1)
```

**矩阵相乘**

```
array1 = np.arange(2, 10).reshape(2, -1)
array2 = np.arange(4, 12).reshape(-1, 2)
# 方式1
print(array1.dot(array2))
# 方式2
print(np.dot(array1, array2))
# 方式3
print(array1 @ array2)
```

**4 切片和索引**
-----------

**4.1 获取指定值的索引**
----------------

获取一个矩阵最大值、最小值以及非零索引。

```
array = np.array(\[\[-6, 6, 1\],
                 \[-2, 1, 7\],
                 \[0, 2, 0\]\])
print(np.argmin(array))   # 获取矩阵最小值索引
print(np.argmax(array))   # 获取矩阵最大值索引
print(np.argmin(array, axis=0)) # 获取矩阵每列的最小值索引
print(np.nonzero(array))  # 可以理解第1个array代表行，第2个代表列
"""
\[0 1 2\]
(array(\[0, 0, 0, 1, 1, 1, 2\], dtype=int64), array(\[0, 1, 2, 0, 1, 2, 1\], dtype=int64))
```

**4.2 获取矩阵块数据**
---------------

**获取矩阵中具体的某个值，获取矩阵指定行，获取矩阵指定列，获取指定的行和列。**

```
array = np.array(\[\[-6, 6, 1\],
                 \[-2, 1, 7\],
                 \[0, 2, 0\]\])
print(array\[1,2\], array\[1\]\[2\])  # 获取索引为1行2列的值
print(array\[1\], array\[:\]\[1\])    # 获取索引为1的行,array\[:\]\[1\]:所有行中的第1行
print(array\[:, 1\])              # 索引为1的列，返回为一个行向量（确定只有1列返回行）
print(array\[:, 1:2\])            # 索引为1的列，返回为一个列向量（:则是切片，保持原来的维度）
"""
7 7
\[-2  1  7\] \[-2  1  7\]
\[6 1 2\]
\[\[6\]
 \[1\]
 \[2\]\]
"""
```

**数据迭代，迭代矩阵的行或列。**

```
array = np.array(\[\[-6, 6, 1\],
                 \[-2, 1, 7\],
                 \[0, 2, 0\]\])
# 行迭代
for row in array:
    print(row)
# 列迭代
for col in array.T:
    print(col)
"""
\[-6  6  1\]
\[-2  1  7\]
\[0 2 0\]
\[-6 -2  0\]
\[6 1 2\]
\[1 7 0\]
"""
```

**将矩阵降低一维 (内存降低一维)，对于二维矩阵来说，最后变成一个行向量**

```
array = np.array(\[\[-6, 6, 1\],
                 \[-2, 1, 7\],
                 \[0, 2, 0\]\])
print(array.flatten())
# \[-6  6  1 -2  1  7  0  2  0\]
```

**4.3 使用 bool 索引进行切片**
----------------------

通过布尔类型选择数据， 布尔类型的矩阵也可以通过是矩阵的比较得到 (ndarray> a， ndarray == ndarray 等)

```
a = np.arange(0, 100, 20)
mask = np.array(\[0, 0, 1, 0, -1\], dtype=bool) # 0表示False，0之外的表示True
print(a)
print(mask)
print(a\[mask\])  # 通过布尔类型来选择数据
"""
\[ 0 20 40 60 80\]
\[False False  True False  True\]
\[40 80\]
"""
```

**5 扩展与分解**
-----------

**5.1 矩阵合并**
------------

**多个行向量合并，矩阵合并**，

```
a = np.arange(1, 4)
b = np.arange(4, 7)
c = np.vstack((a, b, a, b))  # 垂直合并
print(c)
d = np.hstack((a, b, a, b))  # 水平合并，就是拼接
print(d)
c1 = np.vstack((a.T, b.T, a.T, b.T))
"""
\[\[1 2 3\]
 \[4 5 6\]
 \[1 2 3\]
 \[4 5 6\]\]
\[1 2 3 4 5 6 1 2 3 4 5 6\]
"""
a = np.arange(1, 5).reshape(2, -1)
b = np.arange(5, 9).reshape(2, -1)
print(np.vstack((a,b)))  # 垂直合并
print(np.hstack((a,b)))  # 水平合并，就是拼接
"""
\[\[1 2\]
 \[3 4\]
 \[5 6\]
 \[7 8\]\]
\[\[1 2 5 6\]
 \[3 4 7 8\]\]
"""
```

**补充：** 对于行向量，使用转置方法返回还是行向量。

**行向量转为列向量 (矩阵)**

```
a = np.arange(1, 5)
print(a.reshape(1, -1))   # 错误方法
print(a\[:, np.newaxis\])   # 转换成列向量
"""
\[\[1 2 3 4\]\]
\[\[1\]
 \[2\]
 \[3\]
 \[4\]\]
"""
```

**列向量的合并**

```
a = np.arange(1, 5)\[:, np.newaxis\]
b = np.arange(5, 9)\[:, np.newaxis\]
c = np.concatenate((a, b, a))  # 列向量的拼接，矩阵的拼接, axis参数默认为0，在垂直方向拼接（行的拼接）
print(c)
d = np.concatenate((a, b, a), axis=1) # 在水平方向拼接（列的拼接）
print(d)
"""
\[\[1\]
 \[2\]
 \[3\]
 \[4\]
 \[5\]
 \[6\]
 \[7\]
 \[8\]
 \[1\]
 \[2\]
 \[3\]
 \[4\]\]
\[\[1 5 1\]
 \[2 6 2\]
 \[3 7 3\]
 \[4 8 4\]\]
"""
```

**5.2 矩阵分解**
------------

**将矩阵沿水平、垂直方向分解**

```
a = np.arange(12).reshape(3, 4)
print(a)
# 均匀分开
print(np.split(a, 2, axis=1))  # 垂直方向（从上到下劈一刀）均匀（4/2=2）分成2份
print(np.split(a, 3, axis=0))  # 水平方向（从左到右劈三刀）均匀（3/1=3）分成3份
"""
\[\[ 0  1  2  3\]
 \[ 4  5  6  7\]
 \[ 8  9 10 11\]\]
\[array(\[\[0, 1\],
       \[4, 5\],
       \[8, 9\]\]), array(\[\[ 2,  3\],
       \[ 6,  7\],
       \[10, 11\]\])\]
\[array(\[\[0, 1, 2, 3\]\]), array(\[\[4, 5, 6, 7\]\]), array(\[\[ 8,  9, 10, 11\]\])\]
"""
# 非均匀分开
# 垂直方向分三份，最多的在第一份， 方法同：np.vsplit(a, 3)
print(np.array\_split(a, 3, axis=1))
# 水平方向分两份，最多的在第一份， 方法同：np.hsplit(a, 2)
print(np.array\_split(a, 2, axis=0))
"""
\[array(\[\[0, 1\],
       \[4, 5\],
       \[8, 9\]\]), array(\[\[ 2\],
       \[ 6\],
       \[10\]\]), array(\[\[ 3\],
       \[ 7\],
       \[11\]\])\]
\[array(\[\[0, 1, 2, 3\],
       \[4, 5, 6, 7\]\]), array(\[\[ 8,  9, 10, 11\]\])\]
"""
```

**将列向量转换为行向量**

```
array = np.arange(12)\[:, np.newaxis\]
print(array.squeeze())
print(array.flatten())
# \[ 0  1  2  3  4  5  6  7  8  9 10 11\]
# \[ 0  1  2  3  4  5  6  7  8  9 10 11\]
print(array.reshape(3, 4).flatten())  # 可将高维数据亚平，但squeeze()不行
# \[ 0  1  2  3  4  5  6  7  8  9 10 11\]
```

**6 随机模块**
----------

**设置随机数种子，保持每次得到的数据一致**

```
np.random.seed(10)
```

**从 0-10 中随机选出 5 个随机数行向量**

```
print(np.random.choice(11, size=5))  # 选出的元素可重复，但不可是
"""
\[7 0 6 9 9\]
"""
```

**随机生成指定维度，满足 0-1 均匀分布的矩阵** (当 rand() 中无参数，则返回一个标量的随机数值)

```
print(np.random.rand(3, 2)) # 返回服从\[0, 1)均匀分布的随机样本值
"""
\[\[0.8027575  0.09280081\]
 \[0.51815255 0.86502025\]
 \[0.82914691 0.82960336\]\]
"""
```

**返回区间在 \[0, a) 上指定维度的整数矩阵**

```
print(np.random.randint(15, size=(2,3)))
"""
\[\[10  9  8\]
 \[14  7  3\]\]
"""
```

**返回区间在自定义区间上指定维度的整数矩阵**

```
print(np.random.randint(10, 15, size=(2,3)))
"""
\[\[11 11 13\]
 \[14 10 11\]\]
"""
```

```
mu = 0  #  均值
sigma = 1  # 方差
print(np.random.normal(mu, sigma, (2, 5)))
"""
\[\[ 0.37475197 -0.47338275 -0.45452082 -0.08533806  1.50318838\]
 \[ 1.16064112 -0.4829414  -1.80662901 -0.91544761 -0.06973398\]\]
"""
```

**对一数据，随机打散** ()

```
array = np.arange(10).reshape(2,5)
print(array)
np.random.shuffle(array)
print(array)
"""
\[\[0 1 2 3 4\]
 \[5 6 7 8 9\]\]
\[\[5 6 7 8 9\]
 \[0 1 2 3 4\]\]
"""
```

**7 其它操作**
----------

**在使用时需要注意，数据是否共有内存，避免修改一地方的值，另外一个地方的值也随之变化。**

```
a = np.arange(5)
b = a
c = b
d = c # a,b,c,d指向同一数据地址
print(d is a, c is a, b is a) # True True True
d\[0\] = 100
print(a)        # d改动，a随着变化
e = a.copy()    # 拷贝一份a给e，a,e不共享内存
print(e is a)
e\[0\] = 20
print(e, a)
"""
True True True
\[100   1   2   3   4\]
False
\[20  1  2  3  4\] \[100   1   2   3   4\]
"""
```

**矩阵元素排序**

```
array = np.array(\[\[-6, 6, 1\],
                 \[-2, 1, 7\],
                 \[0, 2, 0\]\])
 # 默认对每行的数据从左至右，从小到大排序, 参数axis默认为0
print(np.sort(array))
print(np.sort(array, axis=1)) # 对列进行排序，从上到下，从小到大排序
"""
\[\[-6  1  6\]
 \[-2  1  7\]
 \[ 0  0  2\]\]
\[\[-6  1  0\]
 \[-2  2  1\]
 \[ 0  6  7\]\]
"""
```

**后记**
------

Numpy 能够操作的内容很多，本文也只是个人总结，很多表述也不一定正确，不过基本上能够应付日常操作了，更多操作也还需学习官方文档。当 Numpy、Pandas、Matplotlib 以及更多包配合起来就能够做出很多有意思的事情了。

### **参考资料**

\[1\] 官方文档地址: _[https://numpy.org/](https://numpy.org/)_

\[2\] 中文文档地址 : _[https://www.numpy.org.cn/user/](https://www.numpy.org.cn/user/)_