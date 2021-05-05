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