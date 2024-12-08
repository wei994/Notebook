# Git 分支操作学习笔记
Git 创建任务分支时，并不是将文件全部备份，而是创建新的快照/文件索引。

分支可以当目前版本遇到问题时，进行修改但不影响当前版本内容，待问题解决后可将新的分支合并到原有分支中，分支管理是git的一个核心功能


## 查看分支



```python
git branch #查看所有本地分支
git branch -r #查看所有远程分支李彪
gri branch -a #列出当前仓库所有本地和远程分支
```

## 分支新建与合并
分支创建


```python
git branch test #创建名为test 的新分支
git checkout test #切换到test 分支
git checkout -b test #创建test 分支并切换到test分支
```

当做了修改并在test分支做了提交时，原分支main仍保留之前版本的快照，此时切回main 分支再做修改， test 分支修改的内容不会保留在main 分支上


```python
git log --oneline --decorate --graph -all #查看分叉指向，提交历史以及项目分支分叉情况
```

**分支合并**

当分支内容修改完后可以回到mamin分支，将分支内容合并进主分支


```python
git checkout main #切换回mian分支
git merge test #合并test到当前分支
git branch -d test #删除test 分支，因为不需要了
```

**分支冲突**

当合并不同分支时，如果两条分支都修改了文件同一位置的内容，将会导致冲突，必须解决冲突后再合并

## 分支管理

**查看每一个分支的最后一次提交**

git branch -v

**查看分支合并情况**


```python
git branch --merged #查看已合并到当前分支的分支
git branch --no-merged #查看未合并分支
git branch --no-merged master #查看未合并到master的分支
```

未合并的分支有未合并的工作，git branch -d 删除命令会失效
git barnch -D 强制删除分支并丢弃这些工作

## 远程分支
以上操作都是在本地分支上进行操作，需要同步到远程仓库的分支


```python
git ls -remote origin #获取远程仓库origin 的分支列表

#origin 命名的远程仓库名
#远程分支以 <remote>/<branch>形式命名

```

本地和远程的工作可以分叉 git fetch \<remote\>可以 与给定的远程仓库同步数据，抓取本地没有的数据并更新本地数据库

可以同时连接多个远程仓库和分支

git remote add  origin2 #添加新的远程仓库 ，改远程仓库为目前远程仓库额自己

由于有多个分支，可以选择将想推送的分支推送到服务器上，私人分支将保留在本地
git push remote branch


```python
git push orgin main #推送origin/main 分支到远程服务器
git checkout --track origin/test #跟踪 远程mtest分支
git checkout -b tt origin/test #自动从origin/test 分支拉取并待 tt 本地分支
git branch -vv #产看设置的所有跟踪分支
```


```python
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```


```python
git fetch --all;git branch -vv #更新抓取所有远程仓库的所有分支
git push origin --delete test #删除远程test 分支
```

# 变基 rebase
使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

如果提交存在于你的仓库之外，而别人可能基于这些提交进行开发，那么不要执行变基。


git checkout test
git rebase master #将master 共同祖先的修改应用到 test 分支山
git checout master 
git merge test #合并test到master
