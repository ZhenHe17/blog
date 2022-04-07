# 万字前端效率大提速系列 🚀 ：九、Git 和 Shell 的基础知识和工作实践

工作中我们需要用 Git 进行代码管理、版本控制、团队协作；需要用 Shell 运行命令行工具、控制服务器；本篇从基础到实践带大家一起学习 Git 和 Shell。

## Git

Git 是一个分布式版本控制系统（distributed version control system），在工作中我们用于多个开发者**协作开发**同一个项目。这里先做一些基础介绍，再说一些用得上的项目实践

### 基本概念和机制

既然 Git 是用来多人协作开发项目的，那么就需要有一个公共远程仓库（remote repository）来保管项目文件。要建立一个远程仓库，可以用线上的 GitHub、码云 或者 自建 GitLab 来建立。有了公共仓库，开发人员就可以将公共仓库克隆到本地，在本地开发好代码，然后将改动推送到公共仓库。也就是大家一起为公共仓库添砖加瓦的过程。

大致流程是： 建立远程仓库 -> 克隆到本地 -> 本地开发并暂存 + 提交 -> 本地将改动推送到远程分支，达到远程分支和本地改动一致。

在 Git 中，每一次改动提交是以 commit 为单位，一个 commit 包含的信息有改动文件内容、提交人、提交时间、提交说明等等。下面我们从一个刚建立的新仓库开始操作下试试：

```sh
# 创建并进入一个新文件夹
mkdir example
cd example
# 新建一个 README.md 文件，内容的第一行是 "# example"
echo "# example" >> README.md
# 初始化一个 git 仓库
git init
# 将 README.md 改动暂存
git add README.md
# 将暂存的改动（当前只有一个 README.md 改动）提交到本地仓库，备注"first commit"，此时会记录提交人、提交时间并生成一个 commitID。
git commit -m "first commit"
# 将当前分支命名为 master
git branch -M master
# 将本地仓库关联远程仓库 （仓库地址 https://github.com/xxxxxxx/example.git）
git remote add origin https://github.com/xxxxxxx/example.git
# 将 commitID 的改动推送到远程仓库的 master 分支，此时远程仓库的 master 分支上就有了一个 commit，文件 README.md 的改动成功提交
git push -u origin master
```

上面是首次提交，需要关联仓库和分支，再次提交就简单些了

```sh
# 加一行 "222222"
echo "222222" >> README.md
# 将改动暂存
git add .
# 将改动提交到本地，备注"second commit"
git commit -m "second commit"
# 将本地的 commit 提交到 master
git push
# 如果别的同学提交了，我们可以用 git pull 来同步更新 
```

此时远程仓库的 master 分支就有了两个 commit，看起来是这样的：

<第一次> -> <第二次>

为了保护 master 分支不被异常操作，我们会从 master 分支上切出一个 dev 开发分支：

```sh
git checkout -b dev
```

由于 dev 分支由 master 分支切出，所以 dev 分支当前的项目文件和 master 一致，commit 历史也是 <第一次> -> <第二次>

### 在团队中的操作流程及技巧

在工作中我们通常会为不同的环境准备不同的分支：master 正式生产环境分支；pre 灰度预发分支；test 测试环境分支（不同团队可能不太一样但大同小异），每个环境对应不同的部署环境。master 分支的代码通常是经过测试的安全代码

目前比较流行的是 merge 工作流，举个栗子：

1. 当我们接到一个开发需求时，会从 master 切出一个开发分支格式可以是： `<改动类型>/<改动说明>-<立项日期>`，如 `feat/add-some-page-20220407`，其中改动类型有 feat新功能；fix修复bug、docs文档说明；style格式化；refactor代码重构；chore构建、依赖等杂项修改；test测试用例 等等
2. 切好分支到开发完成前，需要向 master 分支提一个 merge request（MR 或者叫 pull request PR），提交 MR 一般在网页上操作。实践上应该尽早提交 MR，更直观的看到当前的开发情况，对于庞大的需求也可以提前部分 code review。
3. 对于开发分支，开发人员比较少的情况下可以直接 commit 提交到开发分支，开发人员比较多的情况建议切出个人的开发分支，以 MR 的形式提交到开发分支。
4. 开发完成后就准备合并到 master 分支上了。此时如果 master 分支有新的改动，开发分支应该先 rebase master 分支，以形成一个 “添砖加瓦” 形式的渐进迭代。

    关于 rebase，假设我们切开发分支时，master 和 feat 分支的历史都是 <第一次> -> <第二次>，经过了数天的开发，feat 分支变成了 一 -> 二 -> 三 -> 四，master 分支由于其他需求或者修复，变成了 一 -> 二 -> 五 -> 六。此时我们希望合并后的 master 是 一 -> 二 -> 五 -> 六 -> 三 -> 四。这样比较符合改动是基于 master 的。

    我们在 feat 分支上执行 `git rebase origin/master`，此时 feat 分支就会由 一 -> 二 -> 三 -> 四 变成 一 -> 二 -> 五 -> 六 -> 三 -> 四。把自身的改动接到了 origin/master 的后面，此时可能会需要解决 三、四 和 五、六之间的代码冲突，即可能改到了同一行代码。

    `git rebase origin/master` 之后因为改变了当前分支的历史，只能用 `git push -f` 强制将远程 feat 分支和本地同步，在 push -f 之前需要确保远程分支没有新改动。这也是为什么推荐开发人员多的时候使用 MR 来改动 feat 分支。

