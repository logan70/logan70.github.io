---
layout: post
title:  "版本管理工具Git入门"
categories: Tools
date:   2017-09-23 17:48:05
author: Logan
tags:  Git
---

* content
{:toc}

# 版本管理工具发展历史

cvs(集中式 网络环境 1985) -> svn（集中式 网络环境 2000）-> git（分布式  无网环境 2005）-> github（程序猿托管网站 2008）

***

# 安装Git

Windows版的Git[https://git-for-windows.github.io](https://git-for-windows.github.io)

在`Git bash`中设置用户名和邮箱

```git
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

***

# 创建版本库

- 把目录变成Git可以管理的仓库：`git init`

- 把文件添加到仓库：`git add <file>`

	git add readme.txt

- 把文件提交到仓库：`git commit`

	`git commit -m "wrote a readme file"`

	> `-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

	> `commit`可以一次提交很多文件，所以你可以多次`add`不同的文件





**小结**

- 初始化一个Git仓库，使用`git init`命令。
- 添加文件到Git仓库，分两步：

	> - 第一步，使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
	> - 第二步，使用命令`git commit`，完成。

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

***

# 版本控制

## 版本回退

- 仓库当前的状态：`git status`
- 查看difference：`git diff`
- 显示从最近到最远的提交日志：`git log`

	> 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数

	> 退出`git log`,英文输入状态下按`q`

- 查看提交历史：`git log --pretty=oneline`

	>一大串类似`3628164...882e1e0`的是`commit id`（版本号）

	> 在Git中，用`HEAD`表示当前版本,上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

- 回退到历史版本：`git reset`
	
	```git
	$ git reset --hard HEAD^     //回退到上一版本
	```

	>这个指令在Windows的cmd里无法执行，`^`是cmd的特殊字元，命令里要用到文字 `^`時必須用双引号`""`把它引起來<br>
	> `git reset --hard HEAD"^"`

- 查看命令历史：`git reflog`

- 指定回到某个版本：`git reset --hard commit_id`
	
	```git
	$ git reset --hard be57fa
	```

	> 版本号没必要写全，前几位就可以了，Git会自动去找

**小结**

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

## 工作区和暂存区

**工作区（Working Directory）**:就是你在电脑里能看到的目录

**版本库（Repository）**:工作区有一个隐藏目录`.git`，是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为**stage（或者叫index）的暂存区**，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

`git add`命令实际上就是把要提交的所有修改**放到暂存区（Stage）**，然后，执行`git commit`就可以一次性把暂存区的所有修改**提交到分支**。

>用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区

>用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支

## 管理修改

Git跟踪并管理的是修改，而非文件。

用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

每次修改，如果不`add`到暂存区，那就不会加入到`commit`中。

## 撤销修改

**把文件在工作区的修改全部撤销**

丢弃工作区的修改 `git checkout -- file`

	git checkout -- readme.txt

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态

**撤销`git add`到暂存区的内容**

`git reset HEAD file`

可以把暂存区的修改撤销掉（unstage），重新放回工作区

也就是说，现在暂存区是干净的，工作区有修改。

然后用`git checkout -- file`丢弃工作区的修改，整个世界就清净了。

**撤销`git commit`到版本库的内容**

- 如果没有把本地版本库推送到远程，可以用`git reset --hard HEAD^`回退到上一版本
- 如果已经把本地版本库推送到远程，那就跪了

## 删除文件

`rm`命令删除文件

	rm test.txt

删除文件后有**两个选择**：

- 一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`

	```git
	git rm test.txt
	git commit -m "remove test.txt"
	```

- 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本
	
	```git
	git checkout -- test.txt
	```

	>`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

***

# 远程仓库

## 本地仓库和远程仓库关联

- 第1步：创建SSH Key。打开Shell（Windows下打开Git Bash），创建SSH Key：
	
	```git
	$ ssh-keygen -t rsa -C "youremail@example.com"
	```

	> 在用户主目录里找到.ssh目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

- 第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，点“Add Key”，你就应该看到已经添加的Key。

## 添加远程库

- 第1步：创建一个名为`learngit`的Repository
- 第2步：运行下方命令，进行关联：
	```git
	$ git remote add origin git@github.com:logan70/learngit.git
	```

	> 添加后，远程库的名字就是`origin`

- 第三步：运行下方命令，把本地库的所有内容推送到远程库上：
	```git
	$ git push -u origin master
	```

	> 出现错误的主要原因是github中的README.md文件不在本地代码目录中<br>
	> 可以通过如下命令进行代码合并`git pull --rebase origin master`<br>
	> 此时再执行语句 `git push -u origin master`即可完成代码上传到github

	> 由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

	> 从现在起，只要本地作了提交，就可以通过命令：<br>
	> `$ git push origin master`<br>
	>把本地`master`分支的最新修改推送至GitHub

**小结**

- 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`
- 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容
- 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改

## 从远程库克隆

- 第一步，在github上创建一个名为`gitskills`的Repository
- 第二步，用命令`git clone`克隆一个本地库
	```git
	$ git clone git@github.com:logan70/gitskills.git
	```

**小结**

- 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。
- Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。

***

# 分支管理

## 创建、合并、删除分支

- 首先，我们创建`new`分支，然后切换到`new`分支

	```git
	$ git checkout -b new
	```

	`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令

	```git
	$ git branch new
	$ git checkout new
	```

- 然后，用`git branch`命令查看当前分支

	```git
	$ git branch
	```

	> `git branch`命令会列出所有分支，当前分支前面会标一个`*`号

- 把`new`分支的工作成果合并到master分支上
	
	```git
	$ git merge new
	```

	> `git merge`命令用于合并指定分支到当前分支

- 合并完成后，就可以放心地删除`new`分支了

	```git
	$ git branch -d new
	```

**小结**

- 查看分支：`git branch`

- 创建分支：`git branch <name>`

- 切换分支：`git checkout <name>`

- 创建+切换分支：`git checkout -b <name>`

- 合并某分支到当前分支：`git merge <name>`

- 删除分支：`git branch -d <name>`

- 强行删除分支（未被合并过）：`git branch -D <name>`

## 解决冲突

当Git`merge`无法自动合并分支时，就必须首先解决冲突。解决冲突后，再`add`添加、`commit`提交，合并完成

## 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。

```git
$ git merge --no-ff -m "merge with no-ff" newbranch
```

> `--no-ff`参数，表示禁用`Fast forward` <br>
> 因为本次合并要创建一个新的`commit`，所以加上`-m`参数，把`commit`描述写进去。

**小结**

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

## BUG分支

当发现bug需要修复，但是手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除

修复后，再`git stash pop`，回到工作现场

> Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：<br>
> - 一是用`git stash apply`恢复，但是恢复后，`stash`内容并不删除
> - 另一种方式是用`git stash pop`，恢复的同时把`stash`内容也删了

>你可以多次`stash`，恢复的时候，先用`git stash list`查看，然后恢复指定的`stash`，用命令：<br>
> `$ git stash apply stash@{0}`

## 多人协作

**多人协作的工作模式通常是这样：**

1. 首先，可以试图用`git push origin branch-name`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

> 如果`git pull`提示`“no tracking information”`，则说明本地分支和远程分支的链接关系没有创建，用命令`git push --set-upstream origin <branchName>`。

**小结**

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

***

# 标签管理

## 创建标签

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

- 命令`git tag <name>`就可以打一个新标签：

	```git
	$ git tag v1.0
	```

- 命令`git tag`查看所有标签：

	```git
	$ git tag
	```

- 也可以找到历史`commit_id`打上标签

	```git
	$ git tag v1.0 6224937
	```

- 可以用`git show <tagname>`查看标签信息：

	```git
	$ git show v1.0
	```

- 还可以创建带有说明的标签，用`-a`指定标签名，`-m`指定说明文字：

	```git
	$ git tag -a v0.1 -m "version 0.1 released" 3628164
	```

- 还可以通过`-s`用私钥签名一个标签：
	
	```git
	$ git tag -s v0.2 -m "signed version 0.2 released" fec145a
	```

	> 签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错

**小结**

- 命令`git tag <name>`用于新建一个标签，默认为`HEAD`，也可以指定一个`commit id`；

- `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

- `git tag -s <tagname> -m "blablabla..."`可以用PGP签名标签；

- 命令`git tag`可以查看所有标签。

## 操作标签

- 删除标签：`git tag -d <tagname>`
- 推送某个标签到远程：`git push origin <tagname>`
- 一次性推送全部尚未推送到远程的本地标签：`git push origin --tags`
- 如果标签已经推送到远程，要删除远程标签:

	> - 先从本地删除 `git tag -d v0.9`
	> - 然后从远程删除 `git push origin :refs/tags/v0.9`

***

# 配置别名

- `st`表示`status`：`$ git config --global alias.st status`
- `co`表示`checkout`：`$ git config --global alias.co checkout`
- `ci`表示`commit`：`$ git config --global alias.ci commit`
- `br`表示`branch`：`$ git config --global alias.br branch`
- `unstage`表示`reset HEAD`：`$ git config --global alias.unstage 'reset HEAD'`

	>把暂存区的修改撤销掉（unstage），重新放回工作区

- `last`表示`log -1`：`$ git config --global alias.last 'log -1'`

	>显示最后一次提交信息

- `lg`配置：

	```git
	$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
	```

***

# 其他git命令

- `rm -rf .git`：取消git初始化

- `git remote rm origin`： 删除已有的GitHub远程库

- `git add -f <file>`： 强制添加文件到Git（被忽略的格式）

- `git add .`： 提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件

- `git add -u`： 提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

- `git add -A `： 提交所有变化

***

# CMD命令

- 新建文件夹：`mkdir dName`
- 进入文件夹：`cd dName`
- 返回上一级：`cd..`
- 显示d盘目录列表：`dir d:\`
- 显示隐藏文件：`dir /ah`
