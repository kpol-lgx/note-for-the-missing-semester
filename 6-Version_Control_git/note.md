git uses a directed acyclic graph to model history.

o <- o <- o
every state points back to which state preceded it
每个快照还有一个 metadata--作者信息，提交信息

reference: snapshot 的别名，可以避免使用冗长的
hash 值。

git init 
git help init
git status
	> 第一行指明了分支
	> 

git log
git cat-file -p <hash>
	> print out the contents of this commit
	> 跟提交信息的 hash 值就会显示提交信息的内容
	> 跟文本的 hash 值就会显示文本内容
	> git commit/log 显示的信息 就是 commit hash
	> git cat-file -p <commit hash>显示的信息 就有 tree hash
	> git cat-file -p <tree hash> 显示文本的信息

git log --all --graph --decorate
git checkout <prefix>
	切换到某一个历史状态
	<prefix> 是一个 commit 的 hash 值，
	不需要全部写出，给出前面一部分
	足够识别即可
	随后，工作目录会切换到历史状态

git log --all --graph --decorate
	切换到历史提交之后，再次查看 log
	会发现，HEAD 指针的指向变换，
	HEAD 表明我们所处的位置
	而其他历史信息不变

git checkout master
	想要切换回去，当然可以使用 hash 值
	不过使用快照的名字 master 更方便

当前文件存在没有提交的修改时，git checkout 不能返回历史分支，
但会给出警告信息--返回历史分支会摧毁未经提交的修改
当然也可以使用 -f 选项强制切换
**谨慎使用 git checkout**

git diff
	git diff
	HEAD 到 当前工作区，所有文件的改动
	git diff hello.txt
	HEAD 到 当前工作区，在 hello.txt 文件上的改动
	git diff 2a65 hello.txt
	2a65 到 当前工作区，在 hello.txt 文件上的改动
	git diff 2a65 59c4 hello.txt
	2a65 到 59c4 ，这两个快照在 hello.txt 文件上的改动

git checkout file
	抛弃所有没有暂存的文件的修改并且切换到最新分支

git branch
	显示所有分支 当前分支带星标

git branch -vv
	显示分支的更多信息

git branch <name>
	创建一个指定名称的新分支

git checkout <branch name>
	切换到指定分支

git log --all --graph --decorate --oneline
	--oneline 紧凑形式显示

git checkout -b <name>
	-b 创建新的分支并且切换到该分支

git merge cat
	合并某个分支到当前分支
	conflict merge 合并冲突，当 git merge 不能自动合并时抛出给
	用户

git merge --abort
	如果合并之后，需要抛弃之前的合并

git mergetool
	启用 git 的 merge.tool 帮助合并冲突

git 的冲突提示如下

<<<<<<<<< HEAD
sth.
=========
sth.
>>>>>>>>> dog

======= 上面表示来自 HEAD 分支的 sth
下面表示来自 dog 分支 sth

用户需要自行删除这三行提示符
<<<<<<<<<<<<< HEAD
=============
>>>>>>>>>>>>> dog
并且手动修改
最后 git add 
git merge --continue
	继续合并

git remote
	显示所有的远程仓库

git remote add <name> <url>
	让本地仓库意识到存在一个远程仓库<url>，
	并且为远程仓库命名<name>

git push <remote> <local branch>:<remote branch>
	将本地的一个分支的推送到远程的一个仓库的一个分支

git push --set-upstream <remote> <remote branch>
	为当前分支设置远程追踪分支

git commit -m "message"
	-m 提交一个信息不需要打开编辑器

git clone <url> <folder name>
	克隆一个仓库到本地，并且指定文件夹的名字

git branch --set-upstream-to=origin/master
	git push <remote> <local branch>:<remote branch> 很有用但是也很长
	git branch --set-upstream-to=<remote>/<branch> 可以设置当前所在分支的
	上游分支，这样，推送当前分支时，可以使用简化的命令 git push
	git branch -vv 这是会显示每个分支的上游分支
	
git fetch
	拉取上游分支的内容到本地，
	注意这并不会改变本地的内容，但是远程的分支被复制到本地
	随后跟 git merge 可以合并内容

git full
	相当于 git fetch; git merge 两条命令依次执行

git config
	~/.gitconfig

git clone --shallow
	git clone 默认克隆仓库的全部历史版本，如果仓库很大，速度会很慢
	--shallow 克隆时不包含历史信息，仅仅克隆最新的版本，

git diff --staged/--cached
	相同功能，都是显示暂存区和快照的区别

git add -p 
	-p 提供了交互模式
	可以自由选择想要提交那些内容

git blame
	寻找编辑文件的作者以及他的提交
	实际上可以显示文件的每一行是谁编写的，时间地点，以及对应提交信息和 hash

git show <hash>
	展示某一个提交的信息

git stash
	将工作区的内容恢复至上一次快照的状态，但是不像，git checkout
	没有暂存的修改不会消失，而是被保存下来
	git stash pop
		将 stash 保存的修改恢复

git bisect
	用于手动搜索历史信息时
