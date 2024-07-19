
今天发现Tomcat启动提示端口占用，但是又找不到占用的进程。
经过Google后，发现可能是hyper-v的问题。

Windows 中有一个「TCP 动态端口范围」，处在这个范围内的端口，有时候会被一些服务占用。如果安装了 Hyper-V，那么 Hyper-V 会为容器宿主网络服务在「TCP 动态端口范围」中随机挑一些端口号保留。

#### 查看目前「TCP 动态端口」的范围

PowerShell或者CMD窗口输入以下命令：

```shell
netsh int ipv4 show dynamicport tcp 
```

[![](https://img.sephiroth.club/file/cc885021fcf3aaf2b022a.png)](https://img.sephiroth.club/file/cc885021fcf3aaf2b022a.png)

#### 查看当前所有的保留端口范围

```shell
netsh interface ipv4 show excludedportrange protocol=tcp
```

[![](https://img.sephiroth.club/file/d7f68fae5cf94dbf7c724.png)](https://img.sephiroth.club/file/d7f68fae5cf94dbf7c724.png)

如果这些被征用的端口范围随机挑选到 8088、8000、3000 等 Web 开发常用端口，那开发就会受到影响。

#### 解决方案

以管理员权限运行下面的命令，将「TCP 动态端口范围」重新设定为 49152-65535。如果你觉得这个范围太大，还可以改小一点。

```shell
 netsh int ipv4 set dynamic tcp start=49152 num=16384
 netsh int ipv6 set dynamic tcp start=49152 num=16384
```

然后重启电脑即可。

可以再运行命令
```shell
netsh int ipv4 show dynamicport tcp 
```
查看动态端口范围。
