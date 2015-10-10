## git分布式
1. 每个电脑都有版本控制数据库，有很多工作模式。而svn是只有中央库有版本控制数据库
2. 每个历史版本存储完整的文件，svn存储文件差异
3. git有更强的撤销修改和修改版本历史的能力

## git [官方网站](http://git-scm.com/)

```
	git --version
	//git help
	git config --help
	man git-config
	//alias
	git config --global alias.co checkout
	git config --global alias.st status
	git config --global alias.lol "log --oneline"
	//git config crud
	git config --global --add or --unset or name value or --get
```

## git 工作流程

* `git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"`

	因为是分布式所以必须每台机器进行认证,注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
git config -l --local 列出该仓库下的参数
user.name pmzgit user.email pmz00
配置有三个级别，优先级由高到低 --local --global(当前用户) --system（当前系统所有用户）

* git 使用40个16进制字符的	SHA-1 Hash 来唯一标识对象。
	git 有四种对象：tag -> commit -> tree > blob (tree包含tree和blob对象，树形结构)

* 初始化一个Git仓库，1. 使用git init命令。在当前文件夹下初始化 `git init`. 2. git clone local_repo dist_dir_name

* 添加文件到Git仓库，分两步:    
第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件 `git add a.md b.txt`；
第二步，使用命令git commit，完成。


* 要随时掌握工作区的状态，使用git status命令。
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。


HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id（例如git reset --hard HEAD～返回上一个版本）。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。git log --decorate --graph --oneline --all(查看所有分支commit，tag信息)

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。


版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

现在，你又理解了Git是如何跟踪修改的，每次修改，如果不add到暂存区，那就不会加入到commit中


命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。

git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

又到了小结时间。

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

* Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件test.txt到Git并且提交：一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：
现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
现在，文件就从版本库中被删除了。`git rm file` 删除了工作区和暂存区 `git rm --cached file` 只删除了暂存区  
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：git checkout -- test.txt  
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

* `git mv file1.txt file2.txt` 重命名操作，工作区和暂存区文件名都更改了，但是根据文件内容生成的暂存区索引SHA-1 Hash码，并没有更改，因为只是重命名操作，内容并没有更改。

* 要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
$ ssh-keygen -t rsa -C "youremail@example.com"
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

关联后，使用命令git push -u origin master第一次推送master分支的所有内容；（第一次推送远程仓库没有该分支时，使用这个命令可以完成本地新分支与远程薪分支的跟踪）
git checkout

此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

* 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。  
`git clone git@github.com:michaelliao/gitskills.git （destion dir）`克隆到的仓库会有一个origin/master（远程跟踪分支，用户只读），据其还会产生一个master分支（跟踪分支，用户可写），指向最新commit（HEAD）

* Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

* Git鼓励大量使用分支：分支只是指向某个commit对象的引用

查看当前分支：git branch

创建分支：git branch name

切换分支：git checkout name

创建+切换分支：git checkout -b name

删除分支：git branch -d name

* 合并某分支到当前分支：git merge name (当远程分支有新的commit，首先要git fetch origin下来，这时候本地的origin/master移向最新的commit，所以要用git merge origin/master（线性历史） 使本地master指向最新的提交)这两个操作=git pull  推荐分开，因为可以用git diff master origin/master 查看本地与远程的版本历史区别

> [remote "origin"]  

	url = git@github.com:pmzgit/note.git
	fetch = +refs/heads/*:refs/remotes/origin/*
		远程仓库master分支（refs/heads/master：commit SHA1 hash）
		本地远程跟踪origin/master分支（refs/remotes/origin/master：commit SHA1 hash）
		+ 代表强制non-fast-forward  不建议修改远程仓库店历史commit（例如版本倒退，并覆盖），也不建议强制push（git push origin +master）
		否则别人fetch时（此时强制+）


当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用git log --graph命令可以看到分支合并图。 git log --graph --pretty=oneline --abbrev-commit


* 分支管理策略  
在实际开发中，我们应该按照几个基本原则进行分支管理：

* 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；  
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。


Git分支十分强大，在团队开发中应该充分应用。

* 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。


* bug分支
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137602359178794d966923e5c4134bc8bf98dfb03aea3000


开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。


因此，多人协作的工作模式通常是这样：

现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

$ git checkout -b dev origin/dev（此时也完成了远程dev分支与本地新建dev分支的跟踪绑定）

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to=origin/dev dev（旧版本git  该命令为git branch --track | --set-upstream dev origin/dev 此时本地 .git/config 文件zhong新增一项配置：
[branch "dev"]
	remote = origin
	merge = refs/heads/dev
merge指 本地dev 跟踪远程的ref/heads/下的dev分支
这就是多人协作的工作模式，一旦熟悉了，就非常简单。

小结

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致（若此时远程已有分支v1 git fetch origin 后 可直接用 git checkout v1 创建并跟踪远程v1）；

建立本地分支和远程分支的关联，使用git branch --set-upstream-to=origin/dev dev

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


* Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。  
命令git tag name用于新建一个标签，默认为HEAD，也可以指定一个commit id；  
git tag -a tagname -m "blablabla..."可以指定标签信息；  
git tag -s tagname -m "blablabla..."可以用PGP签名标签；

命令git tag可以查看所有标签。`git show tag_name` show detail tag info

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。


在GitHub上，可以任意Fork开源仓库；

自己拥有Fork后的仓库的读写权限；

可以推送pull request给官方仓库来贡献代码。

让Git显示颜色，会让命令输出看起来更醒目：git config --global color.ui true


忽略某些文件时，需要编写.gitignore；

.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

设置别名：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375234012342f90be1fc4d81446c967bbdc19e7c03d3000

git@github.com:pmzgit/note.git