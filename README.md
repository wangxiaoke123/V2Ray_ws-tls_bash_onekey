# V2Ray 基于 Nginx 的 vmess+ws+tls 一键安装脚本 （Use Path）
# 另一种类似V2的方法，比较快，智能分流，具体项目https://github.com/atrandys/trojan

代码curl -O https://raw.githubusercontent.com/atrandys/trojan/master/trojan_install.sh && chmod +x trojan_install.sh && ./trojan_install.sh

# 80端口开启方法
vi /etc/sysconfig/iptables

#允许80端口通过防火墙，加在COMMIT之前

A INPUT -m state –state NEW -m tcp -p tcp –dport 80 -j ACCEPT

#保存退出后，重启防火墙

service iptables restart

#或者 /etc/init.d/iptables restart

#直接command更改(原理也是同1)
#查看防火墙状态

service iptables status

#开启80端口

iptables -I INPUT -p tcp --dport 80 -j ACCEPT 

#保存，可能我之前就是没有保存，导致配置无效。。。

service iptables save 
### 2019-09-19
脚本功能性测试，已通过,目前该脚本依然可正常工作
时间同步请保证服务器当前时间误差不要过大（24h以上）

## 目前支持Debian 8+ / Ubuntu 16.04+ / Centos7
## 如果你选择使用 V2ray，强烈建议你关闭并删除所有的 shadowsocksR 服务端，仅使用标准的 V2ray 三件套（原因请查看 Wiki ）
* 本脚本默认安装最新版本的V2ray core
* V2ray core 目前最新版本为 3.33（同时请注意客户端 core 的同步更新，需要保证客户端内核版本 >= 服务端内核版本）
* 由于新版本增加了 web 伪装，因此强烈建议使用默认的443端口作为连接端口
* 
* **感谢作者 dunizb 的自用 开源 html 计算器源码 项目地址 https://github.com/dunizb/sCalc**
## V2ray core 更新方式
执行：
`bash <(curl -L -s https://install.direct/go.sh)`

（ 来源参考 ：[V2ray官方说明](https://www.v2ray.com/chapter_00/install.html)）
* 如果为最新版本，会输出提示并停止安装。否则会自动更新
* 未来会将相关内容集成到本脚本中并进行交互式操作更新

## 注意事项
* 推荐在纯净环境下使用本脚本，如果你是新手，请不要使用Centos系统。
* 在尝试本脚本确实可用之前，请不要将本程序应用于生产环境中。
* 该程序依赖 Nginx 实现相关功能，请使用 [LNMP](https://lnmp.org) 或其他类似携带 Nginx 脚本安装过 Nginx 的用户特别留意，使用本脚本可能会导致无法预知的错误（未测试，若存在，后续版本可能会处理本问题）。
* V2Ray 的部分功能依赖于系统时间，请确保您使用V2RAY程序的系统 UTC 时间误差在三分钟之内，时区无关。
* 本 bash 依赖于 [V2ray 官方安装脚本](https://install.direct/go.sh) 及 [acme.sh](https://github.com/Neilpang/acme.sh) 工作。
* Centos 系统用户请预先在防火墙中放行程序相关端口（默认：80，443）
## 准备工作
* 准备一个域名，并将A记录添加好。
* [V2ray官方说明](https://www.v2ray.com/)，了解 TLS WebSocket 及 V2ray 相关信息
* 安装好 curl
## 安装方式（不兼容，二选一）
Vmess+websocket+TLS+Nginx+Website
```
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install.sh) | tee v2ray_ins.log
```
Vmess + HTTP2 over TLS
```
bash <(curl -L -s https://raw.githubusercontent.com/wulabing/V2Ray_ws-tls_bash_onekey/master/install_h2.sh) | tee v2ray_ins_h2.log
```
## 启动方式

启动 V2ray：`systemctl start v2ray`

启动 Nginx：`systemctl start nginx`

（其他的应该不用我多说了吧 嘿嘿嘿）


### 测试说明
* V3.1 版本在 Debian 8 / Debian 9 / Ubuntu 16.04 / Centos 7(防火墙着实又坑了我一把) 上进行过测试。
* V3.3 版本在 Ubuntu 16.04 下进行过并通过测试。
* 请携带 v2ray_ins.log 文件内容进行反馈
### 更新说明
## 2018-04-10
* vmess+http2 over tls 脚本更新
## 2018-04-08
v3.3.1（Beta）
* 安装依赖小幅调整
* Readme内容调整
