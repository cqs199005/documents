# Git入门

### 1.初始化git仓储/(仓库)

1. 这个仓库会存放, git 对我们项目代码进行备份的文件

2. 在项目目录右键打开 git bash

3. 命令:

   ```
   git init   (初始化git仓库)
   ```

### 2.自报家门

1. 就是在git中设置当前使用的用户是谁

2. 每次备份都会把当前的备份者信息存储

3. 命令:   --global(全局使用)

   ```
   $  git config --global user.name "cqs"
   $  git config --global user.email "cqs@qq.com"
   ```

### 3.把代码存储到Git仓库中(备份数据)

1. 把代码放到仓储的门口(暂存区)

   ```
   $ git add ./reademe.md	   (把指定文件放到大门口)
   $ git add ./               (把根工作区所有修改的文件搬到大门口)
   ```

2. 把仓储门口的代码放到房间中去(版本库)

   ```
   $ git commit -m "这是备份的说明(必填)"   
   ```

3. 可以一次性把我们修改的代码放到房间里(版本库)

   ```
   $ git commit --all -m "这是一次性移动全部修改过的文件"  
   ```

### 4.查看当前状态

1. 可以用来查看当前代码有没有被放到仓储中去

2. 命令:

   ```
   $ git status
   ```

### 5.设置Git中忽略的文件

1. 新建一个.gitignore文件,在这个文件中可以设置要被忽略的文件或目录.

2. 被忽略的文件不会被提交到仓储里去.

3. 书写要被忽略的文件方式:以/开头,一行写一个,代码如下:

   ```
   .idea			(会忽略.idea文件)
   .gitignore		(会忽略gitignore这个文件)
   js				(会忽略js目录下的所有文件)
   js/*.js		(会忽略js目录下所有.js文件)
   ```

### 6.查看日志

查看保存的历史记录

```
$ git log				(查看历史提交的日志)
$ git log --oneline		(查看到精简版的日志)
$ git reflog			(可以看到每一次切换版本的记录:可以看到所有版本号)
```

### 7.回退到指定版本

回退到之前的版本,后面的版本依然保存,但是查看日志时看不到

```
$ git reset --hard Head~0	(回退到上一次代码提交的状态)
$ git reset --hard Head~2	(回退到上上上次代码提交的状态)
$ git reset --hard d884deb	(通过版本号精确回退到某一次提交时的状态)
```

### 8.分支

1. 默认一个主分支:master,这里保存的版本谁都可以访问并修改

2. 自己创建的分支,是一个独立的空间,只有自己能访问修改,只有当最终合并到主分支上时别人才能看到,在开发时,当代码功能还没完成不能运行时,先保存在分支,等完成后在合并出去

3. 如何建立分支

   ```
   $ git branch dev	(创建一个名为dev的分支)
   ```

4. 查看当前有哪些分支

   ```
   $ git branch	
   ```

5. 切换分支

   ```
   $ git checkout dev	(切换到dev分支)
   ```

6. 合并分支

   ```
   $ git merge dev		(将分支dev合并到主分支master)
   ```

   7.合并分支冲突问题

   ​如果在同时在主分支上修改,又在分支上修改,合并时可能会有冲突,需要手动处理,处理完后还需要在提交一次保存

### 9.命令窗口功能

```
clear	(清除窗口代码)
```

### 10.上传到GitHub

- https://github.com

  - 一个开源的网站,允许上传代码到这里存储(代码托管)

- 在Git 提交代码到GitHub命令,第一次上传时会要求登录账号密码

- 上传前确保

  ```
  $ git push https://github.com/cqs199005/my-document.git master	(这个网址在GitHub创建仓库时获取的,master必填,表示你上传的属于哪个分支) 
  ```

### 11.下载GitHub代码到本地

+ 先在本地项目文件夹初始化,创建出.Git仓储

```
$ git pull https://github.com/cqs199005/my-document.git master
```

+ 克隆岛本地,注意重复克隆会覆盖本地的文件

```
$ git clone https://github.com/cqs199005/my-document.git master
```

### 12.git的源码托管网站oschina(国内的,上传速度快)

### 13 . 生成ssh key

1. 

