# stunnel 使用 demo

[官网地址](https://www.stunnel.org/)

## 启动 stunnel

```
stunnel stunnel.conf
```

## 例子

在 stunnel 的服务端有一个 redis 运行在 `192.168.6.38:6379`。

然后我想在 stunnel 的客户端的 `127.0.0.1:6378` 地址上访问这个 redis。

### server 配置

```
[myredis-server]
client = no
cert = stunnel.pem
connect = 192.168.6.38:6379
accept = 0.0.0.0:19999
```

`myredis-server` 是自己随便写的。

`client = no` 表示这是个服务器

`connect = 192.168.6.38:6379` 表示 stunnel 服务端会去连接这个服务。

`accept = 0.0.0.0:19999` 是 stunnel 服务端监听的地址，用来等待客户端连接。

### client 配置

```
[myredis-client]
client = yes
cert = stunnel.pem
accept = 127.0.0.1:6378
connect = 192.168.6.38:19999
```

`myredis-client` 是自己随便写的

`client = yes` 表示这是个客户端

`connect = 192.168.6.38:19999` 表示 stunnel 客户端会去连接的 stunnel 服务端地址。

`accept = 127.0.0.1:6378` 这个就是 stunnel 客户端监听的地址，相当于 stunnel
服务端上的 redis 服务。

## 生成证书

```
openssl req -new -x509 -days 3650 -nodes -out stunnel.pem -keyout stunnel.pem
```

-days 3650

使这个证书的有效期是10年，之后它将不能再用

-new

创建一个新的证书

-x509

创建一个 X509 证书（自己签名的）

-nodes

这个证书没有密码

-out stunnel.pem

把 SSL 证书写到哪里

-keyout stunnel.pem

把 SSL 证书放到这个文件中


