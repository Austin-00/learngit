# Git学习

## 一、创建版本库

1. 创建一个空目录

   ```
   $ mkdir learngit
   $ cd learngit
   $ pwd
   ```

   * `pwd`用于显示当前目录

2. 通过`git init` 命令把这个目录变成Git可以管理的仓库

   ```
   $ git init
   ```

   * 当前目录下多了一个`.git`的目录,这个目录是Git用来跟踪管理版本库

3. 把文件添加到版本库

   * 首先编写一个**.txt**文件，**一定要放到之前建立的目录中**

   * 用命令`git add`告诉Git，把文件添加到仓库

     ```
     $ git add readme.txt
     ```

   * 用命令`git commit`告诉Git，把文件提交到仓库

     ```
     $ git commit -m "wrote a readme file"
     ```

     * `git commit`命令，`-m`后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
     * `git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的`readme.txt`文件）；`x insertions`：插入了x行内容（`readme.txt`有x行内容）。


## 二、掌握工作区状态

1. **`git status`命令**可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。
2. **`git diff xxx.txt`**顾名思义就是查看difference，显示的格式正是Unix通用的diff格式,可以知道对`readme.txt`作了什么修改。
   * 提交修改的步骤
     * 在第一部提交文件入库前，用`git diff`检查发生了什么改动，方便添加到`git commit`中。
     * `git add`。
     * 在执行git commit`之前，我们再运行`git status`看看当前仓库的状态。
     * `git status`告诉我们，将要被提交的修改包括`readme.txt`，下一步，就可以放心地提交了

3. **`git log`**命令显示从最近到最远的提交日志，告诉我们历史记录，对文件每次都修改了什么内容（显示commit内容）。
   *  如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数 :`$ git log --pretty=oneline`
   *  **按q退出**
4. `cat xxx.txt`用来查文件内容。

## 三、版本回退

1. 在Git中，**用`HEAD`表示当前版本**,**上一个版本就是`HEAD^`**，上上一个版本就是`HEAD^^`，**当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`**。

2. `git reset`命令:要把当前版本回退到上一个版本就可以使用`git reset`命令

   * ```
     $ git reset --hard HEAD^
     /*--hard参数有啥意义？这个后面再讲*/
     ```

3. 如果回退后，想回到回退前的版本：
   * 方法一：可以找到那个版本的commit ID,于是就可以回到未来的某一个版本:**`$ git reset --hard 1094a`**(1049a截取ID前几位)
   * 方法二：找不到新版本的`commit id`，Git提供了一个命令`git reflog`用来记录你的每一次命令，从而获得ID，然后重复方法一。

## 四、工作区和暂存区

1. #### 工作区（Working Directory）

   就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区。

2. #### 版本库（Repository）

   工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

   Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`

3. `git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支

   * 例子：第一次修改 -> `git add` -> 第二次修改 -> `git commit`：

     Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

4. 用**`git diff HEAD -- readme.txt`**命令可以查看工作区和版本库里面最新版本的区别。

## 五、撤销修改

1. **`git checkout -- readme.txt`**意思就是，把`readme.txt`文件在工作区的修改全部撤销，这里有两种情况：

   一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

   一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

   总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

2. **`git reset HEAD <file>`**可以把暂存区的修改撤销，即在修改后，进行了`git add`，可以通过该指令将修改从暂存区撤销退出回到工作区，然后在工作区通过`git checkout -- readme.txt`进行撤销修改。

## 六、删除文件

1. **`rm xxx.txt`命令**可以将文件从工作区删除。
2. **`git rm`命令**可以将文件从版本库中删除。(效果和`git add`相同)。
3. 如果用rm删错了文件，因为版本库中还有，可以通过` git checkout -- test.txt`很容易地将误删的文件恢复到最新的版本到工作区。

## 七、远程仓库

### 一、添加远程库

1. 要关联一个远程库，使用命令`git remote add origin  https://github.com/Austin-00/learngit.git`；

2. 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

3. 此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

4. 分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 二、从远程库克隆

1. 要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

   Git支持多种协议，包括`https`，但`ssh`协议速度最快。

   `GitHub`给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。

   ```
   $ git clone git@github.com:xxx(用户名)/gitskills.git
   $ git clone + 要克隆的网址
   ```

## 八、分支管理

### 一、创建并合并分支

1. Git鼓励大量使用分支：

   * 查看分支：`git branch`

   * 创建分支：`git branch <name>`

   * 切换分支：`git checkout <name>`或者`git switch <name>`（推荐git switch）

   * 创建+切换分支：`git checkout -b <name>`或者`git switch -c <name>`（推荐git switch）

   * 合并某分支到当前分支：`git merge <name>`

   * 删除分支：`git branch -d <name>`

### 二、解决冲突

1. 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

2. 解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

3. 用`git log --graph`命令可以看到分支合并图。
4. 合并分支时，`--no-ff`参数，表示禁用`Fast forward`。

### 三、分支策略

1. 在实际开发中，我们应该按照几个基本原则进行分支管理：

   * 首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

   * 那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

   * 你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

### 四、保存工作现场

1. 场景：在现实的工作中，我们在一个分支上工作上时，这时需要我们对master上的bug进行更改，这时我们需要放下手里的工作，但是我们一旦从该分支的工作区离开再回来时，工作区的内容就会被覆盖，因此需要保存工作现场。

   并且在我们修复了master上的bug后，我们的分支工作区恢复现场后，master的bug还是存在的没有被合并到我们的分支中，因此我们也需要将master中的内容也合并过来。

