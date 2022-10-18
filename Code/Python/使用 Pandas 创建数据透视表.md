>  原文地址 https://www.cnblogs.com/stream886/p/6022125.html

# 1. 使用 Pandas 创建数据透视表
=================

**本文转载自：[蓝鲸的网站分析笔记](http://bluewhale.cc/)[  
](http://www.flickering.cn/)**

**原文链接：[使用 Pandas 创建数据透视表](http://bluewhale.cc/2016-08-04/use-pandas-create-a-pivot-table.html)  
**

目录

*   [pandas.pivot_table()](#q1)
    *   [创建简单的数据透视表](#q2)
    *   [增加一个行维度 (index)](#q3)
    *   [增加一个值变量 (value)](#q4)
    *   [更改数值汇总方式](#q5)
    *   [增加数值汇总方式](#q6)
    *   [增加一个列维度 (columns)](#q7)
    *   [增加多个列维度　](#q8)
    *   [增加数据汇总值](#q9)

数据透视表是 Excel 中最常用的数据汇总工具，它可以根据一个或多个制定的维度对数据进行聚合。在 python 中同样可以通过 pandas.pivot_table 函数来实现这些功能。本篇文章将介绍 pandas.pivot_table 函数与 Excel 数据透视表之间的联系，以及具体的使用方法。文章中的数据源来自 Lending Club 2017-2011 年的公开数据。

![][img-0]

## 1.1. pandas 数据透视表函数
==============

pandas.pivot_table 函数中包含四个主要的变量，以及一些可选择使用的参数。四个主要的变量分别是数据源 data，行索引 index，列 columns，和数值 values。可选择使用的参数包括数值的汇总 方式，NaN 值的处理方式，以及是否显示汇总行数据等。下面是 Pandas 官网给出的函数说明。

[![][img-1]￼￼

我们将 pandas.pivot_table 函数与 Excel 的数据透视表界面做了一个对比，并用不同的颜色和连线画出了两者之间的联系。在其中可以发现 pandas.pivot_table 的行索引 index，列，和数值分别对 应了 Excel 数据透视表中的行，列和值三个部分。在实际的操作中 Excel 是将字段拖拽到相应的字段区间中，而在 pandas.pivot_table 中只需要将字段的的名称输入到等号后面就可以了。下面我们 来看下 pandas.pivot_table 具体的使用方法。

[![][img-2]  
首先导入需要使用的 numpy 和 pandas 功能库，numpy 用于数值计算，Pandas 是基于 numpy 构建的用于科学计算的功能库，pandas.pivot_table 是 Pandas 库 (pd) 中的函数。然后读取 Lending Club 数据 ，并生成名为 lc 的数据表。

```
import pandas as pd
import numpy as np
lc=pd.DataFrame(pd.read_csv('LoanStats3a.csv',header=1))
```

## 1.2. 创建简单的数据透视表
==========

我们选择 Lending Club 数据表中的贷款期限和贷款总额字段来创建一个简单的数据透视表。按贷款期限维度对贷款总额进行聚合，将贷款期限字段 (term) 放在行索引 lndex 中，贷款总额字段 (loan_amnt)放在值 values 中，生成按不同贷款期限维度聚合的贷款总额数据。这里需要说明的是在默认情况下 pandas.pivot_table 对指标的汇总方式是计算平均值。 因此下面的表中显示的是不 同贷款期限的贷款平均值数据。这个简单的数据透视表只有一个维度和一个指标。下面我们将为这个数据透视表增加更多的维度和指标，并增加更多的指标汇总计算方式。

```
pd.pivot_table(lc,index=["term"],values=["loan_amnt"])
```

[![][img-3]

## 1.3. 增加一个行维度 (index)
===============

在贷款期限的维度上增加贷款用户等级维度，创建一个双维度的数据透视表，在 pandas.pivot_table 的行索引 index 中增加贷款用户等级字段 (grade)。这样在行索引维度 index 中共包含了两个维 度，主维度贷款期限(term) 和次级维度贷款用户等级(grade)。指标是按不同贷款期限下贷款用户等级分布进行汇总贷款金额平均值。与之前相比指标数据经过次级维度的细分变的更加精细。

```
pd.pivot_table(lc,index=["term","grade"],values=["loan_amnt"])
```

[![][img-4]  
通过调整 pandas.pivot_table 函数中不同维度的位置可以更改数据透视表中维度的层级，以及数据的显示方式。这里我们将前面代码行索引中两个字段位置互换，此时贷款用户等级 (grade) 成了主维度，贷款期限 (term) 变成了次级维度。

```
pd.pivot_table(lc,index=["grade","term"],values=["loan_amnt"])
```

[![][img-5]  
## 1.4. 增加一个值变量 (value)
====================================================================================================================================================================================================================================================================================

除了增加次级维度以外，还可以增加需要汇总的数据值。在前面数据透视表的基础上我们增加总利息字段作为第二个汇总值。方法与前面增加次级维度很相似，将需要增加的字段放在值 values 中即可。下面是具体的代码和生成的数据透视表，其中 total_rec_int 是新增的值 values 变量。这里需要再次说明的是，默认情况下 pandas.pivot_table 按平均值对数据进行汇总。

```
pd.pivot_table(lc,index=["grade","term"],values=["loan_amnt","total_rec_int"])
```

[![][img-6]  
## 1.5. 更改数值汇总方式
===========================================================================================================================================================================================================================================================================

若要更改 pandas.pivot_table 对值 values 的汇总方式需要在代码中进行设置，下面将贷款总额和总利息字段的汇总方式改为求和。方法是在代码中加入 aggfunc=np.sum。新生成的数据透视表中值字段的计算方式就由之前的平均值改为了求和值。

```
pd.pivot_table(lc,index=["grade","term"],values["loan_amnt","total_rec_int"],aggfunc=np.sum)
```

[![][img-7]  
## 1.6. 增加数值汇总方式
===================================================================================================================================================================================================================================================================================

除了可以对值变量 values 计算平均值和求和以外，还可以进行计数。下面我们在上面数据透视表的基础上分别对贷款总额和总利息字段进行求和，平均值和技术的计算。具体方法是代码中增加以下内容 aggfunc=[np.sum,np.mean,len])，aggfunc 是汇总方式，np.sum 表示求和，np.mean 表示计算平均值，len 表示计数。在下面新创建的数据透视表中可以看到，求和 sum 部分，平均值 mean 部  
分和计数 len 部分的计算结果。

```
pd.pivot_table(lc,index=["grade","term"],values=["loan_amnt","total_rec_int"],aggfunc=[np.sum,np.mean,len])
```

[![][img-8]  
如果数据表中包含有 NaN 值，并且在之前的清洗中没有进行处理，也可以在生成数据透视表的过程中进行处理或替换。在 pandas.pivot_table 函数中有两种处理 NaN 值的方式，第一种是将 NaN 值替换为 0。第二种为放弃 NaN 值，也就是说包含有 NaN 值的数据条目不参加计算。这里我们使用第一种方法，将 NaN 值替换为 0。具体方法是在代码中添加以下部分 fill_value=0。

```
pd.pivot_table(lc,index=["grade","term"],values=["loan_amnt","total_rec_int"],aggfunc=[np.sum,np.mean,len],fill_value=0)
```

[![][img-9]

## 1.7. 增加一个列维度 (columns)
=================

pandas.pivot_table 函数也支持列维度。在 Excel 中需要将对应的字段拖到列区域中，在 Pandas 中的方法是增加列 columns，并将对应的字段名称放在列 columns 变量的值中。下面是具体的代码，其中 columns=[“home_ownership”] 是新增加的部分，表示在数据表中增加列维度 home_ownership。

```
pd.pivot_table(lc,index=["grade"],values=["loan_amnt"],columns=["home_ownership"],aggfunc=[np.sum],fill_value=0)
```

[![][img-10]

## 1.8. 增加多个列维度
=======

与行索引一样，列 columns 中也可以增加多个维度，方法与增加行维度和值一样，这里不再赘述。下面是具体的代码，其中 columns=[“home_ownership”,”term”] 是发生变化的部分，表示列中新增了贷款期限 term 维度。home_owership 为主维度，term 为次级维度。

```
pd.pivot_table(lc,index=["grade"],values=["loan_amnt"],columns=["home_ownership","term"],aggfunc=[np.sum],fill_value=0)
```

[![][img-11]

## 1.9. 增加数据汇总值
=======

pandas.pivot_table 函数中的 margins 参数用于增加数据透视表的汇总值。默认情况下 margins 的状态为 False。需要增加透视表的汇总值时将 margins 值改为 True 即可。此时数据透视表将显示不同维度下数据的汇总值。汇总值的计算方式以 aggfunc 的一致。换句话说，如果 aggfunc 中设置的是求和，那么汇总值也是求和值。

```
pd.pivot_table(lc,index=["grade"],values=["loan_amnt"],columns=["home_ownership","term"],aggfunc=[np.sum],fill_value=0,margins=True)
```

[![][img-12]  
最后，我们总结下 pandas.pivot_table 函数与数据透视表的对应关系。将每部分以不同颜色进行区分，index 对应了数据透视表中行的索引部分 (浅蓝色)，values 对应了数值的部分 (绿色)，columns 对应了列的部分，(橙色表示主维度，黄色表示次级维度)，aggfunc 对应了数值的计算方式 (紫色)，并显示数据透视表的最顶部进行说明 (sum)。Margins 对应了数据透视表中值汇总的部分 (深蓝色)。

[![][img-13]

