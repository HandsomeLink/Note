> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/hejp_123/article/details/92151293)

在 Anaconda 中 conda 可以理解为一个工具，也是一个可执行命令，其核心功能是包管理与环境管理。所以对虚拟环境进行创建、删除等操作需要使用 conda 命令。

## conda 本地环境常用操作

> #获取版本号  
> conda --version 或 conda -V
> 
> #检查更新当前 conda  
> conda update conda
> 
> #查看当前存在哪些虚拟环境  
> conda env list 或 conda info -e
> 
> #查看 -- 安装 -- 更新 -- 删除包
> 
> conda list：  
> conda search package_name# 查询包  
> conda install package_name  
> conda install package_name=1.5.0  
> conda update package_name  
> conda remove package_name

### 创建虚拟环境：

使用 conda create -n your_env_name python=X.X（2.7、3.6 等），anaconda 命令创建 python 版本为 X.X、名字为 your_env_name 的虚拟环境。your_env_name 文件可以在 Anaconda 安装目录 envs 文件下找到。 指定 python 版本为 2.7，注意至少需要指定 python 版本或者要安装的包， 在不指定 python 版本时，自动安装最新 python 版本。

> #创建名为 your_env_name 的环境  
> conda create --name your_env_name  
> #创建制定 python 版本的环境  
> conda create --name your_env_name python=2.7  
> conda create --name your_env_name python=3.6  
> #创建包含某些包（如 numpy，scipy）的环境  
> conda create --name your_env_name numpy scipy  
> #创建指定 python 版本下包含某些包的环境  
> conda create --name your_env_name python=3.6 numpy scipy

### 激活虚拟环境：

使用如下命令即可激活创建的虚拟环境

> #Linux  
> source activate your_env_name
> 
> #Windows  
> activate your_env_name

### 退出虚拟环境：

使用如下命令即可退出创建的虚拟环境

> #Linux  
> source deactivate your_env_name
> 
> #Windows  
> deactivate env_name

### 删除虚拟环境：

> conda remove -n your_env_name --all
> 
> conda remove --name your_env_name --all

### 复制某个环境

> conda create --name new_env_name --clone old_env_name

### 在指定环境中管理包

> conda list -n your_env_name  
> conda install --name myenv package_name   
> conda remove --name myenv package_name

## 使用国内 conda 软件源加速
----------------

> $ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/  
> $ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/  
> $ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/  
> $ conda config --set show_channel_urls yes

### 使用国内 pip 软件源加速, 更多详情请点击：[pip 的使用和清华镜像源的设置](https://blog.csdn.net/hejp_123/article/details/92181614)

> **1. 临时设置方法**：
> 
> 可以在使用 pip 的时候加在最后面加上参数 **-i https://pypi.tuna.tsinghua.edu.cn/simple**
> 
> **例如：**pip install jieba -i https://pypi.tuna.tsinghua.edu.cn/simple  # jieba 是一个包
> 
> **2. 永久设置方法：**
> 
> pip install pip -U  
> pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
> 
> 配置完之后就可以像平常一样安装包，速度提升几十倍
> 
> **例如：**pip install jieba
> 
> 切换为阿里云进行下载
> 
> pip install pandas -i http://mirrors.aliyun.com/pypi/simple/   --trusted-host mirrors.aliyun.com  
> pip install pandas -i http://mirrors.aliyun.com/pypi/simple/
> 
>   
> 阿里云 http://mirrors.aliyun.com/pypi/simple/  
> 豆瓣 (douban) http://pypi.douban.com/simple/   
> 清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/  
> 中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

### 分享环境

> #首先通过 activate target_env 要分享的环境 target_env，然后输入下面的命令会在当前工作目录下生成一个 environment.yml 文件  
> conda env export > environment.yml  
> #小伙伴拿到 environment.yml 文件后，将该文件放在工作目录下，可以通过以下命令从该文件创建环境  
> conda env create -f environment.yml