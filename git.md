# 安装Git

安装 git sudo apt install git

卸载 sudo apt autoremove
---
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。

# 创建版本库

​    版本库又名仓库，英文名repository，你可以简单理解成一个目录，**这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”**。
1.创建一个空目录
*/usr/local/src/gitstudy*
2.通过git init命令把这个目录变成Git可以管理的仓库
ts@ts-OptiPlex-7080:/usr/local/src/gitstudy$ sudo git init
已初始化空的 Git 仓库于 /usr/local/src/gitstudy/.git/
##把文件添加到版本库
$ git add readme.txt
ts@ts-OptiPlex-7080:/usr/local/src/gitstudy$ sudo git commit -m "readme file begin"
[master （根提交） c2c9514] readme file begin
 1 file changed, 2 insertions(+)
 create mode 100755 readme.txt
**初始化一个Git仓库，使用git init命令。**
添加文件到Git仓库，分两步：
​    **使用命令git add <file>，注意，可反复多次使用，添加多个文件；**
​    **使用命令git commit -m <message>，完成。**

# 版本管理

git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。
git diff  查看difference，知道了对readme.txt作了什么修改。可以从上面的命令输出看到，我们在第一行添加了一个distributed单词。
**要随时掌握工作区的状态，使用git status命令。**
**如果git status告诉你有文件被修改过，用git diff可以查看修改内容。**
## 版本回退
git log 命令可以告诉我们历史记录  可以看到 commit Author Date
git log --pretty=oneline  	可以看到版本号 和 commit -m ""里的内容
首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
git reset --hard HEAD^
git reset --hard 'commitid'
git reflog用来查看记录的每一次命令
**HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。**
**git log可以查看提交历史，以便确定要回退到哪个版本。**
**git reflog查看命令历史，以便确定要回到未来的哪个版本**

## 工作区和暂存区
工作区：电脑里能看到的目录
版本库：工作区一个隐藏的目录.git，不算工作区，而是Git的版本库

![git工作区 版本库](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)

第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。

## 管理修改
每次修改，如果不用git add到暂存区，那就不会加入到commit中。
## 撤销修改
1.还没有git add 提交，想直接丢弃工作区的修改时，用命令**git checkout -- file。**
2.当改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，
第一步用命令**git reset HEAD <file>**，就回到了场景1，第二步按场景1操作。
3.已经提交了不合适的修改到版本库时，要撤销本次提交，参考*版本回退*

## 删除文件
git add test.txt
git commit -m "add test.txt"
rm test.txt
1.确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
git rm test.txt		git commit -m "remove test.txt"  现在，文件就从版本库中被删除了。
2.删错了，因为版本库里还有呢，所以可以把误删的文件恢复到最新版本：
git checkout -- test.txt

# [HEAD  master 与 branch](https://juejin.cn/book/6844733697996881928/section/6844733698047213582)

1. `HEAD` 是指向当前 `commit` 的引用，它具有唯一性，每个仓库中只有一个 `HEAD`。在每次提交时它都会自动向前移动到最新的 `commit` 。
2. `branch` 是一类引用。`HEAD` 除了直接指向 `commit`，也可以通过指向某个 `branch` 来间接指向 `commit`。当 `HEAD` 指向一个 `branch` 时，`commit` 发生时，`HEAD` 会带着它所指向的 `branch` 一起移动。
3. master是 Git 中的默认branch，它和其它branch的区别在于：
   1. 新建的仓库中的第一个 `commit` 会被 `master` 自动指向；
   2. 在 `git clone` 时，会自动 `checkout` 出 `master`。
4. branch的创建、切换和删除：
   1. 创建 `branch` 的方式是 `git branch 名称` 或 `git checkout -b 名称`（创建后自动切换）；
   2. 切换的方式是 `git checkout 名称`；
   3. 删除的方式是 `git branch -d 名称`。

# 远程仓库

## 添加远程库
1.登陆GitHub，创建一个新的仓库：
2.在Repository name填入名字
3.git remote add origin git@github.com:niuyuhao/gitstudy  远程库的名字就是origin
  git push -u origin master 把本地库的所有内容推送到远程库上
  *第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，*
  *还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。*
  git push origin master  把本地的master分支的最新修改推送至GitHub

### [push本质](https://juejin.cn/book/6844733697996881928/section/6844733698047229965)

1. `push` 是把当前的分支上传到远程仓库，并把这个分支的路径上的所有 `commits` 也一并上传。
2. `push` 的时候，如果当前分支是一个本地创建的分支，需要指定远程仓库名和分支名，用 `git push origin 分支name` 的格式，而不能只用 `git push`。
3. `push` 的时候之后上传当前分支，并不会上传 `HEAD`；远程仓库的 `HEAD` 是永远指向默认分支（即 `master`）的。

