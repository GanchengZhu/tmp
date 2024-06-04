步骤 1：创建 VPN 连接脚本
首先，创建一个脚本来连接到 GNOME VPN。假设你的 VPN 名称是 MyVPN。

打开终端，创建一个新脚本文件：

bash
复制代码
sudo nano /usr/local/bin/connect-vpn.sh
在文件中添加以下内容：

bash
复制代码
#!/bin/bash

# 使用 nmcli 命令连接 VPN
nmcli connection up id MyVPN
保存文件并退出编辑器（按 Ctrl+X，然后按 Y，最后按 Enter）。

使脚本可执行：

bash
复制代码
sudo chmod +x /usr/local/bin/connect-vpn.sh
步骤 2：创建 systemd 服务文件
接下来，创建一个 systemd 服务文件来运行这个脚本。

创建一个新的服务文件：

bash
复制代码
sudo nano /etc/systemd/system/connect-vpn.service
在文件中添加以下内容：

ini
复制代码
[Unit]
Description=Connect to GNOME VPN
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/connect-vpn.sh

[Install]
WantedBy=default.target
保存文件并退出编辑器。

步骤 3：创建 systemd 定时器文件
然后，创建一个定时器来在开机五分钟后运行服务。

创建一个新的定时器文件：

bash
复制代码
sudo nano /etc/systemd/system/connect-vpn.timer
在文件中添加以下内容：

ini
复制代码
[Unit]
Description=Run Connect VPN script 5 minutes after boot

[Timer]
OnBootSec=5min
Unit=connect-vpn.service

[Install]
WantedBy=timers.target
保存文件并退出编辑器。

步骤 4：启用和启动定时器
重新加载 systemd 配置以使新创建的服务和定时器文件生效：

bash
复制代码
sudo systemctl daemon-reload
启用定时器，使其在开机时自动启动：

bash
复制代码
sudo systemctl enable connect-vpn.timer
启动定时器：

bash
复制代码
sudo systemctl start connect-vpn.timer
现在，Ubuntu 系统将在开机五分钟后自动连接到指定的 GNOME VPN。你可以通过检查定时器的状态来验证是否成功：

bash
复制代码
systemctl status connect-vpn.timer
这样，你就完成了设置。在每次启动后的五分钟后，系统将自动连接到你的 GNOME VPN。
