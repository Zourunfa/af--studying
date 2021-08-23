## git版本控制

> 多个人提交代码，记录提交代码的时间和增加的代码以及删除的代码，并且可以吃后悔药，当这个时间版本提交的代码不行时，可以回退版本，git可以回退之前提交的任意一个版本



## git的基本原理

**git有三个区域**：

- 工作区 （workspace）：就是你平时存放项目代码的地方
- 暂存区  (index/stage)： 用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表的信息
- 本地仓库(Reposity）：就是安全存放数据的位置，这里面有你提交到所有版本的数据，其中HEAD指向最新放入仓库的版本 



- 远程仓库(Remote)：托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

如果加上远程的git仓库就可以分为四个工作区域，文件在这四个区域之间的转换关系：

![image-20210415100458102](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415100458102.png)







![image-20210415102135012](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415102135012.png)



git工作的基本流程如下：

1. 在工作目录中添加修改文件  --       git add .
2. 将需要进行版本管理的文件放入暂存区域  git commit -m "所有文件"
3. 将暂存区域的文件提交到gi仓库             git push

因此git管理的文件有三种状态：

- 已修改
- 已暂存
- 已提交



![image-20210415102622395](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415102622395.png)

## 初始配置

配置文件为 --/.gitconfig,执行任何Git配置命令后文件将自动创建



第一个要配置你的个人的用户名和电子邮件的地址，这两条配置很重要，每次git提交的时候都会引用这两条信息，说明是谁提交了更新，所以会随更新的内容一起被永久的纳入历史



```bash
git config --global user.email "2300071698@qq.com"
git config --global user.name "2300071698@qq.com"
```





查看配置信息

```bash
git config --list --global
```





## 基本命令

|                  功能                  |                         命令                          |
| :------------------------------------: | :---------------------------------------------------: |
|              初始化新仓库              |                       git  init                       |
|                克隆代码                |  git clone    https://gitee.com/houdunwang/hdcms.git  |
|             克隆时指定分支             | git clone -b dev   git@gitee.com:houdunwang/hdcms.git |
|                查看状态                |                      git status                       |
|              提交单个文件              |                   git add index.php                   |
|              提交所有文件              |                      git add -A                       |
|             使用通配符提交             |                     git add *.js                      |
|              提交到仓库中              |               git commit -m '提示信息'                |
|  提交已经跟踪过的文件，不需要执行add   |              git commit -a -m '提交信息'              |
|      删除版本库与项目目录中的文件      |      git rm index.php   //在本地工作区也会被删除      |
| 只删除版本库中文件但保存项目目录中文件 |        git rm --cached index.php //本地还是有         |
|            修改最后一次提交            |                 `git commit --amend`                  |





## .gitignore文件

> 有些文件不想放到版本库中，可以用这个文件忽视掉对版本库中文件的监测





比如下面例子：

创建一个叫shop的项目，并且创建两个文件.gitignore 和 a.txt

![image-20210415114231000](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415114231000.png)

先git status 查看一下状态

![image-20210415114258151](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415114258151.png)





如果这是我想要忽略掉对所有.txt文件的监测



就需要在.gitignore文件中配置如下

![image-20210415114405506](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415114405506.png)





再看一下状态：

![image-20210415114432028](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210415114432028.png)



如果想要忽略所有.txt文件，除了a.txt,可以在.gitignore文件中这样写：

```
*.txt
!a.txt
```





关于.gitignore :https://blog.csdn.net/le_17_4_6/article/details/92789993





## 从（本地仓库）版本库删除资源的技巧

- git rm 文件名 -- 同时删除本地和暂存区的文件
- git rm --cached --只删除暂存区的文件，保留到本地

**注意下面图片中的所有暂存区要改成本地仓库**

![image-20210521094323549](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521094323549.png)

![image-20210521094458174](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521094458174.png)

![image-20210521094517202](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521094517202.png)

![image-20210521094851811](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521094851811.png)

## 从本地仓库（版本库）修改文件名称

git mv 原文件名 修改之后的名字

![image-20210521100106117](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521100106117.png)

## 使用log查看历史操作

> 先初始化一个仓库后创建a.js提交到版本库,创建b.js提交到版本库，修改a.js之后再提交

使用**git log**就能查看到历史操作

![image-20210521101014380](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101014380.png)

想更清楚的查看到文件的变动信息用**git log -p**

![image-20210521101106007](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101106007.png)

如果只想看最近的一次提交用git log -p -1

![image-20210521101201277](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101201277.png)

如果只想看信息的一行,**git log --oneline**,想在此基础上更详细一点用git log --oneline -p



![image-20210521101306108](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101306108.png)

![image-20210521101445297](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101445297.png)



如果直接想看到都有哪些文件的变化,用git log --name-status

![image-20210521101650631](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521101650631.png)





## 修改最近一次提交的描述信息

![image-20210521102321026](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521102321026.png)

输入git commit --amend 按回车之后 就会进入如下界面：

![image-20210521102401289](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521102401289.png)

然后直接在这里面修改提交的信息 保存退出，全部为vim操作

![image-20210521102556628](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521102556628.png)

![image-20210521102635066](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521102635066.png)

**接下来如果有生产了一个b.js文件，如果我认为这个文件的提交也应该归纳到这次提交里面**，应如下操作

![image-20210521102942610](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521102942610.png)





## 管理暂存区中的文件

**如果你想撤销暂存区提交的文件，git rm --cached 文件名**

![image-20210521103415393](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521103415393.png)

如果你修改上面的a.js后 你立即提交到暂存区了，如果你此时有后悔了，

可以用git reset  HEAD 文件名

![image-20210521103755705](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521103755705.png)

如果你认为你这次的修改不好，想要恢复到上次提交的状态，可以用

git checkout -- 文件名

![image-20210521104026512](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521104026512.png)





## git 分支操作

**用先创建mas.js文件，提交到版本仓库,git会自动初始化这个master分支并保存这个文件，**

**然后用git branch 创建 ask分支，用git checkout 切换到ask分支，创建ask.js文件并提交到版本库**

**在用ls 查看一下文件  两个文件都有，**

**但是切换到主分支后，ls 查看文件 只能看到一个mas.js文件**



![image-20210521112920273](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521112920273.png)

![image-20210521112940126](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521112940126.png)

可以有创建并切换分支的简单命令

![image-20210521113113545](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521113113545.png)



## 合并与删除分支

在上面的基础上切换分支到master后然后git merge ask 即可合并master ask分支

![image-20210521113609923](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521113609923.png)

删除ask分支

![image-20210521113643667](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210521113643667.png)





## 解决实际合并分支冲突

![image-20210523104300460](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104300460.png)

![image-20210523104348631](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104348631.png)

![image-20210523104508077](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104508077.png)

![image-20210523104533476](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104533476.png)

![image-20210523104557683](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104557683.png)

![image-20210523104609939](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104609939.png)

![image-20210523104622007](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104622007.png)

![image-20210523104632673](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104632673.png)

![image-20210523104700849](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104700849.png)

![image-20210523104707896](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104707896.png)

![image-20210523104719895](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104719895.png)

![image-20210523104738468](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104738468.png)

![image-20210523104820069](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104820069.png)

![image-20210523104851672](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104851672.png)

![image-20210523104928680](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523104928680.png)

![image-20210523105236253](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523105236253.png)

![image-20210523105314367](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523105314367.png)

![image-20210523105438640](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523105438640.png)

![image-20210523105456104](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523105456104.png)

![image-20210523105508139](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523105508139.png)

## git stash深入理解

先创建了一个mas.js文件到master分支，然后在创建ask和bbs分支，然后在ask分支创建文件ask.js,先git add，git commit将其提交到版本库，然后再修改ask.js文件，并git add .提交到暂存区,这时你想切换分支，会切换不成功并提示:

![image-20210523121702212](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523121702212.png)

这是你就可以使用git stash,使当前工作站台缓存起来,可以通过git stash list 查看，

切换分支回来的时候，是可以用git stash apply 恢复缓存的的

![image-20210523121836525](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523121836525.png)

![image-20210523121936646](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523121936646.png)

恢复回来之后你会发现这个缓存站台依然存在，可以用下面命令删除

![image-20210523122029129](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523122029129.png)



**或者直接用git stash pop 恢复 + 删除**

![image-20210523122138840](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210523122138840.png)



**注意这个命令只有跟版本库有所关联之后的文件才会进行相应的缓存，也就是只有对git add .和git commit 之后的文件有作用**