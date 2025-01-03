一、Debian下shadowsocks-libev一键安装脚本

wget -P /root -N --no-check-certificate https://raw.githubusercontent.com/xiechangan123/shadowsocks_install/master/shadowsocks-libev-debian.sh

chmod +x shadowsocks-libev-debian.sh

./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log

二、快速开启Google BBR/BBR3的方法

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

或者：

echo "net.core.default_qdisc=fq_pie" >> /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

或者：

echo "net.core.default_qdisc=fq" >> /etc/sysctl.d/99-sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.d/99-sysctl.conf

sysctl -p /etc/sysctl.d/99-sysctl.conf


状态检查：

sysctl net.ipv4.tcp_congestion_control

三、使用 root 用户登录，运行以下命令：

./shadowsocks-libev.sh uninstall

Debian/Ubuntu 系统卸载应该是

./shadowsocks-libev-debian.sh uninstall

安装完成后即已后台启动 Shadowsocks-libev ，运行：

/etc/init.d/shadowsocks status 可以查看进程是否启动。

本脚本安装完成后，会将 Shadowsocks-libev 加入开机自启动。

四、【全自动】Debian/Ubuntu/CentOS 网络重装一键脚本

wget --no-check-certificate -O AutoReinstall.sh https://git.io/AutoReinstall.sh && bash AutoReinstall.sh

密码：Pwd@Linux

五、关闭IPV4+IPV6双栈VPS的IPV6（实测Centos 7/8永久关闭，debian9重启后可能会失效）

编辑/etc/sysctl.conf，添加以下内容。
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1
net.ipv6.conf.eth0.disable_ipv6=1

然后运行sysctl -p，测试curl ip.sb变成了ipv4。

六、通过DD命令刷新openwrt固件

使用winscp或其他方法将img镜像文件上传至路由器，例如/tmp路径下。

确认img镜像文件已上传。

ls -l /tmp

以文件名为openwrt-x86-64-generic-squashfs-combined-efi.img为例。

执行DD命令写入
gunzip /tmp/openwrt-x86-64-generic-squashfs-combined-efi.img.gz && dd if=/tmp/openwrt-x86-64-generic-squashfs-combined-efi.img of=/dev/sda
  
会回显一个写入信息
　　594944+0 records in
　　594944+0 records out
  
执行reboot重启机器，固件即生效。

七、Debian删除残余的配置文件
remove将会删除软件包，但会保留配置文件．purge会将软件包以及配置文件都删除．
找出系统上哪些软件包留下了残余的配置文件

dpkg --list | grep "^rc"

其中第一栏的rc表示软件包已经删除（Remove），但配置文件（Config-file）还在. 现在提取这些软件包的名称．

dpkg --list | grep "^rc" | cut -d " " -f 3

删除这些软件包

dpkg --list | grep "^rc" | cut -d " " -f 3 | xargs sudo dpkg --purge

八、卸载旧内核
请确保使用中的内核不要卸载

dpkg --list | grep linux-image
# 卸载指定的旧内核
apt --purge remove linux-image-*.**.*-**-cloud-amd64

删除旧内核后，是时候更新“grub2”配置了：

update-grub2

安装Shadowsocks
执行以下命令来安装Shadowsocks：

apt install shadowsocks-libev

配置Shadowsocks
安装完成后，我们需要配置Shadowsocks以连接到我们的代理服务器。在终端中执行以下命令：

nano /etc/shadowsocks-libev/config.json

{
    "server":["[::0]", "0.0.0.0"],
    "server_port":5050,
    "mode":"tcp_and_udp",
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"xca,./123",
    "timeout":86400,
    "method":"aes-256-gcm"
}

完成配置后，保存文件并退出。

启动Shadowsocks
执行以下命令来启动Shadowsocks服务：

systemctl restart shadowsocks-libev

systemctl status shadowsocks-libev

如果一切顺利，Shadowsocks将会成功启动。

设置自启动

为了使Shadowsocks在系统启动时自动启动，我们需要执行以下命令：

systemctl enable shadowsocks-libev

卸载Shadowsocks

apt autoremove shadowsocks-libev



# 甲骨文 在【替换引导卷】中点击页面下方的【高级选项】，点击【元数据】,在【名称】处输入user_data，【值】处输入新的 公钥 ，或是下面脚本的base64加密值（base64加密方法可在线搜索“在线base64加密”任选一个进行在线加密即可）

#!/bin/bash

echo root:你的密码 | chpasswd root

sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin yes/g' /etc/ssh/sshd_config

sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/g' /etc/ssh/sshd_config

rm -rf /etc/ssh/sshd_config.d/* && rm -rf /etc/ssh/ssh_config.d/*

/etc/init.d/ssh* restart


Copyright (C) 2014-2024 Xlovett
