## Git教程

### Git的作用

记录每次文件的改动、协作编辑

### Git的诞生

在2002年以前，世界各地的志愿者把源代码文件通过diff的方式发给Linus，然后由Linus本人通过手工方式合并代码

Linus坚定地反对CVS和SVN这些集中式版本控制系统，这些玩意不但速度慢，而且必须联网才能使用。有一些商用的版本控制系统，虽然比CVS、SVN好用，但那是付费的，和Linux的开源精神不符。

2002年，Linux系统已经发展了十年了，Linus已经很难继续手动管理代码库，其社区也对这种方式表达了强烈不满，于是Linus选择了一个商业的版本控制系统BitKeeper，BitKeeper的东家BitMover公司出于人道主义精神，授权Linux社区免费使用这个版本控制系统。

2005年，开发Samba的Andrew试图破解BitKeeper的协议（这么干的其实也不只他一个），被BitMover公司发现，于是BitMover公司要收回Linux社区的免费使用权。

最后Linus用两周时间写了一个git

## 集中式vs分布式

**集中式版本控制系统**

版本库是集中存放在中央服务器的，而干活的时候，要先从中央服务器取得最新的版本，然后开始干活，干完活了，再把自己的活推送给中央服务器。

中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。

集中式版本控制系统最大的毛病就是必须联网才能工作，但是网络质量天差地别。

-----------------------------------

**分布式版本控制系统**

没有“中央服务器”，每个人的电脑上都是一个完整的版本库，这样，工作时就不需要联网了，因为版本库就在你自己的电脑上。

多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

--------------------------

分布式版本控制系统的安全性要高很多，因为每个人电脑里都有完整的版本库，某一个人的电脑坏掉了不要紧，随便从其他人那里复制一个就可以了。而集中式版本控制系统的中央服务器要是出了问题，所有人都没法干活了。

在实际使用分布式版本控制系统的时候，其实很少在两人之间的电脑上推送版本库的修改。因此，分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只是交换修改不方便而已。

__后面我们还会看到Git极其强大的分支管理功能。__

### Git安装和初步使用

```
git config --global user.name "Pgl"
git config --global user.email "3120201453@bit.edu.cn"
```

### Git创建版本库
 ```shell
 git init 
 # 撰写一个readme.txt文件之后，可以将其添加到版本库中
 git add readme.txt
 # 可以将此版本库提交
 git commit -m "Add a new readme.txt for git learning."
 
 ```

**疑难解答**

Q：输入`git add readme.txt`，得到错误：`fatal: not a git repository (or any of the parent directories)`。

A：Git命令必须在Git仓库目录内执行（`git init`除外），在仓库目录外执行是没有意义的。

Q：输入`git add readme.txt`，得到错误`fatal: pathspec 'readme.txt' did not match any files`。

A：添加某个文件时，该文件必须在当前目录下存在，用`ls`或者`dir`命令查看当前目录的文件，看看文件是否存在，或者是否写错了文件名。



### Git继续干活

干完活之后（比如修改了前面的readme.txt文件），我们可以：

```shell
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .classpath
        .idea/
        .project
        bin/
        lib/
        monitorDemo.iml
        src/

no changes added to commit (use "git add" and/or "git commit -a")
```

查看变动，可以：

```shell
$ git diff
diff --git a/readme.txt b/readme.txt
index 95cc234..c0b8492 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1 +1,2 @@
-This is a demo for prometheus&pushgateway and for git learning.
\ No newline at end of file
+This is a demo for prometheus&pushgateway and for git learning.
+And it is not that difficult, but I just don't wanna learn.
\ No newline at end of file
```

小结：

* 要随时掌握工作区的状态，使用`git status`命令。
* 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。



### Git版本回退

玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。

Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为`commit`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个`commit`恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

版本控制系统有命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看：

```shell
$ git log
commit ffc0e1cc56d15164f2666cb185f4f40cd752ac40 (HEAD -> master)
Author: Pgl <3120201453@bit.edu.cn>
Date:   Thu Feb 18 18:41:31 2021 +0800

    readme.txt

commit f77dc73714025650ff1a87e96f197d5fca2439dd
Author: Pgl <3120201453@bit.edu.cn>
Date:   Thu Feb 18 18:40:23 2021 +0800

    add monitorDemo.iml

commit 9665154999b5a9932c3d0f4fc5404a854549860f
Author: Pgl <3120201453@bit.edu.cn>
Date:   Thu Feb 18 18:39:49 2021 +0800

    add whole project

commit cd2b2537c76ac80d943960c141aaec74c4465a90
Author: Pgl <3120201453@bit.edu.cn>
Date:   Thu Feb 18 17:52:23 2021 +0800

    add a readme file for learning git

```



