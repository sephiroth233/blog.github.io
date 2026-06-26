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

#### **2. 安装 Docker 与 Docker Compose**

进入 WSL，执行以下命令下载并运行安装脚本：

```shell
wget -O docker-install.sh https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/docker-install.sh
chmod +x docker-install.sh
sudo ./docker-install.sh
```

脚本启动后会显示交互菜单：

```
  1) 正常环境安装（使用 Docker 官方源）
  2) 国内环境安装（使用国内镜像源 + 加速器）
  3) 卸载 Docker（完全移除）
  4) 退出
```

- 网络条件好 → 选 **1**（Docker 官方源）
- 国内网络 → 选 **2**，再选择阿里云或清华 TUNA 镜像源，脚本会自动配置镜像加速器（`docker.1ms.run` / `atomhub.openatom.cn` / `docker.xuanyuan.me`）

脚本会自动完成：
- 卸载旧版本 Docker
- 根据系统（Ubuntu/Debian/CentOS/RHEL/Rocky/Alma）选择 APT/YUM 安装方式
- 安装最新版 Docker Engine + CLI + containerd + Buildx + Compose 插件
- 启动 Docker 并设置开机自启
- 将当前用户加入 docker 组
- 拉取 hello-world 验证安装

支持系统：Ubuntu、Debian、CentOS、RHEL、Rocky、AlmaLinux、Fedora。

---

#### **3. WSL Settings**

点击 Windows 窗口图标，根据需要设置 WSL 内存、网络、内存回收相关属性。

![Image](https://github.com/user-attachments/assets/ab2774f9-57e3-41ce-951f-e87ecbf6f75a)

网络属性：[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/networking)

![Image](https://github.com/user-attachments/assets/da0d219f-3333-4448-b140-ddd747f100c8)

![Image](https://github.com/user-attachments/assets/7189ae01-a2e1-4363-b715-a5ed137383e3)

#### 4. WSL 中开启 SSH

```shell
sudo apt install openssh-server && sudo service ssh start
```

关闭 WSL，然后管理员在 PowerShell 执行：

```powershell
# 放行ssh端口
New-NetFirewallRule -Name "WSL SSH" -DisplayName "WSL SSH" -Direction Inbound -Protocol TCP -LocalPort 22 -Action Allow

# 允许 ping（ICMP），方便调试
New-NetFirewallRule -Name "Allow ICMP" -DisplayName "Allow ICMP" -Direction Inbound -Protocol ICMPv4 -Action Allow

# 确认 SSH 规则面向的端口、协议正确（之前已加过，检查一下即可）
Get-NetFirewallRule -Name "WSL SSH" | Get-NetFirewallPortFilter
```