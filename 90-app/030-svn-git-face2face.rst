Git和SVN面对面
*********************

面对面访谈录
============

**Git：我的提交历史本身就是一幅美丽的图画——DAG（Directed Acylic Graph，\
有向非环图），可以看到各个分支之间的合并关系。而你SVN，你的提交历史怎么\
是一条直线呢？要是在重症监护室看到你，还以为你挂掉了呢？**

SVN：我觉得挺好，至少我每次提交会有一个全局的版本号，而且我的版本号是递\
增的。你的版本号不是递增的吧！

**Git：你说的对，我的版本号不是一个简单递增的数字，而是一个长达40位的十\
六进制数字（哈希值），但是可以使用短格式，只要不冲突。虽然我的提交编号看\
起来似乎是无序的，但实际上我每一个提交都记录了父提交甚至是双亲或多亲提交\
，因此可以很容易的从任意一个提交开始建立一条指向历史提交的跟踪链。**

SVN：是啊，我的一个提交和前一个提交有时根本没有关系，例如一个提交是发生\
在主线\ :file:`/trunk`\ 中的，下一个提交可能就发生在\ :file:`/branches/1.3.x`\
分支中。你要知道要想画出一个像你那样的分支图，我要做多少工作么？我不容易呀。

**Git：我一直很奇怪，你的分支和里程碑怎么看起来和目录一样？我的分支和里\
程碑名字虽然看起来像是目录，实际上和工作区的目录完全没有关系，只是对提交\
ID的一个记号而已。**

SVN：我一开始觉得我用轻量级拷贝的方式实现分支和里程碑会很酷，也很快。但\
是我发现很多人在使用我的时候，直接在版本库的根目录下创建文件而不是把文件\
创建在\ :file:`/trunk`\ 目录下，这就导致这些人无法再创建分支和里程碑了，\
因为是无法将根目录拷贝到子目录的呀！

**Git：那么你是如何对分支合并进行跟踪的呢？我因为有了DAG的提交关系图，是\
很容易看出来分支之间的合并历史，但是你是怎么做到的呢？**

SVN：我用了一点小技巧，就是通过属性（svn:mergeinfo）记录了合并的分支名和\
版本范围，这样再合并的时候，我会根据相关属性确定是否要合并。但是如果经常\
在子目录下合并，有太多的\ ``svn:mergeinfo``\ 属性等待我检查，我会很困扰 。\
还有我的这个功能是在1.5以后版本才提供的，因此老版本会破坏这个机制。

SVN：对了，我的属性能干很多事哦，我甚至可以把我的照片作为属性附加在文件上。

**Git：这点我承认，你的属性非常强大。其实我也支持属性，只不过实现方式不\
同罢了。而且我可以通过评注的方式为任意对象（提交、文件、里程碑等）添加评\
注，也可以实现把照片做为评注附加在文件上，可是这个功能有什么实际用处么？**

SVN：我有轻量级拷贝，而我的分支和里程碑就是通过拷贝实现的，很强大哦。

**Git：我根本就不需要轻量级拷贝，因为我对文件的保存是和文件路径无关的，\
我只关心内容。所以相同内容的文件无论它们的文件名相差有多大，在我这里只保\
存一份。而你SVN，如果用户忘了用轻量级拷贝，版本库是不是负担很重啊。**

SVN：听说你不能针对目录授权，这可是我的强项，所以公司无论大小都在用我作\
为版本控制系统。

**Git：不要说你的授权了，简直是一团糟。虽然这本书的作者为你写了一个图形\
化的授权管理工具（http://www.ossxp.com/doc/pysvnmanager/user-guide/user-guide.html）\
会有所改善，但是你糟糕的分支和里程碑的实现，会导致授权在新的分支和里程碑\
中要一一设置，工作量其大无比。虽然泛路径授权是一个解决方案，\
但是官方并没有提供啊。**

**Git：说说我的授权吧。如果你认真的读过本书服务器架设的相关章节，你会为\
我能够提供的按照分支，以及按照路径进行写操作授权而击掌叫好的。当然我的读\
操作授权还不能做到很精细，但是可以将版本库拆分成若干小的版本库啊，再用参\
照本书介绍的各种多版本库协同模式，会找到一个适合的解决方案的啊。**

**Git：我的工作区很干净。只在工作区的根目录下有一个\ :file:`.git`\ 目录，\
此外再无其他。**

