# 双VPN代理冲突排障

## 记录时间

- 2026-03-04 14:20:06 CST

## 原始背景

Ubuntu 22.04 同时开启内网 VPN 与外网 VPN 时，CLI `git push` 可用，但浏览器访问内网站点失败。

## 原始事实

以下域名与地址已做匿名化替换，仅保留问题结构：

- 域名：
  - `git.corp.example`
  - `wiki.corp.example`
- 地址：
  - `git.corp.example -> 10.10.20.132`
  - `wiki.corp.example -> 10.10.20.110`
- 代理：`127.0.0.1:7890`
- 直连返回：`HTTP/1.1 302`
- 代理返回：`HTTP/1.1 502 Bad Gateway`

## 原始修复动作

- 系统代理绕过加入：
  - `.corp.example`
  - `git.corp.example`
  - `wiki.corp.example`
- Clash 规则加入：
  - `DOMAIN-SUFFIX,corp.example,DIRECT`
  - `IP-CIDR,10.10.0.0/16,DIRECT,no-resolve`

## 原始命令

```bash
getent hosts git.corp.example
ip route get 10.10.20.132
env -u http_proxy -u https_proxy -u all_proxy -u HTTP_PROXY -u HTTPS_PROXY -u ALL_PROXY \
curl -I --max-time 10 http://wiki.corp.example
curl -I --max-time 10 -x http://127.0.0.1:7890 http://wiki.corp.example
```
