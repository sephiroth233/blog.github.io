### WSL Ubuntu + Docker 配置教程

---

#### **1. 安装 WSL Ubuntu**

管理员身份打开 PowerShell

```powershell
wsl --install
```

- 官方文档：[Microsoft WSL 安装指南](https://docs.microsoft.com/zh-cn/windows/wsl/install)
- 安装后设置用户名/密码。

---

#### **2. 安装 Docker Compose 并配置**

进入wsl，然后执行下面指令：

```sh
bash <(curl -sSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/wsl-docker-setup.sh)
```

上面脚本会安装docker、docker-compose，配置免密码sudo、配置docker用户组和权限、配置docker自启动

---

#### **3.WSL Settings**

点击wisnows窗口图标，根据需要设置wsl内存、网络、内存回收相关属性。

![Image](https://github.com/user-attachments/assets/ab2774f9-57e3-41ce-951f-e87ecbf6f75a)


网络属性：[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/networking)

![Image](https://github.com/user-attachments/assets/da0d219f-3333-4448-b140-ddd747f100c8)

![Image](https://github.com/user-attachments/assets/7189ae01-a2e1-4363-b715-a5ed137383e3)

#### 4.wsl中开启ssh

```
sudo apt install openssh-server && sudo service ssh start
```

关闭wsl，然后管理员在powserShell执行：

```
# 放行ssh端口
New-NetFirewallRule -Name "WSL SSH" -DisplayName "WSL SSH" -Direction Inbound -Protocol TCP -LocalPort 22 -Action Allow

# 允许 ping（ICMP），方便调试
New-NetFirewallRule -Name "Allow ICMP" -DisplayName "Allow ICMP" -Direction Inbound -Protocol ICMPv4 -Action Allow

# 确认 SSH 规则面向的端口、协议正确（之前已加过，检查一下即可）
Get-NetFirewallRule -Name "WSL SSH" | Get-NetFirewallPortFilter
```
