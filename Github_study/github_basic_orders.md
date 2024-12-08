# Git 文件版本管理系统学习笔记 --基础命令行

## Git 文件基本状态

**未追踪文件 Untracked files :**  文件已修改，但未被git追踪，commit的文件里不会有这些文件  

**已修改 Changes not staged for commit :**  文件被追踪，追踪文件已被修改，但未暂存

**已暂存  Changes to be committed :**  修改文件已被暂存，下一次commit会更新这些文件





```python
git status #查看文件状态
git status -s # 状态简览
```

## 已存在目录中初始化仓库
初始化文件夹，生成 .git 的子目录


```python
git init
```

## 连接远程仓库
git remote  查看远程仓库信息

若没有任何显示，表示没有仓库信息，需要连接远程仓库


```python
git remote
```

## 克隆远程仓库

git clone url


```python
git clone https://github.com/libgit2/libgit2
```

添加远程仓库

git remote add "\<shortname\> \<url\>" 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写：

关于通过ssh密匙连接远程仓库请看参考或另一篇文章


```python
git remote add origin git@github.com:wei994/notebook.git
#
git remote -v #查看远程仓库信息
```

"wei994/notebook.git" 修改为自己的“用户名/仓库名.git"连接上相应的仓库

origin 为当前远程库的简写， 在命令行中用origin代替整个url

远程仓库重命名 git remote rename


```python
git remote rename pb paul
```

移除远程仓库

git remote remove 或 git remote rm

git remote remove paul
git remote
origin

## 文件暂存


```python
git add . #将所有已修改文件暂存，下一次commit将上传这些已修改文件的暂存文件

git add  filename #将特定文件暂存
```

## 提交更新
文件暂存后就可以提交了


git commit 
git commit -m "Message" #-m 命令可以在提交更新时添加注释
git commit -a #自动把所有已经跟踪的文件暂存一起提交，跳过git add步骤



当git commit 没有带 -m 参数时，会进入 commit change log 输入注释界面

通过以下几种方式退出

**1.保存退出**

（1）按 **Esc**键退出编辑模式，英文模式下输入 :wq ，然后回车(write and quit)。

（2）按 Esc 键退出编辑模式，大写英文模式下输入 ZZ ，然后回车。

**2 不保存退出：**

按 **Esc**键退出编辑模式，英文模式下输入 :q! ，然后回车。

按 **Esc**键退出编辑模式，英文模式下输入 :qa! ，然后回车。



## 将更改推送到远程服务器 
第一次push 时 由于远程库是空的，第一次推送master分支时 加上-u 参数Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来.

推送成功后，可以立刻在Github页面中看到远程库的内容已经和本地一模一样了



```python
git push origin master 
git push -u origin master 
```

## 本地仓库同步远程仓库
如果需要把远程仓库同步到本地仓库我们就要拉取最新数据到本地仓库，命令如下


```python
git pull origin master
```

## 忽略文件 
有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 .gitignore 的文件

一般在创建库时就会选择是否生成 .gitignore 文件，如果未勾选，需要自行创建 .gitignore 

官方给出了 .gitignore 的[模板文件](https://github.com/github/gitignore/tree/main)

可以根据自己的编译器选择模板 在.git 文件夹下新建这个文件


```python
cat .gitignore //查看.gitignore 文件

#可手动编辑 
#基本格式
*.html #忽略html文件
！foo.html #追踪自己些的foo.html文件
# "/"作为目录分隔符 
```

更多 .gitignore 的操作可参靠[官方网站 .giignore](https://git-scm.com/docs/gitignore/zh_HANS-CN)

## 移除文件
要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 git rm 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。



```python
git rm filename #下一次提交时 该文件不会纳入文件管理
rm -force  filename #手动删除某文件
```

## 查看提交历史


```python
git log -p -2 # -p 或-patch 显示每次提交所引入的差异 -2显示最近的两次提交 
#输入q 退出log

git log --stat
```

# 撤销操作
1.有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令来重新提交：


```python
git commit --amend#
#例
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
#最后只会有一个提交
```

2.取消暂存的文件 

git reset HEAD <file> 可以取消暂存的文件，进而重新提交暂存文件


```python
git reset HEAD <file>
```

3.撤销对文件的修改

将它还原成上次提交时的样子

git checkout — <file> 是一个危险的命令。 你对那个文件在本地的任何修改都会消失——Git 会用最近提交的版本覆盖掉它。 除非你确实清楚不想要对那个文件的本地修改了，否则请不要使用这个命令。


```python
$ git checkout -- CONTRIBUTING.md
```

## 打标签



```python
git tag #查看已有标签
git tag -a v0.1 "my version 0.1" #-a 指令给当前版本打标签
git show v0.1 #可以看到标签信息和与之对应的提交信息
git log --pretty=oneline $ 查看过去提交历史
```


```python
a29fabe2285b0a247605d60d6c2386f49023cf6a (HEAD -> master, tag: v0.1, origin/master) Add basic 
orders of git notebook
ca20b4d7c8e523c974903d675eaed5bdc2dfa3ed inital commit
```


```python
git tag -a v0.0 ca20b4d

git push origin --tags #如果想要一次性将所有标签推送到远程服务器

git push origin v0.1 #像是推送标签到远程服务器

git tga -d v0.1 #删除本地仓库标签

git push origin --delete <tagname># 删除远程服务器标签

```

## 参考
[Git可视化极简易教程 — Git GUI使用方法](https://www.runoob.com/w3cnote/git-gui-window.html)

[Git连接github仓库](https://www.cnblogs.com/hdlan/p/14395681.html)

[Git基础](https://git-scm.com/book/zh/v2/Git-%e5%9f%ba%e7%a1%80-%e8%8e%b7%e5%8f%96-Git-%e4%bb%93%e5%ba%93)
