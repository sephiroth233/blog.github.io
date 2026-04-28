
声明：本文仅供学习研究,切勿用于非法用途

Emby通过服务器mb3admin.com验证客户端是否具有高级会员授权，一旦验证通过后，会在客户端内保留一段时间的缓存信息。

因此可以通过修改host劫持域名mb3admin.com，使客户端连接到虚假服务器，获得虚假授权。由于缓存机制的存在，即使短时间离开劫持环境，仍然可以继续使用高级会员功能。

简单记录一下步骤：

1. 修改OP路由器的host文件，使得域名mb3admin.com指向VPS的IP
2. 准备证书，可以通过 [GMCert.org](https://www.gmcert.org/subForm) 进行线上签发：
	- 选择RSA算法，2048位加密，主题名称/CN填入“mb3admin.com”
	- “选择CA”条目的最后面有个“↓”，点击下载CA证书
	- 打开高级选项，选择普通证书
	- 主题备用名称填
	> DNS.1=mb3admin.com  
	> DNS.2=\*.mb3admin.com
	- 密钥用途：数字签名 | 加密密钥 | 加密数据
	- 扩展密钥用途：服务器认证 | 客户端认证
	- 证书有效天数：824
	- 证书链选项中勾选“自动包含CA证书链”
	- 点击“签发证书”，下载密钥和SSL证书
3. 将密钥和SSL证书上传到VPS，将CA证书安装到PC/iOS设备/Android设备等
1. 在VPS上通过nginx部署验证服务器，添加密钥和证书，通过伪静态配置实现验证所需的返回值
```
server { 
    listen      80; 
    listen      443 ssl; 
    server_name mb3admin.com; 
    ssl_certificate     /var/www/embyact/ssl.pem; 
    ssl_certificate_key /var/www/embyact/ssl.key; 
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2; 
    ssl_ciphers         HIGH:!aNULL:!MD5; 
    location / { 
        root   /var/www/embyact; 
        index  index.html; 
    } 
    location /admin/service/registration/validateDevice { 
        default_type application/json; 
        return 200 '{"cacheExpirationDays":365,"message": "Device Valid","resultCode": "GOOD"}'; 
    } 
    location /admin/service/registration/validate { 
        default_type application/json; 
        return 200 '{"featId":"","registered":true,"expDate":"2099-01-01","key":""}'; 
    } 
    location /admin/service/registration/getStatus { 
        default_type application/json; 
        return 200 '{"deviceStatus":"","planType":"","subscriptions":{}}'; 
    } 
    add_header Access-Control-Allow-Origin *; 
    add_header Access-Control-Allow-Headers *; 
    add_header Access-Control-Allow-Method *; 
    add_header Access-Control-Allow-Credentials true; 
}
```
5. 在安装Emby服务端的服务器或Docker中添加CA证书（搜索ca-certificates.crt和ca-bundle.crt文件进行添加）

### 验证是否成功
打开浏览器，访问下列网址：

https://mb3admin.com/admin/service/registration/validateDevice
https://mb3admin.com/admin/service/registration/validateDevice/666

若返回{"cacheExpirationDays": 365,"message": "Device Valid","resultCode": "GOOD"}

即安装成功。


原文地址：[建立虚假Emby验证服务器获得高级会员](https://x64.life/post/emby-fake-auth/)