
[github地址](https://github.com/leitbogioro/Tools "https://github.com/leitbogioro/Tools")

#### Centos7：

```nginx
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh'  \
&& chmod a+x InstallNET.sh && bash InstallNET.sh -centos 7 -pwd '密码'
```

#### Debian12：

```nginx
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh' \
&& chmod a+x InstallNET.sh && bash InstallNET.sh -debian 12 -pwd '密码'
```

#### Ubuntu 22.04：

ps：ubuntu不支持自定义密码

```nginx
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh' \
&& chmod a+x InstallNET.sh && bash InstallNET.sh -ubuntu 
```

#### Windows11：

```nginx
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh' \
&& chmod a+x InstallNET.sh && bash InstallNET.sh -windows

```

#### 重点：

默认用户名:
对于 Linux：`root`
对于 Windows：`Administrator`

默认密码：
对于 Linux：`LeitboGi0ro`
对于 Windows：`Teddysun.com`

修改密码：

```nginx
echo root:password|sudo chpasswd root
```

![image.png](https://s2.loli.net/2024/06/03/aBHbFomGSMsxT8Z.png)
