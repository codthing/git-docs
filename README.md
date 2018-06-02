# git-docs
git 的使用文档小总结

注释: Unix 的哲学是“没有消息就是好消息”

# 一. 开始工作

### (一). Git 命令窗口

使用Git Bash  

### (二). 初始化用户

```
$ git config --global user.name "用户名"`
$ git config --global user.email "用户邮箱"
```
### (三). 创建版本库

1.创建一个空目录

```
$ mkdir <目录名>

```
2.将目录变成可管理的仓库

```
$ git init

```
### (四). 把文件添加到版本库

1.`git add` 把文件添加到仓库,可反复多次使用，添加多个文件

```
$ git add <文件名>` 或 `$ git add .

```
2.`git commit` 把文件一次性提交到仓库

```
$ git commit -m "注释说明"

```
# 二. 时光穿梭

### (五). 查看状态

1.查看版本库状态

```
$ git status

```
2.查看修改的内容

```
$ git diff <文件名>

```
### (六). 版本回退

1.查看历史记录

```
$ git log

```
*HEAD表示当前版本*

2.回退到指定版本

```
$ git reset --hard <版本ID>

```
3.查看内容

```
$ cat <文件名>

```
4.查看命令历史

```
$ git reflog

```

### (七). 工作区和暂存区(HEAD是指针)

1.`git add .` 或 `git add <文件名>`

<img src="https://cdn.liaoxuefeng.com/cdn/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0" />

2.`git commit -m "注释说明"`

<img src="https://cdn.liaoxuefeng.com/cdn/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0" />

### (八). 管理修改

1.如果不用 `git add` 到暂存区，那就不会加入到 commit 中

2.用 `git diff HEAD -- <filename>` 命令可以查看工作区和版本库里面最新版本的区别

### (九). 撤销修改

1.场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

2.场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

3.场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### (十). 删除文件

1.`git rm <文件名>` 删掉，并且 `git commit -m "注释说明"`

2.一键还原最新版 `git checkout -- <文件名>`

# 三. 远程仓库

(十一). 秘钥

- 生成秘钥(C是大写)

1.`ssh-keygen -t rsa -C "用户邮箱"`

*用户主目录里找到.ssh目录，里面有 id_rsa 和 id_rsa.pub 两个文件，这两个就是 SSH Key 的秘钥对， id_rsa 是私钥，不能泄露出去， id_rsa.pub 是公钥，可以放心地告诉任何人。*

2.登陆 GitHub，打开 “Account settings”，“SSH Keys” 页面,
然后，点 “Add SSH Key”，填上任意 Title，在 Key 文本框里粘贴   id_rsa.pub 文件的内容

### (十二).添加远程库

1.登陆 GitHub，然后，在右上角找到 “Create a new repo” 按钮，创建一个新的仓库,在 Repository name 填入本地版本库名(必须一致)，其他保持默认设置，点击 “Create repository” 按钮，就成功地创建了一个新的 Git 仓库

2.链接远程库

```
git remote add origin git@github.com:<用户名>/<版本库名>.git

```
3.第一次推送所有内容

```
git push -u origin master

```
4.之后推送所有的内容可简化为

```
git push origin master

```
### (十三).从远程库克隆(相当于建立了链接)

```
git clone <远程库地址>

```
# 四. 分支管理

### (十四).创建与合并分支

1.查看分支(*表示当前分支*)

```
git branch

```
2.创建分支

```
git branch <分支名>

```
3.切换分支

```
git checkout <分支名>

```
4.创建+切换分支

```
git checkout -b <某分支名>

```
5.合并某分支到当前分支

```
git merge <某分支名>

```
6.删除分支

```
git branch -d <某分支名>

```
### (十五).解决冲突

- 当 Git 无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。


- 解决冲突就是把 Git 合并失败的文件手动编辑为我们希望的内容，再提交。


- 用 `git log --graph` 命令可以看到分支合并图。

### (十六).分支管理策略

- 在实际开发中，我们应该按照几个基本原则进行分支管理：

1.首先，master 分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

2.那在哪干活呢？干活都在 dev 分支上，也就是说，dev分支是不稳定的，到某个时候，比如 1.0 版本发布时，再把 dev 分支合并到 master 上，在 master 分支发布1.0版本；

3.你和你的小伙伴们每个人都在 dev 分支上干活，每个人都有自己的分支，时不时地往 dev 分支上合并就可以了。

- 所以，团队合作的分支看起来就像这样：

<img src="https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0">


- Git分支十分强大，在团队开发中应该充分应用。

- 默认 Git 会用 Fast forward 模式，但这种模式下，删除分支后，会丢掉分支信息

- 合并分支时，加上 --no-ff 参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 fast forward 合并就看不出来曾经做过合并。

- 准备合并 dev 分支，请注意 --no-ff 参数，表示禁用 Fast forward：

```
$ git merge --no-ff -m "merge with no-ff" dev

