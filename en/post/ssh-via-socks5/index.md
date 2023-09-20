
I used to find the process of opening Home Lab ports to the public network inconvenient and insecure. Recently, I configured Xray-core to access the internal network and decided to document the SSH configuration along the way.

P.S. In fact, using [Wireguard](https://www.wireguard.com) is a more elegant and secure solution, but Xray-core can simultaneously implement traffic routing, making it seem like the only choice.

<!--more-->
{{< toc >}}

## Linux and MacOS

Linux and Mac come with [netcat](https://netcat.sourceforge.net) installed by default, so you can use a proxy directly when using SSH.

```bash
ssh -o ProxyCommand="nc -X 5 -x 127.0.0.1:port %h %p" root@address
```

Here, -X specifies the socks protocol, -x specifies the local proxy port, and %h and %p are used as placeholders for the target address and port.

## SSH Configuration

Adding any of the following code snippets to `.ssh/config` will enable automatic connections.

```
Host ip.ip.ip.ip
  	ProxyCommand nc -X 5 -x 127.0.0.1:port %h %p
	# IdentityFile: If accessing via a key, you can input the private key address here.
```

```
Host test
    HostName ip.ip.ip.ip
  	ProxyCommand nc -X 5 -x 127.0.0.1:port %h %p
    User root
	# IdentityFile: If accessing via a key, you can input the private key address here.
```

The above two commands allow access through `ssh root@address` and `ssh test`, and you can add the private key address as needed.

## References and Recommended Reading

- [nc Archwiki](https://man.archlinux.org/man/nc.1.en)
- [Using ssh config file](https://linuxize.com/post/using-the-ssh-config-file/)
