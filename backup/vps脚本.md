
### docker安装

```shell
sudo curl -fsSL https://get.docker.com -o get-docker.sh
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
sudo chmod +x /usr/local/bin/ufw-docker
sudo ufw-docker install
```


### 测速脚本

[项目地址](https://github.com/i-abc/Speedtest "https://github.com/i-abc/Speedtest")

```shell
sudo bash <(curl -sL bash.icu/speedtest)  
```

```shell
sudo bash <(curl -sL https://raw.githubusercontent.com/i-abc/Speedtest/main/speedtest.sh)
```

### 流媒体解锁检测

```shell
sudo bash <(curl -L -s check.unlock.media)
```

### 三网回城路由测试

[项目地址](https://github.com/zhanghanyun/backtrace "https://github.com/zhanghanyun/backtrace")

```shell
sudo curl https://raw.githubusercontent.com/zhanghanyun/backtrace/main/install.sh -sSf | sh
```

### ip质量检测

```shell
sudo bash <(curl -Ls IP.Check.Place)
```