### 删除远程库
git remote rm <name>
git remote -v 查看远程库信息  然后可根据名字删除
### 小结
要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改。
## 从远程库克隆
要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。
`git clone git@github.com:niuyuhao/仓库name.git    SSH`
`git clone https://github.com/niuyuhao/仓库name.git  https`
Git支持多种协议，包括https，但ssh协议速度最快。

# 分支管理
## 创建与合并分支
[创建与合并分支](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424)

`git checkout -b dev`    创建并切换到dev分支
`git checktout -b` 相当于 `git branch dev` `git checkout dev`两条命令
`git branch`查看当前分支
`git checkout master`切换回master分支
`git merge dev`合并dev分支到当前master分支
`git branch -d dev`  合并完成后，就可以删除dev分支了

```
git工作区 版本库查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name> 或者 git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
```

## **merge：合并commits**

1. 从要合并`commit` 和当前 `commit` （即 `HEAD` 所指向的 `commit`）分叉的位置起，把要合并的 `commit` 的路径上的所有 `commit` 的内容一并应用到当前 `commit`，然后自动生成一个新的 `commit`。

<img src="https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2017/11/21/15fddc2aad5a0279~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp" alt="分支合并" style="zoom:67%;" />

2. merge场景：单独开发的 `branch` 用完了以后，合并回原先的 `branch`；
3. merge的三种情况
   1. 冲突
   2. `HEAD`领先于目标commit：Git什么也不做；
   3. `HEAD`落后于目标commit：fast-forward。

## 解决冲突

1. `git checkout -b feature1`  创建并切换到新的分支feature1

   - 修改readme.txt最后一行
   - 在`feature1`分支上提交
     - `git add readme.txt`  `git commit -m "AND simple"`

2. 切换到master分支`git checkout master` 

   - 在master分支上修改readme.txt文件的最后一行
   - 提交
     - `git add readme.txt `
       `git commit -m "& simple"`

3. `git merge feature1`  此时`readme.txt`文件存在冲突，必须手动解决

4. `git status`

   - ```
     位于分支 master
     您有尚未合并的路径。
       （解决冲突并运行 "git commit"）
       （使用 "git merge --abort" 终止合并）
     
     未合并的路径：
       （使用 "git add <文件>..." 标记解决方案）
     
     	双方修改：   readme.txt
     ```

   - readme内容

     ```
     hello first
     <<<<<<< HEAD
     Creating a new branch is quick & simple.
     =======
     Creating a new branch is quick AND simple
     >>>>>>> feature1
     ```

   - 修改后保存  再提交 

     - `git add readme.txt    git commit -m "conflict fixed"`

5. `git log --graph --pretty=oneline --abbrev-commit`查看合并情况

6. `git branch -d feature1` 删除feature分支

### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用git log --graph命令可以看到分支合并图。

## 分支管理策略

创建并切换dev分支 git checkout -b dev

修改readme 并提交一个新的commit

切回 master

准备合并dev分支 `git merge --no-ff -m "merge with no-ff" dev` 

 `--no-ff`参数，表示禁用`Fast forward`
### 小结
合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。
## Bug分支  3.31
```
1.git checkout -b dev    创建并切换到dev分支
2.touch hello.py 模拟在分支上进行操作  此时想要去创建一个分支去修复bug 并不想提交当前分支
3.git add 之后 git stash 把当前工作现场“储藏”起来 
4.git checkout master 换回master分支 并创建 git checkout -b issue-101
5.在issue-101分支 模拟修复bug 之后 add  commit -m "fix bug 101"
6.切回master  git merge --no-ff -m "merged bug fix 101" issue-101完成合并 
  最后删除issue-101分支  git branch -d issue-101
7.回到dev git checkout dev 通过 git stash list查看储藏的工作现场
8. 1）一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
   2）另一种方式是用git stash pop，恢复的同时把stash内容也删了：
   你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
   git stash apply stash@{0}
9. git cherry-pick commitID 让我们能复制一个特定的提交到当前分支,避免重复劳动
```

## Feature分支
`git checkout -b feature-vulcan` 准备一个新的分支
`git add vulcan.py` `git commit -m "add feature vulcan"`
返回dev 准备合并
`git branch -d feature-vulcan`         feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。
`git branch -D feature-vulcan`
## 多人协作

`git remote -v` 显示更详细的信息

