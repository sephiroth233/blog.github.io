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
bash <(curl -sSL https://raw.githubusercontent.com/sephiroth233/bat-reo/main/wsl-docker-setup.sh)
```
上面脚本会安装docker、docker-compose，配置免密码sudo、配置docker用户组和权限、配置docker自启动

---

#### **3.WSL Settings**
点击wisnows窗口图标，根据需要设置wsl内存、网络、内存回收相关属性。

![Image](https://github.com/user-attachments/assets/ab2774f9-57e3-41ce-951f-e87ecbf6f75a)

wsl默认会占据机器一半的资源：

![Image](https://github.com/user-attachments/assets/cce70bba-8c1b-4fb5-9540-308c8370f302)

网络属性：[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/networking)

![Image](https://github.com/user-attachments/assets/da0d219f-3333-4448-b140-ddd747f100c8)

![Image](https://github.com/user-attachments/assets/7189ae01-a2e1-4363-b715-a5ed137383e3)