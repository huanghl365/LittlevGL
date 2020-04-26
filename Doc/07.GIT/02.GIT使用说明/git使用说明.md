# GIT使用

## SSH key

1. SSH key目的 可以让你在你的电脑和Code服务器之间建立安全的加密连接

2. 查看本地电脑是否已经有ssh key：win一般在C:\Users\Administrator.ssh路径，linux一般在路径 ~/.ssh/id_rsa.pub，如果存在则跳过该步骤，否则执行以下命令生成ssh key。

```
$ ssh-keygen -t rsa -C "xy_m@163.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa):
```

注：以上让输入SSH key的路径，直接确定在默认路径下即可。

3. 打开刚才生成的路径，发现里面有三个文件：

![](media/1578836888303.png)

拷贝id_rsa.pub里面的数据到剪切板。

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC8U7lfN5/t8bQzlm5r/T0L/nXW8NKioELuUiUp+QACTTY+UfUo5Jqzuy+8dwhAqgYgrIzXE8k/bLmB/+OHpjVP+AbMJCYn3Lnsr+bDZ1UC6YYcgWMfkzcyH9rgqQdg0wpJOoOO+PrFqhuozx7lL32WavUK7Ggh6xKq0Yc75sxWgAzZGrzpnbsRzkHjv4anhR9Pe4uxLmgiB7hxEsulh6zP7Bvq0+gyc3x2i/5AjskpMTdLysFEBit0YkLAlopn/KbFve0kgmhALJkozjDovATRb/nA7BAm6DBSO2MVQqJFhTU39ta5AEwyp0YG6STkcJU+Ciwe9HGF1RZ6ZaO6C/zV4DJy/pTv9QtEL/jkihjz4idlq9a2sWjqcgrqa1ZUL6kustkcEvkgbCXtusck311mSIq0KARgSyrFPzWW/eLq4hUM1ufgxGgBkq8ksIjYf4u9JCuJJ1r4d0NQKTU2+kIi0KKDvync7CnI7+4Fi/nirQjDtSBNkHl1cYIeoLE= [xy_m@163.com](http://mailto:xy_m@163.com)

4. 到自己的github账户里面添加即可：[github](https://github.com/) 选择右上角头像选择 setings--> SSH and GPG keys，添加即可。 ![](media/1.jpg)

检查是否添加成功，在本地终端执行以下命令

```
$ ssh -T git@github.com
Hi RobotFly! You've successfully authenticated, but GitHub does not provide shell access.
```

## Github远程厂库

![](media/1578836933919.png)

## 创建本地厂库

1. 首先在本地配置git，目的是和code服务器打交道的时候，识别你是谁

```
git config --global user.name "RobotFly"
git config --global user.email "xy_m@163.com"
```

2. 创建本地厂库

```
git init
```

3. 如果在工程目录下添加新文件【readme.txt】查看状态如下，提示

   ![](media/image-20200402113453305.png)





4. 提交文件到本地厂库（暂缓区->本地厂库）

![git-repo](media/0.jpg)

执行如下命令：

```
git commit -m "自己提交log"
```

5.关联本地厂库到远程厂库

```
git remote add origin git@github.com:RobotFly/4G.git
```

6.第一次提交本地厂库代码到远程厂库，后面只需git push origin master即可

```
git push -u origin master
```

如果出现

```
error: failed to push some refs to 'git@github.com:RobotFly/4G.git'
```

说明远程有文件 而本地缺少该文件，只需先拉下来在提交即可，如下：

```
git pull --rebase origin master
```

7.克隆远程厂库

```
git clone git@github.com:RobotFly/4G.git
```

8.查看提交日志

```
git log
```

如果嫌弃输出信息太多，执行以下命令

```
git log --pretty=oneline
```

9.回退到上一个版本

```
git reset --hard HEAD^
```

10：回退到指定版本 1094a是版本号 可以用git log查看 版本号 git reset --hard 1094a

注意：回退后当前版本历史log 在终端没有关闭的时候还是可以用git log查看回退前的版本号，然后用 `git reset --hard 1094a`方式在更新到最新版本，如果已经关闭终端，只能用git reflog了

11.历史命令 可以查看历史所有命令 这样就可以追随版本号

```
git reflog 
```

12.查看暂缓区状态

git status 可以查看暂缓区的状态

1. 查看工作区和最新的版本区别

   ```
   git diff HEAD -- readme.txt
   ```

14.版本回退 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本2-4章节，不过前提是没有推送到远程库。

15.删除文件

如果直接在本地把文件删除了 用git status 查看工作区和最新的版本区别，git会告诉你test.txt被删除了

$ git status On branch master Changes not staged for commit: (use "git add/rm <file>..." to update what will be committed) (use "git checkout -- <file>..." to discard changes in working directory)

```
deleted:    test.txt
```

no changes added to commit (use "git add" and/or "git commit -a")

a:确实要从版本库中删除该文件,执行: git rm test.txt b:删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：只能是当前最新的 git checkout -- test.txt

c：从来没有被添加到版本库就被删除的文件，回收站里面找吧，否则是无法恢复的！

查看远程厂库地址

```
book@pc:~/NUC970_GIT_SDK/NUC970_Linux_Kernel$ git remote -v
origin  git@github.com:RobotFly/NUC970_Linux_Kernel.git (fetch)
origin  git@github.com:RobotFly/NUC970_Linux_Kernel.git (push)
```

## window 下使用Byound Commpare作为git的比较工具

Byound Commpare安装路径：D:/Program Files (x86)/Beyond Compare/BCompare.exe 打开C:\Users\Administrator.gitconfig

添加以下内容保存即可：

```
[gui]
    encoding = utf-8
[diff]
    tool = bc4
[difftool]
    prompt = false
[difftool "bc4"]
    cmd = "\"D:/Program Files (x86)/Beyond Compare/BCompare.exe\" \"$LOCAL\" \"$REMOTE\""
[merge]
    tool = bc
[mergetool]
    prompt = false
    keepBackup = false
[mergetool "bc"]
    cmd = "\"D:/Program Files (x86)/Beyond Compare/BCompare.exe\" \"$LOCAL\" \"$REMOTE\" \"$BASE\" \"$MERGED\""
[user]
	name = FlyRobot
	email = xyan_m@163.com
```

然后执行git diff的时候不影响以前的使用，当执行 git difftool时候，就会启动BCompare.exe来作为工具对比。 

![](media/1578837038566.png)

##  ubuntu下使用Byound Commpare作为git的比较工具