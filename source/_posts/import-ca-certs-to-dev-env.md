---
title: 将CA证书导入各种开发环境
date: 2018-08-22 15:14:20
categories:
- 技术
tags:
---
由于安全原因，公司内网经常会使用一些Proxy(例如ForcePoint)进行网络内容的过滤，没有在whitelist的https请求都会被代理拦截并被替换证书。这会导致一些开发工具下https的请求都会出现问题。

另外一些测试服务器的证书采用自签名的根证书，这些测试服务器也需要在开发环境下能够被信任访问。

为了避免开发人员由于证书问题导致的各种开发不顺畅，我们需要将这代理的CA证书和测试服务器的CA两个根证书加到各种开发工具中去。

## Babun(Cygwin)

将两个证书拷贝到babun的/etc/pki/ca-trust/source/anchors/目录下去。
然后运行
```bash
update-ca-trust
```
执行完毕以后，可以通过curl测试一下https的网站或者测试服务器，都可以不用加-k参数了。

## JDK

JDK的信任证书库在$JAVA_HOME\jre\lib\security\cacerts里面，默认密码为changeit。

可以通过[Protecle](http://portecle.sourceforge.net/)或者Keytools将两个证书导入到cacerts里面。

## IDEA
IDEA有可能采用自带的JRE运行，而非系统安装的JDK。信任证书库在$IDEA_INSTALL_PATH\jre64\lib\security。

可以通过[Protecle](http://portecle.sourceforge.net/)或者Keytools将两个证书导入到cacerts里面。

## Node.js
Node.js默认的证书列表是编译到二进制执行文件里面，不过可以通过设置环境变量来添加新认证书。
[NODE_EXTRA_CA_CERTS=file](https://nodejs.org/api/cli.html#cli_node_extra_ca_certs_file)。

或者直接通过NODE_TLS_REJECT_UNAUTHORIZED忽略掉SSL证书的校验。

### iPhone/iPad

IOS只能通过自带的邮件程序附件程序导入ca.crt。可以发送邮件给iphone用户，然后在iphone中打开邮件附件，按照提示完成安装。

### Andoird

通过各种手段将ca.crt拷贝或者传输到android设备。然后进入设置->系统安全->加密与凭证里面选择从存储设备安装。