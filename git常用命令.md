| git常用命令 |                                                              |
| ----------- | ------------------------------------------------------------ |
|             |                                                              |
|             |                                                              |
|             | 一. 基本配置与常用版本库操作命令                             |
|             |                                                              |
|             | 安装git后的基本配置：                                        |
|             | git config --global user.name "Your Name"                    |
|             | git config --global user.email "email@example.com"           |
|             |                                                              |
|             | mkdir learngit 创建测试目录                                  |
|             | cd learngit 切换到新创建的learngit目录                       |
|             | pwd 显示当前目录(如: /c/Documents and Settings/Administrator/learngit) |
|             |                                                              |
|             | 把learngit目录变成Git可管理的仓库(即初始化一个git仓库)：     |
|             | git init                                                     |
|             | (显示如：Initialized empty Git repository in /Users/michael/learngit/.git/) |
|             | (git init命令会在项目文件夹中生成一个名为".git"的隐藏目录，它就是git版本库，它将记录仓库中所有文件的修改历史。切勿修改其中的文件以免破坏仓库) |
|             |                                                              |
|             | 切换到项目目录并初始化该项目的git版本库：                    |
|             | cd d:/xampp/htdocs/project                                   |
|             | git init                                                     |
|             | (在实际工作中，你的项目文件夹可能存在于电脑上的任意位置，如 D:/xampp/htdocs/project，我们需要先切换到项目文件夹才能初始化它的版本库) |
|             |                                                              |
|             | 把文件添加到版本库需要两步：                                 |
|             | 第一步是：                                                   |
|             | git add readme.txt                                           |
|             | (readme.txt是手工在learngit目录中新建的一个用于测试的文件，可随意输入几行内容) |
|             |                                                              |
|             | 第二步是：                                                   |
|             | git commit -m "wrote a readme file" (-m "***"是提交时的备注说明) |
|             | 显示如：                                                     |
|             | [master (root-commit) cb926e7] wrote a readme file           |
|             | 1 file changed, 2 insertions(+)                              |
|             | create mode 100644 readme.txt                                |
|             | (git commit命令执行成功后会提示，1个文件被改动(新添加的readme.txt文件),插入了两行内容(readme.txt有两行内容)) |
|             |                                                              |
|             | 可一次提交多个文件到版本库：                                 |
|             | git add file1.txt                                            |
|             | git add file2.txt                                            |
|             | git add file3.txt                                            |
|             | git commit -m "add 3 files."                                 |
|             |                                                              |
|             | 添加目录中的所有文件到版本库：                               |
|             | git add .                                                    |
|             |                                                              |
|             | 查看版本库当前状态：                                         |
|             | git status                                                   |
|             |                                                              |
|             | 修改文件后查看修改了哪些地方：                               |
|             | git diff readme.txt                                          |
|             |                                                              |
|             | 提交修改也分两步，第一步也是：                               |
|             | git add readme.txt                                           |
|             |                                                              |
|             | 第二步是：                                                   |
|             | git commit -m "提交说明"                                     |
|             |                                                              |
|             | 查看提交历史(按字母"q"键退出查看log状态)：                   |
|             | git log                                                      |
|             |                                                              |
|             | 以简洁方式查看提交历史(仅显示一行信息而隐藏提交用户和提交时间)： |
|             | git log --pretty=oneline                                     |
|             | (显示如：ea2dd571ef424ddf4aed83be08f1af019c694c78 删除结尾空行) |
|             |                                                              |
|             | 以简洁方式查看提交历史(仅显示一行信息，且commit id仅显示7位): |
|             | git log --pretty=oneline --abbrev-commit                     |
|             | (显示如：ea2dd57 删除结尾空行)                               |
|             |                                                              |
|             | 版本回退(HEAD代表当前版本)：                                 |
|             | git reset --hard HEAD^  回退到上一个版本                     |
|             | git reset --hard HEAD^^  回退到上上个版本                    |
|             | git reset --hard HEAD~100  回退到上100个版本                 |
|             | git reset --hard commit_id(即commit id)                      |
|             |                                                              |
|             | *回退前用 git log 命令查看提交历史以确定要回退到哪个版本(版本号/即commit id) |
|             | *要回到未来某个版本时，先用 git reflog 命令查看命令历史以确定要回到未来哪个版本 |
|             |                                                              |
|             | 在git add后取消add操作(即从暂存区中删除已add到其中的修改)：  |
|             | git reset HEAD readme.txt                                    |
|             |                                                              |
|             | 在工作区修改完文件后取消此修改：                             |
|             | git checkout -- readme.txt                                   |
|             | (场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file |
|             | 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步走，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。 |
|             | 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，用版本回退命令，不过前提是没有推送到远程库) |
|             |                                                              |
|             | 查看工作区和版本库的区别：                                   |
|             | git diff HEAD -- readme.txt                                  |
|             |                                                              |
|             | 查看文件的内容：                                             |
|             | cat readme.txt                                               |
|             |                                                              |
|             | 从工作区删除文件：                                           |
|             | rm readme.txt                                                |
|             |                                                              |
|             | 如果从工作区误删了某个文件(手工在文件管理器中删除或使用 rm file 命令删除)，想恢复该文件： |
|             | git checkout -- file                                         |
|             |                                                              |
|             | 如果真想删除某个文件：                                       |
|             | git rm file                                                  |
|             | git commit -m "remove file"                                  |
|             |                                                              |
|             |                                                              |
|             |                                                              |
|             | 二. git分支管理：                                            |
|             | 创建分支的作用是从主开发流程中分离出来，例如你可以创建一个自己的分支，在自己的分支上开发，当开发完成后再将自己的分支合并到主分支上，这样就可以在工作时不影响其它团队成员(更多有关分支的内容可搜索"git分支管理策略")。 |
|             |                                                              |
|             | 创建并切换到一个新分支：                                     |
|             | git checkout -b dev                                          |
|             | (dev 是新分支的名称)                                         |
|             |                                                              |
|             | 查看版本库中分支的情况(有多少个分支/当前分支前有一个星号)：  |
|             | git branch                                                   |
|             |                                                              |
|             | 在不同分支之间切换，如：                                     |
|             | git checkout master                                          |
|             | git checkout dev                                             |
|             |                                                              |
|             | 把dev分支合并到master分支上：                                |
|             | git checkout master  先切换到master分支                      |
|             | git merge dev   合并dev分支到master分支上                    |
|             |                                                              |
|             | 默认情况下git使用fast forward模式合并分支，如想使用非fast forward模式合并分支： |
|             | git merge --no-ff -m "提交说明" dev                          |
|             | (使用--no-ff模式合并分支可保留分支历史/提交说明必须填写/使用默认的fast forward模式合并分支会丢掉分支历史) |
|             | (可用 git log --graph 命令查看两种合并模式的区别)            |
|             |                                                              |
|             | 删除分支：                                                   |
|             | git branch -d dev                                            |
|             |                                                              |
|             | 强行删除分支：                                               |
|             | git branch -D dev                                            |
|             | (当想删除某个未被合并的分支时git会提醒你该分支还未被合并，如仍要删除，使用此强行删除命令) |
|             |                                                              |
|             |                                                              |
|             | 合并分支时解决冲突：                                         |
|             | 合并分支时，如git提示"conflict"，就表示发生了冲突，即不同分支中的文件内容在某些相同的行中内容不一致，这时就必须先解决冲突。解决冲突后再提交(add和commit)，合并完成。 |
|             |                                                              |
|             | 1. 冲突的标记：                                              |
|             | *Git用　<<<<<<<　=======　>>>>>>>　标记出不同分支中冲突的内容 |
|             |                                                              |
|             | 2. 解决冲突并提交后，可用 git log --graph 命令查看分支合并图 |
|             |                                                              |
|             |                                                              |
|             | 保存工作进度：                                               |
|             | 当正在某个分支中工作时，如果工作到一个杂乱的状态(即此时不适合提交)，这时突然想切换到另一个分支中工作(如创建一个新的分支用于修复bug)，为了不丢失当前的工作进度(不提交的话再返回到这个分支时就会恢复到版本库中已提交的最后一个版本，也就丢失了当前的工作进度)，可使用命令： |
|             | git stash                                                    |
|             |                                                              |
|             | 当由其它分支中返回当前分支后，再恢复保存的工作进度：         |
|             | git stash pop                                                |
|             | (用 git stash apply 命令也会恢复保存的工作进度，但还要使用 git stash drop 来删除进度。而使用 git stash pop 会在恢复工作进度的同时删除保存的进度) |
|             |                                                              |
|             | 可多次保存工作进度，使用如下命令可查看已保存的进度列表：     |
|             | git stash list                                               |
|             |                                                              |
|             | 也可恢复到指定的工作进度，如恢复到stash@{1}：                |
|             | git stash apply stash@{1}                                    |
|             |                                                              |
|             | 删除所有存储的工作进度：                                     |
|             | git stash clear                                              |
|             |                                                              |
|             |                                                              |
|             |                                                              |
|             | 三. git标签管理：                                            |
|             | 可以为版本库中的某个版本定义一个标签，标签其实就是指向一个commit的指针，当我们想发布程序的一个稳定版本时通常会定义一个标签，如"v1.0"(即1.0版)。 |
|             |                                                              |
|             | 首先切换到想打标签的分支上，创建标签：                       |
|             | git checkout dev                                             |
|             | git tag v1.0                                                 |
|             | ("v1.0"只是标签名称的示例，就像本教程中的其它示例一样(比如在分支管理命令中的"dev"也是一个分支名称的示例)) |
|             |                                                              |
|             | 默认情况下标签是打在最新的commit上，如果想为某个以前的提交打标签： |
|             | 第1步先使用git log命令显示提交历史：                         |
|             | git log --pretty=oneline --abbrev-commit                     |
|             |                                                              |
|             | 第2步再为某个commit id打标签：                               |
|             | git tag v1.0 9ecfa7d                                         |
|             |                                                              |
|             | 查看标签库中的所有标签：                                     |
|             | git tag                                                      |
|             |                                                              |
|             | 查看某个标签的详细信息：                                     |
|             | git show v1.0                                                |
|             |                                                              |
|             | 创建带说明的标签(用-a指定标签名，-m指定说明文字)：           |
|             | git tag -a v0.1 -m "version 0.1 released" 3628164            |
|             |                                                              |
|             | 删除标签：                                                   |
|             | git tag -d v1.0                                              |
|             |                                                              |
|             | 推送某个标签到远程库：                                       |
|             | git push origin v1.0                                         |
|             |                                                              |
|             | 推送全部标签到远程库：                                       |
|             | git push origin --tags                                       |
|             |                                                              |
|             | 如果标签已推送到远程，想删除它就需要两步：                   |
|             | 第1步先在本地删除：git tag -d v1.0                           |
|             | 第2步再用命令：git push origin :refs/tags/v1.0               |
|             |                                                              |
|             |                                                              |
|             |                                                              |
|             | 四. 自定义git：                                              |
|             | git有许多可配置项(如开始时我们学的邮箱和用户名配置)，通过自定义配置可让我们的工作更高效。 |
|             |                                                              |
|             | 1. 颜色配置(让命令看上去更醒目)：                            |
|             | git config --global color.ui true                            |
|             |                                                              |
|             |                                                              |
|             | 2. 忽略版本库中的特定文件(或文件夹)：                        |
|             | 有时需要忽略版本库中的某类文件，如保存了数据库密码的配置文件，系统自动生成的垃圾文件等。可以通过在版本库中添加一个叫".gitignore"的文件来实现，添加之后，需要把它也提交到版本库中。git为我们准备了一些现成的gitignore文件，地址是：https://github.com/github/gitignore |
|             |                                                              |
|             | .gitignore文件内容示例(#号后面是注释)：                      |
|             | /upload/* # 忽略根目录下的 upload 目录的全部内容             |
|             | *.a       # 忽略所有 .a 结尾的文件                           |
|             | !lib.a    # 但 lib.a 除外                                    |
|             | /TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO |
|             | build/    # 忽略 build/ 目录下的所有文件                     |
|             | doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt |
|             |                                                              |
|             | 如gitignore无法生效，原因是它只能忽略原来没有被跟踪的文件，所以如果某些文件已经纳入了版本管理中，则修改.gitignore是无效的。解决方法是先把本地缓存删除(改为未track状态)，然后再提交： |
|             | git rm -r --cached .                                         |
|             | git add .                                                    |
|             | git commit -m 'update .gitignore'                            |
|             |                                                              |
|             | *对于某些需要忽略但又不能删除的文件或目录，如某些缓存文件，可先备份它们，在添加.gitignore文件后再把它们手工复制到项目中。 |
|             |                                                              |
|             |                                                              |
|             | 3. 配置git命令的别名：                                       |
|             | 通过为常用命令配置别名可简化操作，如为"以简化形式显示提交历史"的命令定义一个别名： |
|             | git config --global alias.lg 'log --pretty=oneline --abbrev-commit' |
|             | (这样以后只需输入 git lg 就能以简化形式显示提交历史了)       |
|             |                                                              |
|             |                                                              |
|             |                                                              |
|             |                                                              |
|             | --------以下为 github 操作(在继续阅读之前请注册GitHub账号/网址是github.com)--------- |
|             |                                                              |
|             | 一. 创建并添加SSH Key。因为本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要做如下设置—— |
|             |                                                              |
|             | 第1步 在本地创建SSH Key：                                    |
|             | 在本地用户主目录下(如C:\Documents and Settings\Administrator)，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了可直接 跳到下一步。如果没有，打开Git Bash，创建SSH Key： |
|             |                                                              |
|             | $ ssh-keygen -t rsa -C "lsokweb@gmail.com"                   |
|             |                                                              |
|             | (邮件地址换成自己的，然后一路回车，使用默认值即可，无需设置密码。如果顺利可以在用户主目录下找到.ssh目录，其中有id_rsa和id_rsa.pub两个文件，这两个文件就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，后面会把它添加到github中。 |
|             |                                                              |
|             |                                                              |
|             | 第2步 将公钥复制到github：                                   |
|             | 登陆GitHub帐户，“Settings”—“SSH Keys”，点击“New SSH Key”，填上任意Title(如“家里”)，在文本框中粘贴本地id_rsa.pub文件的内容。 |
|             |                                                              |
|             | (GitHub允许你添加多个Key，假定你有多台电脑，有时在公司电脑上提交，有时在家里电脑上提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub提交了。公钥和私钥的工作原理是，公钥和私钥是一个“密钥对”，提交时，你电脑里的私钥会和github上的公钥配对，如果匹配成功则允许提交，否则就无法提交，目的就是保证只有合法用户才能提交) |
|             |                                                              |
|             |                                                              |
|             | 二. 添加远程库并把本地仓库推送到远程库                       |
|             | 远程库有两个作用，一是可以备份本地仓库(也就是备份整个项目)，另一个是与其它人协作共同开发。将本地仓库(如learngit)添加到github远程仓库需要两步，第一步是在github上创建一个新仓库(命名为learngit)并把本地的learngit仓库与之关联，第二步是将本地仓库的数据推送到远程仓库： |
|             |                                                              |
|             | 第1步：在github创建新仓库并将本地仓库与之关联：              |
|             | 登录github帐户，"Create New repository"，"Repository name"填写"learngit"，"Description"可填写"git学习"，其它选项保持默认，点击"Create repository"按钮。 |
|             |                                                              |
|             | 打开git bash并切换到learngit目录，将本地learngit仓库与刚创建的github仓库相关联： |
|             | git remote add origin https://github.com/你的github帐号/learngit.git |
|             |                                                              |
|             |                                                              |
|             | 第2步：将本地learngit仓库推送到远程仓库：                    |
|             | git push -u origin master                                    |
|             | (注意：因为远程库是空的，第一次推送时必须加 -u 参数。推送完成后可看到github远程库中的内容已和本地仓库一致) |
|             |                                                              |
|             | 以后每次需要推送时，使用命令：                               |
|             | git push origin master                                       |
|             | (注意：master是要推送的分支名称)                             |
|             |                                                              |
|             |                                                              |
|             | *删除远程库(先cd到本地仓库再执行此命令可删除对应的远程库)：  |
|             | git remote rm origin                                         |
|             |                                                              |
|             |                                                              |
|             | 三. 从远程库克隆                                             |
|             | 如果新开始一个项目，建议先创建远程库，然后每个项目成员都从远程库克隆一份到自己的电脑。 |
|             |                                                              |
|             | 第1步：在github创建一个新仓库(这里命名为"gitskills")         |
|             |                                                              |
|             | 第2步：执行克隆命令:                                         |
|             | git clone https://github.com/你的github帐号/gitskills.git    |
|             | (克隆的仓库会克隆到哪里呢？如果在执行git clone命令时不指定其它参数，克隆的仓库会克隆到用户主目录中，如 C:/Documents and Settings/Administrator) |
|             |                                                              |
|             | *如需指定克隆目录(即克隆到哪个目录)，命令格式为：            |
|             | git clone 远程库地址 本地目录                                |
|             | 实际命令类似下面：                                           |
|             | git clone https://github.com/你的gthub帐号/gitskills.git d:/xampp/htdocs/gitskills |
|             | (注意，此时d:/xampp/htdocs里并没有gitskills这个目录，执行克隆命令后会自动创建它) |
|             |                                                              |
|             | *当从远程库克隆时，克隆到本地的仓库只有master分支，可使用 git checkout -b dev origin/dev 将远程分支克隆到本地。 |
|             |                                                              |
|             |                                                              |
|             | 四. 将本地分支推送到远程库(的相应分支)                       |
|             | 哪些分支应推送到远程？哪些分支不需要推送呢？原则如下——       |
|             | 1. master分支是主分支，因此要时刻与远程同步；                |
|             | 2. dev分支是开发分支，所有团队成员都需要在上面工作，也需要与远程同步； |
|             | 3. bug分支只用于在本地修复bug，不必推送到远程；              |
|             | 4. feature分支是否推送到远程取决于你是否需要和其它成员合作开发此功能。 |
|             | 总之，本地分支是否推送到远程完全视情况而定。                 |
|             |                                                              |
|             | 查看远程库信息：                                             |
|             | git remote -v                                                |
|             |                                                              |
|             | 将本地分支推送到远程库，如：                                 |
|             | git push origin master                                       |
|             | git push origin dev                                          |
|             | (注意：git push origin 后面要加上本地分支的名称)             |
|             |                                                              |
|             |                                                              |
|             | 五. 抓取远程分支(pull)：                                     |
|             | 多人协作的工作场景通常是这样(以在dev分支上开发为例)——        |
|             | 1. 首先，用 git push origin dev 推送自己的修改；             |
|             | 2. 如果推送失败，是因为远程分支比你的本地分支内容更新(即有人在你之前向这个分支推送了他的修改)，所以你需要先用 git pull 将远程分支合并到自己的本地分支上； |
|             | 3. 如合并后有冲突，则先解决冲突并在本地提交(add和commit)；   |
|             | 4. 如没有冲突或已解决了冲突，再用 git push origin dev 推送到远程分支。 |
|             |                                                              |
|             | *如果 git pull 时提示"no tracking information"，则说明本地分支和远程分支没有创建链接关系，这时可用命令 git branch --set-upstream-to origin/dev 创建链接关系，然后再 git pull。 |
|             |                                                              |
|             |                                                              |
|             | 六. 删除远程分支，如：                                       |
|             | git push origin :feature-post                                |
|             | (注意：origin后面有一个空格。即用一个空的分支代替要删除的远程分支) |
|             |                                                              |
|             |                                                              |
|             | 七. 在github上"Fork"开源项目                                 |
|             | 可以在github上"Fork"其它开源项目，"Fork"的作用就是在自己的帐号下克隆一个开源项目的仓库。然后，你再把它克隆到自己的电脑，这样就能参与开源项目的开发。如果想把自己的修改推送到开源项目的官方帐号，可以发起一个"pull request"请求(需要官方接受)。 |