# Git 教程
## 一、git配置

1. 配置用户名、邮箱
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

2. 生成SSH密钥
```
ssh-keygen -t rsa -C "email@example.com"
```
在当前用户的根目录下生成 `.ssh` 目录，目录里边有两个文件 `id_rsa` 和 `id_rsa.pub` ,打开 `id_rsa.pub` 文件，将里边的公钥添加到 github 上即可。  

## 二、git常用命令

1. 创建一个空目录，进入并执行下面代码：  

```
git init
```
就创建了一个版本库，会生成一个 .git 的目录

2. 可以在版本库中新建一个 readme.txt 文件，并把文件添加到仓库：  

```
git add readme.txt
```

3. 把文件提交到仓库：  

```
git commit -m "wrote a readme file"
```

4. 查看仓库的当前状态：  

```
git status
```

5. 查看具体文件的修改：  

```
git diff
```

6. 查看历史纪录：

```
git log
```
如果嫌输出信息太多，可以加上` --pretty=oneline` 参数：
```
git log --pretty=oneline
```

7. Git中，用 `HEAD` 表示当前版本，上一个版本是 `HEAD^` ，上上个版本是 `HEAD^^` ，往上100个版本是 `HEAD~100` ,回退上一个版本：  

```
git reset --hard HEAD^
```

8. 记录每一次命令：

```
git reflog
```

9. 把 readme.txt 文件在工作区的修改全部撤销

```
git check -- readme.txt
```

10. 暂存区的文件回退到工作区：

```
git reset HEAD file
```

11. 删除文件：

```
git rm
```

12. 添加远程库，根据 github 上的提示，在本地仓库下运行命令：

```
git remote add origin git@github.com:wscfan/myweb.git
```

13. 把本地库推送到远程，使用 `git push` 命令  
    实际上是把当前分支 master 推送到远程。  
    由于远程是空的，第一次推送 master 分支时，加上 -u 参数，Git不但会把本地 master 分支内容推送到远程新的 master 分支，还会把本地的 master 分支和远程的 master 分支关联起来。以后的推送或者拉取就可以简化命令。  
    现在，只要本地作了提交，就可以通过下面命令把本地 master 分支的最新修改推送到GitHub上了。

```
git push origin master
```

14. 获取远程库与本地同步合并（如果远程库不为空必须做这一步，否则后面的提交会失败）

```
git pull --rebase origin master
```

## 三、git其他命令

+ 创建并切换新分支

  ```
  git checkout -b dev
  git switch -c dev
  ```

+ 缓存修改

  ```
  git stash
  ```

+ 查看缓存列表

  ```
  git stash list
  ```

+ 缓存恢复（不删除缓存内容）、删除

  ```	
  git stash apply
  git stash drop
  ```

+ 缓存恢复（删除缓存内容）

  ```
  git stash pop
  ```

+ 复制特定的提交到当前分支

  ```
  git cherry-pick f1b0
  ```

+ 删除分支、强制删除分支

  ```
  git branch -d dev
  git branch -D dev
  ```

+ 创建本地dev分支并同步远程dev分支

  ```
  git checkout -b dev origin/dev
  ```

+ 设置dev和origin/dev的链接

  ```
  git branch --set-upstream-to=origin/dev dev
  ```

+ 强制修改分支位置

  ```
  git branch -f dev HEAD~3
  ```


+ 将指定提交应用于其他分支

  ```
  git cherry-pick <HashA> <HashB>
  ```

+ 打标签、查看标签

  ```
  git tag v1.0
  git tag
  ```

+ 给某次提交打标签

  ```
  git tab v1.0 52cddb6
  ```

+ 查看某个标签对应提交的内容的修改

  ```
  git show v1.0
  ```

+ 创建带有说明的标签

  ```
  git tag -a v1.0 -m 'released version' 52cddb6
  ```

+ 删除标签

  ```
  git tag -d v1.0
  ```

+ 推送某个标签到远程

  ```
  git push origin v1.0
  ```

+ 推送所有尚未推送到远程的本地标签

  ```
  git push origin --tags
  ```

+ 删除远程标签

  先删除本地标签，再删除远程标签

  ```
  git tag -d v1.0
  git push origin :refs/tags/v1.0
  ```

+ 查看远程版本库

  ```
  git remote -v
  ```

+ 关联多个远程版本库

  1. 删除已关联的名为 `origin` 的远程库
  2. 关联远程库，用不同的名字区分

  ```
  git remote rm origin
  git remote add github git@github.com:wscfan/notes.git
  git remote add gitee git@gitee.com:wscfan/notes.git
  ```

  