5. rebase 完成且通过测试可以上线后，就可以安全的在网页上点击 merge 合并代码了，合并后 master 会变成 一 -> 二 -> 五 -> 六 -> 三 -> 四。

QA：

- 在 `git merge` 、 `git rebase`时可能会出现冲突，怎么解决冲突？
  出现冲突时，文件中会出现 current change（当前改动） 和 incoming change（合并方改动），解决冲突的宗旨是最终的代码要是正确的，不能确保的话可以找 incoming 的作者来一起看看逻辑。将代码改为正确的之后，就可以 `git add .` `git rebase/merge --continue` 继续合并了。也可以执行 `git rebase/merge --abort` 来取消合并。

- 怎么合并 commit
  开发分支可能会出现很多 commit 一 -> 二 -> 三 -> 四 -> ...一百。此时我们可以 `git reset --soft 二` 此时二是 master 的历史，三到一百的全部改动（即开发分支的所有改动）会在暂存区，这时候 `git commit -m "合并后的说明"` 就会将 三到一百 合并成一个 commit 了，此时用 `git push -f`，远程分支就会由 一 -> 二 -> 三 -> 四 -> ...一百 变为 一 -> 二 -> 合并后

- cherry-pick 有什么用
  `git cherry-pick <commitID>`可以单独将一个其他分支的 commit 捡取到当前分支，比如在 master 上 `git cherry-pick <合并后>`，就能将 合并后的改动单独拿过来。如果你的开发分支只有 一 -> 二 -> 合并后，那么 cherry-pick 和 merge 的最终代码是一致的。

### 学习资料

- [Git 飞行指南](https://github.com/k88hudson/git-flight-rules/blob/master/README_zh-CN.md)

## Shell

Shell 是运行命令的程序，是人机交互的桥梁，主要在终端（terminal）中运行。工作中我们会用 Shell 来运行命令如 git、npm、vim、nginx、ssh、scp 等等。不同的操作系统打开终端的方式如下：

- Windows 下可以使用 [XShell](https://www.xshellcn.com/)、[Windows Terminal](https://github.com/microsoft/terminal)、[Git](https://git-scm.com/download/win) 等等。
- MacOS 中通过 启动台 -> 其他 -> 终端 打开系统内置终端应用。
- Linux 中根据不同的 Linux 系统，可以通过 应用 或建立 ssh 连接打开终端。
- VSCode 编辑器（适用各个平台，快捷键`` ctrl + ` ``）中可以打开应用内终端窗口，方便边开发边使用。

常见的 Shell 解释器有两种：bash 和 zsh，基础使用上区别不大，zsh 的智能补全体验会更好些，大家使用系统下默认的就行，不用纠结。

### 常用内置命令

```sh
# 查看当前目录
pwd
# 查看当前目录下的文件
ls
# 查看当前目录下的文件，并且显示文件权限、大小等信息
ls -l
# 进入指定目录，cd ./xxx 进入当前目录下的 xxx 文件夹；cd .. 返回上一级目录；cd /usr 进入绝对路径 /usr 目录，路径中 ./ 表示当前相对路径；../ 表示当前路径上一级目录；/xxx 表示绝对路径
cd <路径>
# 创建一个名为 xxx 的目录
mkdir xxx
# 删除 xxx 文件，rm -rf xxxdir 删除 xxxdir 整个文件夹（慎用）
rm xxx
# 将当前目录下的 bg.png 文件复制到 xxxdir 目录下
cp bg.png xxxdir/
# 将当前目录下的 bg.png 文件移动到 xxxdir 目录下
mv bg.png xxxdir/
# 使用 vim 编辑 xxx文件，在 vim 下 按 i 开始输入模式，按 esc 退出输入模式，按 :wq 保存并退出
vim <文件路径>
# 查看 8000 端口的占用情况，此时会返回一行带 PID 的数据
lsof -i:8000
# 将 <PID> 的进程关闭，释放端口占用
kill -9 <PID>
```

这里隆重介绍一个**超级好用**的快捷键（来自老运维前辈的指点），在终端中按 `ctrl + r`，可以快速搜索到你曾经输入的命令，例如：

- 曾经编辑过 nginx config，不想再通过 nginx -t 去找文件路径
  在终端中按 ctrl + r，再输入 vim / ，此时会搜索到 vim /usr/local/etc/nginx/nginx.conf
- 曾经输入过 lsof -i:8000，在终端中按 ctrl + r，再输入 lso，就会搜索到它，如果不符合搜索预期可以再按 ctrl + r
- 曾经执行过的长命令，不仅可以通过 ↑ 和 ↓ 查找最近命令，也可以通过 ctrl + r 查找

本小节学习资料：

- [Linux 命令大全](https://www.runoob.com/linux/linux-command-manual.html)
- [Linux vi/vim 教程](https://www.runoob.com/linux/linux-vim.html)

### 编写自定义全局命令

除了使用 `ctrl + r`寻找曾经输入的命令，还可以将命令编写成全局函数来快速调用。bash 编辑 `~/.bashrc` 文件、zsh 编辑 `~/.zshrc` 文件，下面是我个人常用的快捷全局函数（要注意函数命名不要冲突）：

```sh
# 控制台输入 cob feat/xxx 即可本地创建名为 feat/xxx 的新分支。等同于 git checkout -b feat/xxx
cob() {
    git checkout -b $1
}

# gcmit "feat: 某功能" 等同于 git add . 、git commit -m "feat: 某功能"
gcmit() {
    git add .
    git commit -m "$1"
}

# gcp <commitID> 等同于 git cherry-pick "commitID"
gcp() {
    git cherry-pick "$1"
}

# 等等常用 Git 操作都可以编写成快捷全局函数
gp() {
    git pull --rebase
}

# 输入 jv 8 将 java 切换到 JDK 1.8
jv() {
    version=$1
    case "$version" in
    6)
        # JAVA_HOME 的路径要和本地安装 java 的路径一致
        export JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk1.6.0_132.jdk/Contents/Home'
        ;;
    8)
        export JAVA_HOME='/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home'
        ;;
    esac
}

