这是一个**系统管理工具集的统一入口脚本**，用于快速调用和管理 fail2ban、网络代理、系统备份等工具。脚本会自动从远程仓库下载并执行对应的管理脚本。

项目地址：https://github.com/sephiroth233/sys-toolkit

---

## 📥 快速开始

### 一键运行（推荐）
```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-toolkit.sh)
```

### 下载后运行
```bash
curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-toolkit.sh -o sys-toolkit.sh
chmod +x sys-toolkit.sh
sudo ./sys-toolkit.sh
```

---

## 🎯 包含的工具

| 工具 | 功能 | 对应脚本 |
|:---|:---|:---|
| **fail2ban 管理** | 入侵检测和防护 | `fail2ban-manager.sh` |
| **网络代理配置** | Sing-box / Snell 代理 | `server-proxy.sh` |
| **系统备份恢复** | rclone 云端备份 | `sys-backup-restore.sh` |

---

## 🚀 使用方式

### 交互式菜单
```bash
sudo ./sys-toolkit.sh
```

运行后显示主菜单，可选择：
```
1. fail2ban 管理 (入侵检测和防护)
2. 网络代理配置 (Sing-box/Snell)
3. 系统备份恢复 (rclone)
4. 查看所有工具状态
5. 查看帮助信息
0. 退出
```

### 命令行模式

```bash
# 启动 fail2ban 管理
sudo ./sys-toolkit.sh fail2ban

# 安装 fail2ban
sudo ./sys-toolkit.sh fail2ban install

# 启动网络代理配置
sudo ./sys-toolkit.sh proxy

# 执行系统备份
sudo ./sys-toolkit.sh backup backup

# 查看备份列表
sudo ./sys-toolkit.sh backup list

# 查看所有工具状态
sudo ./sys-toolkit.sh status

# 查看帮助
sudo ./sys-toolkit.sh help
```

---

## 📊 状态监控

脚本会实时显示各工具的安装和运行状态：

```
=== 工具状态 ===
  fail2ban: 已安装 - 运行中
  Sing-box: 已安装 - 运行中
  Snell:    未安装
  rclone:   已安装
```

---

## ⚠️ 前置要求

- Linux 服务器（支持 systemd）
- Root 权限
- curl（用于下载脚本）
- 网络连接

---

## 🔧 工作原理

1. 主脚本从 GitHub 仓库下载对应的子脚本
2. 脚本缓存到 `/tmp/sys-toolkit/` 目录
3. 自动添加执行权限并运行
4. 支持传递命令行参数到子脚本

---

## 📁 相关文件

| 文件 | 说明 |
|:---|:---|
| `sys-toolkit.sh` | 主入口脚本 |
| `fail2ban-manager.sh` | fail2ban 管理脚本 |
| `server-proxy.sh` | 网络代理配置脚本 |
| `sys-backup-restore.sh` | 系统备份恢复脚本 |
| `/tmp/sys-toolkit/` | 脚本缓存目录 |

---

## 🔗 相关文档

- [fail2ban 管理工具文档](./fail2ban-manager.md)
- [网络代理配置文档](./server-proxy.md)
- [系统备份恢复文档](./sys-backup-restore.md)

---
