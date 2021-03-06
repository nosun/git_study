#git 学习

##基本命令
###当前目录初始化
- git init
- git add text.txt 跟踪新文件
- git commit -m 'some comment' 提交说明

###从现有仓库克隆
- git clone git://github.com/nosun/yun-admin
- git克隆的不光是文件，也包含所有的版本信息。

###检查当前文件状态
- git status 查看当前项目的文件状态：clean，untracked or changed

###查看当前文件与提交前文件的改动
- git diff 查看当前文件与当前缓存区文件的差别
- git diff --cached 查看当前缓存区的文件与上次提交的文件之间的差别。

###将修改提交到文件暂存区
- git add 文件修改之后，需要使用git add 将修改的文件提交到暂存区。

###提交修改
- git commit 提交修改，会进入编辑器，要求填写提交修改的说明。
- git commit -m 提交修改注释，在一行命令中填写说明。
- git commit -a -m 'something' 一步式提交，不用git add 直接提交修改。但对新添加的文件似乎不可以。

###移除文件
- git rm 将从文件暂存区中清除文件，并不再跟踪此文件。
- git rm --cached 不删除文件，仅仅从缓存区中删除文件，并取消跟踪

###忽略文件
- vi .gitignore 建`立需要忽略的文件的文件清单。
	
	\# 这里是注释  
	\*.php #忽略所有的php文件  
	!lib.php #除了lib.php  
	/dir #忽略根目录下的dir文件  
    build/ #忽略build 目录下的所有文件
	doc/*.txt #忽略doc下的所有的txt文件，但不做递归 比如/doc/doc/abc.txt

###移动文件
- git mv file\_from fire\_to

###查看提交历史
- git log  查看提交历史
- git log -p 按补丁格式显示每个更新之间的差异
- git log --stat 显示每次更新的文件修改统计信息
- git log --prety=oneline gitlog格式化输出
- git log --graph 显示ascii 图形表示的分支合并历史

###按条件输出提交历史
- git log --since=2.weeks
- git log --author 仅显示指定作者的相关的提交。
- git log --committer 仅显示指定提交者的相关的提交。
- 还可以图形化工具查阅提交历史。
>可以给出各种时间格式，比如具体的某一天，或者多久时间以前。  
>用--author 选项显示指定作者的提交。  
>用--grep 选项搜索提交说明中的关键字。
>如果只关心某些文件或者目录的历史提交，可以在git log 选项的最后指定他们的路径。

###撤销操作
- git commit --amend 修改最后一次提交  
有时候我们提交完了才发现漏掉了几个文件没有提交，或者提交信息写错了，可以使用此命令，将当前的暂存区域快照提交，如果刚才提交完没有做任何改动，直接运行此命令的话，相当于有机会重新编辑提交说明，而所提交的文件快照和之前一样。

###远程仓库的使用
- git remote -v 查看当前的远程库
- git remote add walu https://github.com/walu/phpbook
- git remote -v 此时可以看到有两个远程仓库 一个origin 一个walu  
此时就有了两个远程仓库，可以进行切换，查看更新。
- get fetch 将远程的数据拉取到本地仓库，但并不合并到当前分支。
- git pull 将远程的数据拉取到本地，并与当前分支合并。
- git push origin master 将本地的master分支推送到远程服务器上
- git remote show origin 查看远程仓库的详细信息

###打标签
- git tag tag是对仓库特定版本的版本号。

###技巧和窍门
- 自动补全 tab
- git config --global alias.co checkout 命令别名

##git 分支
###何谓分支
- git保存的不是文件差异或者变化量，而是一系列文件快照
- git提交时，会保存一个提交对象，它包含一个指向暂存内容快照的指针，作者和相关附属信息，以及一定数量指向该提交对象直接祖先的指针。
- git仓库中有3种对象：若干个表示文件快照内容的blob对象，一个记录着目录树内容及各个文件对应blob对象索引的tree对象；以及一个包含指向太热额对象的索引和其他提交信息元数据的commit对象。

###基本操作
- git branch 查看所有分支
- git branch test 新建test分支
- git checkout test 将工作区调整到test分支
- git merge master 将master分支与test分支合并
- git branch -d test 删除test分支，一般当merge完，没有需要的时候就可以删除分支了。
- git branch -v 查看所有分支的最后一次commit信息。
- git branch -d 无法删除 带 * 的分支，因为这个分支上已经有新的提交。
- git branch -D 强行删除一个分支，即使这个分支上有新的提交。
- git branch --merged 查看已经被merged 的branch，对于已经merged的 branch ，可以delete.
- git branch --no-merged 尚未被merged 的branch

###分支式工作流程
由于git 使用简单的三方合并，所以就算在较长的一段时间内，反复多次把某个分支合并到另一分支，也不是难事。经常性的，我们可以同时并行多个分支，随着开发的推进，可以随时把某个特性的分支的成果并到其他的分支中。  

简单的说可以将分支分为主分支，主干分支，特性分支，主分支是唯一的，记录具有里程碑性质的，稳定的版本。主干分支，根据不同的需求可以针对不同的开发方向，比如修复bug的分支，以及开发不同新功能的分支。而特性分支是在主干分支上面的更为临时性的分支，开发测试完就可以删除。
###远程分支
- 远程分支就像是书签，提醒着你上次连接远程仓库时上面各分支的位置。
- 远程的分支为 origin/master
- 本地的分支为 master
- git fetch origin 获取同步
- git push orign master:master 替换远程的master分支
- git checkout -b test orgin/test 在本地检出server上的test分支
- git push origin test 将我本地的test分支推送到origin上

###跟踪分支
- git checkout -b test origin/test 从远程检出test分支到本地
- git checkout --track origin/test 跟踪远程test分支的变化
- git push origin ：test 删除远程的test分支

###衍合
- 将一个分支整合另一个分支的办法有两种，merge和rebase。