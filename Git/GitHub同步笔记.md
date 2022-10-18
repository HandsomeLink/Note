## Github 同步
其实就是简单地利用 Github 同步笔记文件.

在 Github 新建仓库并复制仓库地址

进入笔记目录，运行以下命令:

```git
git init
git remote add origin git@github.com:HandsomeLink/Note.git
git add .
git commit -m "版本信息"
git push --set-upstream origin master
```
> *一般来说 即使是新建的远程仓库 就会有readme.md或者初始化的文件 那么一般先用git pull 把东西拉去下来
> 然后上传 git push -u origin master 意思是提交代码到 别名为origin的远程仓库的master分支 (我没说错吧)
> 一般来说 应该没有错误,,, 但是实际上我每次都出错... 问题是合并失败(有冲突)
> 因为是第一次的关系 远程仓库没有东西 所以可以直接通过 git push -u origin master --force 强行覆盖了远程的代码（超级不推荐 建议找找冲突位置 手动解决）*
 至此，笔记同步完毕。关于 git 的使用我不再多说，程序员必备技能，不熟悉的小伙伴可以自行查阅相关资料.

## 笔记同步
在新电脑安装 VNote 之后，先克隆 Github 仓库到本地:

```git
git clone github.com:HandsomeLink/Note.git
```
打开 VNote, 添加笔记本，路径选择刚才克隆到本地的 Github 仓库，就可以看到同步过来的笔记了

## 日常使用
编辑笔记之前，需要先去笔记目录拉取远程仓库.
```git
git pull
```
编辑完笔记后，需要到笔记目录将变更同步到 Github
```git
git add .
git commit -m "变更信息"
git push
```