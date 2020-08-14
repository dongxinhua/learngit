# Learn Git

## Git 基本命令

### 将文件夹弄成git本地仓库,该文件夹的路径和名字不能包含中文

```git
  git init
```

### 将内容添加到暂存区

```git
  git add <filename> // 添加单个文件到暂存区
  git add <filename> <filename> // 添加多个文件到暂存区
  git add . // 这是将全部文件添加到暂存区
```

### 将暂存区的内容添加到master分支

```git
  git commit -m "本次提交说明"
```

### 查看仓库当前状态

```git
  git status
```

### 查看比较工作区和暂存区的区别

```git
  git diff <filename>
```

### 查看工作区和版本库里面最新版本的区别

```git
  git diff HEAD -- <filename>
```

### 显示从最近到最远的提交日志

```git
  git log
  git log --pretty=oneline   // 带参数，美化显示
```

### 版本回退

> 在Git中，用`HEAD`表示当前版本,`HEAD^`表示上一个版本，`HEAD^^`表示上上个版本，如果我们要看前100个版本怎么办? `HEAD~100`。所以我们平常使用`HEAD~num`就ok
```git
  git reset --hard HEAD^   // 回退到上一个版本
  git reset --hard HEAD^^   // 回退到上上个版本
  git reset --hard HEAD~1   // 回退到上一个版本
  git reset --hard 版本号   // 回退到指定版本
```
> 当我们将版本回退到前一个或者前n个版本后，又想回到当前最新的版本怎么办？使用`git reset -- hard `版本号。但是，我们通过`git log`显示提交日志，我们发现最新的那个版本已经不在了，那么怎么才能找到原来最新的那个版本号呢？`git reflog`，它会记录下来我们操作的记录，就可以知道我们那个版本号是什么了。

### 查看文件内容

```git
  cat <filename>
```

### 记录每个版本的版本号，以及做了什么

```git
  git reflog
```


### 版本库

> 工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。   
> Git的版本库里存了很多东西，其中最重要的就是称为`stage`（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向master的一个指针叫`HEAD`。    
> 前面我们把文件往Git版本库里添加的时候，是分两步执行的：   
> 第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；  
> 第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。    
> 因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。     
> 当我们将暂存区的内容提交到分支上时，暂存区就会边为空。

### 撤销修改

> 文件修改后，想直接丢弃工作区里修改的内容。

```git
  git restore <filename>
  或
  git checkout -- <filename>
```
> 在此操作是存在两种情况：    
> 1.文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；    
> 2.文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

> 如果修改的文件已经被“add”到了暂存区` git restore --staged <filename> `可以将暂存区的修改撤销

```git
  git restore --staged <filename>
```

> 使用 `git restore --staged <filename> `撤销修改后 `git status` 查看文件状态，暂存区的修改以撤销，工作区有修改，接着使用 `git checkout -- <filename> `撤销工作区的修改即可。

### 文件删除后恢复

> 在本地删除了文件之后，想要删除版本库对应的文件，那么，我们使用` git rm/add <filename> `删掉版本库的内容，再`git commit -m "remove `文件"，这样就彻底删除了。

```git
  git rm/add <filename>
  git commit -m "remove filename"
```

> 如果是误删，想要恢复的话，那么我们可以使用 `git restore <filename> `或者 `git checkout -- <filename>`来恢复工作区的文件。注意，从来没有添加到版本库的就被删除的文件，是不可以恢复的。

```git
  git restore <filename> 
  或
  git checkout -- <filename>
```

## 本地仓库和远程仓库

### 将本地仓库与github的仓库相关联

```git
  git remote add origin git@github.com:dongxinhua/<filename>.git
```

### 将本地仓库所有内容推送到远程仓库

```git
  git push -u origin master
```

> 以后再次提交的话，只需要:

```git
  git push origin master   // 推送到主分支
  git push origin <branch name>   // 推送到其他分支
```

### 在本地克隆远程仓库

```git
  git clone git@github.com:dongxinhua/<filename>.git
```

## 分支协作

### 创建分支

```git
  git branch <branch name>
```

### 切换分支

```git
  git checkout <branch name>  // 语义不好建议使用switch
  git switch <branch name>   // 建议使用
```

### 创建并切换分支

```git
  git checkout -b <branch name>  // 语义不好建议使用switch
  git switch -c <branch name>   // 建议使用
```

