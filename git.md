### 廖雪峰 Git 教程

   1.  Git的优势不单是不必联网这么简单，Git极其强大的分支管理

      

​    2. 什么是版本库呢？版本库又名仓库，简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删        除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”



3. 每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit

```git
git reset --hard HEAD^ 回退到上一个版本
```

  ```git
git log --pretty=oneline 历史记录
  ```



 4.`HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`

  穿梭前，用 `git log` 可以查看提交历史，以便确定要回退到哪个版本



 5.要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本

 工作区，就是你在电脑里能看到的目录



6.版本库（Repository）工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库！！！！！！！！！！!



7.**Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区**，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。一般在Git 仓库目录中，是一个叫`index`的文件，通常多数说法还是叫暂存区域；暂存区在版本库里面



8.用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区。用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。暂存区是Git非常重要的概念，弄明白了暂存区，就弄明白了Git的很多操作到底干了什么。`git add`命令实际上就是把要提交的所有修改放到暂存区（index），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支



9.  Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。 那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`



10.每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。



11.撤销修改

​          1 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。`git checkout -- 文件名`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令。

​          2 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD 文件名`，就回到了1的步骤，然后在按照1的步骤做

​          3 已经提交了不合适的修改到版本库时，想要撤销本次提交，`git reset --hard HEAD^`不过前提是没有推送到远程库。



12. 命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。

    

13. `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

    

14. 删除远程库

     如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>` `git remote rm origin`命令。使用前，建议先用`git remote -v`查看远程库信息。关联一个远程库时必须给远程库指定一个名字，`origin`是默认习惯命名；关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

    

15. 从远程克隆

    ​        要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆

    

#### 分支管理

​       1  你创建了一个属于你自己的分支，别人看不到，还继续在原来的分支上正常工作，而你在自己的分支上干活，想提交就提交，直到开发完毕后，再一次性合并到原来的分支上，这样，既安全，又不影响别人工作。

​       2  `HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

​       3  一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点

​       4 当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上。不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变。

​       5 假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并。合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支

     ```
创建dev分支，然后切换到dev分支
    git checkout -b dev    

git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
    git branch dev
    git checkout dev
    
然后，用git branch命令查看当前分支
    git branch
    
把dev分支的工作成果合并到master分支上
    git merge dev (git merge命令用于合并指定分支到当前分支，会出现Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快，也不是每次合并都能Fast-forward，合并完成后，就可以放心地删除dev分支了)

删除dev分支
    git branch -d dev（在用 git branch 查看就没有 dev 分支了）
 
因为创建、合并和删除分支非常快，所以Git鼓励使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
     ```

**switch**

​        我们注意到切换分支使用`git checkout <branch>`，而撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑

​        实际上，切换分支这个动作，用`switch`更科学

```
创建并切换到新的dev分支，可以使用：
   git switch -c dev
   
直接切换到已有的master分支，可以使用：
   git switch master
```

```
查看分支：git branch

创建分支：git branch dev

创建➕切换分支：git checkout -b dev （git switch -c dev）相当于两条命令：git branch dev、git checkout dev

切换分支：git checkout master（git switch master）

合并某分支到当前分支：git merge dev（就是把 dev 分支合并到当前分支，即当前分支也是 dev 分支里面写的内容了）

删除分支：git branch -d dev（删除 dev 分支）
```

**解决冲突**

​     1 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

​     2 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

```
用 git log --graph 命令可以看到分支合并图。
```

**分支管理策略**

​      1、 通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。（git merge dev）

​      2 、如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

```
用 --no-ff方式的git merge

创建并切换dev分支
   git switch -c dev
   
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward，因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去
   git merge --no-ff -m "merge with no-ff" dev
   
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
```

**Bug分支**

​     有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

​    Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
git stash  当你没有写完不想提交，可以用这个，存储起来，在 git status 就干干净净了

工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：
    git stash list
    
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；

另一种方式是用git stash pop，恢复的同时把stash内容也删了

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动
```

