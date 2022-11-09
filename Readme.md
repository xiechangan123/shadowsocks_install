一、Debian下shadowsocks-libev一键安装脚本

wget --no-check-certificate https://raw.githubusercontent.com/xiechangan123/shadowsocks_install/master/shadowsocks-libev-debian.sh

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

Copyright (C) 2014-2022 Xlovett
