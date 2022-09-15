
Git作为一个分布式版本控制软件，可以帮助你或你的团队更好的管理代码和历史记录，并且可以和远程服务器同步。尽管Git的功能非常强大，但是日常使用仅需要掌握10条左右的命令。

<!--more-->

![工作流](https://cdn.jsdelivr.net/gh/jiagengding/pictures@main/uPic/pATn74.jpg)

|   | 最简单的工作流   | 命令                            |
|---|------------------|---------------------------------|
| 1 | 初始化仓库       | `git init` 或 `git clone <url>` |
| 2 | 添加更改到INDEX  | `git add .`                     |
| 3 | 提交至HEAD       | `git commit -m "info"`          |
| 4 | 推送到远程服务器 | `git push`                      |
| 5 | 从HEAD中替换文件 | `git checkout -- <filename>`    |


## 初始化或克隆

使用命令`git init` 来初始化一个文件夹，或者使用命令`git clone [url]`从远程服务器克隆一个仓库。

`git config --global user.name "[name]"`配置用户名

`git config --global user.email "[email address]"`配置用户邮箱

## 添加和提交更改

1. `git add .` 将工作目录中的全部更改提交到**暂存区INDEX**。
2. `git commit -m "更改信息"` 用来将INDEX中的更改提交到**HEAD**。

## 推送到远程仓库

- 远端没有本地仓库，推送本地仓库使用命令 `git remote add origin <server>`。
- 远端已有本地仓库使用命令`git push origin <branch name>`推送到该分支。

## 更新仓库到本地

- `git pull` 用来获取并合并远端仓库至本地仓库。
- `git merge <branch name>` 将远程特定分支合并到本地。

## 回退本地更改

![Reset](https://cdn.jsdelivr.net/gh/jiagengding/pictures@main/uPic/quwenf.jpg)

- 使用HEAD中内容替换目录文件 `git checkout -- <filename>`
- `git reset --soft <commit>` 切换节点至commit, INDEX和目录不会改变
- `git reset <commit>` 切换节点至commit, INDEX会改变, 目录不会改变
- (**谨慎使用，除非你知道自己在做什么!!!**)`git reset --hard <commit>`, 丢弃commit之后的所有内容, INDEX和目录均会改变

## 查看信息

- 列出当前分支的历史记录 `git log`
- 列出特定文件的历史记录 `git log --follow [file]`
- 列出两个分支间的差异 `git diff [first-branch]...[second-branch]`

## 参考资料和推荐阅读

- [.gitignore templates](https://github.com/github/gitignore)
- [ Community Book ](http://book.git-scm.com)
- [ Cheat Sheet ](https://training.github.com/downloads/github-git-cheat-sheet/)
- [ Visual Guide ](http://marklodato.github.io/visual-git-guide/index-en.html)
- [ Oh Shit, Git!?! ](https://ohshitgit.com)