```

编写完之后不要忘记执行 `source ~/.bashrc`、`source ~/.zshrc` 或者重新打开终端来启用改动的配置。

### 使用 NodeJS 安装全局命令行工具

得益于 NodeJS 的生态，其中有我们常用的命令行工具，输入 `<没权限则在前面加上 sudo> npm install -g <工具名>` 安装全局依赖，例如：

- [Vue CLI](https://cli.vuejs.org/zh/guide/)，安装命令 `npm install -g @vue/cli`，安装成功后可以使用命令行工具 `vue create` 创建一个 Vue 项目。
- [serve](https://www.npmjs.com/package/serve)，安装命令 `npm install -g serve`，使用命令 `serve` 启动本地静态文件服务。
- [nrm](https://www.npmjs.com/package/nrm)，安装命令 `npm install -g nrm`，使用命令 `nrm ls` 和 `nrm use cnpm` 切换 npm 依赖镜像源。
- [ts-node](https://www.npmjs.com/package/ts-node)，安装命令 `npm install -g ts-node`，使用命令 `ts-node script.ts` 在控制台直接执行 .ts 文件。
- [yarn](https://www.npmjs.com/package/yarn)，安装命令 `npm install -g yarn`，使用命令 `yarn install` 来代替 npm。

### 使用 ssh 连接服务器并操作

工作中我们不仅会在本机使用命令行工具，还会登上各大云平台买的服务器或者物理机进行操作。下面列出一些常用的命令：

```sh
# ssh root@111.222.333.44 以 root 身份登录 IP 是 111.222.333.44 的服务器
ssh <登录用户>@<服务器IP>

# scp -r /user/repo/my-app/dist root@111.222.333.44:/usr/local/opt/nginx/html 
# 前端部署常用操作 上传整个打包目录（-r 参数）到服务器
scp <本地文件> <登录用户>@<服务器IP>:<服务器目录>

# 展示系统信息，主要是查看系统是32位还是64位，x86_64表示64位，i686 i386表示32位
uname -a

## 以下是 服务器安装 NodeJS 的过程
# 下载 NodeJS 安装包
wget https://registry.npmmirror.com/-/binary/node/v16.14.2/node-v16.14.2-linux-x64.tar.gz

# 解压安装包
tar -zxvf node-v16.14.2-linux-x64.tar.gz

# 把解压文件移动到合适位置
mv node-v16.14.2-linux-x64  /opt

# 编辑 bash 配置文件
vim /etc/profile

# 配置环境变量
vim /etc/profile

# vim 环境下 按 i 进入编辑模式，按 esc 退出编辑，按 :wq 保存并退出
export NODE_HOME=$PATH:/opt/node-v16.14.2-linux-x64/bin

# 应用改动的配置
source /etc/profile

# 将 node 和 npm 命令软链接到全局命令
ln -s /opt/node-v16.14.2-linux-x64/bin/node /usr/local/bin/node
ln -s /opt/node-v16.14.2-linux-x64/bin/npm /usr/local/bin/npm

# 验证是否成功
node -v
npm -v
## NodeJS 安装成功
```

对于不熟悉的命令，建议大家校对确保命令正确再执行；线上环境建议区分用户权限，避免使用 root 权限登录。
