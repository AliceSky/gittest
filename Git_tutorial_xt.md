<!--
 * git_tutorial_xt.md Desc.
 *
 * @history:
 *  2021-12-08 wangxq,xingxt v0.0.0 Created.
 *
 * Copyright (c) 2021~ wangxq,xingxt.
 -->

# Git——分布式版本控制系统

## Git工作机制
Git 是一个分布式版本控制系统，通常有两个主要用途：代码备份和代码版本控制。

* 集中式和分布式
  
    * 集中式
  
        集中式版本控制系统的版本库集中存放在中央服务器，使用时要先从中央服务器取得最新的版本，使用修改后，再把自己的版本推送给中央服务器，所以必须联网才能工作。
    

    * 分布式

        每个电脑上都有一个完整的版本库，工作时不用一直联网，且中央服务器出问题不影响工作，更安全。
        可将当前工作区的内容提交到本机服务器，不需要联网，版本管理可以保留备份每一个版本，也能回退到之前的某个版本。（均是在本地版本库操作）
        若想通过Git分享代码与其他人协作，需要将数据放到一台其他开发人员能连接的服务器上。

        ![Git 远程仓库](https://www.runoob.com/wp-content/uploads/2015/03/Git-push-command.jpeg)


* 工作流程
    * 克隆 Git 资源作为工作目录。
    * 在克隆的资源上添加或修改文件。
    * 如果其他人修改了，可以更新资源。
    * 在提交前查看修改，提交修改
    
    ![Git工作流程](https://pic4.zhimg.com/v2-c368ef7d5448c26b1a5eeee1644bcfdf_b.jpg)  

## 准备工作

* [github]([GitHub](https://github.com/))注册账号，网页加速方法之一：
    * 查询[cdn节点](https://www.ipaddress.com/)。在搜索框里依次键入
    github.com、github.global.ssl.Fastly.net、codeload.github.com。查询并记录ip地址。

    * 修改hosts文件。目录路径是C:\Windows\System32\drivers\etc\hosts。将以下三行（每行开头ip地址是上一步骤查询结果）追加到hosts文件里，保存退出。

        140.82.114.4 github.com

        199.232.69.194 github.global.ssl.Fastly.net
    
        140.82.112.10 codeload.github.com

    * WIN+R输入cmd，输入命令ipconfig/flushdns,重新打开浏览器

* 下载[git软件](https://git-scm.com/downloads)并安装。安装完成后，在开始菜单里找到  Git->Git Bash，点击后出现一个类似命令行窗口的东西，就说明 Git 安装成功。

* 配置git

    * 登录Git hub账号，找到“New repository"，按照顺序依次查找：New repository --> Repository name "仓库名 " --> Description "基于..... （对这个仓库的描述）" --> Public --> ... README --> Create repository
    
    * 搜索“Git Bash"，输入命令：ssh-keygen -t rsa -C "xxx@qq.com"（注册github账户时使用的邮箱），三次默认回车，按照提示找到密钥地址（xxx/.ssh/id_rsa.pub），并用记事本打开并复制。
    
    * 打开github网站，配置ssh key。步骤：setting --> SSH and GPG keys --> New SSH key --> Title "ssh key" -> Key " 刚刚复制的密钥" --> Add SSH key。

    * 验证是否配置成功，在git bash输入命令 ssh -T git@github.com，第一次配置会让你输入yes或者no，此时输入yes即可。

    * 配置全局用户名和邮箱，打开Git Bash在命令行输入 git config --global user.name "用户名"和git config --global user.email "邮箱"。

## Git基本操作

* 右键git的总文件夹选择git bash, 初始化git，拉取远程仓库到本地，命令：git clone gittest.git(复制git地址注意选择第二个ssh选项，不用输入token)

* 切换到clone下来的仓库中，否则会报错，命令：cd <仓库名>
  
* 在电脑上本地目录添加或修改文件，再把刚刚新建的文件更新到缓存区，命令： git add . 或者 git add <刚刚修改或者新增的文件名>

* 将新更新的缓存区提交到本地仓库，git commit -m'git message'

* 再将本地目的上传至github,此时在github账号下即可看到刚刚更新的内容，命令：git push

* 开始结束前记得拉代码，否则可能忘记更新，命令：git pull

## Git版本回退

* 状态检查，确定文件处于哪种状态，git status（git 包含三种状态:已暂存 （staged）、已修改 （modified）、已提交 （committed））
* 查看具体修改了哪些内容，git diff <被修改的文件名>
* 提交修改和提交新文件时一样的命令，git add <被修改的文件名>
* 查看文件修改的历史记录，git log
* 回退上一版本，git reset --hard HEAD^ （上上一个版本就是HEAD^^，往上100个版本写成HEAD~100）
* 回退到某一版本，版本穿梭前先使用 git log 查看版本库状态,可显示出版本号。然后使用命令 git reset --hard <版本号前5位>
* 回退之后想返回之前版本，用git reflog查看命令历史，以便确定要回到未来的哪个版本，获得版本号。


## Git分支管理

多人协同工作，创建属于自己的分支，别人看不到。可在分支上编写代码，备份一些当前正在处理的代码，但这些代码并不完全稳定。也许你要添加一个新功能，你正在尝试和破坏很多代码，但是你仍然希望保留备份以保存进度。想提交就提交，等开发完毕再一次性合并到原来的分支上。

分支使你可以在不影响master分支的情况下处理代码的单独副本,首次创建分支时，将以新名称创建master分支的完整克隆。然后，你可以独立地在此新分支中修改代码，包括提交文件等。一旦你的新功能已完全集成并且代码稳定，就可以将其合并到master分支中。


![Git流程图](https://pic1.zhimg.com/v2-49c265e6c878e26b314702d1f1cb9ee4_b.jpg)

Git将每次提交的内容串成时间线，这条时间线就是一个分支master。Git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交点。每次提交，master分支都会向前移动一步。不断提交，master分支的线也越来越长。

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

master指向提交点，创建并切换到新分支后，例如新分支名叫dev。

![git-br-create](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

Git 新建dev指针，此时对工作区的修改和提交就是针对dev分支，此时提交一次（add加上commit 操作），dev 指针往前移动一步，而master指针不变。

![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

若想master拥有dev最新提交内容，将master指向dev当前提交即可。

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

合并完成后，删除dev分支（把dev指针删除），只剩下master分支

![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)



实现以上步骤的操作命令如下：

- 新建并切换分支 ：  git switch -c <分支名称>  或者 git checkout -b dev
- 切换分支 ：  git switch <分支名称>  或者  git checkout dev
- 查看分支：   git branch                                    (当前所在分支前有星号)
- 合并分支：   git merge <分支名称>                 (切换到当前分支，将目标分支合并到当前分支)
- 删除分支：   git branch -d <分支名称>


## VSCODE使用Git源管理
* 在一个目录下git clone项目，右击通过VSCODE打开
* 修改文件，点击+，相当于git add. 
  
  ![vscode-git add](https://img-blog.csdnimg.cn/20200427104835452.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODAyMzU1MQ==,size_1,color_FFFFFF,t_70)
  
* 点击对号；等于git commit -m "备注信息"；右边的箭头输入需要备注的信息。然后按 Enter 确定。
  ![git commit](https://img-blog.csdnimg.cn/20200427105002367.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODAyMzU1MQ==,size_1,color_FFFFFF,t_70)

* 提交到远程仓库
  ![git push origin master](https://img-blog.csdnimg.cn/20200427105833445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODAyMzU1MQ==,size_1,color_FFFFFF,t_70)
  
## Commit提交规范  

使用Commit Message Editor插件辅助完成。
每一行都不超过72个字符。

```markdown
{type}{scope}: {description}
空一行
{body}
空一行
{footer}
```

说明：
第一行 header，header是必须的，body和footer可以省略。
{type}:

-   build: 影响构建系统或外部依赖关系的更改
-   feat: 提交新功能（feature）
-   fix: 修复了bug
-   docs:只修改了文档（documentation）
-   perf: 性能优化，提高性能的代码更改
-   style: 不影响代码含义的变化（空白，格式化，缺少分号等）
-   refactor: 重构（即不是新增功能，也不是修改bug的代码变动）
-   test: 添加或修改测试代码
-   chore: 对构建流程或辅助工具和依赖库（如文档生成等）的更改
-   revert: 版本回退
-   file: 增加文件、删除文件、重命名文件

{scope}: 此次提交影响范围

{description}: 简短描述

{body}: 详细描述

{footer}: 补充说明

-   breaking_change: 不兼容改变 及描述
-   Issue: Close #123 #456, #789
-   revert: 版本回退 
-   request： Merge pull request #123 from user/branch

```template
docs(README): good commit messages

    How to Write a Git Commit Message

    - Add message type
    - More detailed description
    - More related information

    BREAKING CHANGE: Use Commit Message Editor

    {type}{scope}: {description}

    {body}

    {footer}

    Closes: #123, #434

    revert: This reverts commit 667ecc1654a317a13331b17617d973392f415f02

    Merge pull request #123 from user/branch
```
## Github flow

## Gitlab flow

Gitlab 是一个基于 git 实现的在线代码仓库软件，一般用于在企业、学校等内部网络搭建一个类似 Github 的网站。在这上面进行多人协作开发就使用 Gitlab flow 工作流程，与 Github flow 有许多相似的地方。
典型开发过程（假设项目名称是a,个人用户名是xyz)

* 在 Gitlab 上找到项目主仓库，假设为 fe / a，将其 fork 到自己的空间，变成 xyz / a，然后 clone 到本地进行开发，命令为：
  
```
git clone git@git.yourcompanyname.com:xyz/a.git
```
（ fork 到自己空间的仓库可以随意修改，除非向项目主仓库提交 MR 并合并，不然不会影响主仓库）

* 新建分支，在该分支上进行新功能开发
```
git checkout -b feat-xxx
```

* 开发自测结束后，将修改提交至本地仓库， 命令为：

```
git add .
git commit -m “feat: 增加xxx新功能”
```

* 检查上游版本是否有改动，命令为：
```
git remote add upstream git@git.zhucangku_name.com:fe/a.git

git pull upstream master --rebase
```
（add upstream 是添加上游地址，即主仓库地址，仅第一次需要；使用 --rebase 便不会产生 merge commit，使 commit 干净清爽）

* 若存在代码冲突，执行以下步骤，否则跳过该步。

    * 切换到开发的主分支，命令为 git checkout <主分支名>

    * 本地主分支同步远程主分支,命令为git  pull或者git fetch
  
    * 切回到自己的私有分支，命令为git checkout <私有分支名>
  
    * 将私有分支与更新的主分支合并，命令为 git merge 主分支名
  
    * 分别执行下列指令，重新提交私有分支到远程仓库
        git add .
        git commit 
        git push
        
    * 手动解决冲突后，执行命令
        git add .
        git rebase --continue
        
* 检查 git log 无误，将分支推送至 origin，命令为：git push origin feat-xxx
  
* 完成后在 Gitlab 上向主仓库发起 Merge Request（MR），请同事进行 code review。——若同事对 MR 提出修改意见，则进入以下步骤，否则跳过该步。
  
    * 按照修改意见修改后，执行命令：git add .
    * git commit --amend --no-edit
    * git pull upstream master --rebase
    * git push origin feat-xxx -f
    （使用 --amend 将这次 commit 与上次 commit 合并，只留下一条 commit 记录。如果要修改上一条的 message，则使用 git commit --amend -m “xxx”; push 的时候使用 -f 是因为修改了分支，需要强制合并） 
* MR 被通过后，在 Gitlab 上点击 merge，代码会被合并到主仓库。此时可删除本地分支了，命令为：
```
git checkout master
git branch -D feat-xxx
git push origin :feat-xxx
```
  
* 更新本地代码和 origin 代码为最新，命令为：git pull upstream master --rebase
git push origin master

* 再次开发则从第 2 步开始即可。



## Git flow

## 参考教程

[Github 简明教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/git-guide.html)

[Git教程 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600)

