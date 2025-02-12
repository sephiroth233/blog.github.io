## 终端预览

![Image](https://github.com/user-attachments/assets/0069ba70-33a7-49ab-a85f-8a894277b4bd)

## 笔者部署的一键脚本（对作者的脚本做了一下shadowtls节点配置的修改）
```
bash <(curl -fsSL sing-box-mu.vercel.app)
```
## 项目简介
- 输出客户端配置 url 方便快速设置

- 自带 wireguard 出站解锁 openai netflix disney 

- 安装 sing-box 配置 hy2 vless ss2022+shadowtls

## 安装 sing-box
下载sing-box
```
bash <(curl -fsSL https://sing-box.app/deb-install.sh)
```
修改/etc/sing-box/config.json
```
nano /etc/sing-box/config.json
```
查看config.json
```
cat /etc/sing-box/config.json
```
检查config.json
```
sing-box check -c /etc/sing-box/config.json
```
启动config.json
```
sing-box run -c /etc/sing-box/config.json
```
启动系统服务
```
systemctl enable sing-box
```
启动sing-box
```
systemctl start sing-box
```
停止sing-box
```
systemctl stop sing-box
```
重启sing-box
```
systemctl restart sing-box
```
查看sing-box
```
systemctl status sing-box
```
查看sing-box日志
```
cat /var/log/singbox.log
```
实时sing-box日志
```
tail -f /var/log/singbox.log
```
解码openai.srs
```
sing-box rule-set decompile geosite-openai.srs
```


## 卸载 sing-box
禁用sing-box
```
systemctl stop sing-box.service
systemctl disable sing-box.service
```
卸载sing-box
```
dpkg --purge sing-box
```
删除sing-box
```
rm -rf /etc/sing-box
rm -f /var/log/singbox.log
```
重载systemd
```
systemctl daemon-reload
```

## 原作者项目地址：https://github.com/SagerNet/sing-box
## 笔者fork仓库地址：https://github.com/sephiroth233/sing-box