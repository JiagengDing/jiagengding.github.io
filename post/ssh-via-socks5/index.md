
之前通过开放 Home Lab 端口到公网的操作总觉得不够方便和安全，最近配置了 Xray-core 访问内网，顺便记录一下 ssh 的配置。

P.S. 事实上使用 [Wireguard](https://www.wireguard.com) 更优雅和安全
~~，但是 Xray-core 可以同时实现流量分流所以似乎是唯一选择~~
。

<!--more-->
{{< toc >}}

## Linux 和 MacOS

Linux 和 Mac 下默认安装了 [netcat](https://netcat.sourceforge.net) 所以可以在 ssh 时直接使用代理。

```bash
ssh -o ProxyCommand="nc -X 5 -x 127.0.0.1:port %h %p" root@address
```

其中 -X 指定 socks 协议，-x 指定本地代理端口，%h 和 %p 分别用来为目标地址和端口占位。

## SSH config 配置

将下面任一段代码添加到 `.ssh/config` 中可以实现自动连接。

```
Host ip.ip.ip.ip
  	ProxyCommand nc -X 5 -x 127.0.0.1:port %h %p
	# IdentityFile 如果通过密钥访问可以在这里输入私钥地址
```

```
Host test
    HostName ip.ip.ip.ip
  	ProxyCommand nc -X 5 -x 127.0.0.1:port %h %p
    User root
	# IdentityFile 如果通过密钥访问可以在这里输入私钥地址
```

上面两段命令分别可以通过 `ssh root@address` 和 `ssh test` 进行访问，同时可以按照需要添加私钥地址。


## 参考资料和推荐阅读

- [nc Archwiki](https://man.archlinux.org/man/nc.1.en)
- [Using ssh config file](https://linuxize.com/post/using-the-ssh-config-file/)
