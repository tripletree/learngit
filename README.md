#Git

Git是一款免费开源的分布式版本控制系统，由Linux系统的创始人Linus开发。用来记录项目的变化。

##集中式vs分布式
集中式必须联网才能工作。
分布式无中央服务器，安全性更高，工作时不需联网，Git还有强大的分支管理。

##创建版本库
版本库，仓库（repository）：管理文件的目录
创建一个仓库，创建一个空目录：

    $ mkdir learngit
    $ cd learngit
   
初始化仓库

	$ git init	

所有的版本控制系统，只能跟踪文本文件的改动。对于图片、视频这些二进制文件无法跟踪其变化。

##Git工作流： working directory——>staging Area——> repository 

![Git Workflow](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)

		$ git status //查看工作区和暂存区的状态
		
添加文件到仓库，需要提交的文件修改都放到暂寸区，再一次性提交暂存区所有的修改

	$ git add file //把文件从工作区添加到暂存区，开始追踪。
	$ git add filename1 filename2 //可多次使用，添加多个文件 
	$ git commit //把文件添加到仓库
	
查看文件修改的内容

	$ git diff
	
查看提交历史

	$ git log

##版本回退
用git log查看提交历史，其中的commit id是SHA计算出来的一个很大的数字，用十六进制表示。HEAD表示当前版本，HEAD^表示上一个版本，HEAD^^表示上上个版本，HEAD~100表示前100个版本。

	$ git checkout -- file //丢弃工作区的修改
	$ git reset HEAD file //丢弃暂存区的修改
	$ git reset SHA(commit_id的前7位) //回到之前的某个版本
![git reset](http://www.liaoxuefeng.com/files/attachments/001384907584977fc9d4b96c99f4b5f8e448fbd8589d0b2000/0)

	$ git show HEAD //查看当前版本的commit
	$ git reset --hard HEAD^ 
	$ git reflog //记录每一次命令


## 删除文件
	$ rm <file> //删除文件
	$ git rm <file> //把文件从版本库中删除

## 远程仓库
Git 是分布式版本控制系统，同一个git仓库可以分不到不同的机器。github提供git仓库托管。本地Git仓库和远程Github仓库之间的传输是通过SSH加密的，所以需要SSH Key，Github知道了你的公钥就可以确认是你推送的提交而不是别人。分布式版本系统的好处是在本地工作时不用考虑远程库，没有网络时，在本地可以正常工作，有网络时再把本地提交推送同步。而SVN在没有联网时不能工作。

关联远程仓库

	$ git remote add origin <远程仓库地址>
	
第一次推送master所有分支的所有内容

	$ git push -u origin master

每次本地提交后，可以用以下命令推送修改到远程
	
	$ git push origin master 
	
###从远程库克隆
从远程仓库克隆时，Git把本地master和远程master对应起来。远程仓库的默认名称为origin。

	$ git clone <远程仓库地址>

github给出的地址有：https地址和ssh地址

## 分支
一个项目时会有一些其他的想法，创建分支试验这些想法而不影响master，确定使用这些想法后再合并到master中执行。

	$ git branch //查看所有分支
	$ git branch <name> //创建新的分支
	$ git checkout <name> //切换到某个分支
	$ git checkout -b <name> //创建+切换到新分支
	$ git merge <name> //切换到master后再把新分支合并到当前分支，fast forward表示此次合并是快进模式，即直接将master指向新分支的当前提交。
	$ git branch -d <name> //删除分支
	$ git branch -D <name> //删除没有merge到master中的分支


### 合并冲突
当master中提交的版本内容和分支中的版本内容有冲突时，无法快速合并，就会产生冲突。此时，要手动解决冲突后再提交，查看冲突的文件，Git会标识出冲突的部分。

	$ git log --graph //查看分支合并图

强制禁用fast forward 模式，Git在merge时会生成一个新的commit，这样从分支历史上可以看出分支信息。实际开发中，master分支是很稳定的，用来发布新版本，在各个分支中干活再合并。

	$ git merge --no--ff -m "merge with no--ff" <name> //禁用fast forward 模式合并分支
	
### 分支管理策略
一般一个企业开发一个项目的分支策略：主分支master，开发分支develop，功能分支feature，预发布分支release，bug分支fixbug，其他分支other。

主分支master：一个代码库只有一个，提供给用户的正式版本。
功能分支feature：开发某种特定功能，从develop分支中分出来，开发完成后合并入develop。

	 $ git stash //工作没有完成时，把工作现场储存起来
	 $ git stash pop //恢复现场继续工作
	 
## 多人协作
	$ git remote //查看远程仓库的信息
	$ git remote -v//显示更详细的信息
	origin remote_location(fetch)
	origin remote_location(push)
	
### 推送分支
	$ git push origin <branch_name> //把本地某分支的提交推送到远程库，如果远程库没有此分支，则会创建一个

master和develop分支需要与远程同步，bug、feature等分支不一定必要推送到远程。多人协作时的工作模式是，试图推送自己的修改，如果推送失败，因为远程分支比本地更新，需要先用 

	$ git pull 

试图合并, 此前需要将本地分支和远程分支建立链接：

	$ git branch --set-upstream branch-name origin/branch-name

如果合并有冲突，则解决冲突，在本地提交，再推送。

### 抓取分支
多人协作时，大家都会向master和develop分支上推送各自的修改。首先克隆远程库到本地，默认情况下只能看到master分支，想要在某个branch上开发，创建远程库origin的某个branch到本地：

	$ git checkout -b branch_name origin/branch_name

	$ git fetch （origin branch_name）//从远程库取回更新到本地


## 标签管理
发布一个版本时，通常先在版本库中打一个标签。去某个标签的版本就是把打那个标签的时刻的历史版本取出来。标签是版本库某个时刻的快照。

先切换到需要打标签的分支上，然后用以下命令

	$ git tag <tagname> (commit_id) //默认在HEAD处，也可以加上commit_id
	$ git tag //查看所有标签
	$ git show <tagname> //查看某个标签的信息
	$ git tag -a <tagname> -m "infomation" //指定标签信息
	$ git tag -s <tagname> -m "infomation" //用PGP签名标签
	
	$ git push origin <tagname> //推送某个本地标签
	$ git push origin --tags //推送全部未推送过的标签
	$ git tag -d <tagname> //删除某个本地标签
	$ git push origin :refs/tags/<tagname>//删除一个远程标签



 



	




    


