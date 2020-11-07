---
layout: post
title: Git 操作使用
category: 代码
tags:
  - git
  - github
  - gitlab
  - vcs
bg: git.png
---

# 2020-01-19-git

Git,最强大的vcs系统,如果你是软件开发者, 你没有理由不知道它,不使用它,本文主要为记录Git操作,GitHub操作,gitlab搭建,gogs搭建,等围绕Git而展开的一系列记录.

![git](../.gitbook/assets/git%20%281%29.png)

1. 配置GitHub账户信息

$ git config --global user.name "John Doe" $ git config --global user.email johndoe@example.com

1. create ssh keygen

   > $ ssh-keygen -t rsa -C "youremail@example.com" replace email address with your account.

2. add repository to github.

   > git remote add origin git@github.com:helloalanturing/repo-name.git "origin" is an alias for the real repository.

3. push files to github

   > git push -u origin master for the first time. after that, you don't need the parameter "-u", so just "git push origin master"

4. 创建本地Git仓库
5. cd 到仓库目录,或者mkdir一个目录,并cd到这个目录. 执行命令 git init
6. 添加文件到仓库,\(创建仓库后,即使目录下有文件,也不在仓库内,需要执行添加命令\) git add file \(file可以是目录,或通配符\)
7. 添加文件命令后需要执行提交命令 git commit -m "some message"
8. 删除本地仓库 find . -name ".git" \| xargs rm -Rf 可分2步骤,先找到文件,再删除文件.
9. 提交代码到远程仓库
10. 创建ssh key $ ssh-keygen -t rsa -C "youremail@example.com"
11. id\_rsa.pub下的公钥复制添加到GitHub上
12. 创建远程仓库 在GitHub上new 一个repository 注意:在创建远程仓库时,如果勾选了创建README.md,在与本地仓库关联前, 一定要先pull \(git pull origin master --allow-unrelated-histories\)远程仓库到本地,不然 ​ 直接提交会产生冲突,导致不必要的麻烦.
13. 关联本地仓库到远程仓库 $ git remote add origin git@github.com:michaelliao/learngit.git
14. 推送内容到远程仓库 git push -u origin master\(第一次\) git push origin master\(以后\)

    when push was refused try: 1.git fetch origin 2.git merge origin BRANCH. ​ or simply do: git pull origin BRANCH

**更改仓库链接**

