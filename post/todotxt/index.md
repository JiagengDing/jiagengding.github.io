
todo.txt 是一种使用纯文本管理代办事项的工具，它的优点是轻便且易于跨系统。本文将介绍 todo.txt 的基本格式和在 Mac、Linux、Android 上的使用方法。

<!--more-->
{{< toc >}}

## 基本格式

1. todo.txt 文本文件中**每行表示一个任务**
2. 通过**符号**（`()`, `+`, `@`, `:`, `空格`）**划分**关键信息
3. 不同信息的输入需要如图所示的**特定顺序**，可选内容可以空缺

![todo.txt 任务格式](https://github.com/JiagengDing/pictures/blob/main/uPic/todo.png?raw=true)

### 待办事项（todo.txt)

`(A) 2023-01-01 email to professor @email`

1. `(A)`: 如果要声明优先级，则应出现在行首。以括号中的大写字母表示
2. `2023-01-01`可以选择添加创建日期`YYYY-MM-DD`，应出现于除优先级外的第一个位置
3. 使用`@`引导内容标签，`+`引导项目标签

### 完成事项（done.txt)

`x 2023-01-10 2023-01-01 first coursework @cw`

1. `x`: 以小写字母 x 加空格开头的任务标记已完成
2. `2023-01-10`: 完成日期位于 x 后，以空格分隔

### 附加元数据（可选）

使用 `key:value` 的格式定义附加元数据并放于句尾，例如：

- `due:2023-02-01` 设置截止时间
- `pri:A` 再完成事项中保留原始优先级


## todo.txt-cli

```bash
# Mac OS 安装
brew install todo-txt

# Arch Linux 安装
yay install todotxt

# 其他平台安装方式可以查看官方仓库
# https://github.com/todotxt/todo.txt-cli

# 下载配置文件并修改文件夹路径
wget https://raw.githubusercontent.com/todotxt/todo.txt-cli/master/todo.cfg ~/.todo.cfg
vim .todo.cfg
```

常用命令(`ITEM#`表示 ls 命令中显示的序号）：

- 列出代办事项 `todo.sh ls`
- 添加代办事项 `todo.sh add "THING @context"`
- 标记为完成 `todo.sh do ITEM#[, ITEM#, ITEM#, ...]`
- 将完成事项移动到 done.txt 文件 `todo.sh archive`
- 删除代办事项 `todo.sh rm ITEM# [TERM]`


## Android 应用与同步

我使用的是 [todo.txt for Android](https://play.google.com/store/apps/details?id=net.c306.ttsuper&utm_source=c306_apps&utm_campaign=c306)，其他支持的应用和其他平台的应用可以在[官网](http://todotxt.org/)查看。

可以将 todotxt-cli 配置文件中的路径修改为 Dropbox/GoogleDrive/syncthing/坚果云 等备份路径实现多设备同步。


## 参考资料和推荐阅读

- https://github.com/todotxt/todo.txt
- [todo.txt-cli](https://github.com/todotxt/todo.txt-cli)
- http://todotxt.org/