若是觉得输出信息太多，则可以：

```shell
$ git log --pretty=oneline
ffc0e1cc56d15164f2666cb185f4f40cd752ac40 (HEAD -> master) readme.txt
f77dc73714025650ff1a87e96f197d5fca2439dd add monitorDemo.iml
9665154999b5a9932c3d0f4fc5404a854549860f add whole project
cd2b2537c76ac80d943960c141aaec74c4465a90 add a readme file for learning git
```

类似`ffc0e1...`的是`commit id`（版本号），和SVN不一样，Git的`commit id`不是1，2，3……递增的数字，而是一个SHA1计算出来的一个非常大的数字，用十六进制表示。

为什么`commit id`需要用这么一大串数字表示呢？因为Git是分布式的版本控制系统，后面我们还要研究多人在同一个版本库里工作，如果大家都用1，2，3……作为版本号，那肯定就冲突了。

**下面准备做版本回退**

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交。上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

```shell
$ git reset --hard HEAD^
HEAD is now at f77dc73 add monitorDemo.iml

#看一下当前状态
$ git log --pretty=oneline
f77dc73714025650ff1a87e96f197d5fca2439dd (HEAD -> master) add monitorDemo.iml
9665154999b5a9932c3d0f4fc5404a854549860f add whole project
cd2b2537c76ac80d943960c141aaec74c4465a90 add a readme file for learning git
```

版本回退后，打开其内容，则会发现相关的内容变更为更新前的状态。

想要恢复最新版本，只要sh不关，则可以：

```shell
$ git reset --hard ffc0e1cc56d15164f2666cb185f4f40cd752ac4
HEAD is now at ffc0e1c readme.txt
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把`HEAD`指向回退的版本。



git reflog可以调取所有命令历史记录

```shell
$ git reflog
ffc0e1c (HEAD -> master) HEAD@{0}: reset: moving to ffc0e1cc56d15164f2666cb185f4f40cd752ac4
f77dc73 HEAD@{1}: reset: moving to HEAD^
ffc0e1c (HEAD -> master) HEAD@{2}: commit: readme.txt
f77dc73 HEAD@{3}: commit: add monitorDemo.iml
9665154 HEAD@{4}: commit: add whole project
cd2b253 HEAD@{5}: commit (initial): add a readme file for learning git
```

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。(或者HEAD^~)
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

### Git工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

**工作区（Working Directory）**

就是你在电脑里能看到的目录，比如我的`monitorDemo`文件夹就是一个工作区

__版本库（Repository）__

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。



把文件往Git版本库里添加的时候，是分两步执行的：

* 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到**暂存区**；
* 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到**当前分支**。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

<img src="https://static.liaoxuefeng.com/files/attachments/919020074026336/0">

上面是提交之后的状态。

工作区做出改变，add将改变添加到stage暂存区，commit将改变提交到分支。



### Git管理修改

为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

即git不会对修改做自动跟踪。

可以用：

```shell
$ git diff HEAD -- readme.txt
```

来查看

小结：

* Git的每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。

### Git撤销修改

对工作区的修改进行撤销：

```
$ git checkout -- readme.txt
```

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。



*`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。*

若是已经加入到了暂存区：

Git同样告诉我们，用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

或者

```
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
```

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

最后用checkout丢弃工作区的修改。

现在，假设你不但改错了东西，还从暂存区提交到了版本库，怎么办呢？还记得[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节吗？可以回退到上一个版本。不过，这是有条件的，就是你还没有把自己的本地版本库推送到远程。还记得Git是分布式版本控制系统吗？我们后面会讲到远程版本库，一旦你把`stupid boss`提交推送到远程版本库，你就真的惨了……

小结：

又到了小结时间。

* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。
* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。
* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)一节，不过前提是没有推送到远程库。

