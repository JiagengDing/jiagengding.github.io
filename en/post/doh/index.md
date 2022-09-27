
A simple guide about using encrypted dns in Linux、Mac、iOSand Android.

<!--more-->
{{< toc >}}

## Use DNS-over-https in GNU/Linux

1. Install Golang(version>=1.17)

Debian/Ubuntu:

```
sudo add-apt-repository ppa:longsleep/golang-backports
sudo apt update
sudo apt install golang
```

Arch/Manjaro:

`sudo pacman -S go`

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

## Use DOH in MacOS/iPadOS/IOS

1. [ Download ](https://raw.githubusercontent.com/paulmillr/encrypted-dns/master/profiles/cloudflare-https.mobileconfig) or edit profile

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
					<string>2606:4700:4700::1111</string>
					<string>2606:4700:4700::1001</string>
					<string>1.1.1.1</string>
					<string>1.0.0.1</string>
				</array>
				<key>ServerURL</key>
				<string>https://cloudflare-dns.com/dns-query</string>
			</dict>
			<key>PayloadDescription</key>
			<string>Configures device to use Cloudflare Encrypted DNS over HTTPS</string>
			<key>PayloadDisplayName</key>
			<string>Cloudflare DNS over HTTPS</string>
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
	<string>Adds the Cloudflare DNS to Big Sur and iOS 14 based systems</string>
	<key>PayloadDisplayName</key>
	<string>Cloudflare DNS over HTTPS</string>
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

2. Open this profile and Install in `system preference - profiles`.

## Use DOT or DNS-over-HTTPS/3 in Android

Open system setting - private dns and input the provider hostname, such as `dns.google` or 'one.one.one.one'.

[ Here ](https://blog.diing.uk/post/dns/) are some DNS provider.

## References and Recommended Articles

[ Tutorial to setup your own DNS-over-HTTPS (DoH) server ](https://www.aaflalo.me/2018/10/tutorial-setup-dns-over-https-server/)

[ DNS recommended ](https://blog.diing.uk/post/dns/)

[ MacOS encrypted-dns configs ](https://github.com/paulmillr/encrypted-dns)

[ DNS-over-HTTP/3 in Android ](https://security.googleblog.com/2022/07/dns-over-http3-in-android.html)
