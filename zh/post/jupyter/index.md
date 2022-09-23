

<!--more-->
{{< toc >}}

本文以 Debian/Ubantu 为例

## 安装 JupyterLab

1. 安装 pip 与 JupyterLab

```
sudo apt update
sudo apt install python3
sudo apt install pip3
pip3 install jupyterlab
```

2. 添加PATH

`export PATH=$PATH:/path/to/jupyterlab`

`echo $PATH`

## 安装较新版本 nodejs

1. 添加 apt 源文件并更新源:

`curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -`

2. 安装 nodejs 与 npm

`sudo apt install nodejs`

3. 查看 nodejs 版本

`npm --version`

## 配置 JupyterLab

1. 生成配置文件

`jupyter lab --generate-config`

2. 创建哈希密码

利用`ipython`创建密码

```python
from jupyter_server.auth import passwd
passwd(algorithm='sha1')
# 输入两次密码并保存sha1值

```

3. 修改 JupyterLab 配置文件

`mkdir ~/JupyterLab`

打开配置文件

`vim ~/.jupyter/jupyter_lab_config.py`

修改并取消注释（可以用vim搜索关键词）

```
c.NotebookApp.allow_root = True
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.notebook_dir = 'JupyterLab'
c.NotebookApp.open_browser = False
c.NotebookApp.password = '<上面保存的sha1值>'
c.NotebookApp.port = 8080
```

`:wq` 保存并退出

## 构建 JupyterLab 并启动

`jupyter lab build --dev-build=False --minimize=False`

`nohup jupyter lab &`

> 到现在为止已经可以用 http://公网ip:8080 访问 JupyterLab，后文介绍使用 https 访问以及开机自启动

## 使用 https 访问

将下列代码添加到 nginx 配置文件

```
client_max_body_size 1G;
location /lab {
        proxy_pass   http://127.0.0.1:8080/lab;
        proxy_connect_timeout 3s;
        proxy_read_timeout 5s;
        proxy_send_timeout 3s;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_redirect off;
    }
```

## 开机自启动

使用 systemctl 管理

`vim /etc/systemd/system/jupyter.service`

写入如下代码

```bash
[Unit]
Description=Jupyter Lab
After=network.target
[Service]
Type=simple
ExecStart=/home/aaa/.local/bin/jupyter-lab --config=/home/aaa/.jupyter/jupyter_lab_config.py
User=jiageng
WorkingDirectory=/home/aaa/
Restart=always
RestartSec=10
[Install]
WantedBy=multi-user.target
```

> 将aaa改为自己的用户名，路径等自行修改

启动 JupyterLab

```
systemctl enable jupyter
systemctl start jupyter
```

## 参考资料和推荐阅读
[添加 swap 空间 ](https://cloud.tencent.com/developer/article/1835500)
