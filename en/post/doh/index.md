

<!--more-->
{{< toc >}}

## Use DNS-over-https in GNU/Linux

1. Install Golang

Debian/Ubuntu:

`sudo apt install golang`

Arch/Manjaro:

'sudo pacman -S go'

2. Make GOPATH directory

```
mkdir ~/gopath
export GOPATH=~/gopath
```

3. Download [ dns-over-https ](https://github.com/m13253/dns-over-https) from GitHub

`git clone https://github.com/m13253/dns-over-https.git --depth=1`

4. Install doh-client and enable

```
cd dns-over-https
sudo make install
sudo systemctl start doh-client.service
sudo systemctl enable doh-client.service
```

5. Edit config file and restart doh-client

`sudo vim /etc/dns-over-https/doh-client.conf`

copy and paste

```
listen = [
    "127.0.0.1:53",
    "[::1]:53",
]

[upstream]

upstream_selector = "random"

# Google DNS
[[upstream.upstream_ietf]]
    url = "https://dns.google/dns-query"
    weight = 50

# Cloudflare DNS
[[upstream.upstream_ietf]]
    url = "https://1.1.1.1/dns-query"
    weight = 50

# DNS.SB DNS
[[upstream.upstream_ietf]]
    url = "https://doh.dns.sb/dns-query"
    weight = 50

[others]
bootstrap = [
    "8.8.8.8:53",
		"8.8.4.4:53",
    "1.1.1.1:53",
]

passthrough = [
    "captive.apple.com",
    "connectivitycheck.gstatic.com",
    "detectportal.firefox.com",
    "msftconnecttest.com",
    "nmcheck.gnome.org",

    "pool.ntp.org",
    "time.apple.com",
    "time.asia.apple.com",
    "time.euro.apple.com",
    "time.nist.gov",
    "time.windows.com",
]

timeout = 30

no_cookies = true
no_ecs = false
no_ipv6 = false
no_user_agent = false
verbose = false
insecure_tls_skip_verify = false
```

`:wq` quit vim，`sudo systemctl restart doh-client.service` restart doh-client

6. Change the system dns config

open /etc/resolv.conf file and insert in the first line：

```
nameserver ::1
nameserver 127.0.0.1
```

7. Use `dig` to check (Option)

`dig baidu.com`

## References and Recommended Articles

[ Tutorial to setup your own DNS-over-HTTPS (DoH) server ](https://www.aaflalo.me/2018/10/tutorial-setup-dns-over-https-server/)

[ DNS recommended ](https://blog.diing.uk/post/dns/)
