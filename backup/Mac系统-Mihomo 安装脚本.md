用于在 macOS 系统上自动安装、管理并后台运行 Mihomo（Meta 内核）的 Shell 脚本。

> **更新说明**：原来的 `mihomo-install.sh` 只负责下载安装 Mihomo 内核，现在已经不再需要单独使用。新的 `mihomo-daemon.sh` 已经集成了内核下载、更新、TUN 默认配置、LaunchDaemon 后台运行、开机自启、配置切换和日志查看等能力。

项目地址：https://github.com/sephiroth233/sys-toolkit

---

## 📥 一键安装方式

### 方式一：直接执行（推荐）

如果你已经有自己的 Mihomo 配置文件：

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-daemon.sh) install --config /path/to/config.yaml
```

如果暂时没有配置文件，也可以直接安装：

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-daemon.sh) install
```

这种方式会生成一个最小 TUN 模式配置模板到：

```text
/etc/mihomo/config.yaml
```

但模板不包含真实代理节点，默认规则为 `MATCH,DIRECT`，需要后续自行补充节点、策略组和规则。

### 方式二：保存脚本后执行

```bash
# 下载脚本到 /usr/local/bin
sudo curl -fsSL https://raw.githubusercontent.com/sephiroth233/sys-toolkit/master/mihomo-daemon.sh -o /usr/local/bin/mihomo-daemon
sudo chmod +x /usr/local/bin/mihomo-daemon

# 使用已有配置安装
sudo mihomo-daemon install --config /path/to/config.yaml
```

如果 Mihomo 内核还没有安装，脚本会自动从 GitHub latest release 下载并安装到：

```text
/usr/local/bin/mihomo
```

---

## 🚀 默认运行方式

安装完成后，脚本会创建 macOS LaunchDaemon：

```text
/Library/LaunchDaemons/com.mihomo.daemon.plist
```

Mihomo 会以 root 身份后台运行，并开机自启：

```bash
mihomo -d /etc/mihomo
```

默认配置目录：

```text
/etc/mihomo
```

默认配置文件：

```text
/etc/mihomo/config.yaml
```

---

## 🎯 命令说明

| 命令 | 说明 |
|:-----|:-----|
| `install [--config /path/to/config.yaml] [--bin /path/to/mihomo]` | 安装 Mihomo daemon，缺少内核时自动下载 |
| `start` | 启动 Mihomo daemon |
| `stop` | 停止 Mihomo daemon |
| `restart` | 重启 Mihomo daemon |
| `status` | 查看 LaunchDaemon 状态 |
| `logs` | 查看 Mihomo 日志 |
| `uninstall` | 卸载 LaunchDaemon 服务，保留配置和内核 |
| `config-use /path/to/config.yaml` | 切换正在使用的配置文件 |
| `config-path` | 输出当前实际使用的配置路径 |
| `core-install` | 安装或重装 Mihomo 内核 |
| `core-update` | 更新 Mihomo 内核 |
| `core-version` | 查看 Mihomo 内核版本 |

---

## 🔧 使用示例

### 安装并启用已有配置

```bash
sudo mihomo-daemon install --config ./config.yaml
```

### 指定 Mihomo 内核路径

```bash
sudo mihomo-daemon install \
  --bin /opt/homebrew/bin/mihomo \
  --config ./config.yaml
```

### 查看服务状态

```bash
sudo mihomo-daemon status
```

### 查看日志

```bash
sudo mihomo-daemon logs
```

日志文件位置：

```text
/var/log/mihomo.log
/var/log/mihomo.err.log
```

### 重启服务

```bash
sudo mihomo-daemon restart
```

---

## 🔁 切换配置文件

如果你有多个配置文件，可以直接切换：

```bash
sudo mihomo-daemon config-use ./config-home.yaml
sudo mihomo-daemon config-use ./config-work.yaml
```

脚本会将指定配置复制到：

```text
/etc/mihomo/config.yaml
```

如果原配置存在，会先备份为：

```text
/etc/mihomo/config.yaml.bak
```

如果 Mihomo daemon 已经在运行，切换后会自动重启；如果服务未加载，则只切换配置，不强行启动。

查看当前实际配置路径：

```bash
mihomo-daemon config-path
```

输出：

```text
/etc/mihomo/config.yaml
```

---

## 🌐 WebUI 访问

如果配置文件中包含：

```yaml
external-controller: 127.0.0.1:9090
external-ui: ui
external-ui-name: metacubexd
external-ui-url: "https://gh.sephiroth.club/github.com/MetaCubeX/metacubexd/archive/refs/heads/gh-pages.zip"
```

启动后可以访问：

```text
http://127.0.0.1:9090/ui
```

如果配置了：

```yaml
secret: "你的密钥"
```

WebUI 连接时需要填写对应 Secret。

也可以使用在线面板连接本地控制端口：

```text
https://metacubex.github.io/metacubexd/
```

连接地址：

```text
http://127.0.0.1:9090
```

---

## ⚙️ Mihomo 内核下载加速

脚本默认使用以下代理加速 GitHub release 文件下载：

```text
https://gh.sephiroth.club
```

如需使用其他代理：

```bash
sudo GITHUB_DOWNLOAD_PROXY="https://your-proxy.example" mihomo-daemon core-install
```

如需直连 GitHub：

```bash
sudo GITHUB_DOWNLOAD_PROXY= mihomo-daemon core-install
```

---

## 📊 文件位置

| 文件 | 路径 |
|:-----|:-----|
| Mihomo 内核 | `/usr/local/bin/mihomo` |
| 配置目录 | `/etc/mihomo` |
| 当前配置 | `/etc/mihomo/config.yaml` |
| 配置备份 | `/etc/mihomo/config.yaml.bak` |
| LaunchDaemon | `/Library/LaunchDaemons/com.mihomo.daemon.plist` |
| 标准日志 | `/var/log/mihomo.log` |
| 错误日志 | `/var/log/mihomo.err.log` |
| 管理脚本（可选） | `/usr/local/bin/mihomo-daemon` |

---

## 🧹 卸载

卸载 LaunchDaemon 服务：

```bash
sudo mihomo-daemon uninstall
```

该命令会删除：

```text
/Library/LaunchDaemons/com.mihomo.daemon.plist
```

但不会删除配置、日志和 Mihomo 内核。

如需完全清理，可手动执行：

```bash
sudo rm -rf /etc/mihomo
sudo rm -f /var/log/mihomo.log /var/log/mihomo.err.log
sudo rm -f /usr/local/bin/mihomo
sudo rm -f /usr/local/bin/mihomo-daemon
```

---

## 🎨 特色功能

- **一体化管理**：内核安装、更新、服务管理、配置切换全部集成到一个脚本。
- **默认 TUN 场景**：适合需要 TUN、自动路由、DNS 劫持的 macOS 使用方式。
- **开机自启**：使用 LaunchDaemon 以 root 身份后台运行。
- **自动下载内核**：如果系统中没有 Mihomo，自动下载最新 release。
- **下载加速**：默认使用 `gh.sephiroth.club` 加速 GitHub release 下载。
- **配置切换**：支持 `config-use` 快速切换不同配置。
- **日志查看**：支持 `logs` 命令直接查看运行日志。

---

## ⚠️ 前置要求

- **操作系统**：macOS
- **权限**：需要管理员权限，安装和服务管理命令请使用 `sudo`
- **依赖工具**：`curl`、`gunzip`、`launchctl`，macOS 通常已自带
- **网络**：自动下载 Mihomo 内核时需要能访问 GitHub 或配置的下载代理

