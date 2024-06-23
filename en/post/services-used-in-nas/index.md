


I installed the PVE system on the base layer of my NAS for virtualization, primarily using OpenWRT and Debian virtual machines.
OpenWRT is used for dialing and routing, allocated two CPUs and 1GB of memory, with a passthrough network port for WAN connection to the optical modem.
The Debian system is responsible for running most of the services I use daily, including providing DNS, RSS, and media services.

<!--more-->
{{< toc >}}

## My NAS Overview
- CPU: 4 x Intel(R) N100 
- RAM: 8 GB
- Kernel: Linux 6.2.16-3-pve 
- Storage: One M.2 SSD + Two HDDs
- Two Gigabit Ethernet ports
## List of Services Used
| Category | Service Name | Function Description |
|----------|--------------|----------------------|
| Network | OpenWrt | Router operating system |
| | Proxmox VE | Virtualization platform |
| | AdGuard Home | Network ad blocking and DNS management |
| | MosDns | Provides DNS service as ADG upstream |
| | DDNS-go | Dynamic DNS service |
| | Neko | Remote desktop service |
| RSS & Writing | Miniflux | RSS reader |
| | Wewe-rss | RSS subscription management |
| | Memos | Note-taking tool |
| | Dillinger | Markdown editor |
| File & Media | Aria2 | Download tool |
| | File Browser | File manager |
| | Plex | Media server |
| | Jellyfin | Open-source media system |
| | Alist | File listing tool |
| Miscellaneous | Portainer | Docker container management |
| | Watchtower | Automatic Docker container updates |
| | Vault Warden | Password manager |
| | Ntfy | Notification push service |

The table shows most of the applications I've installed, all of which can be found on Github.
Homepage is the page I open most frequently, containing links to all the applications in the table, as shown below:
![Homepage display](https://r2gallery.diing.uk/%E5%9B%BE%E5%BA%8A%2Fhomepage.png)

## Providing DNS Service
To avoid DNS pollution and ISP monitoring, the most important service on my Debian VM is to obtain the fastest DNS and provide encrypted DNS service for all devices.
Simultaneously, this service also provides DNS caching and traffic splitting functions.
### Adguard Home
My terminal devices **first** access Adguard Home (abbreviated as ADG) via DNS-over-TLS and DNS-over-HTTPS to obtain DNS.
In ADG, the upstream DNS is set to Mosdns and a remote DNS service (e.g., Mosdns on a VPS). Due to distance differences, ADG will first fetch local DNS, only using remote responses when local is abnormal.
### MosDNS
Mosdns implements the following functions:
1. Cache DNS records and extend TTL
2. Filter ads through blacklists
3. Modify response IPs for services like Cloudflare to the best speed using speed testing tools
4. Synchronously query **domestic websites** through Alibaba and Tencent's encrypted DNS
5. Synchronously query **non-domestic addresses** through Cloudflare and DNS.SB encrypted DNS and set ECS
6. Verify IPs from step 5 to ensure no pollution
### Others
To assist the above functions, we need to run a DDNS service to update the NAS's public IP in real-time, and an acme.sh service to obtain TLS certificates.
Also, we need to map the port providing encrypted DNS service by ADG to the public network in OpenWRT.
## Reading and Writing
### RSS
RSS is my daily channel for obtaining information. I use Read You and NetNewsWire for reading on mobile devices and record on Memos.
- To achieve synchronization across different devices, I set up Miniflux in Docker and integrated it into apps through Fever and Google Reader.
- For WeChat public account articles, I use wewe-rss to fetch (need to set automatic full-text retrieval in miniflux).
- For websites without RSS (e.g., Bilibili, X), I can obtain through RSShub.
### Writing
- Memos provides a flomo-like experience but can be self-hosted. I use it to record fragments and manage with tags.
- Dillinger is my favorite open-source Markdown writing tool after Typora became paid.
## File Management
File management in my NAS system mainly relies on the following services:
- Aria2: This is a powerful download tool supporting multiple protocols. I mainly use it for large file downloads and tasks that need to run for a long time. A visualization page for Aria2 is also running.
- File Browser: This is a lightweight file manager providing an intuitive web interface.
- Alist: While its functionality overlaps with File Browser, Alist's feature is its ability to integrate multiple storage sources. I use it to uniformly manage local storage and some cloud storage services, including Alibaba Cloud Drive, Cloudflare R2, and AWS S3.
## Other Services
- Portainer: The most famous Docker container web management interface.
- Watchtower: This service automatically checks and updates Docker containers. It keeps my system always up-to-date and displays real-time update information on Homepage.
- Vault Warden: This is a Rust implementation of Bitwarden, used as the server for my password manager. Self-hosting a password manager gives me more control over sensitive information while avoiding dependence on third-party services.
- Ntfy: I use it to send various system notifications, including backup completion, storage space warnings, SSH login alerts, etc. It supports multiple receiving methods, including mobile apps and email, ensuring I receive important information promptly.
## References and Recommended Articles
- [awesome-sysadmin](https://github.com/awesome-foss/awesome-sysadmin)
- [Self-hosted Selection](https://zituoguan.com/)


