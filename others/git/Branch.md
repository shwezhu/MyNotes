# branch

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`或者`git switch <name>`

创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

`git branch`列出所有本地分支,等价于`ls ./.git/refs/heads/`
`git branch -r`列出所有远程分支

## Example

`$git init`

```
Initialized empty Git repository in /home/luffy/Documents/git_learning/.git/
```

`$vi main.cpp`

`$cat main.cpp`

```
master..1
```

` $git add main.cpp `

`git commit -m "master: add main.cpp"`

```
[master (root-commit) d3ddd3b] master: add main.cpp
 1 file changed, 1 insertion(+)
 create mode 100644 main.cpp
```

`$git checkout -b hippo`

```
Switched to a new branch 'hippo'
```

`$git branch`

```
* hippo
  master
```

`$cat main.cpp`

```
master..1
```

`$vi main.cpp`

`$cat main.cpp`

```
master..1
hippo..1
```

`$git add main.cpp `

`git status`

```
On branch hippo
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  	modified:   main.cpp
```

`$git commit -m "hippo: change main.cpp"`

```
[hippo fd0b5df] hippo: change main.cpp
 1 file changed, 1 insertion(+)
```

`$git log`

```
commit fd0b5dfd38bc88b48af580909fcaf60015f56eff (HEAD -> hippo)
Author: shaowen <17689207868@163.com>
Date:   Sat May 8 23:23:30 2021 +0800

    hippo: change main.cpp

commit d3ddd3b2099834b211f6bf9fee964b5095f3e6c0 (master)
Author: shaowen <17689207868@163.com>
Date:   Sat May 8 23:13:08 2021 +0800

    master: add main.cpp
```

`$git checkout master `

```
Switched to branch 'master'
```

`$cat main.cpp `

```
master..1
```

`$git log`

```
commit d3ddd3b2099834b211f6bf9fee964b5095f3e6c0 (HEAD -> master)
Author: shaowen <17689207868@163.com>
Date:   Sat May 8 23:13:08 2021 +0800

    master: add main.cpp
```

` $git merge hippo`

```
Updating d3ddd3b..fd0b5df
Fast-forward
 main.cpp | 1 +
 1 file changed, 1 insertion(+)
```

注意Updating d3ddd3b..fd0b5df中的d3ddd3b是在master上提交的提交编号，在上面的输出可以找到，fd0b5df是在分支hippo上提交的编号，看下面的git log输出也行，开头几位数便是。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`hippo`的当前提交，所以合并速度非常快。

`$git log`

```
commit fd0b5dfd38bc88b48af580909fcaf60015f56eff (HEAD -> master, hippo)
Author: shaowen <17689207868@163.com>
Date:   Sat May 8 23:23:30 2021 +0800

    hippo: change main.cpp

commit d3ddd3b2099834b211f6bf9fee964b5095f3e6c0
Author: shaowen <17689207868@163.com>
Date:   Sat May 8 23:13:08 2021 +0800

    master: add main.cpp
```

`$git branch`

```
  hippo
* master
```

`$cat main.cpp`

```
master..1
hippo..1
```

合并完成后，就可以安全地删除`hippo`分支了：

```
￥git branch -d hippo 
Deleted branch hippo (was fd0b5df).
```

只剩下一个分支，查看

```
$ git branch
* master
```

切换分支，用`switch`更好一些：

创建并切换到新的`hippo`分支，可以用：

```
$ git switch -c dev
```

直接切换到已有的`master`分支，可以用：

```
$ git switch master
```