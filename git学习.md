一、前置知识
1、git 的管理是目录级别，而不是设备级别
2、push 操作：是用本地仓库的 commits 记录 去覆盖远端仓库的 commits 记录（push 实质和这个说法略有不同），而如果远端仓库含有本地没有的 commits 的时候，push 将会导致远端仓库的 commits 被擦掉。这种是不被允许的，因此 git 会在 push 的时候进行检查，如果出现这样的情况，就会 push 失败。
push 实质：push 是把当前分支上传到远程仓库，并把这个 branch 的路径上所有 commits 也一并上传。
push 的时候之后上传当前分支，并不会上传 HEAD；远程仓库的 HEAD 是永远指向默认分支 master 的。

3、pull：内部实现是把远程仓库使用 git fetch 取下来以后在进行 merge 操作。
4、merge：把不同的内容进行合并，生成新的提交，叫做合并，即 merge。
5、引用：
引用的本质就是一个个字符串，Git 中的每一个引用，都是通过文本文件的形式存储在本地仓库.git 目录中，而 Git 在工作的时候，就是通过这些字符串来判断这些引用是指向谁的。
（1）git log 查看提交历史，每个 commit 后面都会有一串数字即 SHA-1 校验和，类似这个 commit 的 ID，可以作为引用进行操作 commit。但是 SHA-1 校验和是无逻辑的数字，不好记，因此 Git 提供引用的机制，使用固定的字符串作为引用，指向某个 commit，作为操作 commit 时的快捷方式。例如（HEAD -> master, origin/master, origin/HEAD）
（2）HEAD：
（a）是一个永远指向当前 commit 的引用。当前 commit 指的是当前工作目录对应的 commit。（每次有新的 commit，工作目录自动与最新的 commit 对应，HEAD 会转而指向最新的 commit；使用 checkout/reset 等指令手动指定改变当前 commit 的时候，HEAD 也会一起跟过去）。
（b）HEAD 除了可以指向 commit，也可以指向一个 branch。当它指向某个 branch 的时候，会通过这个 branch 间接的指向某个 commit；当 HEAD 在提交时自动向前移动时，会带着它所指向的 branch 一起向前移动。
（3）master：特殊的 branch，Git 默认的主 branch。
（4）branch：分支，是一个指向 commit 的引用。通俗的理解：可以把一个 branch 理解为从初始 commit 到 branch 所指向的 commit 之间的所有 commits 的一个串。
（a）所有的分支都是平等的
（b）branch 包含从初始 commit 到它的所有路径，而不是一条路径，这些路径彼此平等。

二、常用命令
git status 查看工作目录当前状态
git log 查看提交历史
git add . 让 git 开始追踪文件，将文件放到暂存区；暂存区的意思可以理解为，集中改动以待提交
每个文件都有"changed/unstaged（已修改）"、"staged（已修改并暂存）"、"commited(已提交)"三种状态，以及一种特殊状态"untracked（未跟踪）"
git commit 提交
git push 将本地提交推送到远程仓库，联网操作
git pull 将远程代码拉到本地
git clone 把远程仓库 clone 到本地
git clone remoteUrl repositoryName 在 clone 项目时，手动指定本地仓库的根目录名称
git branch 名称 创建 branch
git checkout 分支名 切换分支（HEAD 就会指向新的 branch）
git checkout -b 名称 用指定的名称创建一个分支并切换到这个分支
git branch -d 分支名 删除一个分支
（1）HEAD 指向的 branch 不能删除，如果需要删除 HEAD 指向的 branch，可以先用 checkout 把 HEAD 指向其他地方。
（2）由于 Git 中的 branch 只是一个引用，删除 branch 只会删掉这个引用，不会删除任何 commit。（如果一个 commit 不存在任何一个 branch 上，一定时间后，Git 的回收机制会删除掉它。）

git commit --amend 对最新的提交进行修正（不是修改原 commit，而是生成一条新的 commit）
