这是一个功能强大的 **基于 rclone 的系统备份恢复脚本**，提供了完整的备份、恢复、定时任务管理和云存储集成功能。

项目地址：https://github.com/sephiroth233/sys-toolkit

---

## 📥 下载和安装方式

### 方式一：直接执行（推荐）
```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-backup-restore.sh)
```

### 方式二：保存后执行
```bash
curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-backup-restore.sh -o sys-backup-restore.sh && chmod +x sys-backup-restore.sh && sudo ./sys-backup-restore.sh
```

---

## 🎯 主要功能模块

### 1️⃣ **rclone 管理**
- **自动安装 rclone**：使用官方脚本自动下载安装
- **配置远程存储**：支持交互式配置 rclone remote
- **卸载 rclone**：删除 rclone 程序和配置文件

### 2️⃣ **备份功能** ⭐
- **一键备份**：压缩指定目录并上传到云端
- **自动清理**：根据设置的最大备份数量自动删除旧备份
- **查看备份列表**：列出云端所有备份文件

### 3️⃣ **恢复功能** ⭐
- **选择性恢复**：从云端备份列表中选择要恢复的备份
- **自定义恢复路径**：支持恢复到任意目录
- **恢复确认**：操作前确认，防止误操作

### 4️⃣ **定时备份管理**
- **设置定时任务**：支持每天/每周/每月定时备份
- **自定义 cron 表达式**：灵活配置备份时间
- **查看/删除定时任务**：管理已设置的定时备份

### 5️⃣ **配置管理**
- **备份源目录**：设置需要备份的目录
- **远程存储路径**：配置 rclone 远程存储地址
- **最大备份数量**：控制云端保留的备份数量
- **临时目录**：设置本地临时文件存放路径

### 6️⃣ **完全卸载**
- 删除 rclone 程序和配置
- 删除脚本配置和日志目录
- 删除定时备份任务
- 删除脚本自身

---

## 💻 命令行使用

支持命令行参数直接执行操作：

```bash
# 执行备份
sudo ./sys-backup-restore.sh backup

# 恢复备份（交互式选择）
sudo ./sys-backup-restore.sh restore

# 列出云端备份
sudo ./sys-backup-restore.sh list

# 安装 rclone
sudo ./sys-backup-restore.sh install

# 仅卸载 rclone
sudo ./sys-backup-restore.sh uninstall-rclone

# 完全卸载
sudo ./sys-backup-restore.sh uninstall

# 显示帮助
./sys-backup-restore.sh help

# 进入交互式菜单
sudo ./sys-backup-restore.sh
```

---

## 📋 菜单选项速查表

### **rclone 管理**
```
1.  安装 rclone
2.  配置 rclone 远程存储
3.  卸载 rclone
```

### **备份操作**
```
4.  立即执行备份 ⭐
5.  查看云端备份列表
```

### **恢复操作**
```
6.  从云端恢复备份 ⭐
```

### **定时任务**
```
7.  设置定时备份
8.  查看定时任务
9.  删除定时任务
```

### **配置管理**
```
10. 配置管理
```

### **卸载**
```
99. 完全卸载 (删除所有相关文件)
```

### **通用操作**
```
0.  退出程序
```

---

## 📊 配置文件说明

| 文件位置 | 作用 |
|:----------|:------|
| `/etc/sys-backup/config.conf` | 主配置文件（备份源、远程路径等） |
| `/var/log/sys-backup/backup.log` | 备份操作日志 |
| `/var/log/sys-backup/cron.log` | 定时任务执行日志 |
| `~/.config/rclone/rclone.conf` | rclone 远程存储配置 |

---

## 🔧 配置示例

### 配置文件内容 (`/etc/sys-backup/config.conf`)
```bash
# sys-backup 配置文件

### ✅ 灵活的定时备份
- 支持每天/每周/每月定时备份
- 支持自定义 cron 表达式
- 定时任务日志独立记录

### ✅ 安全的恢复机制
- 恢复前确认操作
- 支持自定义恢复路径
- 显示恢复结果摘要

### ✅ 彩色输出
用不同颜色显示不同信息：
- 🟢 绿色：成功提示
- 🔴 红色：错误警告
- 🟡 黄色：重要提示
- 🔵 蓝色/紫色：分类标题

---

## 🔧 工作流程示例

### 首次使用流程：

```bash
# 1. 下载并执行脚本
sudo bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-backup-restore.sh)

# 2. 选择菜单选项
# ├─ 选项 1：安装 rclone
# │  └─ 自动下载安装 rclone
# │
# ├─ 选项 2：配置 rclone 远程存储
# │  ├─ 运行 rclone config 交互式配置
# │  └─ 设置远程存储路径（如 s3:bucket-name/path）
# │
# ├─ 选项 10：配置管理
# │  ├─ 设置备份源目录
# │  ├─ 设置最大备份数量
# │  └─ 查看当前配置
# │
# ├─ 选项 4：立即执行备份
# │  ├─ 压缩备份目录
# │  ├─ 上传到云端
# │  └─ 自动清理旧备份
# │
# ├─ 选项 7：设置定时备份
# │  ├─ 选择备份频率
# │  └─ 设置备份时间
# │
# └─ 选项 6：从云端恢复备份
#    ├─ 选择备份文件
#    ├─ 设置恢复路径
#    └─ 下载并解压恢复
```

---

## 🚀 使用建议

### **备份使用流程**
- **首次使用**：执行脚本 → 选项 1 安装 rclone → 选项 2 配置远程存储 → 选项 10 设置备份目录
- **手动备份**：选项 4 立即执行备份
- **定时备份**：选项 7 设置定时任务
- **查看备份**：选项 5 查看云端备份列表

### **恢复使用流程**
- **恢复备份**：选项 6 → 选择备份文件 → 确认恢复路径 → 完成恢复
- **查看结果**：恢复完成后显示摘要信息

### **维护操作**
- **查看日志**：`cat /var/log/sys-backup/backup.log`
- **查看定时任务日志**：`cat /var/log/sys-backup/cron.log`
- **修改配置**：选项 10 配置管理

---

## 🌐 支持的云存储

通过 rclone 支持多种云存储服务：

| 存储类型 | 配置名称示例 |
|:---------|:-------------|
| Amazon S3 | `s3:bucket-name/path` |
| Cloudflare R2 | `r2:bucket-name/path` |
| Google Cloud Storage | `gcs:bucket-name/path` |
| Microsoft Azure Blob | `azure:container/path` |
| Backblaze B2 | `b2:bucket-name/path` |
| MinIO | `minio:bucket-name/path` |
| 阿里云 OSS | `oss:bucket-name/path` |
| 腾讯云 COS | `cos:bucket-name/path` |

更多存储类型请参考 [rclone 官方文档](https://rclone.org/overview/)

---

## ⚠️ 前置要求

- Linux 服务器（支持 systemd）
- Root 权限
- 网络连接（下载 rclone 和上传备份）
- 已配置的云存储账户

---

## 📝 注意事项

1. **备份前确认**：确保备份源目录存在且有足够权限
2. **存储空间**：确保云端存储有足够空间
3. **网络稳定**：大文件备份需要稳定的网络连接
4. **定时任务**：设置定时备份后，确保服务器保持运行
5. **完全卸载**：卸载操作不会删除云端备份数据

---

这个脚本是一个**完整的系统备份恢复解决方案**，特别适合需要定期备份服务器数据到云存储的用户！ 🎯



