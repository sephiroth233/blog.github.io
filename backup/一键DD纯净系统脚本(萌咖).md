
[转载：原文地址](https://www.moeelf.com/archives/293.html)

注意事项：

* Vicer脚本目前不支持重装为CentOS 7系统，支持CentOS 6.9以下版本。
* 重装的系统源自官方发行版。
* 安装过程全自动进行，无需VNC操作，无需进入救援模式。
* 系统安装完成后的默认用户名为root，**默认密码为: `MoeClub.org`**

#### **DD脚本示例：**

##### **重装为CentOS：**

以下命令中的 -c 后面为CentOS版本号，-v 后面为64位/32位，可根据需求进行替换。

```nginx
# CentOS 6.10 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -c 6.10 -v 64 -a
# CentOS 6.10 32位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -c 6.10 -v 32 -a
```

##### **重装为Debian：**

以下命令中的 -d 后面为Debian版本号，-v 后面为64位/32位，可根据需求进行替换。

```nginx
# Debian 8 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -d 8 -v 64 -a
# Debian 9 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -d 9 -v 64 -a
# Debian 10 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -d 10 -v 64 -a
# Debian 11 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -d 11 -v 64 -a
# Debian 12 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -d 12 -v 64 -a
```

##### **重装为Ubuntu**

以下命令中的 -u 后面为Ubuntu版本号，-v 后面为64位/32位，可根据需求进行替换。

```nginx
# Ubuntu 12.04 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -u 12.04 -v 64 -a
# Ubuntu 14.04 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -u 14.04 -v 64 -a
# Ubuntu 16.04 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -u 16.04 -v 64 -a
# Ubuntu 18.04 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -u 18.04 -v 64 -a
# Ubuntu 20.04 64位：
bash <(wget --no-check-certificate -qO- 'https://www.moeelf.com/attachment/LinuxShell/InstallNET.sh') -u 20.04 -v 64 -a
```

#### **关于系统重装过程**

运行包含正确系统版本号的脚本后，新系统的安装会自动进行，无需人工干预。

**可能的三种情况：**

正常情况下10分钟左右就可以安装成功了，期间可以在VNC中观察安装过程：

[![](https://www.moeelf.com/wp-content/uploads/2020/09/dd-1.jpg)](https://www.moeelf.com/wp-content/uploads/2020/09/dd-1.jpg)

如果安装CentOS 7版本，会出现下图提示，表示不支持：

[![](https://www.moeelf.com/wp-content/uploads/2020/09/dd-2-1.jpg)](https://www.moeelf.com/wp-content/uploads/2020/09/dd-2-1.jpg)

如果输入了其它不支持或不存在的系统版本，则会出现下图提示，中止安装：

[![](https://www.moeelf.com/wp-content/uploads/2020/09/dd-3.jpg)](https://www.moeelf.com/wp-content/uploads/2020/09/dd-3.jpg)

#### 重装系统后更改root密码

耐心等待系统安装成功后，出于安全考虑，建议立即更改默认密码。
