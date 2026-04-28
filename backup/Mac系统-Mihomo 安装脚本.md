用于在 macOS 系统上自动安装和管理 Mihomo（Meta 内核）的 Shell 脚本。

项目地址：https://github.com/sephiroth233/sys-toolkit

---

## 📥 下载和安装方式

### 方式一：直接执行（推荐）
```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-install.sh) install
```

### 方式二：保存后执行
```bash
# 下载脚本到 /usr/local/bin
sudo curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-install.sh -o /usr/local/bin/mihomo-install
sudo chmod +x /usr/local/bin/mihomo-install

# 执行安装
mihomo-install install
```

---

## 🎯 命令说明

| 命令 | 说明 |
|:-----|:-----|
| `install` | 安装 Mihomo（默认命令） |
| `update` | 更新到最新版本 |
| `version` | 查看当前安装版本 |
| `uninstall` | 卸载 Mihomo |
| `cleanup` | 删除安装脚本 |
| `purge` | 完全卸载（Mihomo + 脚本） |

---

## 🔧 使用示例

```bash
# 安装 Mihomo
mihomo-install install

# 更新到最新版本
mihomo-install update

# 查看当前版本
mihomo-install version

# 卸载 Mihomo
mihomo-install uninstall

# 完全卸载
mihomo-install purge
```

---

## 📊 文件位置

| 文件 | 路径 |
|:-----|:-----|
| Mihomo 二进制文件 | `/usr/local/bin/mihomo` |
| 安装脚本（可选） | `/usr/local/bin/mihomo-install` |

---

## 🎨 特色功能

- **自动架构检测**：支持 arm64 (Apple Silicon) 和 amd64 (Intel)
- **智能版本管理**：自动获取 GitHub 最新版本，避免重复下载
- **安全确认机制**：重新安装和删除操作需用户确认
- **彩色输出**：清晰的状态提示

---

## ⚠️ 前置要求

- **操作系统**：macOS
- **权限**：需要 `/usr/local/bin` 写权限（使用 sudo）
- **依赖工具**：curl, gunzip（系统自带）

