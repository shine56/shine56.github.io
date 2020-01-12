---
title: 同步hexo配置文件到git page分支
date: 2019-12-26 17:16:21
edited:
tags: hexo博客搭建
categories:
---
## 为什么要同步hexo配置文件？

 - 问：通过执行hexo g; hexo d; 我的本地文件不是已经同步到github了吗?
 - 答：**不！** 并没有。让我们看一下博客本地目录
<!-- more -->
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122623570271.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
当我们执行完hexo g，hexo d；时，hexo只会帮我们做两件事情：
1、将md文件以及其他你在hexo 所做的配置，**转化成静态网页文件**（例如：html、css等），而转化之后的文件会放在 **.deploy_git**（上图箭头所指文件）这个文件夹中。
2、把 **.deploy_git** 文件夹 **推送到github仓库**。

OK，这就意味着什么呢，意味着你可能会遇到一个悲伤的故事：如果某一天你像我一样把博客本地文件丢了。你只能在xxx.github.io这个仓库找到.deploy_git目录下的静态网页文件。**md格式的博文、hexo根目录配置、主题目录配置都没了。** 当然你的博客网站还是可以访问的，毕竟你的网站一直托管在github page上面。但是你如果想再像之前那样修改你的博客就不行了。

* 所以我也吸取教训，在xxx.github.io新开一个分支，同步的配置文件。
* 没必要新开一个仓库来管理博客，而且也不好管理。

## 在xxx.github.io仓库新开一个分支![在这里插入图片描述](https://img-blog.csdnimg.cn/20191227002301999.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
我新开的是 **source** 分支。然后将该分支设为**默认分支**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191227005654777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
## 同步本地文件到分支
* 找个地方新建一个文件夹，在这里**git clone**你的xxx.github.io仓库
* git clone结束后进入xxx.github.io文件夹。这时候执行 git branch命令你会发现你正处于**source分支**下（因为前面你已经在github将source设置为默认分支了）
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191227003927776.png)
* **删除**xxx.github.io目录**除了.git**的剩下其他所有目录。
* 把你**想要同步的文件**放到该目录下。我想同步我的.md文件所以我把本地博客文件的source文件**复制**了过来。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122700433131.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjYxOTg1Ng==,size_16,color_FFFFFF,t_70)
接下来便是git常规操作，执行下面命令
```git
// 删除所关联的远程仓库地址
$ git remote rm origin
//关联新的远程仓库
$ git remote add origin https://github.com/youName/youName.github.io.git
$ git add .
$ git commit -m "something"
//推送到远程库source分支
$ git push orign source
//失败则执行
$ git pull origin sourse
//再执行
$ git push orign source
```
最后查看远程库的source分支便能看到上传的文件了。