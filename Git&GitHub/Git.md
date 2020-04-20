# Git使用方法简洁版

- `git clone [链接地址]`  克隆指定链接下的项目

- `git status` 查看仓库当前状态(是否有增删改)

- `git add .` 将代码保存到暂存区(注意add后面有个“.”点)

- `git commit -m "备注内容"` 提交更改

  首次使用会要求输入邮箱和用户名

- `git push` 将代码进行上传同步

  首次使用会弹出对话框要求输入账号密码

- 为单一的仓库配置用户名和邮箱，命令分别为：

  ```cmd
  git config user.name "username"
  git config user.email "email"
  ```
- 配置全局的用户名和邮箱，命令分别为:

  ```cmd
  git config --global user.name "username"
  git config --global user.email "email"
  ```
- git更新远程仓库代码到本地
	git pull : 首先，基于本地的FETCH_HEAD记录，比对本地的FETCH_HEAD记录与远程仓库的版本号，然后git fetch 获得当前指向的远程分支的后续版本的数据，然后再利用git merge将其与本地的当前分支合并。所以可以认为git pull是git fetch和git merge两个步骤的结合。 
	git pull的用法如下：
	
	```bash
	//取回远程主机某个分支的更新，再与本地的指定分支合并。
git pull <远程库名> <远程分支名>:<本地分支名>
	 
	//取回远程库中的master分支，与本地的master分支进行合并更新，要写成：
git pull origin master:master
	 
	//如果是要与本地当前分支合并更新，则冒号后面的<本地分支名>可以不写
	git pull origin master
	```
# Git相关问题处理

1. warning: LF will be replaced by CRLF in readme.txt

	出现此问题是因为不同操作系统的使用的换行符不同：
	Linux / Unix 采用换行符LF表示下一行
	Windows  采用回车+换行 CRLF表示下一行
	解决：可以通过设置 core.autocrlf 的值解决
   
	```cmd
	git config --global core.autocrlf false
	```
2. 把本地文件添加到远端新建仓库

	在本地电脑文件夹打开git bash，执行`git clone [新建仓库的连接地址]`命令把远端新建的仓库克隆到本地，会生成一个远端仓库名的文件夹，把该文件夹内的全部内容复制到需要push文件的目录里，然后就是常规操作了。