---
layout: post
title: "Git | Git分支管理-Git Flow"
date: 2019-12-21 
description: "Git | Git分支管理-Git Flow"

tag: Git 
---   

## Git分支管理-Git Flow

-----

## 一、什么是Git Flow

### 1、为什么要有分支管理

​	大部分开发人员现在使用Git就只是用三个，两个分支，一个是Master, 一个是Develop, 还有一个是基于Develop打得各种分支，甚至有的团队就一个Master分支。这个在小项目规模的时候还勉强可以支撑，因为很多人做项目就只有一个Release, 但是人员一多，而且项目周期一长就会出现各种问题。就像代码需要代码规范一样，代码管理同样需要一个清晰的流程和规范。

### 2、什么是Git Flow

​	Vincent Driessen曾经写过一篇博文，题为“[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)“（一个成功的Git分支模型）。Gitflow工作流程就是从这篇文章里来的。下面是Git Flow的流程图

![1Git Flow流程图.png](http://q33kciz5a.bkt.clouddn.com/Git Flow模型.png)

<center>Git Flow流程图</center>
​	Gitflow工作流程围绕项目发布定义了严格的分支模型。尽管它比Feature Branch Workflow更复杂一些，但它也为管理更大规模的项目提供了坚实的框架。

###3.Git Flow常用的分支

- **用于记录历史的分支：**Master和develop

  分支命题

  ​	Master：版本（标签/tag）号

  ​	develop：develop/迭代周期

  Gitflow使用两个分支来记录项目开发的历史，而不是使用单一的master分支。在Gitflow流程中，master只是用于保存官方的发布历史，而develop分支才是用于集成各种功能开发的分支。**使用版本号为master上的所有提交打版本（标签/tag）。**
  
  ![2Master和develop.png](http://q33kciz5a.bkt.clouddn.com/gitflow:Master和develop.png)
  
  <center>Master和develop</center>

- **用于功能开发的分支：**Feature

  分支命名

  ​	Feature：reature/taskName(任务/功能名)

  **每一个新功能的开发都应该各自使用独立的分支。**为了备份或便于团队之间的合作，这种分支也可以被推送到中央仓库。但是，*在创建新的功能开发分支时，父分支应该选择develop（而不是master）。***当功能开发完成时，改动的代码应该被合并（merge）到develop分支。功能开发永远不应该直接牵扯到master。**

  ![3Feature.png](http://q33kciz5a.bkt.clouddn.com/Feature.png)

  <center>Feature</center>

- **用于发布的分支：**Release

  分支命名

  ​	Release：release/版本（标签/tag）号

  一旦develop分支积聚了足够多的新功能（或者预定的发布日期临近了），你可以基于develop分支建立一个用于产品发布的分支。**建立Release分支后，我们只能在这个Release分支上测试，修改Bug等**。同时，其它开发人员可以基于这条release分支开发新的Feature，但是这个新的Feature分支只能合并入下一个版本
  **记住：一旦打了Release分支之后不要从Develop分支上合并新的改动到Release分支。发布Release分支时，合并Release到Master和Develop， 同时在Master分支上打个Tag记住Release版本号，然后可以删除Release分支了。**

  ![4Release.png](http://q33kciz5a.bkt.clouddn.com/gitflow:Release.png)

  <center>Release</center>

- **用于维护的分支：**hotfix

  分支命名

  ​	hotfix/bugCode（缺陷号）

  发布后的维护工作或者紧急问题的快速修复也需要使用一个独立的分支。这是唯一一种可以直接基于master创建的分支。一旦问题被修复了，所做的改动应该被合并入master和develop分支（或者用于当前发布的分支）。在这之后，master上还要使用更新的版本号打好版本（标签/tag）号。

  ![5hotfix.png](http://q33kciz5a.bkt.clouddn.com/gitflow:hotfix.png)

  <center>hotfix</center>

----------------

## 二、实际应用

### 1、角色说明及对应虚拟人物

- 管理员（Master）[拥有建立和管理GIT项目，合并分支和代码，master打tag版本权限]
- 开发者（Developer）[拥有浏览，push非主分支，提交mr（merge request）工单权限]
- 浏览者（Guest）[如：测试人员，只能浏览，无push，mr等所有写权限]

### 2、实际操作

1. 管理员初始化Master

   ![master.png](http://q33kciz5a.bkt.clouddn.com/gitflow:master.png)

   <center>master</center>

2. 管理员创建develop分支

   简单的做法：让一个管理员在本地建立一个空的develop分支（如，2019第一个迭代，就可以命名为develop/2019-001），然后把它推送到服务器。develop分支将包含项目在某个开发阶段的所有历史，而master会是一个缩减版本。

   ```shell
   git branch develop/2019-001
   git push -u origin develop/2019-001
   ```

   

   ![创建develop分支.png](http://q33kciz5a.bkt.clouddn.com/gitflow:创建develop分支.png)

   <center>从maser上拉develop分支</center>

3. 管理员创建开发分支Feature，开发人员拉取对应的开发分支进行开发

   - 管理员创建开发分支Feature

     现在有两个开发任务，（如，一个叫用户管理（user），一个叫角色管理（role）），管理员在创建这两条开发分支时，父分支不能选择master，而要选择develop。

     ```shell
     # 从develop/2019-001上拉用户管理（user）开发分支
     git checkout -b reature/user develop/2019-001
     git push -u origin reature/user
     # 从develop/2019-001上拉角色管理（role）开发分支
     git checkout -b reature/role develop/2019-001
     git push -u origin reature/role
     ```

   - 开发人员拉取对应的开发分支进行开发

     ```shell
     # 开发人员A拉取远端用户管理（user）开发分支到本地进行开发
     git checkout -b reature/user origin/reature/user
     # 开发人员B拉取远端角色管理（role）开发分支到本地进行开发
     git checkout -b reature/role origin/reature/role
     ```

   ![开发新功能.png](http://q33kciz5a.bkt.clouddn.com/gitflow:开发新功能.png)

   <center>从develop分支上拉开发分支Feature</center>

4. 开发人员开发及开发完成

   - 开发人员开发

     Git三部曲：add/commit/push

     ```shell
     git add <fileName>
     git commit -m "commit message"
     git push
     ```

   - 开发完成

     用户管理功能做完了。开发团可以提出一个将所完成的功能合并入develop分支的请求。或者，团队中有一人可以自行将他的代码合并入本地的develop分支，然后再推送到中央仓库，然后删除功能分支

     **新开发的功能代码永远不能直接合并入master**

     ```shell
     # 将远端develop拉到本地，方便解决冲突
     git pull origin develop/2019-001
     # 切换到develop分支
     git checkout develop/2019-001
     # 合并reature功能分支
     git merge reature/user
     # 推送远端
     git push
     # 删除reature功能分支
     git branch -d reature/user
     ```

     ![功能开发完.png](http://q33kciz5a.bkt.clouddn.com/gitflow:功能开发完.png)

     <center>Feature分支开发完后，往develop分支合</center>

5. 完成整合发布版本，开始测试

   完成开发后要发版，（如，版本号定为V1.1），从开发分支develop/2019-001上拉版本分支release/V1.1.0

   ```shell
   # 从开发分支develop/2019-001上拉版本分支release/V1.1.0
   git checkout -b release/V1.1.0 develop/2019-001
   ```

   *这个分支专门用于发布前的准备，包括一些清理工作、全面的测试、文档的更新以及任何其他的准备工作。它与用于功能开发的分支相似，不同之处在于它是专为产品发布服务的。*

   *一旦创建了这个分支并把它推向中央仓库，这次产品发布包含的功能也就固定下来了。任何还处于开发状态的功能只能等待下一个发布周期。*

   ![发布.png](http://q33kciz5a.bkt.clouddn.com/gitflow:发布.png)

   <center>从develop分支拉发版分支，并开始测试修改bug等</center>

6. 完成版本发布

   版本测试完成后，就要把发布分支合并入master和develop分支，然后再将发布分支删除。

   发布分支扮演的角色是功能开发（develop）与官方发布（master）之间的一个缓冲。

   **注意，往develop分支的合并是很重要的，因为开发人员可能在发布分支上修复了一些关键的问题，而这些修复对于正在开发中的新功能是有益的。**

   **并且，如果团队强调代码评审（Code Review），此时非常适合提出这样的请求。**

   ```shell
   # 将远端master拉到本地，方便解决冲突
   git checkout master
   # 合并版本分支
   git merge release/V1.1.0
   # 将合并结果推送远端
   git push
   # 将远端develop拉到本地，方便解决冲突
   git checkout develop/2019-001
   # 合并版本分支
   git merge release/V1.1.0
   # 将合并结果推送远端
   git push
   # 删除版本分支
   git branch -d release/V1.1.0
   ```

   无论什么时候我们把一些东西合并入master，我们都应该随即打上合适的标签。

   ```shell
   # 在master上打上V1.1.0的标签
   git tag -a V1.1.0 -m "权限管理" master
   # 推送tags到远端
   git push --tags
   ```

   ![完成发布.png](http://q33kciz5a.bkt.clouddn.com/gitflow:完成发布.png)

<center>将release分支分别合入master和develop分支</center>
7. 遇到紧急BUG

   随着上线的完成，突然发现一个紧急bug需要修复，这个时候测试人员分配一个bug号（如，20191221-001）并做记录以便后续测试，管理员拿到bug号基于master创建了一个用于维护的分支（如，issue/20191221-001）。开发人员在这个分支上修复了那个bug，然后把改动的代码直接合并入master，并打上版本，V1.1.1。

   ```shell
   # 从master上拉维护分支
   git checkout -b issue/20191221-001 master
   # 修改并完成测试后合入master
   # 切换到master
   git checkout master
   # 合并维护分支
   git merge issue/20191221-001
   # 将合并结果推送到远端
   git push
   ```

   ![bug分支.png](http://q33kciz5a.bkt.clouddn.com/gitflow:bug分支.png)

   <center>从master拉issue分支修改bug，最后合入master</center>
跟发布分支一样，在维护分支上的改动也需要合并入develop分支，用于记录该过程。随后，就可以将维护分支删除。
   
```shell
   # 切换到开发分支
   git checkout develop
   # 合并维护分支
   git merge issue/20191221-001
   # 讲合并结果推送到远端
   git push
   # 删除维护分支
   git branch -d issue/20191221-001
   ```



参考：[workflow-gitflow How it works](https://www.atlassian.com/git/tutorials/comparing-workflows#!workflow-gitflow)

---------------------------

转载请注明原地址，宋德凌的博客：[http://CoderOfSong.github.io](http://CoderOfSong.github.io) 谢谢！