在clash系列软件中新建一个本地配置，然后将下面的内容复制、粘贴。
最后将配置中的 **节点订阅链接** 替换为你自己的节点订阅链接即可
```yaml
mode: rule
allow-lan: false
bind-address: '*'
port: 7899
socks-port: 7891
mixed-port: 7890
ipv6: false
external-controller: 127.0.0.1:9090

unified-delay: true
tcp-concurrent: true

profile:
  store-selected: true
  store-fake-ip: true

geox-url:
  geoip: https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat
  geosite: https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat
  mmdb: https://testingcf.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/country.mmdb
  asn: https://github.com/xishang0128/geoip/releases/download/latest/GeoLite2-ASN.mmdb

sniffer:
  enable: true
  sniff:
    HTTP:
      ports: [ 80, 8080-8880 ]
      override-destination: true
    TLS:
      ports: [ 443, 8443 ]
    QUIC:
      ports: [ 443, 8443 ]
  skip-domain:
  - '+.push.apple.com'

tun:
  enable: true
  stack: mixed
  dns-hijack:
  - "any:53"
  - "tcp://any:53"
  auto-route: true
  auto-redirect: true
  auto-detect-interface: true
  strict-route: true

dns:
  enable: true
  ipv6: false
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter-mode: blacklist
  fake-ip-filter:
  - "*.lan"
  - "+.local"
  - "+.localdomain"
  - "+.home.arpa"
  - "localhost"
  - "localhost.*"
  - "+.msftconnecttest.com"
  - "+.msftncsi.com"
  - "time.*.com"
  - "time.*.gov"
  - "time.*.edu.cn"
  - "time.*.apple.com"
  - "time-ios.apple.com"
  - "+.pool.ntp.org"
  - "ntp.*.com"
  - "ntp.*.com.cn"
  - "ntp.*.org"
  - "ntp.*.org.cn"
  - "+.push.apple.com"

  default-nameserver:
  - 223.5.5.5
  - 119.29.29.29
  nameserver:
  - https://doh.pub/dns-query
  - https://dns.alidns.com/dns-query

######### 锚点 start #######
# proxy策略相关
pg: &pg { type: select, proxies: [ 美国节点, 香港节点, 台湾节点, 日本节点, 韩国节点, 新加坡节点, 手动选择, 自动选择, DIRECT ] }
#订阅更新相关
pu: &pu { type: http, interval: 86400, health-check: { enable: true, url: "https://www.apple.com/library/test/success.html", interval: 300, timeout: 5000 } }
#延迟测试相关
url-test: &url-test { type: url-test, url: "https://www.apple.com/library/test/success.html", interval: 300, lazy: true, tolerance: 50, timeout: 1000, max-failed-times: 3, hidden: true, include-all: true }
fall-back: &fall-back { type: fallback, url: "https://www.apple.com/library/test/success.html", interval: 300, lazy: true, tolerance: 50, timeout: 1000, max-failed-times: 3, hidden: true, include-all: true }
#节点选择
select: &select { type: select, proxies: [ DIRECT ], use: [ Subscribe ] }
bc: &bc { type: http, format: text, behavior: classical, interval: 86400 }

proxy-providers:
  Subscribe:
    url:  节点订阅链接
    <<: *pu

rule-providers:
  AI:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/AI.list
    path: ./ruleset/Tool/AI.yaml
  Emby:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Emby.list
    path: ./ruleset/Tool/Emby.yaml
  Apple:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Apple.list
    path: ./ruleset/Tool/Apple.yaml
  Telegram:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Telegram.list
    path: ./ruleset/Tool/Telegram.yaml
  YouTube:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/YouTube.list
    path: ./ruleset/Tool/YouTube.yaml
  Microsoft:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Microsoft.list
    path: ./ruleset/Tool/Microsoft.yaml
  Github:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Github.list
    path: ./ruleset/Tool/Github.yaml
  Lan:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/Lan.list
    path: ./ruleset/Tool/Lan.yaml
  ChinaDomain:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/ChinaDomain.list
    path: ./ruleset/Tool/ChinaDomain.yaml
  ChinaIP:
    <<: *bc
    url: https://gh.sephiroth.club/raw.githubusercontent.com/sephiroth233/Tool/master/mihomo/Rules/ChinaIP.list
    path: ./ruleset/Tool/ChinaIP.yaml

proxies: []

proxy-groups:

#策略组
- { name: 手动选择, <<: *select, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Rocket.png" }
- { name: Final, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Final.png" }
- { name: Ai, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/ChatGPT.png" }
- { name: Telegram, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Telegram.png" }
- { name: YouTube, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/YouTube.png" }
- { name: Github, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/GitHub.png" }
- { name: Microsoft, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Microsoft.png" }
- { name: Emby, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Emby.png" }
- { name: Apple, <<: *pg, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Apple.png" }

#节点筛选
- { name: 自动选择, hidden: false, <<: *fall-back, icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Auto.png" }
- { name: 美国节点, hidden: false, <<: *fall-back, filter: "(?i)🇺🇸|美|美国|波特兰|达拉斯|俄勒冈|凤凰城|费利蒙|硅谷|拉斯维加斯|洛杉矶|圣何塞|圣克拉拉|西雅图|芝加哥|US|United States", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/United_States.png" }
- { name: 香港节点, hidden: false, <<: *fall-back, filter: "(?i)🇭🇰|港|香港|HK|HongKong|Hong Kong", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Hong_Kong.png" }
- { name: 台湾节点, hidden: false, <<: *fall-back, filter: "(?i)🇹🇼|台|台湾|TW|Tai|Taiwan", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Taiwan.png" }
- { name: 日本节点, hidden: false, <<: *fall-back, filter: "(?i)🇯🇵|日|日本|川日|东京|大阪|泉日|埼玉|JP|Japan|Osaka|Tokyo", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Japan.png" }
- { name: 韩国节点, hidden: false, <<: *fall-back, filter: "(?i)🇰🇷|韩|韩国|韓|首尔|KR|Korea", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Korea.png" }
- { name: 新加坡节点, hidden: false, <<: *fall-back, filter: "(?i)🇸🇬|新加坡|狮|SG|Singapore", icon: "https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Singapore.png" }

rules:

- RULE-SET,AI,Ai
- RULE-SET,Emby,Emby
- RULE-SET,Apple,Apple
- RULE-SET,Telegram,Telegram
- RULE-SET,YouTube,YouTube
- RULE-SET,Microsoft,Microsoft
- RULE-SET,Github,Github
- RULE-SET,ChinaDomain,DIRECT
- RULE-SET,ChinaIP,DIRECT
- RULE-SET,Lan,DIRECT
- MATCH,Final

```
