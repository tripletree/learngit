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
Git 是分布式版本控制系统，同一个git仓库可以分不到不同的机器。github提供git仓库托管。本地Git仓库和远程Github仓库之间的传输是通过SSH加密的，所以需要SSH Key，Github知道了你的公钥就可以确认是你推送的提交而不是别人。



	




    


