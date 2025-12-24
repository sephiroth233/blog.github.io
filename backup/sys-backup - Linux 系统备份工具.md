这是一个强大的 **Linux 系统备份工具**，支持将指定目录备份并上传到 Cloudflare R2（S3 兼容存储），提供完整的配置管理、定时任务和备份管理功能。

项目地址：https://github.com/sephiroth233/sys-toolkit

---

## 📥 下载和安装方式

### 方式一：直接执行（推荐）
```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-backup.sh)
```

### 方式二：保存后执行
```bash
curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/sys-backup.sh -o sys-backup.sh && sudo bash sys-backup.sh
```

### 方式三：克隆仓库后执行
```bash
git clone https://github.com/sephiroth233/sys-toolkit.git
cd sys-toolkit
sudo ./sys-backup.sh
```

---

## 🎯 主要功能模块

### 1️⃣ **S3 配置管理**
- **创建配置**：配置 Cloudflare R2 S3 端点、访问密钥、存储桶信息
- **查看配置**：显示当前 S3 连接配置信息
- **修改配置**：更新 S3 端点、密钥、备份源目录等参数
- **删除配置**：清除所有配置信息
- **测试连接**：验证 S3 连接是否正常

### 2️⃣ **备份管理** ⭐
- **创建备份**：
  - 支持备份多个目录（空格分隔）
  - 支持排除特定文件/目录（支持通配符）
  - 自动压缩（gzip）
  - 生成带时间戳的备份文件名（`主机名-日期-时间.tar.gz`）
  - 自动上传到 S3 存储桶

- **查看备份列表**：
  - 显示所有已备份的文件
  - 显示文件大小和总容量

- **删除备份**：
  - 支持选择性删除单个或多个备份
  - 确认提示防止误删

### 3️⃣ **定时任务管理**
- **创建定时任务**：
  - 支持 Cron 表达式自定义备份时间
  - 自动添加到系统 Crontab
  - 支持日志记录

- **查看定时任务**：
  - 显示所有已配置的定时备份任务

- **删除定时任务**：
  - 移除所有定时备份任务

### 4️⃣ **系统初始化**
- 自动创建配置目录：`/etc/sys-backup`
- 自动创建日志目录：`/var/log/sys-backup`
- 自动初始化元数据文件（JSON 格式）
- 设置安全的文件权限（700）

---

## 🔧 工作流程示例

### 完整使用流程：

```bash
# 1. 下载并执行脚本
sudo ./sys-backup.sh

# 2. 交互式菜单操作
# ├─ 选项 1.1：创建 S3 配置
# │  ├─ 输入 Cloudflare R2 端点
# │  ├─ 输入访问密钥 (Access Key ID)
# │  ├─ 输入秘密密钥 (Secret Access Key)
# │  ├─ 输入桶名称
# │  ├─ 指定备份源目录 (例: /home /root /etc)
# │  ├─ 指定排除模式 (例: *.log *.tmp)
# │  └─ 测试连接验证
#
# ├─ 选项 1.5：测试 S3 连接
# │  └─ 验证配置是否有效
#
# ├─ 选项 2.1：创建备份
# │  ├─ 压缩指定目录
# │  ├─ 自动生成带时间戳的备份文件
# │  ├─ 上传到 S3 存储桶
# │  └─ 记录备份元数据
#
# ├─ 选项 2.2：查看备份列表
# │  └─ 显示 S3 中所有备份文件
#
# ├─ 选项 2.3：删除备份
# │  ├─ 列出所有备份
# │  ├─ 选择要删除的备份
# │  └─ 确认删除
#
# ├─ 选项 3.1：创建定时任务
# │  ├─ 输入 Cron 表达式 (例: 0 2 * * *)
# │  ├─ 自动添加到 Crontab
# │  └─ 设置日志输出
#
# ├─ 选项 3.2：查看定时任务
# │  └─ 显示当前配置的定时任务
#
# └─ 选项 4.2：查看日志
#    └─ 查看备份执行日志
```

---

## 📊 配置文件说明

| 文件位置 | 作用 |
|:---------|:------|
| `/etc/sys-backup/config.conf` | 主配置文件（S3 连接、备份源等） |
| `/etc/sys-backup/backup-metadata.json` | 备份元数据（JSON 格式） |
| `/var/log/sys-backup/backup.log` | 备份操作日志 |
| `/var/log/sys-backup/cron.log` | 定时任务执行日志 |

---

## 📋 菜单选项速查表

### **S3 连接配置**
```
1.1 创建配置
1.2 查看配置
1.3 修改配置
1.4 删除配置
1.5 测试连接
```

### **备份管理**
```
2.1 创建备份
2.2 查看备份列表
2.3 删除备份
```

### **定时任务**
```
3.1 创建定时任务
3.2 查看任务列表
3.3 删除任务
```

### **其他操作**
```
4.1 查看帮助
4.2 查看日志
```

---

