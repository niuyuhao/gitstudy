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
# 远程仓库
add SSH Key 在Key文本框里粘贴id_rsa.pub文件的内容
## 添加远程库
1.登陆GitHub，创建一个新的仓库：
2.在Repository name填入名字
3.git remote add origin git@github.com:niuyuhao/gitstudy  远程库的名字就是origin
  git push -u origin master 把本地库的所有内容推送到远程库上
  *第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，*
  *还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。*
  git push origin master  把本地的master分支的最新修改推送至GitHub

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
[创建与合并分支][https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424]

`git checkout -b dev`    创建并切换到dev分支

`git checktout -b` 相当于 `git branch dev` `git checkout dev`两条命令

`git branch`查看当前分支

`git checkout master`切换回master分支

`git merge dev`合并dev分支到当前master分支

`git branch -d dev`  合并完成后，就可以删除dev分支了

```
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name> 或者 git switch <name>
创建+切换分支：git checkout -b <name>或者git switch -c <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>
```

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
## Bug分支
## Feature分支
## 多人协作
## Rebase