SVN：我要在工作区的每一个目录下都放置一个\ :file:`.svn`\ 目录，这个目录\
在Linux下可是隐藏的哦。这个目录下不但有跟踪工作区文件状态的跟踪文件，而\
且还有每一个文件的原始拷贝呢。这样有的操作就可以脱离网络执行了，例如：\
差异比较，工作区文件的回滚。

**Git：嗯，你要是像我一样再保存多一点内容（整个版本库）就更好了。像你这\
样在每个工作区子目录下都有一个\ :file:`.svn`\ 目录，而且每个\
:file:`.svn`\ 目录下都有文件原始拷贝，在进行内容搜索的时候会搜索出来两份吧，\
太干扰了。而且你这么做和CVS一样有安全风险，造成本地文件名的信息泄漏，\
千万不要在Web服务器上用SVN检出哦。**

**Git：我的操作可以不需要网络。因为我在本地拥有完整的版本库，几乎所有操\
作都是在本地完成。**

SVN：正如前面说到的，我有部分命令可以不需要网络，但是其他绝大多数命令还\
是要依赖网络的。

SVN：你怎么没有更新（update）命令？还有你为什么老是要执行检出命令\
（checkout）？对我而言，检出命令只在工作区创建时一次完成的。

**Git：你的这个问题怎么和CVS问的一样。你的更新（update）命令执行的很慢对\
么？首先你要用检出命令（checkout）建立工作区，然后你要经常的执行更新\
（update）命令进行更新，否则容易造成你的更改和他人更改发生冲突。**

**Git：之所以你需要更新是因为你的版本库在远程啊。别忘了我的版本库是在本\
地，我的每一步操作工作区和版本库都是同步的，所以更新操作就没有存在的必要\
了。而我的检出（checkout）操作一般是用户切换分支，或者从本地版本库检出丢\
失的文件或覆盖本地错误改动的文件时用到。如果我没记错的话，你切换分支用的\
是\ :command:`svn switch`\ 命令对么？**

**Git：实际上我也有一个比较耗时的网络操作命令叫做\ :command:`git fetch`\
或\ :command:`git pull`\ ，这两个操作是从远程版本库获取他人改动。一般使\
用我（Git）做团队协作的时候，会部署一个集中共享的版本库，我就从这个共享\
的版本库执行拉回操作。也也许你（SVN）会觉得\ :command:`git fetch`\ 或\
:command:`git pull`\ 和你的\ :command:`svn update`\ 命令更像吧。至于你的\
检出命令（\ :command:`svn checkout`\ ），实际上和我克隆命令\
（\ :command:`git clone`\ ）很相似，只不过我的克隆命令不但创建了本地工作区，\
而且在本地还复制了和远程版本库一样的本地版本库。**

SVN：为什么你的检入命令（commit）命令执行的那么快？

**Git：是的，我的检入命令飞一般就执行完了，也是因为版本库就在本地。也许\
你（SVN）会觉得我的推送命令（\ :command:`git push`\ ）和你的检入命令\
（\ :command:`svn commit`\ ）更相像，其实这是一个误会。如果我不做本地\
提交，是不能通过推送命令（\ :command:`git push`\ ）将我的本地的提交共享给\
（推送给）其他版本库的。你（SVN）每一次提交都要和版本库进行网络通讯，而\
我可以在本地版本库进行多次提交，直到我的主人想喝咖啡了才执行一次\
:command:`git push`\ ，将我本地版本库中新的提交推送给远程版本库。**

SVN：我能一次检出一个目录，你好像不能吧？

**Git：所以我有子模组，以及repo等第三方工具，可以帮助我把一个大的版本库\
拆开多个版本库组合来使用啊。**

SVN：我能添加空目录，你好像不能吧！

**Git：是的，我现在还不能记录空目录，但是用户往往在空目录下创建一个隐含\
文件，并将该隐含文件添加到版本库中，也就实现了空目录添加的功能。**


SVN和Git命令对照
====================

