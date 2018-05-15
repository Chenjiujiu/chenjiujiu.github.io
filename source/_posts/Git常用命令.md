---
title: Git常用命令
comments: true
mathjax: false
date: 2016-02-11 10:58:35
tags: github
id: gjsy-0211105835
categories: 工具使用
keywords: github的简单入门以及常用命令
---
## 前言
{% note warning %} 如果你还不知道git是什么的话建议你看看[廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)，我觉得比较适合新手入门，博主在这就不再重复介绍了，本博客总结一些GIT的常用命令，以备不时之需 {% endnote %}

<!--more-->

## 1.安装
可以从[Git官网](https://git-scm.com/downloads)直接下载安装程序，然后按默认选项安装即可。

## 2.本地配置
### 2.1配置用户名邮箱
全局配置：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
特定项目配置：
```
$ git config user.name "Your Name"
$ git config user.email "email@example.com"
```
### 2.2查看配置信息
查看git配置信息列表
```
$ git config --list
```
查看git单个配置信息
```
$ git config user.name
$ git config user.email
```
### 2.3查看命令手册
有三种方法可以找到 Git 命令的使用手册：
```
$ git help <verb>
$ git <verb> --help
$ man git-<verb>

$ git help config
$ git help push
```
## 3.本地仓库

### 3.1创建git仓库
```
$ git init
```

### 3.1添加文件到暂存区
可以一次添加多个
```
git add <file> <file>
```
添加所有文件，有三个命令，功能细小区别
```
$ git add . ：监控工作区的状态树，把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

$ git add -u ：仅监控已经被add的文件，将被修改的文件提交到暂存区。不会提交新文件。（git add --update的缩写）

$ git add -A ：是上面两个功能的合集（git add --all的缩写）
```
取消暂存区文件
```
git reset HEAD <file>...
```

### 3.2暂存区文件提交
暂存区文件提交进当前分支
```
git commit -m "本次提交说明"
```
添加`-a`参数来跳过`git add`直接把所有已经跟踪过的文件暂存起来一并提交
```
$ git commit -a -m 'added new benchmarks'
```
添加`--amend`参数尝试重新提交
```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
最终只会有一个提交记录,第二次提交将代替第一次提交的结果。
```


### 3.3查看仓库状态 
```
git status

git status -s/ git status --short 
#将得到一种更为紧凑的格式输出。
```
状态标记： 
- A 表示新添加
- 左边M 文件被修改并已添加到暂存区
- 右边M 文件被修改但未添加到暂存区

### 3.4忽略文件
我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件模式  
- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的`glob`模式匹配。
- 匹配模式可以以(/)开头防止递归。
- 匹配模式可以以(/)结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号(!)取反。

`glob` 模式是指 shell 所使用的简化了的正则表达式。 
- 星号`(*)`匹配零个或多个任意字符；
- `[abc]`匹配任何一个列在方括号中的字符(一个a或一个b或一个c)；
- 问号`(?)`只匹配一个任意字符；
- `[0-9]` 表示匹配所有 0 到 9 的数字)。 
- 两个星号`(**)`表示匹配任意中间目录，比如`a/**/z` 可以匹配 a/z, a/b/z 或 a/b/c/z等。  
 
**提示**：GitHub 有一个十分详细的针对数十种项目及编程语言的 .gitignore 文件列表，你可以在 http://github.com/github/gitignore 找到它。

### 3.5查看改动

```
$ git diff 只显示尚未暂存的改动
$ git diff --cached  查看已经暂存起来的变化
$ git diff test 当前目录和另一个叫’test‘分支的差别
$ git diff SHA1 SHA2 两个历史版本之间的差异
```

### 3.6撤销文件
1. 移除文件=直接删除文件：
```
$ git rm test.txt
```

2. 移除暂存区文件：
```
$ git rm --cached test.txt
```

3. 丢弃工作区，文件恢复到最后一次commit 或者add 时的状态
```
$ git checkout -- <file>...
```

4. 暂存区退回工作区
```
$ git reset HEAD
```

### 3.7移动文件
```
git mv 源文件名 目标文件名
```
相当于
```
$ mv README.md README 重命名
$ git rm README.md  移除老文件
$ git add README    添加新文件
```

### 3.8提交日志
命令：`git log`  
参数：
 - -p，显示每次提交的内容差异.
 - -2 仅显示最近两次提交.
 - --stat 显示每次提交的简略的统计信息.
 - --pretty=oneline/short/full/fuller 使用不同格式来显示
 - --pretty=format 可以定制显示的格式
 - --graph 显示分支合并记录
```
$ git log
$ git log -p
$ git log -p -2
$ git log --pretty=oneline
$ git log --pretty=format:"%h - %an, %ar : %s"

f2c1071 - chenjiujiu , 4 minutes ago :test-commit
```
常见的format选项
```
#选项     #说明
%H      提交对象(commit)的完整哈希字串
%h      提交对象的简短哈希字串
%T      树对象(tree)的完整哈希字串
%t      树对象的简短哈希字串
%P      父对象(parent)的完整哈希字串
%p      父对象的简短哈希字串
%an     作者(author)的名字
%ae     作者的电子邮件地址
%ad     作者修订日期(可以用 -date= 选项定制格式)
%ar     作者修订日期，按多久以前的方式显示
%cn     提交者(committer)的名字
%ce     提交者的电子邮件地址
%cd     提交日期
%cr     提交日期，按多久以前的方式显示
%s      提交说明
```

查看历史命令:
```
$ git reflog
```