### 查看分支

> 带*的代表当前分支

```git
  git branch
  git branch -a   // 查看所有分支
  git branch -r   // 查看远程分支
```

### 合并分支到master主分支

> 这种方法是`Fast forward`模式，让master直接指向分支，合并完成后，甚至可以删除分支

```git
  git merge <branch name>
```

> 如果想要强制禁用`Fast for word`模式，这样Git就会在merge时生成一个新的commit，这样咋在分支历史上就可以看到分支信息。

```git
  git merge --no-ff -m "merge with no-ff" <branch name>
```

### 在本地创建远程分支

```git
  // 先建立本地分支
  git switch -c <branch name>
  // 将新分支推送至GitHub.
  git push origin <branch name>
```

### 删除分支

```git
  git branch -d <branch name>
```

### 强制删除未合并的分支

```git
  git branch -D <branch name>
```

### 解决冲突

> 当两个分支各有修改，然后`git merge <branch name>` 就会报错，提示两个各有修改，然后，我们查看文件的内容，可以看到冲突的地方，然后我们把那一段符号和文字改为我们最终想要修改的内容。再进行提交，并查看分支合并的情况 `git log --graph --pretty=oneline --abbrev-commit`  然后就合并成功了。

### 储藏工作现场

> `Git` 提供了一个`stash`(储藏)功能，可以把工作现场储藏起来，等以后恢复现场后继续工作

```git
  git stash
```

### 查看储藏的工作现场列表

```git
  git stash list
```

### 恢复工作现场不删除工作现场

```git
  git stash apply
```

> 再删除工作现场 `git stash drop`

```git
  git stash drop
```

### 恢复工作现场并删除工作现场

```git
  git stash pop
```

### 恢复指定status

```git
  git stash apply stash@{number}
```

### 查看远程库的信息

```git
  git remote
  git remote -v    // 查看详细信息
```

### 多人协作

> 首先，可以试图用`git push origin <branch name>` 推送自己的修改；    
> 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；    
> 如果合并有冲突，则解决冲突，并在本地提交；    
> 没有冲突或者解决掉冲突后，再用`git push origin <branch name>`推送就能成功！   
> 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch name> origin/<branch name>`。


> 查看远程库信息，使用`git remote -v`；
> 从本地推送分支，使用`git push origin <branch name>`，如果推送失败，先用`git pull`抓取远程的新提交；
> 在本地创建和远程分支对应的分支，使用`git checkout -b <branch name> origin/<branch name>`，本地和远程分支的名称最好一致；
> 建立本地分支和远程分支的关联，使用`git branch --set-upstream <branch name> origin/<branch name>`；
> 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。


## 使用标签tag

### 创建标签

```git
  //先切换到需要打标签的分支上
  git switch <branch name>
  git tag <tag name>  // 再打tag   例如git tag v1.0
  git tag -a <tag name> -m "描述内容" <commit id>  // 创建带有说明的标签
```

### 查看所有标签

```git
  git tag
```

### 查看标签信息 

```git
  git show <tag name>
```

> 如果我们忘记了打标签，但是已经过去了好久，怎么办呢？    
> 我们先通过 `git log --pretty=oneline --abbrev-commit`  查询历史提交的`commit id `   
> 然后找到我们要打tag的那个commit  通过 `git tag <tag name> <commit id>` 来打tag    
> 注意， 标签不是按照时间顺序列出的，而是按字母排序的。可以用 `git show <tag name>` 来查看标签信息    
> 用命令 `git tag -a <tag name> -m "描述内容" <commit id>  ` 可以创建带有说明的标签

### 删除标签

```git
  git tag -d <tag name>
```

### 推送某个标签到远程

```git
  git push origin <tag name>
```

### 一次性推送尚未推送到远程的标签

```git
  git push origin --tags
```

> 想要删除远程的标签      
> 1.先删除本地的 `git tag -d <tag name>  `      
> 2.从远程删除 ` git push origin :refs/tags/<tag name>`

## git push到GitHub的时候遇到! [rejected] master -> master (non-fast-forward)的问题

1、git pull origin master --allow-unrelated-histories //把远程仓库和本地同步，消除差异  

2、重新add和commit相应文件  

3、git push origin master  

4、此时就能够上传成功了  
