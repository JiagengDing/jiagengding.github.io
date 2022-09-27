

<!--more-->
{{< toc >}}

> This article uses **Debian/Ubuntu** as an example.

## Install JupyterLab

1. Install pip & JupyterLab

```
sudo apt update
sudo apt install python3
sudo apt install pip3
pip3 install jupyterlab
```

2. Add PATH

`export PATH=$PATH:/path/to/jupyterlab`

`echo $PATH`

## Install the latest version of nodejs

1. Add apt repository and update:

`curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash - `

2. Install nodejs & npm

`sudo apt install nodejs`

3. Check nodejs version

`npm --version`

## Configure JupyterLab

1. Generate config file

`jupyter lab --generate-config`

2. Generate HASH key

Use `ipython` generate

```python
from jupyter_server.auth import passwd
passwd(algorithm='sha1')
# input password twice and save the HASH key
```

3. Modify JupyterLab config file

`mkdir ~/JupyterLab`

open config file by vim or nano

`vim ~/.jupyter/jupyter_lab_config.py`

modify and uncomment ( Using vim to search keyword )

```
c.NotebookApp.allow_root = True
c.NotebookApp.base_url = '/jupyter'
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.notebook_dir = 'JupyterLab'
c.NotebookApp.open_browser = False
c.NotebookApp.password = '<HASH key>'
c.NotebookApp.port = 8080
```

`:wq` save and quit

## Build JupyterLab and start

`jupyter lab build --dev-build=False --minimize=False`

`nohup jupyter lab &`

> So far JupyterLab can be accessed using http://public-ip:8080, access using https and auto-start will be described later.

## Access by https

Add the followed config into nginx-config-file (`/etc/nginx/nginx.conf`)

```
client_max_body_size 1G;
location /jupyter {
        proxy_pass   http://127.0.0.1:8080;
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

## Auto start

Manage by systemctl

`vim /etc/systemd/system/jupyter.service`

Write followed codes

```bash
[Unit]
Description=Jupyter Lab
After=network.target
[Service]
Type=simple
ExecStart=/home/aaa/.local/bin/jupyter-lab --config=/home/aaa/.jupyter/jupyter_lab_config.py
User=aaa
WorkingDirectory=/home/aaa/
Restart=always
RestartSec=10
[Install]
WantedBy=multi-user.target
```

> Change aaa to your own username, and change the path to yourself.

Start JupyterLab

```
systemctl enable jupyter
systemctl start jupyter
```
## References and Recommended Articles