### 3.9版本切换
HEAD表示当前版本，HEAD^表示上一个版本，HEAD^^表示上两个版本，上10个版本就是就是HEAD～10，可使用commit id进行回退(只需要写前几位)
```
git reset --hard HEAD^ 
git reset --hard id
```

### 3.10保存工作现场
```
$ git stash    //保存 工作区 暂存区

$ git stash list    //查看

$ git stash apply    //仅仅取出
$ git stash drop    //删除
$ git stash pop    //取出并删除
```

## 4.分支管理
### 4.1创建和切换分支
```
$ git branch    //查看分支
$ git branch <name>    //新建分支
$ git checkout <name>    //切换分支
$ git checkout -b <name>    //新建并且切换分支
```

### 4.2删除分支
```
$ git branch -d <name>  //删除某分支
```

### 4.3合并分支
```
//合并某分支到当前分支,如果主分支没有修改则会用fast forward快速模式合并
$ git merge <name>;
$ git merge origin/src

//查看分支合并图
$ git log --graph 

//把dev用普通方式合并到当前分支
$ git merge --no-ff -m "合并描述" dev 

//将分支maint合并到当前分支中，但不要自动进行新的提交
$ git merge --no-commit maint 

//合并不相关历史
git pull 失败 ,提示：fatal: refusing to merge unrelated histories

在进行git pull 时，添加一个可选项
git pull origin master --allow-unrelated-histories
```

### 4.4删除分支
```
$ git branch -d <name> //删除某分支
$ git branch -D <name>  //删除某分支
```
### 4.5删除远程分支
```
$ git push origin --delete dev2
```

## 5.远程仓库
### 5.1创建ssh key
查看用户目录下有没有.ssh目录和id_rsa，id_rsa.pub两个文件  
没有的话创建ssh key
```
$ ssh-keygen -t rsa -C "youname@XXX.com"
```
一路回车使用默认值  
在用户目录的.ssh目录下找到以下两个文件  
id_rsa    //私钥
id_rsa.pub    //公钥

### 5.2克隆远程仓库
```
git clone git@github.com:Chenjiujiu/gitdemo.git
git clone https://github.con/Chenjiujiu/gitdemo.git
```
### 5.2远程仓库地址
#### 5.2.1添加远程仓库地址

可以使用ssh或者https协议，但是https协议速度慢，并且每次推送需要输入口令，建议使用ssh
```
git remote add <shortname> <url>

git remote add origin-https https://github.con/Chenjiujiu/gitdemo.git
git remote add origin-git git@github.com:Chenjiujiu/gitdemo.git
```
#### 5.2.2删除远程仓库地址：  
```
$ git remote rm <shortname>
$ git remote rm origin-https
```
#### 5.2.3查看远程仓库地址
```
git remote -v
```

### 5.3抓取与拉取远程仓库更新
#### 5.3.1git fetch
从远程仓库中获得数据，不会自动合并或修改当前的工作
```
$ git fetch [remote-name]
$ git fetch origin
```
#### 5.3.2git pull
如果一个分支设置为跟踪一个远程分支，可以使用 `git pull` 命令来自动的抓取然后合并远程分支到当前分支
```
$ git pull <远程主机名> <远程分支名>:<本地分支名>
$ git pull origin src:master
```
#### 5.3.2设置追踪关系
如果当前分支与远程分支存在追踪关系,就可以省略远程分支名。
```
$ git branch --set-upstream master origin/src
$ git pull origin
```

### 5.4推送到远程仓库
```
$ git push <远程主机名> <本地分支名>:<远程分支名>
$ git push origin master:src
```

#### 5.4.1删除指定的远程分支
如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。
```
$ git push origin :master
等同于
$ git push origin --delete master
```

#### 5.4.2指定默认主机
如果当前分支与多个主机存在追踪关系，可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。
```
$ git push -u origin master
```

#### 5.4.3推送所有分支
```
$ git push --all origin
```

#### 5.4.4强制推送
忽略冲突强制推送分支到远程，会在远程产生一个"非直进式"(non-fast-forward)的合并
```
$ git push --force origin
```

### 5.5查看远程仓库
```
$ git remote show origin
* remote origin
  Fetch URL: http://git.oschina.net/yiibai/git-start.git
  Push  URL: http://git.oschina.net/yiibai/git-start.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (fast-forwardable)
```

## 6.标签管理
### 6.1添加标签
切换到要打标签的分支
```
$ git tag <name>     //打一个标签
$ git tag    //查看标签
```
按照ID 打标签
```
$ git log    //查找到id
$ git tag <name> <id>
```
添加描述的标签
```
$ git tag -a <tag> -m "描述" <id>
```

### 6.2查看标签信息
```
$ git tag -l
$ git show <tag>
```

### 6.3删除标签
```
$ git tag -d <tag>    //删除本地标签
$ git push origin :refs/tags/<tag>    //删除远程标签
```

### 6.4推送标签到远程
```
$ git push origin <tag>    //推送单个标签
$ git push origin --tags    //推送所有标
```

## 7.子模块
一个单独的git项目存在父项目中，子项目可以有自己的独立的commit，push，pull。而父项目以Submodule的形式包含子项目，父项目可以指定子项目header，父项目中会的提交信息包含Submodule的信息，在clone父项目的时候可以把Submodule初始化。
### 7.1添加子模块
```
$ git submodule add <url>
```
### 7.2克隆含有子模块的项目
先克隆父项目，这时候子模块是空目录，使用如下命令
```
$ git submodule init
$ git submodule update
```
或者直接在克隆副项目的时候添加参数`--recursive`它就会自动初始化并更新仓库中的每一个子模块。
```
$ git clone --recursive http://github.com/chaconinc/MainProject
```


