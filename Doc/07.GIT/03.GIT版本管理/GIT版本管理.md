# GIT版本管理

## git 发布版本

1.发布版本命令

```
git tag -a v1.0.0 -m"发布初始版本，后续可在此版本上修改应用程序"
```

-a 证明这个标签是注解的

-m 标签消息在本地打上标签

![](media/image-20200413111230767.png)

2. 使用命令`git show v1.0.0`（v1.0.0是标签名），来查看某标签的信息

![](media/image-20200413111422829.png)

3. 确认无误后，可以将其通过`git push origin v1.0.0`命令推送到远程库，当然，较为罕见的情况，你需要推送多个标签 `git push origin --tags`来代替

![](media/image-20200413111524751.png)

![](media/image-20200413112008233.png)

4. 删除标签并推送到远端     

   `git push origin :refs/tags/v1.0.0                                             `    

![](media/image-20200413111908826.png)

![](media/image-20200413112032696.png)



##  git分支管理

1. 查看所有分支     `git branch --all`    

   ![](media/image-20200413112328109.png)

2. 