git remote set-url origin [https://github.com/USERNAME/REPOSITORY.git](https://github.com/USERNAME/REPOSITORY.git)

**删除GitHub上多余的文件及目录**

**前言**

    上传项目到GitHub上时，误把本地IDE的配置文件上传了上去，以下是Git的删除命令。

```text
#删除src及下级目录的所有文件
git rm -r --cached src/\*

#提交操作
git commit -m "删除说明"

#推送到GitHub服务器
git push origin master
```

1. 上传项目到GitHub
2. 检查本地是否安装Git

> git —version

1. 检查本地是否存在SSH公钥和密钥

> ls ~/.ssh

如果存在_id\_rsa_ 和_id\_rsa.pub_ 说明已经存在。跳至`添加密钥到GitHub`

1. 创建SSH Key
2. 执行命令

> ssh-keygen -t rsa -b 4096 -C "你注册GitHub的邮箱"

1. 一直敲回车键
2. 添加密钥到GitHub

   进入GitHub，点击头像，选择`settings`

   ```text
   然后选择`SSH and GPG keys`

   然后`New SSH key`

   Title中输入邮箱

   key中输入刚才本地创建SSH key生成的`id_rsa.pub`文件的内容，复制方法为：

   pbcopy < ~/.ssh/id_rsa.pub    //执行此命令拷贝文件下的内容

   最后粘贴到key项。点击继续。
   ```

3. 测试是否关联好本地和GitHub

> ssh -T git@github.com

遇到选择选`yes`看到

> Hi xxxxx! You've successfully authenticated, but GitHub does not provide shell access.

说明关联成功。

1. 关联本地项目到GitHub

待续。。。

```java
public class Github{
    public static void main(String[] args){
        System.out.println("Hello world");
    }
}
```

1. 删除分支

```text
git push origin -d branch_name
```

1. **常用命令**

| 功能 | 命令 |
| :--- | :--- |
| 添加文件/更改到暂存区 | git add filename |
| 添加所有文件/更改到暂存区 | git add . |
| 提交 | git commit -m msg |
| 从远程仓库拉取最新代码 | git pull origin master |
| 推送到远程仓库 | git push origin master |
| 查看配置信息 | git config --list |
| 查看文件列表 | git ls-files |
| 比较工作区和暂存区 | git diff |
| 比较暂存区和版本库 | git diff --cached |
| 比较工作区和版本库 | git diff HEAD |
| 从暂存区移除文件 | git reset HEAD filename |
| 查看本地远程仓库配置 | git remote -v |
| 回滚 | git reset --hard 提交SHA |
| 强制推送到远程仓库 | git push -f origin master |
| 修改上次 commit | git commit --amend |
| 推送 tags 到远程仓库 | git push --tags |
| 推送单个 tag 到远程仓库 | git push origin \[tagname\] |
| 删除远程分支 | git push origin --delete \[branchName\] |
| 远程空分支（等同于删除） | git push origin :\[branchName\] |
| 查看所有分支历史 | gitk --all |
| 按日期排序显示历史 | gitk --date-order |

1. Q&A

   如何解决git中文乱码，git ls-files 中文文件名乱码问题？

   在~/.gitconfig中添加如下内容

   ```text
   [core]
      quotepath = false
   [gui]
      encoding = utf-8
   [i18n]
      commitencoding = utf-8
   [svn]
      pathnameencoding = utf-8
   ```

2. 如何处理本地有更改需要从服务器合入新代码的情况？

   ```text
   git stash
   git pull
   git stash pops
   tash
   ```

3. stash

查看 stash 列表：

```text
git stash list
```

查看某一次 stash 的改动文件列表（不传最后一个参数默认显示最近一次）：

```text
git stash show stash@{0}
```

以 patch 方式显示改动内容

```text
git stash show -p stash@{0}
```

1. 如何合并 fork 的仓库的上游更新？

```text
git remote add upstream https://upstream-repo-url
git fetch upstream
git merge upstream/master
```

1. 如何通过 TortoiseSVN 带的 TortoiseMerge.exe 处理 git 产生的 conflict？
2. 将 TortoiseMerge.exe 所在路径添加到 `path` 环境变量。
3. 运行命令 `git config --global merge.tool tortoisemerge` 将 TortoiseMerge.exe 设置为默认的 merge tool。
4. 在产生 conflict 的目录运行 `git mergetool`，TortoiseMerge.exe 会跳出来供你 resolve conflict。

   > 也可以运行 `git mergetool -t vimdiff` 使用 `-t` 参数临时指定一个想要使用的 merge tool。

5. 不想跟踪的文件已经被提交了，如何不再跟踪而保留本地文件？

`git rm --cached /path/to/file`，然后正常 add 和 commit 即可。

1. 如何不建立一个没有 parent 的 branch？

```text
git checkout --orphan newbranch
```

此时 `git branch` 是不会显示该 branch 的，直到你做完更改首次 commit。比如你可能会想建立一个空的 gh-pages branch，那么：

```text
git checkout --orphan gh-pages
git rm -rf .
// add your gh-pages branch files
git add .
git commit -m "init commit"
```

1. submodule 的常用命令

## 放弃本地

```text
git fetch --all
git reset --hard origin/master
git pull
```

**添加 submodule**

```text
git submodule add git@github.com:philsquared/Catch.git Catch
```

这会在仓库根目录下生成如下 .gitmodules 文件并 clone 该 submodule 到本地。

```text
[submodule "Catch"]
path = Catch
url = git@github.com:philsquared/Catch.git
```

**更新 submodule**

```text
git submodule update
```

当 submodule 的 remote 有更新的时候，需要

```text
git submodule update --remote
```

**删除 submodule**

在 .gitmodules 中删除对应 submodule 的信息，然后使用如下命令删除子模块所有文件：

```text
git rm --cached Catch
```

**clone 仓库时拉取 submodule**

```text
git submodule update --init --recursive
```

1. 删除远程 tag

```text
git tag -d v0.0.9
git push origin :refs/tags/v0.0.9
```

或

```text
git push origin --delete tag [tagname]
```

1. 清除未跟踪文件

```text
git clean
```

可选项：

| 选项 | 含义 |
| :--- | :--- |
| -q, --quiet | 不显示删除文件名称 |
| -n, --dry-run | 试运行 |
| -f, --force | 强制删除 |
| -i, --interactive | 交互式删除 |
| -d | 删除文件夹 |
| -e, --exclude  | 忽略符合  的文件 |
| -x | 清除包括 .gitignore 里忽略的文件 |
| -X | 只清除 .gitignore 里忽略的文件 |

1. 忽略文件属性更改

因为临时需求对某个文件 chmod 了一下，结果这个就被记为了更改，有时候这是想要的，有时候这会造成困扰。

```text
git config --global core.filemode false
```

1. patch

将未添加到暂存区的更改生成 patch 文件：

```text
git diff > demo.patch
```

将已添加到暂存区的更改生成 patch 文件：

```text
git diff --cached > demo.patch
```

合并上面两条命令生成的 patch 文件包含的更改：

```text
git apply demo.patch
```

将从 HEAD 之前的 3 次 commit 生成 3 个 patch 文件：

（HEAD 可以换成 sha1 码）

```text
git format-patch -3 HEAD
```

生成 af8e2 与 eaf8e 之间的 commits 的 patch 文件：

（注意 af8e2 比 eaf8e 早）

```text
git format-patch af8e2..eaf8e
```

合并 format-patch 命令生成的 patch 文件：

```text
git am 0001-Update.patch
```

与 `git apply` 不同，这会直接 add 和 commit。

1. 只下载最新代码

```text
git clone --depth 1 git://xxxxxx
```

这样 clone 出来的仓库会是一个 shallow 的状态，要让它变成一个完整的版本：

```text
git fetch --unshallow
```

或

```text
git pull --unshallow
```

1. 基于某次 commit 创建分支

```bash
git checkout -b test 5234ab
```

表示以 commit hash 为 `5234ab` 的代码为基础创建分支 `test`。

1. 恢复单个文件到指定版本

```bash
git reset 5234ab MainActivity.java
```

恢复 MainActivity.java 文件到 commit hash 为 `5234ab` 时的状态。

1. 设置全局 hooks

```bash
git config --global core.hooksPath C:/Users/mazhuang/git-hooks
```

然后把对应的 hooks 文件放在最后一个参数指定的目录即可。

比如想要设置在 commit 之前如果检测到没有从服务器同步则不允许 commit，那在以上目录下建立文件 pre-commit，内容如下：

```bash
#!/bin/sh

CURRENT_BRANCH=$(git rev-parse --abbrev-ref HEAD)

git fetch origin $CURRENT_BRANCH

HEAD=$(git rev-parse HEAD)
FETCH_HEAD=$(git rev-parse FETCH_HEAD)

if [ "$FETCH_HEAD" = "$HEAD" ];
then
    echo "Pre-commit check passed"
    exit 0
fi

echo "Error: you need to update from remote first"

exit 1
```

1. 查看某次 commit 的修改内容

```bash
git show <commit-hash-id>
```

1. 查看某个文件的修改历史

```bash
git log -p <filename>
```

1. 查看最近两次的修改内容

```bash
git log -p -2
```

1. 应用已存在的某次更改 / merge 某一个 commit

```bash
git cherry-pick <commit-hash-id>
```

cherry-pick 有更多详细的用法，可以参见帮助文档。

1. 命令行自动补全

在 shell 里加载 git-completion 系列脚本，详见 [https://github.com/git/git/tree/master/contrib/completion](https://github.com/git/git/tree/master/contrib/completion)

1. 文件每一行变更明细

```bash
git blame <filename>
```

1. 找回曾经的历史

```bash
git reflog
```

列出 HEAD 曾指向过的一系列 commit，它们只存在于本机，不是版本仓库的一部分。

还有：

```bash
git fsck
```

1. 记住 http\(s\) 方式的用户名密码

在有些情况下无法使用 git 协议，比如公司的 git 服务器设置了 IP 白名单，只能在公司内网使用 ssh，那么在外面就只能使用 http\(s\) 上传下载源码了，但每次都手动输入用户名/密码特别惨，于是乎就记住吧。

设置记住密码（默认 15 分钟）：

```bash
git config --global credential.helper cache
```

自定义记住的时间（如下面是一小时）：

```bash
git config credential.helper 'cache --timeout=3600'
```

长期存储密码：

```bash
git config --global credential.helper store
```

1. git commit 使用 vim 编辑 commit message 中文乱码

这个问题在 Windows 下出现了，没找到能完美解决的办法，一种方法是在 vim 打开后输入：

```bash
:set termencoding=GBK
```

这就有点太麻烦了，折衷的方法是改为使用 gVim 或其它你喜欢的编辑器来编辑 commit message：

```bash
git config --global core.editor gvim
```

另外在升级 Vim 到 8.1 之后，由于 PATH 环境变量里加的还是 vim80 文件夹，导致 git commit 时提示：

```text
error: cannot spawn gvim: No such file or directory
error: unable to start editor 'gvim'
Please supply the message using either -m or -F option.
```

使用 `which gvim` 查看：

```text
$ which gvim
/usr/bin/which: no gvim in xxxxxxx
```

将 PATH 里添加的 vim80 路径改为 vim81 后解决。

1. git log 中文乱码

只在 Windows 下遇到。

```bash
git config --global i18n.logoutputencoding gbk
```

编辑 git 安装目录下 etc/profile 文件，在最后添加如下内容：

```text
export LESSCHARSET=utf-8
```

1. git diff 中文乱码

只在 Windows 下遇到，目前尚未找到有效办法。

1. 统计代码行数

CMD 下直接执行可能失败，可以在右键，Git Bash here 里执行。

1. 统计某人的代码提交量

```bash
git log --author="$(git config --get user.name)" --pretty=tformat: --numstat | gawk '{ add += $1 ; subs += $2 ; loc += $1 - $2 } END { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }'
```

1. 仓库提交排名前 5

如果看全部，去掉 head 管道即可。

```bash
git log --pretty='%aN' | sort | uniq -c | sort -k1 -n -r | head -n 5
```

1. 仓库提交者（邮箱）排名前 5

这个统计可能不太准，可能有同名。

```bash
git log --pretty=format:%ae | gawk -- '{ ++c[$0]; } END { for(cc in c) printf "%5d %s\n",c[cc],cc; }' | sort -u -n -r | head -n 5
```

1. 贡献者排名

```bash
git log --pretty='%aN' | sort -u | wc -l
```

1. 提交数统计

```bash
git log --oneline | wc -l
```

1. 修改文件名时的大小写问题

修改文件名大小写时，默认会被忽略（在 Windows 下是这样），让 git 对大小写敏感的方法：

```bash
git config --global core.ignorecase false
```

或者使用 `git mv oldname newname` 也是可以的。

1. 修复 gitk 在 macOS 下显示模糊的问题

gitk 很方便，但是在 Mac 系统下默认显示很模糊，影响体验。

根据网上搜索的结果，解决方法有两种，我采用第一种解决，第二种未尝试。

方法一：

1. 重新启动机器，按 command + R 等 Logo 和进度条出现，会进入 Recovery 模式，选择顶部的实用工具——终端，运行以下命令：

   ```bash
   csrutil disable
   ```

2. 重新启动机器。
3. 编辑 Wish 程序的 plist，启动高分辨率屏支持。

   ```text
   sudo gvim /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/Wish.app/Contents/Info.plist
   ```

   在最后的 &lt;/dict&gt; 前面加上以下代码

   ```bash
   <key>NSHighResolutionCapable</key>
   <true/>
   ```

4. 更新 Wish.app。

   ```bash
   sudo touch Wish.app
   ```

5. 再次用 1 步骤的方法进入 Recovery 模式，执行 `csrutil enable` 启动对系统文件保护，再重启即可。

方法二：

```bash
brew cask install retinizer
open /System/Library/Frameworks/Tk.framework/Versions/Current/Resources/
```

打开 retinizer，将 Wish.app 拖到 retinizer 的界面。

