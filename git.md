## 概念

### 为什么要有 VCS

通过提交记录形成历史版本，改动可以被说明记录，方便查看历史改动，对撤销历史改动等，同时也方便多人共同编辑。

### 中央式 VCS 与分布式的区别

- 分布式与中央式的主要区别是每个参与者都有本地仓库，提交会在本地仓库形成历史，而中央式只能将改动提交到中央仓库。
- 其次，分布式的本地仓库比较占用空间、首次 clone 耗时，但只包含文本的仓库耗费资源很少，一般是游戏开发这类包含了大尺寸文件的仓库会用到中央式仓库。

### 改动状态

- 未跟踪，新增的文件
- 已改动/未暂存，被跟踪的文件发生改动
- 已暂存，被跟踪文件的改动添加到暂存区
- 已提交，commit 到历史

### 分支

分支是指向某个 commit 的引用，HEAD 是当前 commit 的引用。这些引用都以文本方式存放在.git 目录中。

### pr、mr

github、gitlab 这些 git 仓库服务商提供的功能，方便对 merge 的代码进行 review。

## 配置

### .gitignore

** 两个型号代表匹配所有，比如 **/foo 代表匹配任何目录下的 foo 目录、文件，/foo/\*\* 表示匹配 foo 目录下的任何目录、文件。

## 操作

### commit

进入编辑器，默认是 vi

### push

将当前分支和分支包含的 commit 记录推送到远端仓库。

### merge

从分叉的 commit 开始到目标 commit 的 commit 记录应用到当前引用，并生成一个新的 commit。
一般 3 种情形：

- 冲突，修改了同一块内容，手动解决
- HEAD 领先目标 commit，啥也不做
- fast-forward，HEAD 落后目标 commit，将 HEAD 快速移动指向目标 commit

### rebase

与 merge 不同，rebase 将当前分支从分叉起到目标 commit 间的 commit 记录线性整合到目标分支上，具体操作就是将当前分支的 commit 记录依次追加到目标分支的 commit 记录里。

比如 git rebase master 就是将当前分支的记录追加到 master 分支，还需要在 master 分支执行 git merge branch1 将 master 分支的 HEAD 移动到最前面。

#### 修改历史 commit（本地）

- git rebase -i HEAD^^ 进入编辑页面
- 移动到需要修改的行，将 pick 改成 edit，wq 退出
- 再执行 git commit --amend，进入编辑页面
- 重新编辑 commit 注释，wq 保存退出
- 执行 git rebase --continue 更新 commit 记录

### reset

细节：

- reset 的 commit 如果是 merge，merge 进来的 commit 记录也会被 reset
