## 已完成项目首次提交到仓库

假设：**～/test/**该目录下的所有文件都是需要提交的。提交的远程仓库地址为：https://github.com/zhukaigit/test.git

**方式一**

1. cd进入到~/test/目录下，执行`git init`，创建本地仓库
2. 添加远程仓库，命令格式为：`git remote add {short_name} {git_url}`。
   执行命令`git remote add originname https://github.com/zhukaigit/test.git`。
   然后使用`git remote -v`查看远程分支情况，确认是否添加成功。
   **注意：short_name名字可以随意起，一般都取名叫origin**
3. 提交，命令格式为：`git push -u {short_name} <local_branch>:<remote_branch>`
   执行命令：`git push -u originname master:master`



## 常用命令

### 查看远程仓库

```
git remote -v
```

### 添加远程仓库

```bash
# <远程主机名>可自己随意命名，不一定非的命名为origin
git remote add <远程主机名> <远程仓库地址>
```

### 删除远程仓库

```
git remote rm <远程主机名>
```

### **拉取代码**

```bash
git pull <远程主机名> <远程分支名>:<本地分支名>
```

举例详解：

```bash
# 比如，要取回origin主机的next分支，与本地的master分支合并，需要写成下面这样
git pull origin next:master
# 如果远程分支(next)要与当前所在分支合并，则冒号后面的部分可以省略。上面命令可以简写为
git pull origin next
# 如果当前分支与origin主机某个分支存在追踪关系，那么写成如下形式，表示取回远程关联分支代码，与当前分支合并
git pull origin
# 如果当前分支只有一个追踪分支，连远程主机名都可以省略
git pull
```

### 推送代码

```bash
# 注意<本地分支名>:<远程分支名>，“:”左右不要有空格
# 即不要写成这个形式：git push origin master : mater
# 应该是：git push origin master:master
git push <远程主机名> <本地分支名>:<远程分支名>

# 参数解释
# -u表示将本地分支与远程分支建立追踪关系
git push -u <远程主机名> <本地分支名>:<远程分支名>
# -f表示用本地分支代码【强制覆盖】远程分支代码，不用考虑冲突问题，慎用！
# 可用这个命令来实现远程分支的代码回滚
git push -f <远程主机名> <本地分支名>:<远程分支名>
```

### 建立分支追踪关系

```bash
# 将当前分支与origin主机的master分支建立追踪关系
git branch --set-upstream-to=origin/master
```

### 查看分支

```bash
# 查看本地分支
git branch

# 查看所有分支
git branch -a

# 查看本地分支详情
git branch -v

# 查看所有分支详情
git branch -av

# 查看本地分支详情及关联分支
git branch -avv
```

### 创建分支

```bash
git branch <分支名>

# 已当前分支为基础，创建一个dev分支
git branch dev

# 以master分支为基础，创建一个dev分支
git branch dev master

# 以origin主机上的master分支为基础，创建一个dev分支
git branch dev origin/master
```

### 删除分支

```bash
git branch -d <分支名>
# -D表示强制删除
git branch -D <分支名>
```

### 删除远程分支

```
git push <远程主机名> :<远程分支名>
```

### 切换分支

```bash
git checkout <分支名>

# 切换到dev分支
git checkout dev
```

### 创建并切换分支

```bash
git checkout -b <分支名>

# 已当前分支为基础，创建一个dev分支，并切换到dev分支
git checkout -b dev

# 以master分支为基础，创建一个dev分支，并切换到dev分支
git checkout -b dev master

# 以origin主机上的master分支为基础，创建一个dev分支，并切换到dev分支
git checkout -b dev origin/master
```

### 合并分支

```bash
# 即将指定分支的代码合并到当前分支
git merge <分支名>

# 将本地dev分支代码合并到master
git checkout master	#第一步：跳转到master分支
git merge master 	#第二步：合并代码
```

### tag

```bash
# 查看
git tag

# 给指定的commit打tag
git tag release_1.0 29138e0b23013bb12d60cfc6fde80278660ee1da

# 本地推送名为release_1.0的tag到线上
git push origin release_1.0

# 本地删除名为release_1.0的tag
git tag -d release_1.0

# 删除远程tag
git push origin :refs/tags/release_1.0
```

### 撤销

```bash
# 丢弃【工作区】的修改，将文件回到最近一次git commit或git add时的状态。
# 注意：暂存区的修改无法通过此命令撤销
git checkout -- <file> # 指定文件
git checkout .	#所有文件

# 将暂存区的修改撤销掉，重新放回到工作区
git reset HEAD <file> #针对指定文件
git reset HEAD #针对所有文件

# 1、暂存区：撤销所有修改，删除所有新增文件；2、工作区：撤销所有修改，新增文件（即未跟踪文件）不会变动
git reset --hard HEAD
```

### 本地版本回滚

```bash
# 回滚到指定提交的版本，有下面三种方式

# 第一种
# “commit_id之后提交的代码”和“暂存区所有新增文件”都会被删除，且“暂存区与工作区的修改将会回滚”，未跟踪的文件不会变动
# 这个操作执行完，本地文件会发生改变
git reset --hard commit_id

# 第二种 
# commit_id之后提交的代码保存到暂存区，暂存区原有的文件不作变动。未跟踪的文件不会变动
# 这个操作执行完，本地文件不会有任何改动
git reset --soft commit_id

# 第三者
# commit_id之后提交的代码变成未跟踪文件，暂存区中若有commit_id之后提交的文件，会变成未跟踪文件，其余文件不作变动。未跟踪的文件不会变动
# 这个操作执行完，本地文件不会有任何改动
git reset --mixed commit_id
```

### 未跟踪文件的删除

在利用 `git` 工作时，工程目录下经常会出现一些未跟踪文件，虽然 `git` 支持通过 `.gitingore` 文件添加一些忽略文件类型和文件目录。但有时需要清理一些临时文件和自动生成的文件，手动删除显得太麻烦，这时你可以利用 `git clean` 命令来帮你完成这项操作。`git clean` 命令支持以下参数：

```
git clean [-d] [-f] [-i] [-n] [-q] [-e <pattern>] [-x | -X] [--] <path>...
```

其中几个主要参数用法如下：

> -d   # 删除未跟踪目录以及目录下的文件，如果目录下包含其他git仓库文件，并不会删除（-dff可以删除）。
> -f   # 如果 git cofig 下的 clean.requireForce 为true，那么clean操作需要-f(--force)来强制执行。
> -i   # 进入交互模式
> -n   # 查看将要被删除的文件，并不实际删除文件**

```bash
# 删除当前目录下的未跟踪文件
git clean -f

# 删除当前目录下的未跟踪文件及其子文件夹。
git clean -df

# 删除当前目录下的未跟踪文件及其子文件夹，连在.gitignore中忽略的未跟踪文件都会被删除，慎用！
git clean -xdf

# 特别注意：删除前加上-n参数，先查看有哪些文件将被删除，避免误删
git clean -nf
git clean -ndf
git clean -nxdf
```

