###11/10/2019 7:52:49 AM 
###There is no gene for the human spirit.
**没有哪个基因能够决定人的灵魂。**
<center><h1><a href="https://git-scm.com/book/zh/v2/">git</a></center></h1>

###什么是版本控制

* 版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。
* 本地版本控制系统

<img src="local.png"/>
* 集中化版本控制系统
<img src="https://git-scm.com/book/en/v2/images/centralized.png"/>
* 分布式版本控制系统
<img src="https://git-scm.com/book/en/v2/images/distributed.png"/>


#[git简史](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-Git-%E7%AE%80%E5%8F%B2)

* 速度

* 简单的设计

* 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）

* 完全分布式

* 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

###git基础

* 直接记录快照，而非差异比较
* 近乎所有操作都是本地执行
* git保持完整性
* git一般只添加数据

* 三种状态
   1. 已提交（committed）数据安全的保存在数据库中
   2. 已修改（modified）修改了文件，还没有保存到数据库中
   3. 已暂存（staged）对一个已修改的文件当前版本做了标记，使之包含在下次提交的快照中

###git项目的三个工作区域
* Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。

* 工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。

* 暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作“索引”，不过一般说法还是叫暂存区域。

<img src="https://git-scm.com/book/en/v2/images/areas.png"/>

* git的基本工作流程
   1. 在工作目录中修改文件。

   2. 暂存文件，将文件的快照放入暂存区域。

   3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

>>>如果 Git 目录中保存着特定版本的文件，就属于已提交状态。 如果作了修改并已放入暂存区域，就属于已暂存状态。 如果自上次取出后，作了修改但还没有放到暂存区域，就是已修改状态。

###[起步命令行](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%91%BD%E4%BB%A4%E8%A1%8C)
###[git的安装](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)
###[git的详细安装](https://blog.csdn.net/Night2233/article/details/89304835)

###git命令
1. 初始化 git init
2. 创建文件 vim a.txt
3. 查看状态 git status | git status -s | git status --short
4. 查看历史记录 history
5. 工作区提交到暂存区 git add a.txt
6. 将所有文件提交到暂存区 git add .
7. 暂存区提交到本地库 git commit -m "注释" a.txt
8. 将暂存区所有的文件提交到本地库 gi commit -m "阿萨达" .
>>三种状态的转换  工作区 git add 暂存区 git commit 本地库
###获得git仓库
* 在现有的目录下初始化仓库
  * git init该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 但是，在这个时候，我们仅仅是做了一个初始化的操作，你的项目里的文件还没有被跟踪。 
* 克隆现有的仓库
  * 克隆命令的格式 git clone [url]
  * 克隆的可连接库 libgit2
  * 要克隆git的可连接库libgit2 $ git clone https://github.com/libgit2/libgit2
  * 如果克隆时自定义库的名字 $ git clone https://github.com/libgit2/libgit2 mylibgit

###忽略文件
* 总有一些文件无需git的管理，比如日志等。因此我们创建一个.gitignore的文件	<br/>
	`$ cat.gitignore`  
    `*.[oa]`  
  * 第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文件都是编译过程中出现的 
  * 第二行告诉 Git 忽略所有以波浪符（~）结尾的文件，许多文本编辑软件（比如 Emacs）都用这样的文件

* .gitignore格式规范如下
   * 所有空行或者以 ＃ 开头的行都会被 Git 忽略。

   * 可以使用标准的 glob 模式匹配。

   * 匹配模式可以以（/）开头防止递归。

   * 匹配模式可以以（/）结尾指定目录。

   * 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。	
   
###查看提交历史
* git log  查看提交命令
* git log -p -2 查看最近两次提交命令 
* git log --stat 每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。
* git log --pretty  这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 oneline 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 short，full 和 fuller 可以用，展示的信息或多或少有些不同，请自己动手实践一下看看效果如何。
* git long --pretty=format 可以定制要显示的记录格式。 这样的输出对后期提取分析格外有用 — 因为你知道输出的格式不会随着 Git 的更新而发生改变：
    $ git log --pretty=format:"%h - %an, %ar : %s"  
    ca82a6d - Scott Chacon, 6 years ago : changed the version number  
	085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
	a11bef0 - Scott Chacon, 6 years ago : first commit
* [其他详细查询](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)
###创建远程库地址别名
* git remote add ys https://github.com/system822/java34.git
* git remote -v
###删除别名
* git remote rem ys(远程库别名)
###查看配置文件列表/设置邮箱 账号
* git config --list
* git config user.name ""
* git config user.email ""
###推送
* git push ys master 把名单推送到远程nba仓库中 ys是别名
###显示命令（extend）
* ll
* ll -la
###克隆的效果
* 完整的吧远程库下载到本地
* 创建origin远程地址别名
* 初始化本地库

###团队成员邀请
1. 先去gitup上发一个邀请
2. 对方接收后获得你的克隆地址，git clone 克隆地址
3. 对方进入克隆文件夹后修改文件信息
4. 通过 git add file(暂存)
5. git commit -m fileName（本地库）
6. 推送 git push 克隆地址 master
7. 组长进行合并
   1. 拉取远程库到本地库 git fetch ys(别名) master
    (只是将文件下载下来，并没有改变工作区文件 )
   2. 想要查看 可以切换到ys的master git checkout ys/master
   3. 查看文件 cat fileName 切回去 继续操作 git checkout master
   4. 然后改变工作区的文件需要合并 git merge ys/master
8. a修改b的克隆文件，提交到b的远程库中，b使用pull 拉出最新的数据 git pull ys master\
###新修改的代码的文件，将会被git服务器上的代码覆盖；我当然不想刚刚写的代码被覆盖掉，看了git的手册，发现可以这样解决：
1. git stash //暂存当前进行的工作
2. git pull origin master //拉取服务器的代码
3. git stash pop //合并暂存的代码

###解决冲突
* 当两个文件修改的是同一个位置 冲突怎么办
   1. 先拉取 git pull origin(别名) master
   2. 编辑冲突文件
   3. 添加暂存区，提交，推送

  
  
***
###git 查看修改邮箱
	先查看用户名：git config user.name
	在查看邮  箱：git config user.email
	
	修改：
	git config --global user.name "xiayiye5"
	git config --global user.email "13343401268@qq.com"

##[Eclipse和git的操作](https://www.cnblogs.com/jpfss/p/8027347.html)

***
###问题原因：远程库与本地库不一致造成的，在hint中也有提示把远程库同步到本地库就可以了。
* 解决办法：使用命令行
	git pull --rebase origin master
	该命令的意思是把远程库中的更新合并到（pull=fetch+merge）本地库中，–-rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中。出现如下图执行pull执行成功后，可以成功执行git push origin master操作。

###[Eclipse解决项目冲突的方法](https://www.cnblogs.com/haimishasha/p/5980416.html)

  