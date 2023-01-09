# 该项目已经合并至 [https://github.com/Paper-Dragon/muti_xray](https://github.com/Paper-Dragon/muti_xray)


# 本项目已经被标记为结束状态


# 对于站群服务器模拟多xray节点的解决方案

## What it can do?

> 本程序有以下功能
>
> - 本程序有自动检测ip机制，获取站群服务器所有ip，并对每个节点生成不同的协议不同的端口，真正做到一个服务器模仿无数节点
> - 本程序具有随机端口功能，每个ip所代表的节点，都能够独立运行，单个端口可以重复利用
> - 得益于xray-core项目，支持多种协议，理想条件下支持具体如下协议【末尾】，**目前仅实现ws 和tcp模式**
> - 对于ws协议，我们增加了随机host段，减少被封的机率



## How to work?

>  本程序分为三段式结构，
>
> 从语言种类上来看，第一段采用Shell语言编写，第二段采用Python语言编写，第三段采用xray项目的内核，每一段可以独立运行，欢迎fork。
>
> 从功能上来看，第一段主要是实现了人性化菜单和环境预部署功能，第二段是采用Python封装，实现对配置文件的解析和无痕切换配置，第三段功能如下https://www.v2fly.org
>
> 总而言之，每一段都可以独立运行，具有完整的低耦合机制。



## How to run it？

### 基本用法

#### clone code

```bash
git clone https://github.com/Paper-Dragon/xray-muti-ip-station-cluster.git
```

#### Install

```bash
cd xray-muti-ip-station-cluster && bash install.sh install
```

#### Delete

```bash
cd xray-muti-ip-station-cluster && bash install.sh uninstall
```



### Menu
```bash
(PyCharmProject) [root@monther PyCharmProject]# bash install.sh 
         xray站群服务器 安装管理脚本 [0.0.0.1]
           仅适用与站群服务器 乱用后果自负 
        ---authored by PaperDragon---
        https://github.com/Paper-Dragon

当前已安装版本: 0.0.0.1

—————————————— 安装向导 ——————————————
0.  升级 脚本
1.  安装 V2Ray (ws)
2.  安装 V2Ray (http/2)
3.  升级 V2Ray core
—————————————— 配置变更 ——————————————
4.  变更 传输层协议
6.  变更 port
—————————————— 查看信息 ——————————————
8.  查看 实时访问日志
9.  查看 实时错误日志
10. 查看 V2Ray 配置信息
—————————————— 其他选项 ——————————————
14. 卸载 V2Ray
17. 退出 

请输入数字：^C




```




### 扩展用法

#### main menu

```bash
(PyCharmProject) [root@monther PyCharmProject]# python configFactory.py  --help
usage: configFactory.py [-h] [--list] {install,modify} ...

Mutilation IP Cluster Server Management Script

positional arguments:
  {install,modify}  choose into sub menu
    install         Full Install
    modify          Edit the name of a node

optional arguments:
  -h, --help        show this help message and exit
  --list, -L        list all nodes in this Cluster server

```

#### Install

```bash
(PyCharmProject) [root@monther PyCharmProject]# python configFactory.py install  --help
usage: configFactory.py install [-h] [--name NAME] [--mode MODE]

optional arguments:
  -h, --help   show this help message and exit
  --name NAME  Prefix name of the generated node
  --mode MODE  Transport Layer Protocol


```

#### modify

```bash
(PyCharmProject) [root@monther PyCharmProject]# python configFactory.py modify --help
usage: configFactory.py modify [-h] [--name NAME] [--port PORT]
                               [--network NETWORK] [--path PATH]

optional arguments:
  -h, --help         show this help message and exit
  --name NAME        NodeName
  --port PORT        Port
  --network NETWORK  Network
  --path PATH        path


```

### 高级用法

#### 基本组件

```bash
#config.json

{
    "log": {},
    "api": {},
    "dns": {},
    "routing": {},
    "policy": {},
    "inbounds": [],
    "outbounds": [],
    "transport": {},
    "stats": {},
    "reverse": {},
    "fakedns": [],
    "browserForwarder": {},
    "observatory": {}
}
```

#### inbound

```json
{
  "protocol":"vmess",
  "settings":{},
  "port":"",
  "listen":"",
  "tag":"",
  "sniffing":{},
  "streamSettings":{}
}

```



`protocol`: name of `[inbound]`

入站协议名称。

`settings`: settings of `[inbound]`

入站协议设置。

`port`: string

接受的格式如下:

- 整型数值：实际的端口号。
- 字符串：可以是一个数值类型的字符串，如 `"1234"`；或者一个数值范围，如 `"5-10"` 表示端口 5 到端口 10，这 6 个端口。

`listen`: string

监听地址，只允许 IP 地址，默认值为 `"0.0.0.0"`，表示接收所有网卡上的连接。除此之外，必须指定一个现有网卡的地址。

v4.32.0+，支持填写 Unix domain socket，格式为绝对路径，形如 `"/dev/shm/domain.socket"`，可在开头加 `"@"` 代表 [abstract](https://www.man7.org/linux/man-pages/man7/unix.7.html)

，`"@@"` 则代表带 padding 的 abstract。

填写 Unix domain socket 时，`port` 将被忽略，协议暂时可选 VLESS、VMess、Trojan，传输方式可选 TCP、WebSocket、HTTP/2。

`tag`: string

此入站连接的标识，用于在其它的配置中定位此连接。当其不为空时，其值必须在所有 `tag` 中唯一。

`sniffing`: 

入站连接的流量探测设置。流量探测允许路由根据连接的内容和元数据转发连接。

`streamSettings`: 

底层传输配置。



#####  支持的代理协议

- [SOCKS]
- [VMess]
- [VLite]
- [Shadowsocks]
- [HTTP]
- [Dokodemo]
- [Trojan]
- [VLESS]



#####  SniffingObject

`enabled`: true | false

是否开启流量探测。

`destOverride`: ["http" | "tls" | "quic" | "fakedns" | "fakedns+others"]

当流量为指定类型时，按其中包括的目标地址重置当前连接的目标。

`fakedns+others` 选项会优先进行 FakeDNS 虚拟 DNS 服务器匹配。如果 IP 地址处于虚拟 DNS 服务器的 IP 地址区间内，但是没有找到相应的域名记录时，使用 `http`、`tls` 的匹配结果。此选项仅在 `metadataOnly` 为 `false` 时有效。

`metadataOnly`: true | false

是否仅使用元数据推断目标地址而不截取流量内容。只有元数据流量目标侦测模块会被激活。

如果关闭仅使用元数据推断目标地址，客户端必须先发送数据，代理服务器才会实际建立连接。此行为与需要服务器首先发起第一个消息的协议如 SMTP 协议不兼容。





## 支持的协议

### V2ray

| 协议      | 支持情况                                             |
| --------- | ---------------------------------------------------- |
| VMess     | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |
| VMessAEAD | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |
| VLess     | tcp, tcp+tls/xtls, ws, ws+tls/xtls, h2c, h2+tls/xtls |
| VLite     | √                                                    |



### Trojan

| 协议   | 支持情况 |
| ------ | -------- |
| Trojan | √        |

### Shadowsocks

| 协议            | 支持情况 | 加密方法                                    |
| --------------- | -------- | ------------------------------------------- |
| ShadowsocksAEAD | √        | aes-128-gcm, aes-256-gcm, chacha20-poly1305 |

### Socket

| 协议   | 支持情况 |
| ------ | -------- |
| Socket | √        |

