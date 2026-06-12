# reinstall一键 VPS 重装脚本

项目地址：https://github.com/bin456789/reinstall

## 简介

`reinstall` 是一键 DD/重装系统脚本，支持在 VPS / 独服上重装到主流 Linux 发行版和 Windows，全程自动识别硬盘、自动配置网络。

## 核心特性

- 支持 **19 种** Linux 发行版（Debian、Ubuntu、Alpine、Arch、Gentoo、Fedora、Rocky、openSUSE 等）
- 支持 Windows 官方原版 ISO 安装，自动注入 VirtIO 等公有云驱动
- **任意方向重装**：Linux → Linux、Linux → Windows、Windows → Windows、Windows → Linux
- **极低内存要求**：最低 256 MB 即可运行（比官方 netboot 更省资源）
- 自动识别 IP（动静态、纯 IPv6、跨网卡等情况）
- 支持 BIOS / EFI 引导，支持 ARM 服务器
- 不含自制包，所有资源实时从源站获取
- 安装过程可通过 SSH / HTTP / VNC / 串口监控，出错可手动救砖

## 基本使用

### Linux 下载

```bash
# 国外
curl -O https://raw.githubusercontent.com/bin456789/reinstall/main/reinstall.sh
# 国内
curl -O https://cnb.cool/bin456789/reinstall/-/git/raw/main/reinstall.sh
```

### Windows 下载

```batch
certutil -urlcache -f -split https://raw.githubusercontent.com/bin456789/reinstall/main/reinstall.bat
```

### 常用命令

```bash
bash reinstall.sh debian 12              # 重装到 Debian 12
bash reinstall.sh ubuntu 24.04           # 重装到 Ubuntu 24.04
bash reinstall.sh alpine 3.21            # 重装到 Alpine（超低内存）
bash reinstall.sh arch                   # 重装到 Arch Linux
bash reinstall.sh windows --iso="链接"   # 重装到 Windows（ISO）
bash reinstall.sh reset                  # 取消重装（重启前）
```

### 可选参数

| 参数 | 说明 |
|------|------|
| `--ssh-key KEY` | 预设 SSH 公钥 |
| `--password PWD` | 预设密码（默认随机） |
| `--ssh-port PORT` | 修改 SSH 端口 |
| `--hold 1` | 仅进入安装环境不安装（网络验证） |
| `--hold 2` | 安装后不重启（手动修改系统） |
