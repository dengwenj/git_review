### 对于`git`的原理我的理解：

​        git在每次`commit`的时候都会生成一个`tree`对象、一个`parent`对象（不是第一次提交的话）、和一个commit对象，这个最顶层的tree就是对应整个工作目录（文件夹名称都是`hash`值的前两位），tree下面还有`blob`、tree对象，都是一串`hash`值指向对应的内容，当某个文件发生变动的时候会重新保存一次快照，如果文件没有发生变动，保留的快照依然是上次的。数据都是保存在`objects`中的。



**我新建了一个 git 仓库，文件下只有一个 master.txt，来验证测试**

![1](/Users/dengwenjie/Desktop/1.png)



**我提交一次，git log --oneline 看提交历史**

![2](/Users/dengwenjie/Desktop/2.png)

**然后执行 git cat-file -p f2b2866**

![4](/Users/dengwenjie/Desktop/4.png)

​       这里就得出结论了：在第一次提交的时候是没有 `parent`对象的，看到`tree`对象的hash值，我在继续执行这个 hash 值

![5](/Users/dengwenjie/Desktop/5.png)

​      这里有得出一个结论：blob对象也对应一个hash值，然后我在对此hash执行

![6](/Users/dengwenjie/Desktop/6.png)                     

​      这里就完全出来了，这个`blob`对象就是存的我`echo '理解.git文件夹下的objects文件'`内容，这个引用指向的就是我写的文件内容。

**这里我把objects文件夹打开了，正如我说的名称就是hash值的前两位**

![7](/Users/dengwenjie/Desktop/7.png)

**接着我有验证了不是第一次commit提交的时候就会有parent对象，这里我已经第二次提交了，git log --oneline 查看**

![8](/Users/dengwenjie/Desktop/8.png)

**执行 git cat-file -p a8775发现这里已经有parent对象了（不是第一次提交）**

![9](/Users/dengwenjie/Desktop/9.png)



**git有三个对象，commit，blob，tree，我测试了我觉得这三个对象不是平级关系，在第一次commit后，会生成commit对象，这个commit对象下面有一个顶层的tree对象，然后tree对象下面再包含tree对象或者blob对象。最底层的blob对象就是文件的内容，而且git就是依照这些对象的hash值找到对应的文件，记录每次commit的状态，每次提交commit后变化的文件会保存一个新的快照（hash），没有变化的文件依然保存原来的快照（hash）**

