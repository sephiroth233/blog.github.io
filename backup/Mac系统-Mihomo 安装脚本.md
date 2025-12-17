## 简介
`mihomo-install.sh` 是一个用于在 macOS 系统上自动安装和更新 Mihomo（Meta 内核）的 Shell 脚本。
## 功能特性
✅ 自动检测系统架构（arm64/amd64）

✅ 从 GitHub 获取最新版本

✅ 智能版本检测，避免重复下载

✅ 支持安装、更新、查看版本

✅ 友好的交互提示和错误处理
## 系统要求
**操作系统**: macOS**架构**: arm64 (Apple Silicon) 或 amd64 (Intel) 

**权限**: 需要对 `/usr/local/bin` 目录的写权限

**依赖工具**: curl, gunzip（系统自带）
## 下载和安装脚本
### 推荐方法：直接下载到 /usr/local/bin（推荐）
将脚本下载到 `/usr/local/bin` 目录，与 mihomo 内核放在同一位置，方便管理：

```bash
# 下载脚本到 /usr/local/bin
sudo curl -L -o /usr/local/bin/mihomo-install https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-install.sh

# 设置执行权限
sudo chmod +x /usr/local/bin/mihomo-install

# 执行脚本（现在可以直接使用 mihomo-install 命令）
mihomo-install install
```





### 方法二：下载到当前目录





### 方法三：克隆仓库

```bash
# 克隆整个仓库（需要 git）
git clone https://github.com/sephiroth233/sys-toolkit.git
cd sys-toolkit
```

### 方法四：直接从 GitHub 下载执行

直接从 GitHub 下载脚本并立即执行，无需保存到本地：

```bash
# 下载并执行最新版本
bash <(curl -sL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-install.sh) install

# 或者使用 wget
bash <(wget -qO- https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-install.sh) install
```

**注意：**
- 将 `用户名` 替换为实际的 GitHub 用户名
- 将 `sys-toolkit` 替换为实际的仓库名称
- 此方法直接执行远程脚本，建议先检查脚本内容再执行
- 可以添加 `version`、`update`、`uninstall` 等参数





## 使用方法
### 1. 安装 Mihomo
首次安装或重新安装：
**如果脚本已安装到 /usr/local/bin（推荐）：**





**如果脚本在当前目录：**





**或者使用绝对路径：**





**执行流程：**
检测是否已安装如果已安装，会提示是否重新安装（需要用户确认）获取最新版本号下载并安装到 `/usr/local/bin/mihomo`
**示例输出：**





### 2. 更新 Mihomo
更新到最新版本：





**执行流程：**
检查是否已安装（未安装会报错）获取当前版本和最新版本比较版本号如果已是最新版本，提示并退出如果有新版本，自动下载并更新
**示例输出（已是最新版）：**





**示例输出（有新版本）：**





### 3. 查看版本
查看当前安装的版本：





**示例输出：**





### 4. 查看帮助





或者不带参数：





## 权限说明
脚本需要对 `/usr/local/bin` 目录的写权限。
### 推荐做法：修改目录权限





### 或者使用 sudo





## 安装位置
安装后的文件位置：





两个文件在同一目录，方便管理。
确保 `/usr/local/bin` 在你的 `PATH` 环境变量中，这样就可以直接使用命令。
验证 PATH：





如果没有，添加到 `~/.zshrc` 或 `~/.bash_profile`：





## 快速开始示例
完整的安装和使用流程：





## 使用 Mihomo
安装完成后，可以直接使用 `mihomo` 命令：





## 卸载
### 卸载 Mihomo
仅卸载 Mihomo 二进制文件，保留安装脚本：

```bash
# 如果脚本在 /usr/local/bin
sudo mihomo-install uninstall

# 如果脚本在当前目录
sudo ./mihomo-install.sh uninstall
```

### 卸载安装脚本
仅删除安装脚本，保留 Mihomo 二进制文件：

```bash
# 如果脚本在 /usr/local/bin
sudo mihomo-install cleanup

# 如果脚本在当前目录
sudo ./mihomo-install.sh cleanup
```

### 完全卸载
同时卸载 Mihomo 和删除安装脚本：

```bash
# 如果脚本在 /usr/local/bin
sudo mihomo-install purge

# 如果脚本在当前目录
sudo ./mihomo-install.sh purge
```



