# Mihomo 安装脚本使用文档

## 简介

`mihomo-install.sh` 是一个用于在 macOS 系统上自动安装和更新 Mihomo（Meta 内核）的 Shell 脚本。

## 功能特性

- ✅ 自动检测系统架构（arm64/amd64）
- ✅ 从 GitHub 获取最新版本
- ✅ 智能版本检测，避免重复下载
- ✅ 支持安装、更新、查看版本
- ✅ 友好的交互提示和错误处理

## 系统要求

- **操作系统**: macOS
- **架构**: arm64 (Apple Silicon) 或 amd64 (Intel)
- **权限**: 需要对 `/usr/local/bin` 目录的写权限
- **依赖工具**: curl, gunzip（系统自带）

## 下载和安装脚本

### 推荐方法：直接下载到 /usr/local/bin（推荐）

将脚本下载到 `/usr/local/bin` 目录，与 mihomo 内核放在同一位置，方便管理：

```bash
# 下载脚本到 /usr/local/bin
sudo curl -o /usr/local/bin/mihomo-install https://raw.githubusercontent.com/sephiroth233/bat-reo/main/mihomo-install.sh

# 添加执行权限
sudo chmod +x /usr/local/bin/mihomo-install

# 现在可以在任何位置直接使用
mihomo-install install
```

### 方法二：下载到当前目录

```bash
curl -O https://raw.githubusercontent.com/sephiroth233/bat-reo/main/mihomo-install.sh
chmod +x mihomo-install.sh
./mihomo-install.sh install
```

### 方法三：克隆仓库

```bash
git clone https://github.com/sephiroth233/bat-reo.git
cd bat-reo
chmod +x mihomo-install.sh
./mihomo-install.sh install
```

## 使用方法

### 1. 安装 Mihomo

首次安装或重新安装：

**如果脚本已安装到 /usr/local/bin（推荐）：**

```bash
# 在任何位置直接运行
mihomo-install install
```

**如果脚本在当前目录：**

```bash
./mihomo-install.sh install
```

**或者使用绝对路径：**

```bash
/bin/bash /path/to/mihomo-install.sh install
```

**执行流程：**
- 检测是否已安装
- 如果已安装，会提示是否重新安装（需要用户确认）
- 获取最新版本号
- 下载并安装到 `/usr/local/bin/mihomo`

**示例输出：**
```
[INFO] 开始安装...
[INFO] 系统架构: arm64
[INFO] 正在获取最新版本号...
[INFO] 版本号: alpha-2211789
[INFO] 下载地址: https://github.com/MetaCubeX/mihomo/releases/download/Prerelease-Alpha/mihomo-darwin-arm64-alpha-2211789.gz
[INFO] 正在下载...
[INFO] 下载完成
[INFO] 正在解压...
[INFO] 设置权限...
[INFO] 安装到 /usr/local/bin/mihomo...
[INFO] 验证安装...
[INFO] 安装成功！
[INFO] 安装完成！
[INFO] 当前已安装 Mihomo
Mihomo Meta alpha-2211789
```

### 2. 更新 Mihomo

更新到最新版本：

```bash
# 如果脚本已安装到 /usr/local/bin
mihomo-install update

# 或者在脚本所在目录
./mihomo-install.sh update
```

**执行流程：**
- 检查是否已安装（未安装会报错）
- 获取当前版本和最新版本
- 比较版本号
- 如果已是最新版本，提示并退出
- 如果有新版本，自动下载并更新

**示例输出（已是最新版）：**
```
[INFO] 开始更新...
[INFO] 检查当前版本...
[INFO] 当前版本: Mihomo Meta alpha-2211789
[INFO] 检查最新版本...
[INFO] 最新版本: alpha-2211789
[INFO] 已经是最新版本，无需更新
```

**示例输出（有新版本）：**
```
[INFO] 开始更新...
[INFO] 检查当前版本...
[INFO] 当前版本: Mihomo Meta alpha-2211788
[INFO] 检查最新版本...
[INFO] 最新版本: alpha-2211789
[INFO] 发现新版本，开始更新...
[INFO] 系统架构: arm64
[INFO] 正在下载...
[INFO] 更新完成！
```

### 3. 查看版本

查看当前安装的版本：

```bash
# 如果脚本已安装到 /usr/local/bin
mihomo-install version

# 或者在脚本所在目录
./mihomo-install.sh version
```

**示例输出：**
```
[INFO] 当前已安装 Mihomo
Mihomo Meta alpha-2211789
```

### 4. 查看帮助

```bash
# 如果脚本已安装到 /usr/local/bin
mihomo-install help

# 或者在脚本所在目录
./mihomo-install.sh help
```

或者不带参数：

```bash
mihomo-install
```

## 权限说明

脚本需要对 `/usr/local/bin` 目录的写权限。

### 推荐做法：修改目录权限

```bash
# 将 /usr/local/bin 的所有权改为当前用户
sudo chown -R $(whoami) /usr/local/bin

# 之后就可以不用 sudo 直接运行
mihomo-install install
```

### 或者使用 sudo

```bash
sudo mihomo-install install
```

## 安装位置

安装后的文件位置：

```
/usr/local/bin/mihomo-install  # 安装脚本（如果使用推荐方法）
/usr/local/bin/mihomo          # Mihomo 内核
```

两个文件在同一目录，方便管理。

确保 `/usr/local/bin` 在你的 `PATH` 环境变量中，这样就可以直接使用命令。

验证 PATH：

```bash
echo $PATH | grep "/usr/local/bin"
```

如果没有，添加到 `~/.zshrc` 或 `~/.bash_profile`：

```bash
export PATH="/usr/local/bin:$PATH"
source ~/.zshrc  # 或 source ~/.bash_profile
```

## 快速开始示例

完整的安装和使用流程：

```bash
# 1. 下载并安装脚本到 /usr/local/bin
sudo curl -o /usr/local/bin/mihomo-install https://raw.githubusercontent.com/sephiroth233/bat-reo/main/mihomo-install.sh
sudo chmod +x /usr/local/bin/mihomo-install

# 2. 修改目录权限（可选，避免每次使用 sudo）
sudo chown -R $(whoami) /usr/local/bin

# 3. 安装 Mihomo
mihomo-install install

# 4. 验证安装
mihomo -v

# 5. 后续更新
mihomo-install update
```

## 使用 Mihomo

安装完成后，可以直接使用 `mihomo` 命令：

```bash
# 查看版本
mihomo -v

# 查看帮助
mihomo -h

# 运行 Mihomo（需要配置文件）
mihomo -d /path/to/config/directory
```

## 卸载

### 卸载 Mihomo

```bash
sudo rm /usr/local/bin/mihomo
```

### 卸载安装脚本

```bash
sudo rm /usr/local/bin/mihomo-install
```

### 完全卸载

```bash
sudo rm /usr/local/bin/mihomo /usr/local/bin/mihomo-install
```

