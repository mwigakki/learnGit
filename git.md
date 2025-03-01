> 在vs中文件后面: 
>  A 表示 Added。
>  D 表示 Deleted
>  M 表示 Modified.
>  U 表示 Untracked。

> 就好比玩RPG游戏一样，每通过一关(完成一次的工作)都要保存一下游戏(add 和commit一下)。如果某一关没过去(某一次编辑出现了问题)，我们可以选择读取前一关的状态(回退上一个版本)。并且在你随时想暂停的时候都可以暂停(add,commit保存一个版本)。

 cmd 进入git怎么退出: 按q 回车

拉取github 仓库连接超时，直接在仓库前加上 ` https://ghproxy.com`。例如：`git clone https://github.com/Kalasearch/grafana-tutorial.git`

 查看分支合并情况： `git log --graph --pretty=oneline --abbrev-commit`

# 安装与常用操作

### 安装git

#### windows

官网下载后，一直下一步就行。只有把默认编辑器从vim改成VScode需要注意

#### linux （Ubuntu 20.04）

安装非常直接，仅仅以 sudo 权限用户身份运行下面的命令：

```text
sudo apt update
sudo apt install git
```

运行下面的命令，打印 Git 版本，验证安装过程：

```text
git --version
```

### 配置git
安装git完成后，需要在命令行设置：

> `git config --global user.name "Your Name"`
>
> `git config --global user.email "email@example.com"`

相当于自报家门。上面命令加了 `global`表示设置的是全局的user，如果只想设置本仓库的user就别加 `global`。

然后还要给github加上一个ssh key以便方便地使用github，见后面。

查看`git`的`user.name` 的命令：`git config user.name`

查看`git`的`user.email` 的命令：`git config user.email`

### 初始化

初始化一个Git仓库：
- 新建一个文件夹，在此文件夹中打开命令行
- 使用`git init`命令。
- 就可以在此目录中看到一个`.git`隐藏文件夹了，这就说明此目录已经是一个git仓库了

### 添加文件到Git仓库
分两步：
- 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
    - 使用`git add` 后的文件就进入`Staged Changes` 的状态了，修改后还没 add 的话就只是`Changes` 的状态
- 使用命令`git commit -m <message>`，完成。

### 查看当前仓库的状态
使用命令`git status` 查看当前仓库的状态，(是否有修改没有add或commit)

### 查看上次的修改
`git diff`查看工作区和暂存区差异，
`git diff --cached`查看暂存区和仓库差异，
`git diff HEAD `查看工作区和仓库的差异，

### commit 模板

在 Git 中，使用 commit 模板可以标准化提交信息，使团队成员遵循统一的提交格式。

**创建模板文件**：下面是一个模板文件示例

``` txt
feat/fix: 编写此次commit是加功能还是修bug

# 类型: (可选的范围): 提交描述

# 示例:
# fix: 修复了按钮点击无效的问题

# 详情:
# 本次提交修复了按钮点击后无响应的问题，通过添加必要的事件监听实现了功能。

# 关联 Issue:
# #1234
```

**配置 Git 使用模板**：接下来，你需要告诉 Git 使用这个模板文件。可以通过全局配置或单个仓库的配置来实现。

- **全局配置**：适用于所有本地 Git 仓库。

  ```bash
  git config --global commit.template D:\Git\.gitmessage[模板地址如 ~/.gitmessage]
  ```

**使用commit 模板**

执行 `git commit` 时，Git 会自动打开编辑器，并加载你配置的模板文件。你可以根据模板填写 commit 信息。

# 版本回退

### 查看提交日志
`git log`命令显示从最近到最远的提交日志，`git log 分支名` 查看具体分支提交日志
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：即 `git log --pretty=oneline`
该命令最主要是用来看版本号的，下面是一些示例：

```  bash
76c1fafea484761fc13ad96062e5baa5751863f8 (HEAD -> master) add log
f680593ab67427a0ecc9feb2e06296dd39d09bde add some analogy rhetoric
4e14ce03b86b0418967b0aff285e6005d8ad21d4 commmmmmit
13cdea478b95d596a5e431340453da6873d4b8d4 commmmmmit
6480669c9130f720bd633c11629acc7bbe7a812b delect .txt and add .md
23739862302591dbf1cd0652df14723b851318c8 second commit
d9677798cd9c97b0ba6a3ba6d695c3e06cb78c3a first time commit
```