## 🔐 S3 配置信息

### Cloudflare R2 获取密钥步骤：
1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 **R2** 服务
3. 点击 **API 令牌** → **创建 API 令牌**
4. 生成访问密钥和秘密密钥
5. 创建 R2 存储桶获取桶名称
6. 获取 S3 API 端点：`https://<account-id>.r2.cloudflarestorage.com`

### 配置文件示例：
```bash
# Cloudflare R2 S3 配置
S3_ENDPOINT="https://12345678.r2.cloudflarestorage.com"
S3_ACCESS_KEY="your_access_key"
S3_SECRET_KEY="your_secret_key"
S3_BUCKET="my-backup-bucket"
S3_REMOTE_DIR="/backups/server1"

# 备份配置
BACKUP_SOURCE_DIRS="/home /root /etc"
BACKUP_EXCLUDE_PATTERNS="*.log *.tmp /var/cache/*"

# 其他配置
ENABLE_COMPRESSION="true"
ENABLE_ENCRYPTION="false"
```

---

## ⏰ Cron 表达式速查

| 表达式 | 说明 |
|:-------|:------|
| `0 2 * * *` | 每天凌晨 2 点 |
| `0 */6 * * *` | 每 6 小时执行一次 |
| `0 0 1 * *` | 每月 1 号凌晨 0 点 |
| `0 0 * * 0` | 每周日凌晨 0 点 |
| `30 1 * * 1-5` | 工作日每天凌晨 1:30 |
| `0 */4 * * *` | 每 4 小时执行一次 |

**Cron 表达式格式：** `分 时 日 月 周`

---

## 🎨 命令行使用示例

### 使用命令行参数快速执行：

```bash
# 创建 S3 配置
sudo ./sys-backup.sh config create

# 查看 S3 配置
sudo ./sys-backup.sh config view

# 修改配置
sudo ./sys-backup.sh config modify

# 删除配置
sudo ./sys-backup.sh config delete

# 创建备份
sudo ./sys-backup.sh backup create

# 查看备份列表
sudo ./sys-backup.sh backup list

# 删除备份
sudo ./sys-backup.sh backup delete

# 创建定时任务
sudo ./sys-backup.sh schedule create

# 查看定时任务
sudo ./sys-backup.sh schedule list

# 删除定时任务
sudo ./sys-backup.sh schedule delete

# 显示帮助信息
sudo ./sys-backup.sh help
```

---

## ⚙️ 系统要求

### 前置依赖：
- Linux 系统（支持 systemd）
- Root 权限
- 必要的工具包：
  - `tar` - 压缩工具
  - `gzip` - 压缩格式
  - `aws` - AWS CLI（用于 S3 操作）
  - `curl` - 下载工具
  - `jq` - JSON 处理

### 支持的发行版：
- Ubuntu / Debian（apt 包管理器）
- CentOS / RHEL（yum 包管理器）
- 其他支持 systemd 的 Linux 发行版

---

## 🚀 使用建议

### **首次配置**
1. 执行脚本进入交互菜单
2. 选择 `1.1` 创建 S3 配置
3. 输入 Cloudflare R2 连接信息
4. 选择 `1.5` 测试连接验证配置

### **备份工作流**
1. 手动创建备份：`2.1` 创建备份
2. 查看备份文件：`2.2` 查看备份列表
3. 删除过期备份：`2.3` 删除备份

### **自动化备份**
1. 选择 `3.1` 创建定时任务
2. 输入 Cron 表达式（如：`0 2 * * *` 每天凌晨 2 点）
3. 系统会自动定时执行备份

### **日志监控**
- 查看日志：`4.2` 查看日志
- 查看实时日志：`tail -f /var/log/sys-backup/backup.log`
- 查看定时任务日志：`tail -f /var/log/sys-backup/cron.log`

---

## 💡 特色功能

✅ **自动压缩** - 使用 gzip 压缩备份文件，节省存储空间
✅ **时间戳命名** - 带主机名和时间戳的备份文件名，易于管理
✅ **选择性排除** - 支持通配符排除不需要备份的文件
✅ **元数据记录** - JSON 格式记录所有备份信息
✅ **定时自动化** - 集成 Crontab 支持自动定时备份
✅ **彩色输出** - 不同颜色区分信息、警告和错误
✅ **完整日志** - 详细的备份操作日志便于问题排查
✅ **安全权限** - 配置文件权限设置为 600，日志文件权限为 600

---

## ⚠️ 注意事项

- 首次运行需要 **root 权限** 创建配置目录
- 秘密密钥（Secret Access Key）不会在屏幕上显示，保护隐私
- 定时任务需要 root 权限才能正确执行
- 建议定期检查 S3 存储桶容量，避免超配额
- 删除备份操作无法撤销，请谨慎确认

---

这个脚本是一个**完整的系统备份和远程存储解决方案**，特别适合需要定期备份重要数据到云存储的用户！ 🎯