```
origin	git@github.com:niuyuhao/gitstudy (fetch)
origin	git@github.com:niuyuhao/gitstudy (push)
```
上面显示了可以抓取和推送的`origin`的地址。
### 推送分支
`git push origin master`
`git push origin dev`推送dev分支
### 抓取分支
1. 创建一个新的文件夹`git clone git@github.com:michaelliao/learngit.git`
    当从远程库clone时，默认情况下，只能看到本地的master分支。
    现在要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：`$ git checkout -b dev origin/dev`
    现在可以在dev上继续修改，然后，时不时地把dev分支push到远程：
    `git add xxx ; git commit -m "asdsadfasf"` `git push origin dev`
2. 此时在另一个文件夹对同样的文件进行了修改  git add xxx    git commit -m "132154"
  - `git push origin dev` 推送失败提交有冲突。先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送。
  - git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：`git branch --set-upstream-to=origin/dev dev`
  - 再  git pull成功，但是合并有冲突。[解决冲突](https://www.liaoxuefeng.com/wiki/896043488029600/900004111093344) 解决后 再提交`git commit -m "131231"`，再push  `git push origin dev`

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。
### 小结
- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## [Feature Branching工作流：最流行的工作流](https://juejin.cn/book/6844733697996881928/section/6844733698051407885)

这种工作流的核心内容可以总结为两点：

1. 任何新的功能（feature）或 bug 修复全都新建一个 `branch` 来写；
2. `branch` 写完后，合并到 `master`，然后删掉这个 `branch`。

<img src="git/15fde6edbfe362c4tplv-t2oaga2asx-zoom-in-crop-mark1304000.webp" style="zoom:67%;" />

创建一个分支 例如dev，在dev分支上进行开发工作。完成之后push到中央仓库，等其他人拉取review，确认可以合并到master分之后，合并完push之后删除分支。

如果代码确认后需要修改的话，修改之后做一个新的提交，再push上去。

### Pull Request

Pull Request可以让团队的成员方便地讨论一个branch，并在结束之后一键合并这个`branch`和`master`。

## 关于add

add . 把工作目录下的所有改动全部放进暂存区

add 添加的是文件的改动，而不是文件名

```
比如 先修改了xx.txt，然后将它add进了暂存区
然后又往xx.txt里写了几行东西。此时新改动不会被添加到暂存区。
```



## 01 [Rebase](https://juejin.cn/book/6844733697996881928/section/6844733698055602183)

`rebase` 的意思是，给你的 `commit` 序列重新设置基础点（也就是父 `commit`）。展开来说就是，把你指定的 `commit` 以及它所在的 `commit` 串，以指定的目标 `commit` 为基础，依次重新提交一次。

- 在master分支  
  - `git checkout branch1` 
  - `git rebase master`
  - `git checkout master`
  - `git merge branch1`

`master 分支的 git merge xx分支 = xx分支的git rebase master + master分支 git merge xx分支`

## 02 刚刚提交的代码，发现写错了怎么办

`git commit --amend`可以修复当前提交的错误

```
修改之后  git add  
git commit --amend   修复并且可以添加  -m " " 里的内容
```

需要注意的有一点：`commit --amend` 并不是直接修改原 `commit` 的内容，而是生成一条新的 `commit`。

## 03 写错的不是最新的提交，而是倒数第二个？

1. `git rebase -i HEAD^^`
2. 选择 commit 和对应的操作   旧的 `commit` 会排在上面，新的排在下面。
   - 把`pick`改成`edit`  退出
   - 修改  ----> add  ----->  git commit --amend
3. 修复完成之后用 `rebase --continue` 来继续 `rebase` 过程，把后面的 `commit` 直接应用上去

```
说明：在 Git 中，有两个「偏移符号」： ^ 和 ~。

^的用法：在 commit 的后面加一个或多个 ^ 号，可以把 commit 往回偏移，偏移的数量是 ^ 的数量。例如：master^ 表示 master 指向的 commit 之前的那个 commit; HEAD^^ 表示 HEAD 所指向的 commit 往前数两个 commit。

~ 的用法：在 commit 的后面加上 ~ 号和一个数，可以把 commit 往回偏移，偏移的数量是 ~ 号后面的数。例如：HEAD~5 表示 HEAD 指向的 commit往前数 5 个 commit。
```

## 04 比错还错，想直接丢弃刚写的提交？

**`git reset --hard 目标commit`**
git reset --hard HEAD^		前一个

## 05  想丢弃的也不是最新的提交？

撤销倒数第二条commit
`git rebase -i HEAD^^`   >  删除要撤销的commit  >  修改冲突并提交 > `git rebase --continue`

## 06代码已经 push 上去了才发现写错？

1. 出错的内容在自己的branch
   - 把写错的从哦秘密他修改或者删除，然后再push上去
     - 由于在本地对已有的 `commit` 做了修改，这时你再 `push` 就会失败，因为中央仓库包含本地没有的 `commit`s。此时用本地的内容覆盖掉中央仓库的内容`git push origin branch -f`  -f 是 --force的缩写，《忽略冲突，强制`push`》
2. 出错的内容已经合并到master
   - 在这种时候，只能选用另一种策略：增加一个新的提交，把之前提交的内容抹掉。例如之前你增加了一行代码，你希望撤销它，那么你就做一个删掉这行代码的提交；如果你删掉了一行代码，你希望撤销它，那么你就做一个把这行代码还原回来的提交。这种事做起来也不算麻烦，因为 Git 有一个对应的指令：`revert`。
     - 希望撤销哪个`commit`,就把它填在后面：
       - `git revert HEAD^`
       - 上面这行代码就会增加一条新的 `commit`，它的内容和倒数第二个 `commit` 是相反的，从而和倒数第二个 `commit` 相互抵消，达到撤销的效果。
       - 在 `revert` 完成之后，把新的 `commit` 再 `push` 上去，这个 `commit` 的内容就被撤销了。它和前面所介绍的撤销方式相比，最主要的区别是，这次改动只是被「反转」了，并没有在历史中消失掉，你的历史中会存在两条 `commit` ：一个原始 `commit` ，一个对它的反转 `commit`。

## 07 reset 的本质——不止可以撤销提交

**reset 的本质：移动 HEAD 以及它所指向的 branch**

`reset可以撤销commit，但它的实质性为并不是撤销，而是移动HEAD，并且捎待上HEAD所指向的branch（有的话）`。它是用来重置HEAD以及它所指向的`branch`的位置的。

`git reset --hard branch2` 可以把HEAD 和 branch移动到branch2

**reset --hard: 重置工作目录**

`reset --hard` 会在重置 `HEAD` 和 `branch` 的同时，重置工作目录里的内容。当你在 `reset` 后面加了 `--hard` 参数时，你的工作目录里的内容会被完全重置为和 `HEAD` 的新位置相同的内容。换句话说，就是你的未提交的修改会被全部擦掉。

1. 在一次`commit`之后,进行修改
2. 执行`git  reset --hard HEAD^`之后，HEAD 和 当前 branch切到上一条commit的同时，工作目录里的新改动也一起全都消失了，不管是否被放进暂存区。

**reset --soft: 保留工作目录**

`reset --soft` 会在重置 `HEAD` 和 `branch` 时，保留工作目录和暂存区中的内容，并把重置 `HEAD` 所带来的新的差异放进暂存区。

`--hard` 会清空工作目录的改动，而 `--soft` 则会保留工作目录的内容，并把因为保留工作目录内容所带来的新的文件差异放进暂存区。

**reset 不加参数：保留工作目录，并清空暂存区**

`reset` 如果不加参数，那么默认使用 `--mixed` 参数。它的行为是：**保留工作目录，并且清空暂存区。**

```
--hard：重置位置的同时，清空工作目录的所有改动；
--soft：重置位置的同时，保留工作目录和暂存区的内容，并把重置 HEAD 的位置所导致的新的文件差异放进暂存区。
--mixed（默认）：重置位置的同时，保留工作目录的内容，并清空暂存区。
```

## 08 checkout的本质

实质上，`checkout` 并不止可以切换 `branch`。`checkout` 本质上的功能其实是：
把 `HEAD` 指向指定的 `branch`，然后签出这个 `branch` 所对应的 `commit` 的工作目录。所以不止可以切换branch，也可以直接指定`commit`作为参数，来把HEAD移动到指定的`commit`

### checkout和reset的不同

`reset` 在移动 `HEAD` 时会带着它所指向的 `branch` 一起移动，而 `checkout` 不会。当你用 `checkout` 指向其他地方的时候，`HEAD` 和 它所指向的 `branch` 就自动脱离了。

`git checkout --detach`

执行这行代码，Git 就会把 `HEAD` 和 `branch` 脱离，直接指向当前 `commit`：

## 09 stash 临时存放工作目录的改动

参考Bug分支3.31

## 10branch 删过了才想起来有用？

`git reflog` 查看branch被删除之前的位置的commit。

`git checkout commit`

`git checkout -b branch`再重新创建回来

`reflog` 默认查看 `HEAD` 的移动历史，除此之外，也可以手动加上名称来查看其他引用的移动历史，例如某个 `branch`：

git reflog master

## 忽略文件

