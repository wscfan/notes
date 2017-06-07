# Git 安装

下载地址：[官方](https://git-for-windows.github.io/)、[国内](https://pan.baidu.com/s/1kU5OCOB#list/path=%2Fpub%2Fgit)

1. 安装完安装包后，还需要最后一步的设置：  
开始菜单->"Git"->"Git Bash"，输入
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

2. 生成SSH密钥
```
ssh-keygen -t rsa -C "email@example.com"
```
在当前用户的根目录下生成 `.ssh` 目录，目录里边有两个文件 `id_rsa` 和 `id_rsa.pub` ,打开 `id_rsa.pub` 文件，将里边的公钥添加到 github 上即可。  

3. 创建一个空目录，进入并执行下面代码：  
```
git init
```
就创建了一个版本库，会生成一个 .git 的目录


4. 可以在版本库中新建一个 readme.txt 文件，并把文件添加到仓库：  
```
git add readme.txt
```  

5. 把文件提交到仓库：  
```
git commit -m "wrote a readme file"
```

6. 查看仓库的当前状态：  
```
git status
```  

7. 查看具体文件的修改：  
```
git diff
```  

8. 查看历史纪录：
```
git log
```  
如果嫌输出信息太多，可以加上` --pretty=oneline` 参数：
```
git log --pretty=oneline
```  

9. Git中，用 `HEAD` 表示当前版本，上一个版本是 `HEAD^` ，上上个版本是 `HEAD^^` ，往上100个版本是 `HEAD~100` ,回退上一个版本：  
```
git reset --hard HEAD^
```

10. 记录每一次命令：
```
git reflog
```

11. 把 readme.txt 文件在工作区的修改全部撤销
```
git check -- readme.txt
```

12. 暂存区的文件回退到工作区：
```
git reset HEAD file
```

13. 删除文件：
```
git rm
```

14. 添加远程库，根据 github 上的提示，在本地仓库下运行命令：
```
git remote add origin git@github.com:wscfan/myweb.git
```

15. 把本地库推送到远程，使用 `git push` 命令  
实际上是把当前分支 master 推送到远程。  
由于远程是空的，第一次推送 master 分支时，加上 -u 参数，Git不但会把本地 master 分支内容推送到远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来。以后的推送或者拉取就可以简化命令。  
现在，只要本地作了提交，就可以通过下面命令把本地 master 分支的最新修改推送到GitHub上了。
```
git push origin master
```