2. 方法：

   * **储存现场**：`git stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。

   * **查看现场**：`git stash list`命令。

   * **恢复现场**：

     * `git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除。

     * `git stash pop`，恢复的同时把stash内容也删了。

     可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令`$ git stash apply stash@{0}`来恢复某一个现场。

   * **将在master中的修改也合并到当前分支**：
     
     * `git cherry-pick <commit>`在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

### 五、Feature分支

1. 开发一个新feature，最好新建一个分支；

2. 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 六、多人协作

1. 查看远程库信息，使用`git remote -v`；
2. 本地新建的分支如果不推送到远程，对其他人就是不可见的；
3. 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
4. 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
5. 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
6. 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

### 七、rebase

1. rebase操作可以把本地未push的分叉提交历史整理成直线；
2. rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 九、标签管理

### 一、创建标签

1. 步骤方法：
   * 首先，切换到需要打标签的分支上。
   * 敲命令`git tag <name>`就可以打一个新标签。
   * 可以用命令`git tag`查看所有标签。

2. 默认标签是打在最新提交的commit上的。

3. 想要给过去的commit打标签:

   * 用`$ git log --pretty=oneline --abbrev-commit`。找到历史提交的commit ID。

   * 比如找到了commit ID，则可以通过下面的方法给该提交打标签。、

     ```
     $ git tag v0.9 f52c633
     ```

4. 标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息。

5. 还可以创建带有说明的标签，**用`-a`指定标签名，`-m`指定说明文字：**

   ```
   $ git tag -a v0.1 -m "version 0.1 released" 1094adb
   ```

6.  注意：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

### 二、 操作标签

1. **删除本地标签**：`$ git tag -d v0.1`

2. **推送标签**：

   1. 命令`git push origin <tagname>`。推送某一个标签到远程。

   2. 一次性推送全部尚未推送到远程的本地标签：

      ```
      $ git push origin --tags
      ```

3. **删除远程标签**：

   从本地删除：

   ```
   $ git tag -d xxx
   ```

   然后，从远程删除。删除命令也是push，但是格式如下：

   ```
   $ git push origin :refs/tags/xxx（标签名）
   ```

## 十、使用Github

1. 在GitHub上，可以任意Fork开源仓库。

   参与一个开源项目，从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址`git@github.com:twbs/bootstrap.git`克隆，因为没有权限，你将不能推送修改。

   Bootstrap的官方仓库`twbs/bootstrap`、你在GitHub上克隆的仓库`my/bootstrap`，以及你自己克隆到本地电脑的仓库，他们的关系就像下图显示的那样：

   ```ascii
   ┌─ GitHub ────────────────────────────────────┐
   │                                             │
   │ ┌─────────────────┐     ┌─────────────────┐ │
   │ │ twbs/bootstrap  │────>│  my/bootstrap   │ │
   │ └─────────────────┘     └─────────────────┘ │
   │                                  ▲          │
   └──────────────────────────────────┼──────────┘
                                      ▼
                             ┌─────────────────┐
                             │ local/bootstrap │
                             └─────────────────┘
   ```

   如果你想修复bootstrap的一个bug，或者新增一个功能，立刻就可以开始干活，干完后，往自己的仓库推送。

   如果你希望bootstrap的官方库能接受你的修改，你就可以在GitHub上发起一个pull request。

## 十一、使用Gitee

1. 使用Gitee和使用GitHub类似，我们在Gitee上注册账号并登录后，需要先上传自己的SSH公钥。选择右上角用户头像 -> 菜单“修改资料”，然后选择“SSH公钥”，填写一个便于识别的标题，然后把用户主目录下的`.ssh/id_rsa.pub`文件的内容粘贴进去：

2. 如果我们已经有了一个本地的git仓库（例如，一个名为learngit的本地库），如何把它关联到Gitee的远程库上呢？

   首先，我们在Gitee上创建一个新的项目，选择右上角用户头像 -> 菜单“控制面板”，然后点击“创建项目”：

3. 项目名称最好与本地库保持一致：

   然后，我们在本地库上使用命令`git remote add`把它和Gitee的远程库关联：

   ```
   git remote add origin git@gitee.com:liaoxuefeng/learngit.git
   ```

4. 一个本地库能不能既关联GitHub，又关联Gitee呢？

   * 使用多个远程库时，我们要注意，git给远程库起的默认名称是`origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

   * 仍然以`learngit`本地库为例，我们先删除已关联的名为`origin`的远程库：

   * 然后，先关联GitHub的远程库：

     ```
     git remote add github git@github.com:michaelliao/learngit.git
     ```

     注意，远程库的名称叫`github`，不叫`origin`了。

     接着，再关联Gitee的远程库：

     ```
     git remote add gitee git@gitee.com:liaoxuefeng/learngit.git
     ```

     同样注意，远程库的名称叫`gitee`，不叫`origin`。

     现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

     ```
     git remote -v
     gitee	git@gitee.com:liaoxuefeng/learngit.git (fetch)
     gitee	git@gitee.com:liaoxuefeng/learngit.git (push)
     github	git@github.com:michaelliao/learngit.git (fetch)
     github	git@github.com:michaelliao/learngit.git (push)
     ```