**feature分支**

​		添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

```
销毁失败。Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的-D参数。。
   强行删除： git branch -D feature-vulcan
   
开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

版本回退（git reset --hard hash值、git reflog 查看命令历史）

git diff (查看未暂存的修改)
git diff --cache （查看未提交的暂存）
```

**拉取远程**

```
将远程主机 origin 的 master 分支拉取过来，与本地的 dev 分支合并:
  git pull origin master:dev
  
如果远程分支是与当前分支合并，则冒号后面的部分可以省略
  git pull origin master
```

**git fetch和git pull都可以将远端仓库更新至本地那么他们之间有何区别?**

```
git pull = git fetch + git merge
```

```
在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
```

```
查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突
```
**标签管理**

​       标签也是版本库的一个快照，tag就是一个让人容易记住的有意义的名字，它跟某个`commit`绑在一起

​       类似姓名和身份证的关系(其实就是别名)

```
切换到需要打标签的分支上

敲命令git tag <name>就可以打一个新标签
  git tag v1.0
  
可以用命令git tag查看所有标签
  git tag
  
默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？
方法是找到历史提交的commit id，然后打上就可以了：
比方说要对 11月 这次提交打标签，它对应的commit id是f52c633，敲入命令：
    git tag v1.1 f52c633
这样就在这个版本上打上标签了可以用：git tag 查看

标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：
   git show v1.1
   
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
  git tag -a v1.2 -m 'version 1.2' 1094adb
```

**操作标签**

```
如果标签打错了，也可以删除
   git tag -d v0.1

如果要推送某个标签到远程，使用命令git push origin <tagname>
   git push origin v1.0
   
或者，一次性推送全部尚未推送到远程的本地标签
   git push origin --tags
   
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
   git tag -d v1.2
然后，从远程删除。删除命令也是push，但是格式如下：
   git push origin :refs/tags/v0.9
```
#### Git 的核心部分是一个简单的键值对数据库（key-value data store）可以向数据库中插入任意内容，它会返回一个用于取回该值的 hash 键。

**树对象**

```
树对象解决了文件名的问题，它的目的将多个文件名组织在一起，其内包含多个文件名称与其对应的Key和其它树对像的用引用，可以理解成操作系统当中的文件夹，一个文件夹包含多个文件和多个其它文件夹。
```

Git是一个内容寻址的文件系统，其次才是一个版本控制系统。

在Git中，数据对象相当于文件内容，树对象相当于文件目录树，提交对象则是对文件系统的快照。

**数据对象**

```
数据对象是文件的内容，不包括文件名、权限等信息。Git会根据文件内容计算出一个hash值，以hash值作为文件索引存储在Git文件系统中。
git hash-object可以用来计算文件内容的hash值
数据对象只是解决了文件内容存储的问题，而文件名的存储则需要通过树对象来解决。
```

Git中的数据对象解决了数据存储的问题，树对象解决了文件名存储问题，提交对象解决了提交信息的存储问题

```
git hash-object git底层命令，用于向Git数据库中写入数据
git cat-file git底层命令，用于查看Git数据库中数据

git命令查看新生成的空git仓库的结构，会发现其中有一个objects文件夹，这就是git数据库的存储位置

Git的核心是它的对象数据库，其中保存着git的对象，其中最重要的是blob、tree和commit对象，blob对象实现了对文件内容的记录，tree对象实现了对文件名、文件目录结构的记录，commit对象实现了对版本提交时间、版本作者、版本序列、版本说明等附加信息的记录。这三类对象，完美实现了git的基础功能：对版本状态的记录。

Git引用是指向git对象hash键值的类似指针的文件。通过Git引用，我们可以更加方便的定位到某一版本的提交。Git分支、tags等功能都是基于Git引用实现的
```

**提交对象**

​       用git进出版本控制的时候，进行提交操作时，Git 会保存一个提交对象（commit object）







