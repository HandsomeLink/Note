> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.cnblogs.com](https://www.cnblogs.com/feffery/p/13199472.html)

1 简介[#](#2219507214)
====================

　　提起`kite`相信不少朋友都有印象，它是一个功能非常强大的代码补全工具，目前可用于`Python`与`javascript`，为许多知名的编辑器譬如`Vs Code`、`Pycharm`提供对应的插件。

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193521162-1883821437.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193521162-1883821437.png)

图 1

　　而最近`kite`开源了针对`jupyter lab`的代码补全插件，使得我们在代码提示补全功能较弱的`jupyter lab`平台上也可以体验到强大的`kite`功能，本文就将带大家来学习如何在`jupyter lab`中使用`kite`引擎。

2 在 jupyter lab 中使用 kite[#](#3245046157)
========================================

　　下面我们分步骤讲解：

2.1 安装 kite 软件[#](#229953901)
-----------------------------

　　要使用`kite`服务，首先我们需要下载`kite`引擎软件，你可以到官方网站（ [https://kite.com/](https://kite.com/) ）去下载`kite`的安装包：

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193523874-1558119688.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193523874-1558119688.png)

图 2

　　考虑到是国外网站下载速度很慢，我们准备了百度云下载连接（链接：https://pan.baidu.com/s/15GxJXhv0VM1AK341N4t5_A 提取码：yevd），下载完成后，双击打开安装，根据提示选择自己想要的配置方式，这里可以不注册直接跳过：

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193526356-673614273.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193526356-673614273.png)

图 3

　　安装完成后，保持`kite`软件开启，下面我们来配置`jupyter lab`的部分。

2.2 jupyter lab 的配置[#](#3740631612)
-----------------------------------

　　为了更好地演示，下面我们利用`conda`创建新的环境：

```
conda create -n kite python=3.7


```

　　激活新环境后，我们需要安装`2.2.0`以上版本的`jupyter lab`，但是目前`jupyter lab`的最新正式版本为`2.1.5`，因此我们需要使用`pip`来安装其提前发行版本，这里我选择`2.2.0a1`：

```
pip install --pre jupyterlab==2.2.0a1


```

　　安装完成之后，我们把`jupyter lab`运行插件所需的`nodejs`也一并安装上：

```
conda install nodejs


```

　　最后再以此运行下面的命令行来安装`kite`在`jupyter lab`中运行所需的依赖：

```
pip install jupyter-kite
jupyter labextension install @kiteco/jupyterlab-kite


```

　　一切准备就绪，下面我们来看看效果如何。

### 2.3 kite 的使用[#](#397472605)

　　为了检验效果，我们可以装上常用的`pandas`、`numpy`、`scikit-learn`等库，再运行`jupyter lab`命令启动，刚进入`jupyter lab`界面打开`ipynb`文件后，左下角会出现正处于`indexing`状态的`kite`图标：

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193528526-1415337311.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193528526-1415337311.png)

图 4

　　当你开始书写代码时，`kite`图标状态会变成`ready`，随着你书写代码，代码提示功能也随即运作起来：

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193531012-177325372.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193531012-177325372.png)

图 5

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193533968-622914608.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193533968-622914608.png)

图 6

　　并且在你开启光标跟踪功能之后，打开的`kite`界面里的文档还会自动跟踪你鼠标停留的地方：

[![](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193536544-929673509.png)](https://img2020.cnblogs.com/blog/1344061/202006/1344061-20200630193536544-929673509.png)

图 7

> 目前`kite`面向`jupyter lab`的插件还处于实验阶段，如果你在使用体验过程中遇到问题，可以到官方`Github`（ [https://github.com/kiteco/jupyterlab-kite](https://github.com/kiteco/jupyterlab-kite) ）仓库下提问

　　以上就是本文的全部内容，欢迎在评论区与我讨论！