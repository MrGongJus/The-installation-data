# 安装

使用命令安装vim编辑器 : sudo  apt-get  install  vim.

————————————————————————————————————

Go安装包下载网址：https://www.golangtc.com/download

有zip压缩版和msi安装版两个按本下载。（这里使用msi安装版，比较方便）。

运行msi安装文件，千万不要在安装路径中出现中文，一路Next。

## 学习资料

Go语言官网(需要翻墙)：<https://golang.org/>

go中文社区：<https://studygolang.com>

go中文在线文档：<https://studygolang.com/pkgdoc>

————————————————————————————————————

##  Git安装与配置:

(1)    安装命令如下：

​         sudo apt-get install git

(2)    安装成功后，运行如下命令:

`        `  	 git

创建一个版本库:

```
git init
目录下创建了一个.git隐藏目录，这就是版本库目录。
```

## 版本创建与回退:

(1)    目录下创建一个文件, vim编辑内容

(2)    使用如下两条命令可以创建一个版本：

```
        git add code.txt
        git commit ``–m '``版本``1'
```

添加身份标识（git不做检查）(无法提交,校验)

git config --global user.email "you@example.com"

git config --global user.name "Your Name"

然后再执行git commit -m ‘版本一’

(1)    使用如下命令可以查看版本记录：

```
        git log
```

(1)    现在若想回到某一个版本，可以使用如下命令：

```
        git reset --hard HEAD^
        或:
        git reset --hard HEAD~1
```

根据版本号回退

​	  git reset --hard ``版本号`

查看我们的操作记录:

```
  git reflog
```



(1)    使用如下命令查看当前工作树的状态：

​	git status

## 撤销修改:

 git checkout -- <文件> 来丢弃工作区的改动。

git reset HEAD file可以把暂存区的修改撤销掉，重新放回工作区。



# 小结：

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，   版本回退。Git reset –hard 版本号

————————————————————————————————————

(1)    现在要对比工作区中两文件HEAD版本的不同。使用如下命令：

Git diff HEAD  - - 文件名

(2)    现在要对比HEAD和HEAD^版本中code.txt的不同，使用如下命令：

Git diff HEAD HEAD^ -- code.txt

## 删除文件

删除一个文件,工作区(本地)文件被删除, 执行 git status 版本库会提示那些文被删除 1. 确实要从版本库中删除文件, git  rm  文件名  删除, 并执行 git  commit  - m "删除" .

2.误删 , git  checkout  —  文件名。 文件回来了.

## 小结：

命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

————————————————————————————————————

查看当前有几个分支并且看到在哪个分支下工作:

​	git  branch

创建一个分支并切换到其上进行工作:

​	git checkout -b 分支名

切换回分支:

​	git  checkout  分支名

合并指定分支到当前分支:

​	git  merge	

删除分支:

​	branch  -d  分支名

# #小结：

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>

# 解决冲突

(1). master分支dev分支各自都分别有新的提交, git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们删除git标记, git add 文件名 , git commit 

-m. "解决冲突".

用带参数的git log也可以看到分支的合并情况：

Git log  --graph --pretty=oneline

删除分支 : 

Git  branch  -d  文件名

# #分支管理策略

两个分支个自提交了不同文件 ,主分支合并分支, 系统弹出合并说明, 这是因为这次不能进行快速合并，所以git提示输入合并说明信息，输入之后合并内容之后git会自动创建一次新的提交。

如果要强制禁用fast forward模式，git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息

--no-ff参数，表示禁用Fast forward：

​	git merge --no-ff -m "禁用fast-forward合并" 分支名

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去

# Bug分支

每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

​	git stash

恢复作业现场:

​	git stash list              查看

​	git stash pop.           恢复

————————————————————————————————————

## github上传文件

(1). GitHub 上创建一个新的储存库

(2).添加ssh账户:

​	点击账户头像后的下拉三角，选择'settings'

​	如果某台机器需要与github上的仓库交互，那么就要把这台机器的ssh公钥添加到这个github账户上

​	在ubuntu的命令行中，回到用户的主目录下，编辑文件  .gitconfig，修改成自己的用户名和邮箱.

(3). 使用如下命令生成ssh密钥。

```
ssh-keygen -t rsa -C "``邮箱地址``"
```

(4). 进入主目录下的.ssh文件，下面有两个文件。

公钥为id_rsa.pub

私钥为id_rsa

查看公钥内容，复制此内容

回到浏览器中，填写标题，粘贴公钥

#### 克隆项目

(1)在浏览器中点击进入github首页，再进入项目仓库的页面

(2)复制git地址

(3)在命令行中复制仓库中的内容 :   git  clone  复制git的地址

#### 上传分支

(1).项目克隆到本地之后，进入创建的储存库目录中，创建一个分支.

(2).复制或移动文件, 先上传本地, 在上传远端.

​	git add 文件名

​	git commit -m "版本名"

(3).推送分支，就是把该分支上的所有本地提交推送到远程库

​	git push origin  分支名称

#### 将本地分支跟踪服务器分支

​	git branch --set-upstream-to=origin/远程分支名称 本地分支名称

#### 从远程分支上拉取代码

​	工作中: 拉取主分支代码, git pull origin 分支名称

​		      合并到主分支, git merge  master

​		      在上传文件 , git push  origin  分支名称

提交之前要保证本机代码是最新的, 上传到自己分支的远端	

Git和svn的区别<https://www.cnblogs.com/dazhidacheng/p/7478438.html>

## 文件操作--其他目录操作API

获取更多文件、目录操作API可查看Go标库文档： https://studygolang.com/pkgdoc

