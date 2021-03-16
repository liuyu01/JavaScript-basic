##### GIT

1.git与svn的区别在哪里？

```
git和svn最大的区别在于git是分布式的，而svn是集中式的。因此我们不能再离线的情况下使用svn。如果服务器出现问题，我们就没有办法使用svn来提交我们的代码。
svn中的分支是整个版本库的复制的一份完整目录，而git的分支是指针指向某次提交，因此git的分支创建开销更小并且分支上的变化不会影响到其他人。svn的分支变化会影响到所有的人。
svn的指令相对于git来说要简单一些，比git更容易上手。
```

2.经常使用的git命令？

```
git init      //新建git代码库
git add       //添加指定文件到暂存区
git rm        //删除工作区文件，并且将这次删除放入暂存区
git commit -m [message] //提交暂存区到仓库区
git branch    //列出所有分支
git checkout -b [branch] //新建一个分支，并切换到该分支
git status    //显示有变更的文件
```

3.git pull 和 git fetch的区别？

```
git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并
git pull 会将远程仓库的变化下载下来，并和当前分支合并
```

4.git rebase 和git merge的区别？

```
git merge 和 git rebase都是用于分支合并，关键在commit记录的处理上不同。
git merge 会新建一个新的commit对象，然后两个分支以前的commit记录都指向这个新commit记录。这种方法会保留之前每个分支的commit历史。
git rebase 会先找到两个分支的第一个共同的commit祖先记录，然后将提取当前分支这之后的所有commit记录，然后将这个commit记录添加到目标分支的最新提交后面。经过这个合并后，两个分支合并后的commit记录就变为了线性的记录了。
```

