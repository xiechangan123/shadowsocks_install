一、Debian下shadowsocks-libev一键安装脚本

wget -P /root --no-check-certificate https://raw.githubusercontent.com/xiechangan123/shadowsocks_install/master/shadowsocks-libev-debian.sh

chmod +x shadowsocks-libev-debian.sh

./shadowsocks-libev-debian.sh 2>&1 | tee shadowsocks-libev-debian.log

二、快速开启Google BBR的方法

echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf

echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf

sysctl -p

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

使用winscp或其他方法将img镜像文件上传至路由器，例如/tmp/upload/路径下。

以文件名为openwrt-x86-64-combined-squashfs.img为例。

执行DD命令写入
　　dd if=/tmp/upload/openwrt-x86-64-combined-squashfs.img of=/dev/sda
  
会回显一个写入信息
　　594944+0 records in
　　594944+0 records out
  
执行reboot重启机器，固件即生效。

Copyright (C) 2014-2022 Xlovett
