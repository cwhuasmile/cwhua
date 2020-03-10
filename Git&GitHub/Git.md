# Git使用方法简洁版

- `git clone [链接地址]`  克隆指定链接下的项目

- `git status` 查看仓库当前状态(是否有增删改)

- `git add .` 将代码保存到暂存区(注意add后面有个“.”点)

- `git commit -m "备注内容"` 提交更改

  首次使用会要求输入邮箱和用户名

- `git push` 将代码进行上传同步

  首次使用会弹出对话框要求输入账号密码
  
- 配置用户名和邮箱：

  - 为单一的仓库配置用户名和邮箱，命令分别为：

    ```git
    $ git config user.name "username"
    $ git config user.email "email"
    ```
  
  - 配置全局的用户名和邮箱，命令分别为:
  
    ```git
    $ git config --global user.name "username"
    $ git config --global user.email "email"
    ```

# Git相关问题处理

1. warning: LF will be replaced by CRLF in readme.txt

   出现此问题是因为不同操作系统的使用的换行符不同：
   Linux / Unix 采用换行符LF表示下一行
   Windows  采用回车+换行 CRLF表示下一行
   解决：可以通过设置 core.autocrlf 的值解决
   
   ```git
   $ git config --global core.autocrlf false
   ```
   
   