
基本命令、创建用户、修改文件权限、解压缩、grep

<!--more-->
{{< toc >}}

## 基本命令

| 命令   | 举例           | 作用                  |
| ---    | ---            | ---                   |
| ls     | ls [-la]       | 列出文件（详细信息）  |
| cd     | cd dir/        | 进入 dir/ 目录        |
| pwd    | pwd            | 显示当前路径          |
| mv     | mv a.txt b.txt | 将 a.txt 移动到 b.txt |
| cp     | cp a.txt b.txt | 复制                  |
| rm     | rm a.txt       | 删除文件              |
| rm -rf | rm -rf ~/dir/  | 删除 dir 目录及子文件 |
| touch  | touch a.txt    | 新建 a.txt 文件       |
| mkdir  | mkdir dir/     | 新建 dir/ 目录        |
| cat    | cat a.txt      | 显示 a.txt 文件内容   |
| which  | which cat      | 查找可执行文件路径    |
| find   | find a.txt     | 查找文件位置          |

| 命令    | 举例                 | 作用                 |
| ---     | ---                  | ---                  |
| chsh -s | chsh -s /usr/bin/zsh | 修改当前用户 shell   |
| login   | login bob            | 登陆用户             |
| crontab | crontab -e           | 修改定时自动执行任务 |

## 新建用户和修改用户密码

- `useradd bob`  新建 bob 用户

- `useradd -g root bob` 新建位于 root 用户组的 bob 用户

- `useradd -d /home/bob bob` 指定 bob 的家目录

- `passwd bob` 设置/修改 bob 密码

## 修改文件所有者和权限

- `chown bob a.txt` 将 a.txt 所有者修改为 bob
- `chown -R bob dir/ ` 将 dir/ 目录及子文件 所有者修改为 bob

- `chmod +[w][r][x] a.py ` 为 a.py 添加 [读][写][执行] 的权限
- `chmod -R +[w][r][x] dir/ ` 修改 dir/ 目录下所有文件的权限

P.S. [ 详细内容 ](https://www.runoob.com/linux/linux-comm-chmod.html)

## 打包和解压缩

| tar 命令参数 | 作用         |
| ---          | ---          |
| -c           | 新建压缩文件 |
| -v           | 显示操作过程 |
| -f           | 指定压缩文件 |

- `tar -cvf logs.tar a.log,b.log` 打包至 logs.tar
- `tar -cvf logs.tar ~/logs/` 打包 log/ 目录及子文件

| tar 命令参数 | 作用               |
| ---          | ---                |
| -r           | 添加文件到压缩文件 |
| -x           | 从压缩包中抽取文件 |
| -t           | 显示压缩文件内容   |

-	`tar -zcvf logs.tar.gz ~/logs/` 打包并用 gzip 压缩 logs 目录及子文件
- `tar -ztvf logs.tar.gz` 查看 logs.tar.gz 内容
- `tar -zxvf logs.tar.gz` 解压 logs.tar.gz

| tar 命令参数 | 作用          | 文件名 | 压缩算法                                                              | 特点     |
| ---          | ---           | ---    | ---                                                                   | ---      |
| -z           | 使用gzip压缩  | .gz    | Deflate(结合了LZSS和霍夫曼编码)                                       | 快       |
| -j           | 使用bzip2压缩 | .bz2   | Burrows–Wheeler transform, move-to-front transform and Huffman coding | 高压缩率 |





## 参考资料和推荐阅读
