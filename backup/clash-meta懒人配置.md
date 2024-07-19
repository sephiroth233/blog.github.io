
将配置中的 **节点订阅** 替换为你自己的节点订阅链接即可
```yaml
mode: rule
ipv6: true
log-level: info
allow-lan: true
mixed-port: 7890
external-controller: 127.0.0.1:9090

geodata-mode: true
geox-url:
  geoip: https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat
  geosite: https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat
  mmdb: https://raw.githubusercontent.com/Loyalsoldier/geoip/release/Country.mmdb

dns:
  enable: true
  ipv6: true
  listen: 0.0.0.0:53
  default-nameserver:
    - 223.5.5.5 
    - 119.29.29.29
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  fake-ip-filter:
    - '*.lan'
    - localhost.ptlogin2.qq.com
    - '+.srv.nintendo.net'
    - '+.stun.playstation.net'
    - '+.msftconnecttest.com'
    - '+.msftncsi.com'
    - '+.xboxlive.com'
    - 'msftconnecttest.com'
    - 'xbox.*.microsoft.com'
    - '*.battlenet.com.cn'
    - '*.battlenet.com'
    - '*.blzstatic.cn'
    - '*.battle.net'
  nameserver:
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query




######### 锚点 start #######
# proxy策略相关
pr: &ap1 { type: select, proxies: [ 香港节点,台湾节点,日本节点,美国节点,韩国节点,新加坡节点,手动选择,自动选择,DIRECT ] }
#订阅更新相关
p: &ap2 { type: http, interval: 3600, health-check: { enable: true, url: https://www.apple.com/library/test/success.html, interval: 300 } }
#延迟测试相关
auto: &ap3 { type: url-test, lazy: true,  url: https://www.apple.com/library/test/success.html, interval: 900, use: [ Subscribe ] }
#节点选择
use: &ap4 { type: select,   use: [ Subscribe ] }
d: &ap5 { type: http,  format   behavior: domain,    interval: 86400 }


proxy-providers:
  Subscribe:
    url: "节点订阅"
    <<: *ap2
proxies: null
proxy-groups:

  #策略组
  - { name: 手动选择, <<: *ap4 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Rocket.png}
  - { name: Global,   <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Global.png}
  - { name: Github,  <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/GitHub.png}
  - { name: Spotify, <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Spotify.png}
  - { name: Openai,  <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Bot.png}
  - { name: YouTube, <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/YouTube.png}
  - { name: Emby, <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Emby.png}
  - { name: Microsoft, <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Microsoft.png}
  - { name: StreamCN, <<: *ap1 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Steam.png}


  #节点筛选
  - { name: 自动选择, <<: *ap3 ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Auto.png}
  - { name: 香港节点, <<: *ap3, filter: "(?i)🇭🇰|香港|(\b(HK|Hong)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Hong_Kong.png}
  - { name: 台湾节点, <<: *ap3, filter: "(?i)🇹🇼|台湾|(\b(TW|Tai|Taiwan)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Taiwan.png}
  - { name: 日本节点, <<: *ap3, filter: "(?i)🇯🇵|日本|川日|东京|大阪|泉日|埼玉|(\b(JP|Japan)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Japan.png}
  - { name: 美国节点, <<: *ap3, filter: "(?i)🇺🇸|美国|波特兰|达拉斯|俄勒冈|凤凰城|费利蒙|硅谷|拉斯维加斯|洛杉矶|圣何塞|圣克拉拉|西雅图|芝加哥|(\b(US|United States)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/United_States.png}
  - { name: 韩国节点, <<: *ap3, filter: "(?i)🇰🇷|韩国|韓|首尔|(\b(KR|Korea)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Korea.png}
  - { name: 新加坡节点, <<: *ap3, filter: "(?i)🇸🇬|新加坡|狮|(\b(SG|Singapore)\b)" ,icon: https://raw.githubusercontent.com/Koolson/Qure/master/IconSet/Color/Singapore.png}

rule-providers:
  ad-rules: { type: http, format: text ,behavior: domain, interval: 86400, url: https://raw.githubusercontent.com/Cats-Team/AdRules/main/adrules_domainset.txt, path: ./ruleset/ad-rules.yaml }
  emby: { type: http, format: text ,behavior: classical, interval: 86400, url: https://raw.githubusercontent.com/Repcz/Tool/X/Clash/Rules/Emby.list, path: ./ruleset/emby.yaml }
  streamCN: { type: http, format: text ,behavior: classical, interval: 86400, url: https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/rule/Clash/SteamCN/SteamCN.yaml, path: ./ruleset/streamCN.yaml }
rules:
  - RULE-SET,ad-rules,REJECT
  - RULE-SET,streamCN,StreamCN
  - RULE-SET,emby,Emby
  - GEOSITE,github,Github
  - GEOSITE,openai,Openai
  - GEOSITE,spotify,Spotify
  - GEOSITE,youtube,YouTube
  - GEOSITE,microsoft,Microsoft
  - GEOSITE,private,DIRECT
  - GEOSITE,cn,DIRECT
  - GEOIP,CN,DIRECT
  - MATCH,Global
```
