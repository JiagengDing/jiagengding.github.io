
Git is software for tracking changes in any set of files, usually used for coordinating work among programmers collaboratively developing source code during software development. Simple use just need to master about 10 commands.

<!--more-->

![工作流](https://cdn.jsdelivr.net/gh/jiagengding/pictures@main/uPic/pATn74.jpg)

|   | Simple Workflow   | Command                         |
|---|-------------------|---------------------------------|
| 1 | Initialize repo   | `git init` or `git clone <url>` |
| 2 | Add to INDEX      | `git add .`                     |
| 3 | Commit to HEAD    | `git commit -m "info"`          |
| 4 | Push to remote    | `git push`                      |
| 5 | Replace from HEAD | `git checkout -- <filename>`    |


## Initialize or Clone

Create a	new directory and run `git init` in this directory to create a repo.

Or use `git clone <url>` to clone from a remote server.

`git config --global user.name "<name>"` sets your user name.

`git config --global user.email "<email address>"` sets your email.

## Add and Commit Changes

1. `git add .` to add all changes, `git add <filename>` to add this file, changes will be added to INDEX.
2. `git commit -m "describe your changes"`, changes will be committed to HEAD.

## Push Changes

- If the repo is not in remote server, you need to add it with `git remote add origin <server>`.
- If the repo is in remote server, you can send it with `git push origin <server>`.

## Update Local Repo

- Update and merge your local repo to newest with `git pull`.
- Update specific remote branch to local with `git merge <branch name>`.

## Checkout and Reset

![Reset](https://cdn.jsdelivr.net/gh/jiagengding/pictures@main/uPic/quwenf.jpg)

- `git checkout -- <filename>` replaces your changes with the content in HEAD
- `git reset --soft <commit>` moves current branch to the given commit, INDEX and DIRECTORY are not updated
- `git reset <commit>` moves current branch to the given commit, INDEX will be updated
- (Dangerous Command!!!) `git reset --hard <commit>` discards ALL history and changes back to the specified commit

## View information

- List history for current branch `git log`
- List history for a specific file `git log --follow [file]`
- Show content differences between two branches `git diff [first-branch]...[second-branch]`

## References and Recommended Articles

- [.gitignore templates](https://github.com/github/gitignore)
- [ Community Book ](http://book.git-scm.com)
- [ Cheat Sheet ](https://training.github.com/downloads/github-git-cheat-sheet/)
- [ Visual Guide ](http://marklodato.github.io/visual-git-guide/index-en.html)
- [ Oh Shit, Git!?! ](https://ohshitgit.com)
