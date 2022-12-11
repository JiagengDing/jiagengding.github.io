
本文以 Ubuntu Vps 为例，使用 snap 安装 Nextcloud，并使用 Caddy v2 反向代理，力求部署最简。

<!--more-->
{{< toc >}}

## 安装 Caddy v2

```bash
sudo apt update && sudo apt upgrade
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

> 默认配置文件：`/etc/caddy/Caddyfile`


## 安装 Nextcloud

1. 安装 snap `sudo apt install snap`
2. 安装 nextcloud `sudo snap install nextcloud`
3. 修改默认端口 `sudo snap set nextcloud ports.http=8080`
4. 启动 nextcloud `sudo snap start nextcloud`

> 到目前为止已经可以使用 http 访问 nextcloud 页面，下面配置 https 反向代理

## 修改配置 

### 配置 Nextcloud

打开配置文件 `/var/snap/nextcloud/current/nextcloud/config/config.php`

```php
'trusted_domains' => 
  array (
    0 => '默认 ip',  
    1 => '自定义域名',      

```

文件末尾括号内加入

```php
'overwriteprotocol' => 'https', 
```

重启 Nextcloud `sudo snap restart nextcloud`

### 配置 Caddy v2

打开默认配置文件并清空原有配置 `/etc/caddy/Caddyfile`，aaa.bbb.com 需改为自己的域名

```
{
http_port 80
https_port 443
}

aaa.bbb.com {

	encode gzip zstd

	rewrite /.well-known/carddav /remote.php/dav
    rewrite /.well-known/caldav /remote.php/dav

	reverse_proxy  localhost:8080
	
	header {
		 enable HSTS
	         Strict-Transport-Security max-age=31536000;includeSubdomains;preload;
	}
｝

log {
output file /var/log/caddy/access.log
}
```

### 如果使用 Nginx 反向代理配置（未验证）

```
erver {

	listen 80;

	listen [::]:80;


	server_name example.com;

	return 301 https://$host:443$request_uri;

}

server {

	listen 443 ssl http2;

	listen [::]:443 ssl http2;


	server_name example.com;


	client_max_body_size 0;

	underscores_in_headers on;


	location ~ {

		proxy_set_header Host $host;

		proxy_set_header X-Real-IP $remote_addr;

		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

		proxy_set_header X-Forwarded-Proto $scheme;

		add_header Front-End-Https on;	


		proxy_headers_hash_max_size 512;

		proxy_headers_hash_bucket_size 64;


		proxy_buffering off;

		proxy_redirect off;

		proxy_max_temp_file_size 0;

		proxy_pass http://127.0.0.1:8080;

	}

}
```

## 开启开机自启动

1. `sudo snap enable nextcloud`
2. `sudo systemctl enable caddy`
3. `sudo systemctl start caddy`

## 常用文件位置

- Caddy 配置文件：`/etc/caddy/Caddyfile`
- Nextcloud 配置文件：`/var/snap/nextcloud/current/nextcloud/config/config.php`
- Nextcloud 数据文件：`/var/snap/nextcloud/common/nextcloud/data/`


## 参考资料和推荐阅读

- [ Caddy 官方文档 ](https://caddyserver.com/docs/)
- [ Nextcloud 官方文档 ](https://docs.nextcloud.com/server/latest/admin_manual/installation/index.html)