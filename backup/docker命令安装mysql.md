### 1. 准备目录结构

首先，在你的主机上创建一个目录结构，以便挂载MySQL的数据、配置和日志文件。假设我们将所有文件挂载到`/opt/mysql`：

```sh
mkdir -p /opt/mysql/data
mkdir -p /opt/mysql/conf.d
mkdir -p /opt/mysql/logs
```

### 2. 创建MySQL配置文件

在`/opt/mysql/conf.d`目录下创建一个自定义配置文件`my.cnf`。这个文件可以用于配置MySQL的各种参数。为简单起见，我们可以先创建一个空文件或一个基本的配置文件：

```sh
touch /opt/mysql/conf.d/my.cnf
```

如果需要，可以在`my.cnf`中添加自定义配置。例如：

```ini
[mysqld]
user=mysql
# 表示允许任何主机登陆MySQL
bind-address = 0.0.0.0
port=3306
default-storage-engine=INNODB
#character-set-server=utf8
character-set-client-handshake=FALSE
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'
secure-file-priv=
[client]
default-character-set=utf8mb4
[mysql]
default-character-set=utf8mb4
```

### 3. 拉取并运行MySQL Docker容器

使用以下命令拉取MySQL 8.0镜像并运行容器：

```sh
docker run --name mysql \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -v /opt/mysql/data:/var/lib/mysql \
  -v /opt/mysql/conf.d:/etc/mysql/conf.d \
  -v /opt/mysql/logs:/var/log/mysql \
  -p 3306:3306 \
  -d mysql:8.0
```

#### 参数说明：

- `--name mysql8`：为容器指定一个名称。
- `-e MYSQL_ROOT_PASSWORD=123456`：设置MySQL root用户的密码。
- `-v /opt/mysql/data:/var/lib/mysql`：挂载数据目录。
- `-v /opt/mysql/conf.d:/etc/mysql/conf.d`：挂载配置文件目录。
- `-v /opt/mysql/logs:/var/log/mysql`：挂载日志目录。
- `-p 3306:3306`：将容器的3306端口映射到主机的3306端口。
- `-d mysql:8.0`：指定使用MySQL 8.0镜像并在后台运行。

### 4. 配置MySQL以允许root远程连接

默认情况下，MySQL的root用户只能从localhost连接。要允许远程连接，需要进行以下配置：

#### 进入MySQL容器：

```sh
docker exec -it mysql bash
```

#### 登录MySQL：

```sh
mysql -u root -p
# 输入密码：123456
```

#### 允许root用户远程连接：

在MySQL命令行中执行以下命令：

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
FLUSH PRIVILEGES;
```

这将更新root用户的认证方式，并允许从任何主机连接。

### 5. 验证远程连接

确保你的防火墙允许3306端口的外部连接。你可以使用MySQL客户端工具（如MySQL Workbench）或命令行工具从远程主机连接到你的MySQL服务器进行测试：

```sh
mysql -h <your-host-ip> -u root -p
# 输入密码：123456
```

