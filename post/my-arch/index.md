
安装 + lightDM + i3wm

<!--more-->
{{< toc >}}

## 系统安装

### 启动 Live 环境

1. [下载](https://archlinux.org/download/) ISO 文件，并验证签名
2. `# dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx status=progress && sync` 写入 U 盘 (Windows 可以使用 Rufus 应用)
3. 关闭 Secure Boot 并使用 U 盘启动（一般是开机按 F2）
4. 选择 Arch Linux install medium


### 安装系统

设置字体　`setfont /usr/share/kbd/consolefonts/LatGrkCyr-12*12.psfu.gz` 

1. 联网
```bash
ip link # 查看网络接口
ip link set wlan0 up # 打开接口
wpa_passphrase "<wifi-name>" "<wifi-password>" > wifi.conf # 写入 Wifi 配置
wpa_supplican -c wifi.conf -i wlan0 & # 连接网络
dhcpcd & # 分配　IP　地址
```

2. `timedatectl set-ntp true` 设置时间
3. 分区(`fdisk /dev/nvme0n1`)

|　分区　| 挂载点|　大小 | 举例|
|---|---|---|---|
|boot | /mnt/boot | 512M |/dev/nvme0n1p1|
|swap(可选)| | 1G | /dev/nvme0n1p2|
|root| /mnt | 剩余空间　|/dev/nvme0n1p3|

```bash
# 格式化各分区
mkfs.fat -F 32 /dev/nvme0n1p1
mkswap /dev/nvme0n1p2
swapon /dev/nvme0n1p2
mkfs.ext4 /dev/nvme0n1p3
```

4. 挂载分区

```bash
mount /dev/nvme0n1p3 /mnt
mkdir /mnt/boot
mount /dev/nvme0n1p1 /mnt/boot
```

5. 安装到硬盘

- `pacstrap -K /mnt base linux linux-firmware ` 安装内核与固件
- `genfstab -U /mnt >> /mnt/etc/fstab`  生成　fstab 文件

6. 配置系统

- 进入系统　`arch-chroot /mnt`
- 生成 local 信息　`local-gen`
- 编辑　/etc/locale.gen 文件　`LANG=en_US.UTF-8`
- 编辑　/etc/hostname 文件并写入主机名　`myhomename`
- 配置 localhost , 编辑　/etc/host `127.0.0.1 localhost` `::1 localhost`
- 设置密码 `passwd`
- 安装 grub `pacman -S grub efibootmgr intel-ucode os-prober`

7. 安装引导程序
```bash
mkdir /boot/grub
grub-mkconfig > /boot/grub/grub.conf

uname -m
grub-install --target==x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
pacman -S wpa_supplican netctl neovim dhcpcd ranger
```
8. 重启
```bash
exit
kill wpa_supplican dhcpcd
reboot
```






## 参考资料和推荐阅读

- https://wiki.archlinux.org/title/Installation_guide
