---
layout: post
title: "Git操作大全简洁版"
date: 2015-11-11 11:45:49 +0800
comments: true
thumbnail: /img/2015/10/git.jpg
cover: /img/2015/gitfront.jpg
bootstrap: true
categories: 
---
<b> *HEAD* 表示当前分支<!--more-->  
 *master*表示只想提交  
 *Origin* 表示远程库  </b>
##### 1、`git init`  
创建git仓库  
##### 2、`git add xxx.x`  
把文件添加到仓库  
##### 3、`git commit -m “…”`  
把文件提交到仓库  
##### 4、`git status`  
查看仓库当前的状态  
##### 5、`git diff`  
查看difference  
##### 6、`git log`  
显示从最近到最远的提交日志‘  
`git log —pretty=oneline`  
省略显示  
##### 7、`git reset --hard HEAD^`  
会退到上一个版本  
##### 8、`git reflog`  
查看你的命令  
##### 9、Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。  
![](/img/2015/10/gitTree.jpeg)   
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：  
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；  
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。  
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。  
##### 10、modified 被修改；  
untracked：没添加过；  
##### 11、第一次修改 -> `git add` -> 第二次修改 -> `git commit`  
你看，我们前面讲了，Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。    
提交后，用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：  
最终：第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`  
##### 12、`git checkout -- file`  
撤销文件在工作区的修改，有两种情况：  
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；  
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。  
总之，就是让这个文件回到最近一次git commit或git add时的状态。  
##### 13、`git add`到暂存区还没commit,要撤销：  
##### 14、 git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。暂存区干净了；  
丢弃工作区的修改：`git checkout -- xxx.x`  
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。  
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。  
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。  

##### 15、`git rm file`  
删除文件  


##### 16、`git push`  
把本地库的内容推送到远程  
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。    



######  *小结*
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；  
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；  
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；  
分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！  

##### 17、`git clone`
克隆一个本地库

##### 18、创建分支，然后切换到分区  
~$ git checkout -b dev
~Switched to a new branch 'dev'  

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：  
~$ git branch dev  
~$ git checkout dev  
~Swtiched to a new branch 'dev'  

##### 19、`git branch`
查看当前分支，列出所有分支，当前分支前面带`*`号

##### 20、{合并}分支
切换回主线master  
~$ git checkout master  
~Switched to branch 'master'  
合并分支  
~$ git merge dev  
~Updating xxxxxxxx  
~Fast-forward  
~    .....  
~    1 file changed, 1 insertion(+)  
合并完成后，可以删除dev分支  
~$ git branch -d dev  

###### *小结*
Git鼓励大量使用分支：  
查看分支：`git branch`  
创建分支：`git branch <name>`  
切换分支：`git checkout <name>`  
创建+切换分支：`git checkout -b <name>`  
合并某分支到当前分支：`git merge <name>`  
删除分支：`git branch -d <name>`  

##### 21、**解决冲突**

`git status`可以告诉我们冲突的文件  
Git用*<<<<<<<*，*=======*，*>>>>>>>*标记出不同分支的内容  
先手动修改冲突，再提交合并。  

用带参数的**git log**看到分支的合并情况  
`git log --graph --pretty=oneline --abbrev-commit`  

##### 22、`git merge --no-ff -m "merge with no-ff" dev`
不使用**fast forward**模式分支合并，合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

##### 23、**分支策略**
在实际开发中，我们应该按照几个基本原则进行分支管理：  
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；  
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；  
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。  
所以，团队合作的分支看起来就像这样：  
![](/img/2015/10/gitBranch.png) 

##### 24、Bug分支
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

修复bug时，需创建分支来修复它，但现在手上的工作还没做完不可提交的状态，可以用**stash**功能，把工作现场“储藏”起来，等以后回复现场后继续工作；  
~$ git stash  
~Saved working directory and index state WIP on dev: 6224937 add merge  
~HEAD is now at 6224937 add merge  
  
   现在切换值需要修复的分支上创建临时分支：  
~$ git check master  
~$ git checkout -b issue-101  
修复提交  
~$ git add xxx.x  
~$ git commit -m "fix bug 101"  
修复完成后，切换到master分支，完成合并，删除bug分支：  
~$ git checkout master  
~$ git merge --no-ff -m "merged bug fix 101" issue-101  
~$ git branch -d issue-101  



返回原工作区  
~$ git checkout dev  
~$ git status  
此时工作区是干净的，需要我们找回工作现场：  
~$ git stash list  
需要恢复一下，有两个办法：  
一是用**git stash apply**恢复，但是恢复后，stash内容并不删除，你需要用**git stash drop**来删除；  
另一种方式是用**git stash pop**，恢复的同时把stash内容也删了;  
你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：  
~$ git stash apply stash@{0}  

##### 25、Feature分支
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。  
开发一个新feature，最好新建一个分支；  
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。  

##### 26、**多人协作**
要查看远程库的信息，用`git remote`  
或者用`git remote -v`显示更详细的信息  
哪些分支需要推送，哪些不需要呢？  
 
	•	master分支是主分支，因此要时刻与远程同步；	 
	•	dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；	 
	•	bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；	 
	•	feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

抓取分支。

**多人协作**的工作模式通常是这样：
 
	1.	首先，可以试图用git push origin branch-name推送自己的修改；	 
	2.	如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；		 
	3.	如果合并有冲突，则解决冲突，并在本地提交；	   
	4.	没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！	   
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。  

**小结**  
	•	查看远程库信息，使用git remote -v；  
	 
	•	本地新建的分支如果不推送到远程，对其他人就是不可见的；  
	 
	•	从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；  
	 
	•	在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；  
	  		 
	•	建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；  
	•	从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。   
##### 27、*创建标签*
`git tag <name>`  
如果忘了打标签，可以查历史提交的commit id，  
`git tag <name> commit_id`  
查看标签信息：  
`git show <tagName>`  
创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：  
`git tag -a v0.1 -m "version 0.1 released" 3628164`  
删除标签：  
`git tag -d v0.1`  
推送某个标签到远程，用命令`git push origin <tagname>`:  
或者一次性推送全部尚未推送的标签：`git push origin --tags`  

删除远程标签：  
先从本地删除：`git tag -d v0.9`  
再远程删除：`git push origin :refs/tags/v0.9`   


 
**小结**  
	•	命令git push origin <tagname>可以推送一个本地标签；  
	 
	•	命令git push origin --tags可以推送全部未推送过的本地标签；  
	 
	•	命令git tag -d <tagname>可以删除一个本地标签；  
			 
	•	命令git push origin :refs/tags/<tagname>可以删除一个远程标签。 
