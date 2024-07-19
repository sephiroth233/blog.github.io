
### docker安装

```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
### docker-compose安装

[docker-compose最新版本](https://github.com/docker/compose/releases "docker-compose最新版本")
```shell
sudo curl -L "https://github.com/docker/compose/releases/download/v2.22.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose version
```

### ufw-docker安装
[ufw-docker教程](https://github.com/chaifeng/ufw-docker "ufw-docker教程")
```shell
sudo wget -O /usr/local/bin/ufw-docker  https://github.com/chaifeng/ufw-docker/raw/master/ufw-docker
chmod +x /usr/local/bin/ufw-docker
sudo ufw-docker install
```

### 三网回程路由测试脚本

```shell
bash <(curl -L -s check.unlock.media)
```
### ip质量检测脚本

```shell
bash <(curl -Ls IP.Check.Place)
```