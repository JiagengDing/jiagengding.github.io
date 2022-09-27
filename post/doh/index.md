
传统 DNS 使用 UDP/TCP 查询和响应, 这很容易被中间人（特别是 ISP）监听和篡改。
后来基于 DNSSEC 机制的引入解决了 DNS 篡改的问题，但是依旧没有解决监听的可能。

使用 dns-over-https(doh) 和 dns-over-tls(dot) 可以彻底解决监听和篡改的问题，这两者的主要区别在于传输协议（https 或 tls）和端口。

<!--more-->

HTTPS 的安全基础就是 TLS 协议，但是有了 HTTPS 协议的封装可以使得 DNS 查询与访问网页相同，并且使用与 https 网页443端口（ DOT 使用853端口）。
**因此相比 DOT 更加推荐使用 DOH** ，除非是在只支持 DOT 的 Android 设备。

{{< toc >}}

## 在 GNU/Linux 设备上使用加密 DNS

1. 安装 Golang(version>=1.17)

Debian/Ubuntu:

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt update
sudo apt install golang
```

Arch/Manjaro:

'sudo pacman -S go'

2. 创建 GOPATH 路径

```
mkdir ~/gopath
export GOPATH=~/gopath
```

3. 从 GitHub 下载 [ dns-over-https ](https://github.com/m13253/dns-over-https)

`git clone https://github.com/m13253/dns-over-https.git --depth=1`

4. 安装并开启自启动

```
cd dns-over-https
sudo make install
sudo systemctl start doh-client.service
sudo systemctl enable doh-client.service
```

5. 编辑配置文件并重启 doh

`sudo vim /etc/dns-over-https/doh-client.conf`

写入如下配置

```
listen = [
    "127.0.0.1:53",
    "[::1]:53",
]

[upstream]

upstream_selector = "random"

# DNSPod Public DNS（https://www.dnspod.cn/Products/Public.DNS）
[[upstream.upstream_ietf]]
    url = "https://doh.pub/dns-query"
    weight = 50

# Ali DNS
[[upstream.upstream_ietf]]
    url = "https://dns.alidns.com/dns-query"
    weight = 50

[others]
bootstrap = [
    "223.5.5.5:53",
    "119.29.29:53",
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

`:wq`退出编辑器，`sudo systemctl restart doh-client.service` 重启 doh-client

6. 修改系统配置

打开 /etc/resolv.conf 文件并在首行加入：

```
nameserver ::1
nameserver 127.0.0.1
```

7. 使用 dig 命令查看是否完成修改（可选）

`dig baidu.com`

## 在 MacOS/iPadOS/IOS 上通过安装描述文件使用 DOH

1. [下载](https://raw.githubusercontent.com/paulmillr/encrypted-dns/master/profiles/alibaba-https.mobileconfig) 或自行编辑下列证书

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>PayloadContent</key>
	<array>
		<dict>
			<key>DNSSettings</key>
			<dict>
				<key>DNSProtocol</key>
				<string>HTTPS</string>
				<key>ServerAddresses</key>
				<array>
					<string>2400:3200::1</string>
					<string>2400:3200:baba::1</string>
					<string>223.5.5.5</string>
					<string>223.6.6.6</string>
				</array>
				<key>ServerURL</key>
				<string>https://dns.alidns.com/dns-query</string>
			</dict>
			<key>PayloadDescription</key>
			<string>Configures device to use AliDNS Encrypted DNS over TLS</string>
			<key>PayloadDisplayName</key>
			<string>AliDNS DNS over HTTPS</string>
			<key>PayloadIdentifier</key>
			<string>com.apple.dnsSettings.managed.9d6e5fdf-e404-4f34-ae94-27ed2f636ac4</string>
			<key>PayloadType</key>
			<string>com.apple.dnsSettings.managed</string>
			<key>PayloadUUID</key>
			<string>35d5c8a0-afa6-4b36-a9fe-099a997b44ad</string>
			<key>PayloadVersion</key>
			<integer>1</integer>
			<key>ProhibitDisablement</key>
			<false/>
		</dict>
	</array>
	<key>PayloadDescription</key>
	<string>Adds the AliDNS to Big Sur and iOS 14 based systems</string>
	<key>PayloadDisplayName</key>
	<string>AliDNS over HTTPS</string>
	<key>PayloadIdentifier</key>
	<string>com.paulmillr.apple-dns</string>
	<key>PayloadRemovalDisallowed</key>
	<false/>
	<key>PayloadType</key>
	<string>Configuration</string>
	<key>PayloadUUID</key>
	<string>A4475135-633A-4F15-A79B-BE15093DC97A</string>
	<key>PayloadVersion</key>
	<integer>1</integer>
</dict>
</plist>
```

2. 打开描述文件并在`设置-描述文件`中安装证书

## 在 Android 设备上使用 DOT 或 DNS-over-HTTPS/3

打开系统设置-连接与共享-私人 DNS 并输入 DNS 提供商地址

`dns.alidns.com` 或 `dot.pub`


## 参考资料和推荐阅读

[ 配置私有 DOH 服务器 ](https://www.aaflalo.me/2018/10/tutorial-setup-dns-over-https-server/)

[ DNS 推荐 ](https://blog.diing.uk/post/dns/)

[ MacOS 其他加密 DNS 描述文件 ](https://github.com/paulmillr/encrypted-dns)

[ DNS-over-HTTP/3 in Android ](https://security.googleblog.com/2022/07/dns-over-http3-in-android.html)