### Git删除文件

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`，另一个是`git checkout -- <file>`

`git checkout`其实是**用版本库里的版本替换工作区的版本**，无论工作区是修改还是删除，都可以“一键还原”。



### Git远程仓库

如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别。为了保证你现在所学的Git物超所值，将来绝对不会后悔，同时为了打击已经不幸学了SVN的童鞋，本章开始介绍Git的杀手级功能之一（注意是之一，也就是后面还有之二，之三……）：远程仓库。

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上。怎么分布呢？最早，肯定只有一台机器有一个原始版本库，此后，别的机器可以“克隆”这个原始版本库，而且每台机器的版本库其实都是一样的，并没有主次之分。

你肯定会想，至少需要两台机器才能玩远程库不是？但是我只有一台电脑，怎么玩？

其实一台电脑上也是可以克隆多个版本库的，只要不在同一个目录下。不过，现实生活中是不会有人这么傻的在一台电脑上搞几个远程库玩，因为一台电脑上搞几个远程库完全没有意义，而且硬盘挂了会导致所有库都挂掉，所以我也不告诉你在一台电脑上怎么克隆多个仓库。

实际情况往往是这样，找一台电脑充当服务器的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的提交推送到服务器仓库里，也从服务器仓库中拉取别人的提交。

完全可以自己搭建一台运行Git的服务器，不过现阶段，为了学Git先搭个服务器绝对是小题大作。好在这个世界上有个叫[GitHub](https://github.com/)的神奇的网站，从名字就可以看出，这个网站就是提供Git仓库托管服务的，所以，只要注册一个GitHub账号，就可以免费获得Git远程仓库。

### Git添加远程库

已经在github上建好了一个空的repo。

目前，在GitHub上的这个`monitorDemo`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```shell
$ git remote add origin git@github.com:PenguinLeee/monitorDemo.git
```

（请千万注意，把上面的`michaelliao`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。）

添加后，远程库的名字可以取为`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

```shell
$ git remote add origin git@github.com:PenguinLeee/monitorDemo.git

$ git push -u origin master
The authenticity of host 'github.com (13.229.188.59)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.229.188.59' (RSA) to the list of known hosts.
Enumerating objects: 35, done.
Counting objects: 100% (35/35), done.
Delta compression using up to 4 threads
Compressing objects: 100% (29/29), done.
Writing objects: 100% (35/35), 93.36 KiB | 229.00 KiB/s, done.
Total 35 (delta 5), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (5/5), done.
To github.com:PenguinLeee/monitorDemo.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。



从现在起，只要本地作了提交，就可以通过命令：

```shell
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！



**git 删除远程库**

如果远程库的名字写错了咋办？可以直接把git对应的指针删除掉！

首先调查一手remote仓库的情况：

```shell
Lenovo@Lenovo-PC MINGW64 /a/硕士/学科/现代优化方法 (master)
$ git remote -v
optimization    git@github.com:PenguinLeee/NastoyashyaOptimizatsia.git (fetch)
optimization    git@github.com:PenguinLeee/NastoyashyaOptimizatsia.git (push)
origin  git@github.com:PenguinLeee/NastoyashyaOptimizatsia.git (fetch)
origin  git@github.com:PenguinLeee/NastoyashyaOptimizatsia.git (push)

#其中optimization是我git remote add optimization git@github.com/PenguinLeee/NastoyashyaOptimizatsia.git
得到的
```

下面可以删这个指针了：

```
git remote rm origin
```

则本地git的指针被删除，但是远程的东西仍然在。



### 小结

* 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

* 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；`-u`表示关联；

* 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！



### Git从远程库克隆

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

远程库准备好之后，下一步是用命令`git clone`克隆一个本地库：

你也许还注意到，GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。

```
ssh: git@github.com:PenguinLeee/gitskills.git
https://github.com/PenguinLeee/gitskills.git
```



### 小结

* 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

* Git支持多种协议，包括`https`，但`ssh`协议速度最快。



### Git分支管理

当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！

分支在实际中有什么用呢？假设你准备开发一个新功能，但是需要两周才能完成，第一周你写了50%的代码，**如果立刻提交，由于代码还没写完，不完整的代码库会导致别人不能干活了**。如果等代码全部写完再一次提交，又存在丢失每天进度的巨大风险。

现在有了分支，就不用怕了。你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

其他版本控制系统如SVN等都有分支管理，但是用过之后你会发现，这些版本控制系统创建和切换分支比蜗牛还慢，简直让人无法忍受，结果分支功能成了摆设，大家都不去用。

但Git的分支是与众不同的，无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

### Git创建与合并分支

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

<img src="https://static.liaoxuefeng.com/files/attachments/919022325462368/0">



一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点。

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

<img src="https://static.liaoxuefeng.com/files/attachments/919022363210080/l">



`HEAD`指向的就是当前分支；

Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

<img src="https://static.liaoxuefeng.com/files/attachments/919022387118368/l">

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

<img src="https://static.liaoxuefeng.com/files/attachments/919022412005504/0">

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支。

```shell
# 小坑
$ git checkout master
error: Your local changes to the following files would be overwritten by checkout:
        readme.txt
Please commit your changes or stash them before you switch branches.
Aborting
```

将分支转换成master之后合并dev分支：

```shell
$ git merge dev
Updating d13bad0..bf7ed6d
Fast-forward
 readme.txt | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```

合并完成后，就可以放心地删除`dev`分支了：

```shell
$ git branch -d dev
Deleted branch dev (was bf7ed6d).
```

### switch

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。



小结：

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

### Git解决冲突

### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。



### Git分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```
$ git switch -c dev
Switched to a new branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```
$ git switch master
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...
```