### 回退


首先，Git必须知道当前版本是哪个版本，在Git中，**用HEAD表示当前版本，也表示当前位于的分支**，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上10个版本写10个^比较容易数不过来，所以写成HEAD~10。


使用命令：`git reset --hard HEAD^` 回退到上一个版本 ,(在cmd下要写两个^, 因为cmd中^是转义符号，相当于linux的\ )

也可使用命令：`git reset --hard <版本号>` , 版本号不必输全，输入前几个能够唯一分辨就行

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把HEAD从指向了指定的版本
``` bash
┌────┐
│HEAD│
└────┘
   │
   └──> ○ third commit
        │
        ○ second commit
        │
        ○ first commit
改为指向add distributed：

┌────┐
│HEAD│
└────┘
   │
   │    ○ third commit
   │    │
   └──> ○ second commit
        │
        ○ first commit
```

### 重返未来
如果版本回退之后后悔了想回到之前最新的版本：
重新使用`git reset --hard <版本号>`就行，只要知道想去的版本的版本号就行
但如果之前的命令行关掉了，不记得之前的版本号了，也没有关系
使用命令：`git reflog` 查看所有版本历史

## 工作区和暂存区
- **工作区（Working Directory）**: 就是你在电脑里能看到的目录
- **暂存区：**英文叫 **stage 或 index**。一般存放在 **.git** 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
- **版本库（Repository）**: 工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
![img](img/1352126739_7909.jpg)

当修改、删除或添加一些文件进去时(使用了`add或rm`)，**工作区里的更改会记录到暂存区**`stage`中

一旦提交(`commit`)后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：
```
git status
On branch master
nothing to commit, working tree clean
```
现在所有的更新内容都在版本库，暂存区就没有任何内容了：


# 管理修改
- Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。
### 丢弃工作区的修改
新版命令`git restore file` 更容易记忆，记住这个就好

命令`git checkout -- file`也可立马丢弃 **工作区** 的修改：
   - `git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。
### 丢弃暂存区的修改
我们**使用`git add .`后想要撤销此操作**，可以**使用`git reset`将刚存入暂存区的内容重新**放入工作区。

新版命令`git restore --staged <file>` 更容易理解，记住这个就好

命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：
`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

### git commit 后想取消本次提交

`git reset --soft HEAD~1` ： 这会将HEAD指针回退到上一次提交，但保留索引（暂存区）和工作目录中的更改。

### 删除和恢复文件
使用命令：`git rm <file>`，相当于是删除工作目录中的test.txt文件,并把此次删除操作提交到了暂存区

**误删文件后的恢复**
1. 手动将文件删除：此时相当与将工作区删除了文件，直接使用使用命令：`git restore <file>` 恢复
2. 使用命令：`git rm <file>`删除了文件，此时删除的操作已经被提交到暂存区staged了。此时需要先使用命令`git restore --staged <file>` 将暂存区的删除恢复到工作区，再使用命令：`git restore <file>` 恢复文件

# 远程仓库
查看远程仓库地址：

``` bash
git remote -v
```

自行注册GitHub账号。由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：

- 第1步：创建SSH Key。在用户主目录（windows的.ssh目录在`c盘:/user(或者是用户)/你的用户名/.ssh`下，看看有没有.ssh目录，如果有，再看看这个目录中有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash或CMD），创建SSH Key：
  
  ```bash
  ssh-keygen -t rsa -C "youremail@example.com"
  ```
  
  > 注意：运行此命令的用户需要与之后拉取远程仓库的用户一致
  
  你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，无需密码
  
  如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，**id_rsa是私钥**，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。
  
- 第一步（2）：在linux下：
  
  - 首先 `cd ~`
  - 查看是否已经存在SSH-Key【其实就是查看.ssh这个隐藏目录是否存在】` ls -al ~/.ssh`
  - 如果没有就新建，如果有，建议删除再新建，
  - 删除命令【其实就是删除.ssh这个隐藏目录目录】 `rm -rf .ssh`
  - 新生成SSH-key【替换成你自己的邮箱】`ssh-keygen -t rsa -C "xxx@xx.com"`
  - 键入命令后，会让你输入密码用来保护你的密钥等，总共三次需要输入的，你都直接三次回车就好！！
  - 生成后，会在 当前用户的目录下，生成一个.ssh隐藏目录，目录中会有【id_rsa】和【id_rsa.pub】两个文件，一个是私钥，一个是公钥。你现在就可以复制公钥使用了
  