```

>因为本次合并要创建一个新的 commit，所以加上 -m 参数，把 commit 描述写进去。

<img src="https://cdn.liaoxuefeng.com/cdn/files/attachments/001384909222841acf964ec9e6a4629a35a7a30588281bb000/0">


### (十七).Bug分支

1.当手头工作没有完成时，先把工作现场藏存一下，然后去修复 bug

```
git stash

```
2.修复后，再回到工作现场

```
git stash pop

```
3.查看stash藏储

```
git stash list

```
### (十八).Feature分支

1.开发一个新 feature，最好新建一个分支；

2.丢弃一个没有被合并过的分支

```
git branch -D <分支名>

```
### (十九).多人协作

1.查看远程库信息

```
git remote -v

```
2.本地新建的分支如果不推送到远程，对其他人就是不可见的

3.从本地推送分支，如果推送失败，先用 `git pull` 抓取远程的新提交

```
git push origin <分支名>

```
4.在本地创建和远程分支对应的分支，本地和远程分支的名称最好一致

```
git checkout -b <本地分支名> origin/<远程分支名>

```
5.建立本地分支和远程分支的关联

```
git branch --set-upstream <本地分支名> origin/<远程分支名>

```
6.从远程抓取分支，如果有冲突，要先处理冲突

```
git pull

```
### (二十).Rebase

1.把本地未 push 的分叉提交历史整理成直线

```
git rebase

```
- `git rebase` 的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# 五.标签管理

- 注: commit_id 是 6a5819e..., 而 tag 是 v1.2, 所以更易于人类阅读

### (二十一).创建标签

1.新建一个标签，默认为 HEAD，也可以指定一个 commit_id；

```
git tag <tagname>

```
2.指定标签信息

```
git tag -a <tagname> -m "blablabla..."

```
3.查看所有标签

```
git tag

```
4.查看说明文字

```
git show <tagname>

```
### (二十二).操作标签

1.推送一个本地标签

```
git push origin <tagname>

```
2.推送全部未推送过的本地标签

```
git push origin --tags

```
3.删除一个本地标签

```
git tag -d <tagname>

```
4.删除一个远程标签

```
git push origin :refs/tags/<tagname>

```

# 六.使用GitHub

- 在 GitHub 上，可以任意 Fork 开源仓库

- 自己拥有 Fork 后的仓库的读写权限

- 可以推送 pull request 给官方仓库来贡献代码。

<img src="https://cdn.liaoxuefeng.com/cdn/files/attachments/001384926554932eb5e65df912341c1a48045bc274ba4bf000/0">

# 七.使用码云

- 注意，git 给远程库起的默认名称是 origin，如果有多个远程库，我们需要用不同的名称来标识不同的远程库

1.先删除已关联的名为 origin 的远程库

```
git remote rm origin

```
2.然后，先关联 GitHub 的远程库

```
git remote add github git@github.com:<用户名>/<版本库名>.git

```
3.接着，再关联 码云 的远程库

```
git remote add gitee git@gitee.com:<用户名>/<版本库名>.git

```
4.查看远程库信息，可以看到两个远程库

```
git remote -v

```
5.如果要推送到GitHub，使用命令

```
git push github master

```
6.如果要推送到码云，使用命令

```
git push gitee master

```

- 这样一来，我们的本地库就可以同时与多个远程库互相同步

# 八.自定义Git

### (二十三).忽略特殊文件

- 忽略文件的原则是

1.忽略操作系统自动生成的文件，比如缩略图等

2.忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 Java 编译产生的 .class 文件

3.忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件

- 不需要从头写 .gitignore 文件，GitHub 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore


- 有些时候，你想添加一个文件到 Git，但发现添加不了，原因是这个文件被 .gitignore 忽略了

```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.

```
- 如果你确实想添加该文件，可以用 -f 强制添加到 Git

```
$ git add -f App.class

```
- 或者你发现，可能是 .gitignore 写得有问题，需要找出来到底哪个规则写错了，可以用 git check-ignore 命令检查

```
$ git check-ignore -v App.class
.gitignore:3:*.class    App.class

```
### (二十四).配置别名,例如用 st 替代 status

```
$ git config --global alias.st status

```

### (二十五).搭建Git服务器

(待续)