+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 比较项目              | SVN命令                                            | Git命令                                                    |
+=======================+====================================================+============================================================+
| URL                   | svn://host/path/to/repos                           | git://host/path/to/repos.git                               |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | https://host/path/to/repos                         | ssh://user@host/path/to/repos.git                          |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | file:///path/to/repos                              | user@host:path/to/repos.git                                |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       |                                                    | file:///path/to/repos.git                                  |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       |                                                    | /path/to/repos.git                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 版本库初始化          | svnadmin create <path>                             | git init [--bare] <path>                                   |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 导入数据              | svn import <path> <url> -m ...                     | git clone; git add .; git commit                           |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 版本库检出            | svn checkout <url/of/trunk> <path>                 | git clone <url> <path>                                     |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 版本库分支检出        | svn checkout <url/of/branches/name> <path>         | git clone -b <branch> <url> <path>                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 工作区更新            | svn update                                         | git pull                                                   |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 更新至历史版本        | svn update -r <rev>                                | git checkout <commit>                                      |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 更新到指定日期        | svn update -r {<date>}                             | git checkout HEAD@'{<date>}'                               |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 更新至最新提交        | svn update -r HEAD                                 | git checkout master                                        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 切换至里程碑          | svn switch <url/of/tags/name>                      | git checkout <tag>                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 切换至分支            | svn switch <url/of/branches/name>                  | git checkout <branch>                                      |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 还原文件/强制覆盖     | svn revert <path>                                  | git checkout -- <path>                                     |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 添加文件              | svn add <path>                                     | git add <path>                                             |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 删除文件              | svn rm <path>                                      | git rm <path>                                              |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 移动文件              | svn mv <old> <new>                                 | git mv <old> <new>                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 清除未跟踪文件        | svn status | sed -e "s/^?//" | xargs rm            | git clean                                                  |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 清除工作锁定          | svn clean                                          | \-                                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 获取文件历史版本      | svn cat -r<rev> <url/of/file>@<rev> > <output>     | git show <commit>:<path> > <output>                        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 反删除文件            | svn cp -r<rev> <url/of/file>@<rev> <path>          | git add <path>                                             |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 工作区差异比较        | svn diff                                           | git diff                                                   |
|                       |                                                    +------------------------------------------------------------+
|                       |                                                    | git diff --cached                                          |
|                       |                                                    +------------------------------------------------------------+
|                       |                                                    | git diff HEAD                                              |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 版本间差异比较        | svn diff -r <rev1>:<rev2> <path>                   | git diff <commit1> <commit2> -- <path>                     |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 查看工作区状态        | svn status                                         | git status -s                                              |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 提交                  | svn commit -m "<msg>"                              | git commit -a -m "<msg>" ; git push                        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 显示提交日志          | svn log | less                                     | git log                                                    |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 逐行追溯              | svn blame                                          | git blame                                                  |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 显示里程碑/分支       | svn ls <url/of/tags/>                              | git tag                                                    |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn ls <url/of/branches/>                          | git branch                                                 |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       |                                                    | git show-ref                                               |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 创建里程碑            | svn cp <url/of/trunk/> <url/of/tags/name>          | git tag [-m "<msg>"] <tagname> [<commit>]                  |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 删除里程碑            | svn rm <url/of/tags/name>                          | git tag -d <tagname>                                       |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 创建分支              | svn cp <url/of/trunk/> <url/of/branches/name>      | git branch <branch> <commit>                               |
|                       |                                                    +------------------------------------------------------------+
|                       |                                                    | git checkout -b <branch> <commit>                          |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 删除分支              | svn rm <url/of/branches/name>                      | git branch -d <branch>                                     |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 导出项目文件          | svn export -r <rev> <path> <output/path>           | git archive -o <output.tar> <commit>                       |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn export -r <rev> <url> <output/path>            | git archive -o <output.tar> --remote=<url> <commit>        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 反转提交              | svn merge -c -<rev>                                | git revert <commit>                                        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 提交拣选              | svn merge -c <rev>                                 | git cherry-pick <commit>                                   |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 分支合并              | svn merge <url/of/branch>                          | git merge <branch>                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 冲突解决              | svn resolve --accept=<ARG> <path>                  | git mergetool                                              |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn resolved <path>                                | git add <path>                                             |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 显示文件列表          | svn ls                                             | git ls-files                                               |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn ls <url> -r <rev>                              | git ls-tree <commit>                                       |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 更改提交说明          | svn ps --revprop -r<rev> svn:log "<msg>"           | git commit --amend                                         |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 撤消提交              | svnadmin dump、svnadmin load 及 svndumpfilter      | git reset [ --soft | --hard ] HEAD^                        |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
| 属性                  | svn:ignore                                         | .gitignore 文件                                            |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn:mime-type                                      | text 属性                                                  |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn:eol-style                                      | eol 属性                                                   |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn:externals                                      | git submodule 命令                                         |
|                       +----------------------------------------------------+------------------------------------------------------------+
|                       | svn:keywords                                       | export-subst 属性                                          |
+-----------------------+----------------------------------------------------+------------------------------------------------------------+
