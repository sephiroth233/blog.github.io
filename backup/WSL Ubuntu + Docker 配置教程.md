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

#### **2.设置 WSL2 后台启动**

1. 在你的 WSL2 的 Debian 或 Ubuntu 中执行以下命令，允许用户执行所有命令而不需要密码：

```sh
sudo bash -c "echo '$USER ALL=(ALL) NOPASSWD: ALL' >/etc/sudoers.d/$USER"
```

2. 在 Windows 开机启动文件夹中添加一个以.vbs 结尾的文件，内容如下：

```vbs
set ws=wscript.CreateObject("wscript.shell")
ws.run "wsl -d Ubuntu", 0
```

要快速进入开机启动文件夹，可以按 Windows+R，然后输入 `shell:startup` 。你可能还会用到 `taskschd.msc` 。

---

#### **3. 安装 Docker Compose 并配置**

```sh
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装docker
curl -fsSL https://get.docker.com | sh

# 下载最新稳定版 Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 赋予可执行权限
sudo chmod +x /usr/local/bin/docker-compose

# 添加用户到 docker 组 ,刷新组权限
sudo usermod -aG docker $USER
newgrp docker  

# 配置 Docker 自启动（需 root 权限）
sudo systemctl enable docker
```

- 验证：`docker run hello-world`
- 官方文档：[Docker Compose 安装](https://docs.docker.com/compose/install/linux/)

---

#### **验证**

打开终端输入 `wsl`，检查 Docker 是否运行：`docker ps`。