- 第2步：登陆[GitHub](https://github.com/)，点击头像`setting`，“`SSH and GPG Keys`”页面：然后，点“`New SSH Key`”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容，最后add即可

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

### 配置多个公钥

1. 按照上面的步骤已经配置了一个ssh key了。我们用不同的邮箱注册了另一个github账号。假设账号名为 account2。邮箱为` account2@123.com`。然后我们打开Shell（Windows下打开Git Bash或CMD），给这个邮箱创建SSH Key：

```bash
ssh-keygen -t rsa -C "account2@123.com"

# 然后它会提示我们保存到哪个文件
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\a/.ssh/id_rsa):
# 这里我们为了防止把之前的ssh key文件重写了，需要键入新的保存的文件名，如 github2_id_rsa
```

然后提示我们输入密码啥的都不管，直接回车。看到如下显示就成功了。它就会生成`github2_id_rsa`和`github2_id_rsa.pub`两个文件。

``` bas
The key fingerprint is:
SHA256:gsgpgNOVQF8OPUGVATYnd0OiMNozvIMaTPGhiRgwXBU luotong@bjtu.edu.cn
The key's randomart image is:
+---[RSA 3072]----+
|*o++*EX+*++      |
|+==*oBo*.o .     |
|*+o.* o.         |
|+o + =           |
|o.= + . S        |
| +   . .         |
|.                |
|                 |
|                 |
+----[SHA256]-----+
```

2. 然后就想之前一样，使用account2账号登陆[GitHub](https://github.com/)，点击头像`setting`，“`SSH and GPG Keys`”页面：然后，点“`New SSH Key`”，填上任意Title，在Key文本框里粘贴`github2_id_rsa.pub`文件的内容，最后add即可。

3. 我们使用新建的账号account2 在github上新建一个仓库，然后把新仓库clone到本地。在本地此仓库中，我们使用`git config user.name`发现显示的是我们第一个账号，即此电脑git 的全局账号，但是我们想让此仓库使用我们刚刚新建的账号account2 。于是设置一下：

``` bash
git config user.name "{name}"
git config user.email "account2@123.com"
```

在本地仓库修改commit后，使用`git push`或`git push origin [远端主分支]`推送，提示让我们去网页输入账号密码。输入完成后即可推送成功。

### 添加远程仓库
登陆GitHub，然后，在右上角找到`Create a new repo`按钮，然后...，创建一个新的仓库。

目前，在GitHub上的这个learngit仓库还是空的，可以看看github给了很多的提示（包括从命令行创建一个新的仓库，或者从已有的仓库push），GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：
> git remote add origin git@github.com:mwigakki/learnGit.git

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。
下一步，就可以把本地库的所有内容推送到远程库上：

> git push -u origin master

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`**推送**到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

### 从远程拉取

- `git fetch` 只是从远程仓库下载最新的提交信息到本地仓库，但不会自动合并到当前分支。
- `git pull` 则是在 `fetch` 的基础上，还会自动将远程分支的最新提交合并到当前分支。

直接从远程仓库拉取并合并最新的代码到当前分支：`git pull origin <branch-name>`

### 推送到远程
从现在起，只要本地作了提交，就可以通过命令：

``` git
git push <远程仓库名> <要推送的分支>

例

git push origin master
```

把本地master分支的最新修改推送至GitHub。

如果单独使用`git push`时，表示将当前分支推送到远程的同名分支。当然这需要保证远程有这个分支，不然推送报错。

如果在本地新建了一个分支，需要推送到远程，可以使用如下命令。

``` shell
git push --set-upstream origin [分支名]
# 或
git push origin [分支名]:[分支名] # 这个命令如果远程仓库中不存在该分支，Git 将会自动创建该分支。
```

#### SSH警告
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：

``` bash
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
```
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。

Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
> Warning: Permanently added 'github.com' (RSA) to the list of known hosts.

这个警告只会出现一次，后面的操作就不会有任何警告了。
如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。

 ```crystal
 # github测试SSH密钥是否能够正常工作
 ssh -T git@github.com
 ```

### 删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm <name>命令。使用前，建议先用git remote -v查看远程库信息

``` bash
git remote -v

origin  git@github.com:michaelliao/learn-git.git (fetch)
origin  git@github.com:michaelliao/learn-git.git (push)
```
然后，根据名字删除，比如删除origin：
> git remote rm origin

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。

“删除”后再次使用

> git remote add origin git@github.com:mwigakki/learnGit.git

可以重新将本地和远程绑定。

### 从远程库克隆
在github项目右上角`CODE`里找到 SSH地址，然后在本地合适的位置打开CMD或git bash，使用命令：
> git clone <ssh地址>

即可将项目克隆下来了

# 分支
在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

下面是一个简单的分支使用示例：

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用HEAD指向`master`，就能确定当前分支，以及当前分支的提交点：

![master分支](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![master和dev分支1](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

Git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一次后，dev指针往前移动一步，而master指针不变：

![master和dev分支2](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

假如我们在dev上的工作完成了(commit)，就可以把dev合并到master上。Git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并：

![master和dev分支3](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

当我们把`dev`分支的工作成果合并到`master`分支上：
``` bash
$ git merge dev
Updating d46f35e..b17d20e
Fast-forward
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

`git merge`命令用于合并指定分支到当前分支。合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除dev分支。删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支：

![master和dev分支3](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)


**下面是具体命令：**

- 查看当前所有分支：`git branch` , 当前分支前面会标一个*号。
- 创建一个从HEAD处创建一个新分支：`git branch dev` （它表示从当前分支创建一个分支，并不是默认从master分支）
- 切换分支：`git switch dev`或`git checkout dev`
- 创建并切换分支：`git switch -c dev`或`git checkout -b dev`，切换分支后进行相应的修改和提交commit
- 将指定分支合并到**当前分支**： `git merge <分支名>`，此时HEAD需要位于主分支或合并后保留的分支。注意：**此时master 分支就会新增一个-m为 “merge ...”的提交，而被合并的分支不会有任何变化。**
- 删除分支：`git branch -d dev` , `-D`表示强制刪除

注：在dev分支上修改了文件，但是并没有执行git add. git commit命令，然后切换到master分支，仍然能看到dev分支的改动。只是因为所有的修改还只是在工作区，连暂存区都不是，git都还不知道有任何的修改。

### 需要解决冲突的分支合并
准备一个新的分支`feature1`
> git switch -c feature1

对文件进行修改，然后再`feature1`分支上提交。
切换到`master`分支

> git switch master

再对同一文件进行修改并提交。
现在，master分支和feature1分支各自都分别有新的提交，变成了这样：
![分支冲突](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：
``` bash
git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```
果然冲突了！Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。这时我们查看master 的分支情况也能发现，master并没有新增要给名叫 “merge ...”的分支，而我们查看出现冲突的文件，我们发现git将出现分支的内容全部放入当前分支的工作区了。git status也可以告诉我们冲突的文件

``` bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

我们可以直接查看readme.txt的内容：
``` bash
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```
git已经帮我们做了标记,用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，方便我们对冲突的地方查询修改，

接下来我们就在此基础上修改文件并重新`add`, `commit`，再次`merge`合并即可。

现在，`master`分支和`feature1`分支变成了下图所示：
![冲突分支合并](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

用带参数的`git log`也可以看到分支的合并情况：
``` bash
git log --graph --pretty=oneline --abbrev-commit
*   cf810e4 (HEAD -> master) conflict fixed
|\  
| * 14096d0 (feature1) AND simple
* | 5dc6824 & simple
|/  
* b17d20e branch test
* d46f35e (origin/master) remove test.txt
* b84166e add test.txt
```

最后，删除feature1分支

### 分支管理策略
通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在`merge`时生成一个新的`commit`，这样，从分支历史上就可以看出分支信息。
下面我们实战一下`--no-ff`方式的`git merge`：
首先，仍然创建并切换dev分支：

> git switch -c dev
> Switched to a new branch 'dev'

修改readme.txt文件，并提交一个新的commit：
``` bash
$ git add readme.txt 
$ git commit -m "add merge"
[dev f52c633] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回master：
``` bash
$ git switch master
Switched to branch 'master'
```

准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
``` bash
git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

``` bash
$ git log --graph --pretty=oneline --abbrev-commit
*   e1e9c68 (HEAD -> master) merge with no-ff
|\  
| * f52c633 (dev) add merge
|/  
*   cf810e4 conflict fixed
...

而使用fast forward方式的就是这样的
* 1496baa (HEAD -> master, dev) ff commit in dev
*   2328ae0 merge with no-ff
```
可以看到，不使用Fast forward模式，merge后就像这样：
![不使用Fast forward模式merge](https://www.liaoxuefeng.com/files/attachments/919023225142304/0)

**分支策略**
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

所以，团队合作的分支看起来就像这样：
![合作开发](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

### bug分支
软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：

``` bash
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
``` bash
git stash
Saved working directory and index state WIP on dev: f52c633 add merge

# git stash save "在这里可以写一些自己看的信息"
```
<hr/>
[此句写于issue-101分支，作为bug分支的练习] : 现在修复了一个bug
<hr/>

现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
``` bash
git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，...，然后提交：
``` bash
git add readme.txt 
git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到master分支，并完成合并，最后删除issue-101分支：
``` bash
git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到dev分支！
使用`git status`工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：
``` bash
git stash list
stash@{0}: WIP on dev: f52c633 add merge
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用**`git stash pop`**，恢复的同时把stash内容也删了：

再用git stash list查看，就看不到任何stash内容了：

> 

```shell
git stash list

# 你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：

git stash apply stash@{0}

# 如果有多次暂存的修改，通过 **git stash apply stash@{1}** 应用编号为1  的修改，**git stash pop stash@{1} ** 也可以
```

默认情况下，**git stash 命令只暂存 Git 已追踪的文件更改，不会暂存未追踪的文件和 .gitignore 文件中忽略的文件**

解决办法：**使用git add 文件名 对新建文件进行追踪，再进行git stash 操作即可**。如**：git add club.vue 再git stash**



在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

### 复制单笔提交到分支

同样的bug，要在dev上修复，我们只需要把4c805e2 fix bug 101这个提交所做的修改“复制”到dev分支。注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作**，Git专门提供了一个`cherry-pick`命令**，**让我们能复制一个特定的提交到当前分支**：
``` bash
git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

`git cherry-pick`可以理解为”挑拣”提交，它会获取某一个分支的单笔提交，并作为一个新的提交引入到你当前分支上。当我们需要在本地合入其他分支的提交时，如果我们不想对整个分支进行合并，而是只想将某一次提交合入到本地当前分支上，那么就要使用`git cherry-pick`了。

Git自动给dev分支做了一次提交，注意这次提交的commit是1d4b803，它并不同于master的4c805e2，因为这两个commit只是改动相同，但确实是两个不同的commit。用git cherry-pick，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要git stash命令保存现场，才能从dev分支切换到master分支。

`cherry-pick`命令当然也可能冲突，自己解决即可。


### Feature分支
软件开发中，总有无穷无尽的新的功能要不断添加进来。

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

现在，你终于接到了一个新任务：开发代号为Vulcan的新功能，该功能计划用于下一代星际飞船。

于是准备开发:

> git switch -c feature-vulcan
> Switched to a new branch 'feature-vulcan'

5分钟后，开发完毕：使用`add`， `commit`将开发提交到feature分支

切回`dev`，`git switch dev`准备合并

一切顺利的话，feature分支和bug分支是类似的，合并，然后删除。

但是！

就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个包含机密资料的分支还是必须就地销毁：

``` bash
git branch -d feature-vulcan
error: The branch 'feature-vulcan' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
```

销毁失败。Git友情提醒，`feature-vulcan`分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用大写的`-D`参数。

现在我们强行删除：
``` bash
$ git branch -D feature-vulcan
Deleted branch feature-vulcan (was 287773e).
```

终于删除成功！

总结：

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

### 拉取分支
多人协作时，大家都会往master和dev分支上推送各自的修改。

当你的小伙伴（注意要把SSH Key添加到GitHub）从远程库clone时，默认情况下，你的小伙伴只能看到本地的master分支。

现在，你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

``` git
git switch -c dev origin/dev
```

现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：

... 

现在，你的小伙伴已经向origin/dev分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：
``` bash
git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：

``` bash
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```
`git pull`也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：

``` bash
git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

``` bash
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push：

因此，多人协作的工作模式通常是这样：
1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

**如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，**用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

### 常用操作

查看所有分支：`git branch`  git switch -c ad

查看当前所在分支：`git branch --show-current`

修改当前分支名：`git branch -m <new-branch-name>`

删除远程仓库中的旧分支（如果需要）：`git push origin --delete <old-branch-name>`

**推送**当前本地分支到对应的远程分支：`git push`

**推送**新的本地分支到远程：`git push origin <new-branch-name>`

列出所有本地分支及其对应的远程跟踪分支：`git branch -vv`

**合并** br1 的内容到br2（此时位于br2分支）：`git merge br1`

切换当前分支对应的远端分支：`git branch --set-upstream-to=origin/remote-branch local-branch`

**拉取**远端分支最新代码到本地（所有分支信息）（不合并）：`git fetch origin `

**拉取**远程分支并**合并到本地当前分支**：`git pull origin <remote-branch>`

**拉取**远程其他分支：

- 创建一个本地分支，该分支跟踪远程分支 `origin/feature-branch`：`git branch feature-branch origin/feature-branch`  
- 检出刚刚创建的本地分支：`git checkout feature-branch`
- 或者，可以一次性创建并检出远程分支的本地副本：`git checkout -b feature-branch origin/feature-branch`

### rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# 标签管理
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个**快照**。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

在Git中打标签非常简单，首先，切换到需要打标签的分支上：

然后，敲命令`git tag <name>`就可以打一个新标签：

可以用命令`git tag`查看所有标签：

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了：
``` bash
git log --pretty=oneline --abbrev-commit
2dff384 (HEAD -> dev, tag: v1.0, origin/dev) fix confilct 6.21 15.27
c6d0545 cmt 6.21 15.25
51e6b23 cmt 6.21 15.19
3ecce4b feature 6.21 14.45
0b9e615 cmt dev in dorm 6.20 22.43
b253c9a cmt in dorm 6.20 22.38
...
```

比方说要对`cmt 6.21 15.19`这次提交打标签，它对应的commit id是`51e6b23`，敲入命令：
> `git tag v0.9 51e6b23`

再用命令git tag查看标签：
``` bash
$ git tag
v0.9
v1.0
```

注意，标签不是按时间顺序列出，而是按字母排序的。可以用`git show <tagname>`查看标签信息。
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
例： ` git tag -a v0.1 -m "version 0.1 released" 1094adb`

 - 注意：**标签总是和某个commit挂钩**。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。

删除标签：
`git tag -d v0.1`

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令

`git push origin <tagname>`：

或者，一次性推送全部尚未推送到远程的本地标签：

`git push origin --tags`

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
然后，从远程删除。删除命令也是push，但是格式如下：

`git push origin :refs/tags/v0.9`

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。

#### 切换标签

`git checkout -b <branchName> <tagName>`

因为 tag 本身指向的就是一个 commit，所以和根据commit id 检出分支是一个道理。

但是需要特别说明的是，如果我们想要修改 tag检出代码分支，那么虽然分支中的代码改变了，但是 tag标记的 commit还是同一个，标记的代码是不会变的，这个要格外的注意。



# 自定义Git

### 忽略特殊文件

- **问题**：有些时候，你必须把某些文件放到Git工作目录中，但又不能提交它们，比如资源文件、日志文件、保存了数据库密码的配置文件等等。

- **解决方法**：在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。不需要从头写`.gitignore`文件，GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

举个例子：

假设你在Windows下进行Python开发，Windows会自动在有图片的目录下生成隐藏的缩略图文件，如果有自定义目录，目录下就会有`Desktop.ini`文件，因此你需要忽略Windows自动生成的垃圾文件：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini
```

然后，继续忽略Python编译产生的`.pyc`、`.pyo`、`dist`等文件或目录：

```
# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build
```

加上你自己定义的文件，最终得到一个完整的`.gitignore`文件，内容如下：

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa

# 忽略gitignore自身
.gitignore

# 忽略qwer文件夹
qwer/

# 忽略resources下的img文件夹里的所有文件
resources/img/
```

最后一步就是把`.gitignore`也提交到Git，就完成了！当然检验`.gitignore`的标准是`git status`命令是不是说`working directory clean`。

有些时候，你想添加一个文件到Git，但发现添加不了，原因是这个文件被`.gitignore`忽略了：

```
$ git add App.class
The following paths are ignored by one of your .gitignore files:
App.class
Use -f if you really want to add them.
```

如果你确实想添加该文件，可以用`-f`强制添加到Git：

```
$ git add -f App.class
```

或者你发现，可能是`.gitignore`写得有问题，需要找出来到底哪个规则写错了，可以用`git check-ignore`命令检查：

```
$ git check-ignore -v App.class
.gitignore:3:*.class	App.class
```

Git会告诉我们，`.gitignore`的第3行规则忽略了该文件，于是我们就可以知道应该修订哪个规则。

还有些时候，当我们编写了规则排除了部分文件时：

```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class
```

但是我们发现`.*`这个规则把`.gitignore`也排除了，并且`App.class`需要被添加到版本库，但是被`*.class`规则排除了。

虽然可以用`git add -f`强制添加进去，但有强迫症的童鞋还是希望不要破坏`.gitignore`规则，这个时候，可以添加两条例外规则：

```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class

# 不排除.gitignore和App.class:
!.gitignore
!App.class
```

把指定文件排除在`.gitignore`规则外的写法就是`!`+文件名，所以，只需把例外文件添加进去即可。

**对于已经提交的文件(夹)我们还需要忽略它们的话：** 

1. `git rm --cached xxxx`

2. 如果是文件夹需要使用 `git rm -r --cached xxxx`
3. 如果提示某个文件无法忽略，可以添加`-f`参数强制忽略 `git rm -f --cached xx/xx.xx`
4. 将xxx文件加入到 `.gitignore` 文件中, 忽略掉目标文件
5. `git commi -m "你的提交内容"`

`.gitignore`文件的书写规则

``` txt
一些规则
x.y			# 忽略具体x.y
a # *.a		# 忽略所有 .a 结尾的文件
!lib.a		# 但lib.a 除外
/TODO		# 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/		# 忽略build/ 目录下的所有文件
doc/.txt	# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt 
```

### 搭建Git服务器

Github总是连不上？gitee私有仓库有人数限制？那就只能自己搭建一台Git服务器作为私有仓库使用！

远程仓库实际上和本地仓库没啥不同，纯粹为了7x24小时开机并交换大家的修改。

搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的`apt`命令就可以完成安装。

[搭建Git服务器 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)

假设你已经有`sudo`权限的用户账号，下面，正式开始安装。

#### 1、安装`git`：

```bash
sudo apt install git
```

#### 2、创建用户

创建一个`git`用户组和用户，用来运行`git`服务：

```bash
sudo groupadd git
sudo useradd git -g git
# passwd git	# 给git设置密码
```

更多：[Ubuntu 创建、管理用户、组和git库、项目_bon_ami的博客-CSDN博客_ubuntu创建bendi新用户](https://blog.csdn.net/bon_ami/article/details/45538777)

#### 3、创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的`id_rsa.pub`文件，把所有公钥导入到`/home/git/.ssh/authorized_keys`文件里，如果没有该文件创建它，一行一个。

```bash
touch authorized_keys
chmod 644 authorized_keys 
```

`id_rsa.pub`文件位置如下：

> id_rsa为私钥，id_rsa.pub为公钥，没有的话就初始化一个
>
> - windows：C:\Users\ \[当前用户] \\.ssh
>
> - Linux：/root/.ssh

#### 4、初始化Git仓库

首先我们选定一个目录作为Git仓库，假定是`/home/gitrepo/mwigakki.git`，在`/home/gitrepo`目录下输入命令:

```bash
cd /home
mkdir gitrepo
chown git:git gitrepo/
cd gitrepo
```

接着使用命令初始化一个空仓库。服务器上的Git仓库通常都以`.git`结尾。

``` bash
sudo git init --bare mwigakki.git

Initialized empty Git repository in /home/gitrepo/mwigakki.git/
```

然后，把仓库所属用户改为git：

```bash
sudo chown -R git:git mwigakki.git
```

#### 5、禁用git的shell登录

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑`/etc/passwd`文件完成。找到类似下面的一行：

```
git:x:1001:1001:,,,:/home/git:/bin/bash
```

改为：

```
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
```

这样，`git`用户可以正常通过ssh使用git，但无法登录shell，因为我们为`git`用户指定的`git-shell`每次一登录就自动退出。

#### 6、克隆仓库

最后在自己的电脑运行：

```bash
$ git clone git@[服务器IP]:/home/gitrepo/mwigakki.git
Cloning into 'mwigakki'...
warning: You appear to have cloned an empty repository.
Checking connectivity... done.
```

这样我们的 Git 服务器安装就完成。

接下来的工作就是推送拉取之类的，和之前的一样，只是可能要输密码。

#### 7、推送到远端

对于本地已有的仓库准备推送到自己的git服务器，先把本机的`id_rsa.pub`中的key文本添加到github (或自己的远程地址)的sshkey中，然后在服务器上建一个相应的仓库（不要直接mkdir，需要像上面第4步初始化一个仓库），再添加远程仓库地址即可。（远程仓库和本地仓库名字可以不一样）

可能需要先将github的地址删掉再加上自己的，命令如下：

``` bash
git remote rm origin
git remote add origin git@[自己的ip]:/home/gitrepo/mwigakki.git
```

之后第一次推送需要

``` bash
git push -u origin master
# 或
git push --set-upstream origin master
```

之后的修改直接 `git push` 就行了。

 

# 问题汇总

### 统计代码行数

``` shell
git log --author="Author Name" --since="YYYY-MM-DD" --until="YYYY-MM-DD" --pretty=tformat: --numstat | awk '{added+=$1; removed+=$2} END {print "Added lines:", added, "Deleted lines:", removed, "Net change:", added-removed}'

```

1. 当使用`git pull` 或者 `git push` 时提示：`There is no tracking information for the current branch.
    Please specify which branch you want to merge with.`，说明当前`pull`对象没有远程分支的跟踪信息，即本地分支和远程分支没有关联起来。我们查看提交情况`git log --pretty=oneline`发现：

![image-20230908162258120](img/image-20230908162258120.png)zh

只有本地master分支的情况没有远程master分支。我们使用命令：

``` bash
git branch --set-upstream-to=origin/master master
# 这里第一个master 是远程分支名，第二个master是本地分支名。如果不是这个名字需要相应修改。
```

此时我们先拉去远程分支`git pull`，再查看提交情况`git log --pretty=oneline`发现：

![image-20230908162653941](img/image-20230908162653941.png)

已经有远程分支的信息了，而且落后本地两个版本。此时就可以正常使用`git push`推送到远程。

### 使用`git pull` 或者 `git push` 时提示

``` bash
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

这时就打开本机的config文件，windows下在 `C:\Users\[自己用户]\.ssh`，linux下在`~/.ssh/`。在config文件最后添加 

``` txt
Host github.com
 Hostname ssh.github.com
 Port 443
```

Then, run the command `ssh -T git@github.com` to confirm if the issue is fixed.

3. 推送到远端后提示：remote: Permission to mwigakki/myZinx.git denied to 【不是你的github账号】.

可能是由于 Git 还记住了你之前的账号的授权。可以试着清理 Git 的授权缓存，

### **git 对换行符的处理**

使用 Git 进行版本管理时，可能会遇到换行符不一致的问题。这个问题是由于不同的操作系统使用不同的换行符导致的。例如，Windows 系统使用 CRLF（回车换行）作为换行符，而 Linux 和 MacOS 系统使用 LF（换行）作为换行符。

这种差异可能会给跨平台协作开发和运行带来一些困扰，比如 `git diff` 中显示整个文件都被修改了，或者合并分支时出现冲突等。为了解决这个问题，我们需要了解 Git 是如何处理换行符的，并且如何配置 Git 来适应不同的场景。

Git 有一个全局配置项叫做 `core.autocrlf`，它可以控制 Git 在提交和检出时是否对换行符进行转换。它有三个可选值：

- `true`：表示在提交时将 CRLF 转换为 LF，在检出时将 LF 转换为 CRLF 。这个选项适合 Windows 用户使用。

``` sh
git config --global core.autocrlf true
```

- `input`：表示在提交时将 CRLF 转换为 LF，在检出时不进行转换。这个选项适合 Linux 和 MacOS 用户使用。
- `false`：表示不进行任何转换。这个选项适合想保持原始换行符不变的用户使用。
