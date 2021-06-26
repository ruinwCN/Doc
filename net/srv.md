# SRV 记录

**SRV记录**（英语：**Service Record**，中文又名**服务定位记录**）是域名系统中用于指定服务器提供服务的位置（如主机名和端口）数据。此数据于[RFC 2782](https://www.rfc-editor.org/rfc/rfc2782.txt)中定义，类型代码为33。记录服务器提供的服务，SRV记录的作用是说明一个服务器能够提供什么样的服务。

### 1.1 记录格式

服务记录的格式为：

```
_服务._协议.名称. TTL 类别 SRV 优先级 权重 端口 主机.
_Service._Proto.Name TTL Class SRV Priority Weight Port Target
```

各字段详细说明：

| 字段     | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| Service  | 所需服务的符号名称  在Assigned Numbers或本地定义。 服务标识符前面加上下划线`_`，以避免与自然界中出现的DNS标签发生冲突。 |
| Proto    | 所需协议的符号名称  前面加下划线`_`，以防止与自然界中出现的DNS标签发生冲突。 `_TCP`和`_UDP`目前是该字段最常用的值，但可以使用由Assigned Numbers或本地定义的任何名称。 Proto不区分大小写。 |
| Name     | 此RR所指的域名。在这个域名下SRV RR是唯一的。（英文原文好像有些语法问题）【此记录生效的所在域名，以半角逗号结尾。】 |
| TTL      | 标准DNS[存活时间](https://zh.wikipedia.org/wiki/存活時間)值。 |
| Class    | 标准DNS类别值（此值总为IN）。                                |
| Priority | 目标主机的优先级，值越小越优先。                             |
| Weight   | 相同优先度记录的相对权重，值越大越优先。                     |
| Port     | 服务在目标主机上的端口。所需协议的符号名称，前面加下划线（_），以防止与自然界中出现的DNS标签发生冲突。 _TCP和_UDP目前是该字段最有用的值，但可以使用由Assigned Numbers定义的任何名称或本地的（如Service）。 Proto不区分大小写。 范围是0-65535。 这是网络字节顺序中的16位无符号整数。 通常在Assigned Numbers中指定，但不一定如此。 |
| Target   | 目标主机的域名。  域名必须有一个或多个地址记录，域名绝不能是别名。敦促（但不强求）实现在附加数据部分中返回地址记录。除非并且直到将来的标准操作允许，否则名称压缩不能用于此字段。  值为“.” 表示该域名明确无法提供该服务，提供服务的规范主机名，以半角句号结尾。 |

以下是[区域文件](https://zh.wikipedia.org/w/index.php?title=区域文件&action=edit&redlink=1)中的服务记录示例：

```
_minecraft._tcp.example.com. 86400 IN SRV 0 5 25565 mc.example.com.
```

此记录指向在TCP端口25565上监听[Minecraft](https://zh.wikipedia.org/wiki/Minecraft)游戏协议的 `mc.example.com`。此处的优先级为0，权重为5。

与[MX记录](https://zh.wikipedia.org/wiki/MX记录)一样，SRV记录中的主机必须指向已有地址记录（[A](https://zh.wikipedia.org/wiki/DNS记录类型列表)或[AAAA记录](https://zh.wikipedia.org/w/index.php?title=AAAA记录&action=edit&redlink=1)）的主机名。指向带有[CNAME记录](https://zh.wikipedia.org/wiki/CNAME)的主机名则无效。

#### 1.1.1 例子说明

下面是DNS中master file，可以看到有example.com域名提供了一个名为_foobar._tcp的服务，这个服务有两个SRV记录，分别指向了sysadmins-box.example.com:9和server.example.com:9

```
  $ORIGIN example.com.
  @               SOA server.example.com. root.example.com. (
                      1995032001 3600 3600 604800 86400 )
                  NS  server.example.com.
                  NS  ns1.ip-provider.net.
                  NS  ns2.ip-provider.net.
  ; foobar - use old-slow-box or new-fast-box if either is
  ; available, make three quarters of the logins go to
  ; new-fast-box.
  _foobar._tcp     SRV 0 1 9 old-slow-box.example.com.
                   SRV 0 3 9 new-fast-box.example.com.
  ; if neither old-slow-box or new-fast-box is up, switch to
  ; using the sysdmin’s box and the server
                   SRV 1 0 9 sysadmins-box.example.com.
                   SRV 1 0 9 server.example.com.
  server           A   172.30.79.10
  old-slow-box     A   172.30.79.11
  sysadmins-box    A   172.30.79.12
  new-fast-box     A   172.30.79.13
  ; NO other services are supported
  *._tcp          SRV  0 0 0 .
  *._udp          SRV  0 0 0 .
```

### 1.2 协议应用

在wiki中有详细说明 https://zh.wikipedia.org/wiki/SRV%E8%AE%B0%E5%BD%95

目前，必须知道服务器的确切地址才能联系它或者广播问题。
SRV RR允许管理员为单个域名使用多个服务器，轻松地将服务从一个主机移动到另一个主机，并将某些主机指定为服务的主服务器，将其他主机指定为备份。
客户端要求特定域名的特定服务/协议（这里使用的词汇”域名”是RFC 1034上严格定义的），并获取任何可用服务器的名称。
请注意，在本文档引用“地址记录”时，它表示A RR，AAAA RR或其最现代的等价物。

SRV记录通常与下列[标准化](https://zh.wikipedia.org/wiki/互联网标准)[传输协议](https://zh.wikipedia.org/wiki/网络传输协议)同时使用：

- [CalDAV](https://zh.wikipedia.org/w/index.php?title=CalDAV&action=edit&redlink=1)或[CardDAV](https://zh.wikipedia.org/w/index.php?title=CardDAV&action=edit&redlink=1)
- [Ceph](https://zh.wikipedia.org/w/index.php?title=Ceph&action=edit&redlink=1)[[1\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-1)
- [DANE](https://zh.wikipedia.org/w/index.php?title=基于DNS的命名实体身份验证&action=edit&redlink=1)
- [DNS服务探索（DNS-SD）](https://zh.wikipedia.org/w/index.php?title=零配置网络&action=edit&redlink=1)
- [主机标识协议](https://zh.wikipedia.org/w/index.php?title=主机标识协议&action=edit&redlink=1)
- [Kerberos](https://zh.wikipedia.org/wiki/Kerberos)[[2\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-2)
- [LDAP](https://zh.wikipedia.org/wiki/轻型目录访问协议)[[3\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-3)
- [SMTP](https://zh.wikipedia.org/wiki/简单邮件传输协议)[邮件投递](https://zh.wikipedia.org/w/index.php?title=邮件提交代理&action=edit&redlink=1)、[POP](https://zh.wikipedia.org/wiki/郵局協定)及[IMAP](https://zh.wikipedia.org/wiki/因特网信息访问协议)[[4\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-4)
- [Matrix.org](https://zh.wikipedia.org/wiki/Matrix_(协议))[[5\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-5)
- [Minecraft](https://zh.wikipedia.org/wiki/我的世界)[[6\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-6)
- [IMPS](https://zh.wikipedia.org/w/index.php?title=OMA即时消息传递及在线状态服务&action=edit&redlink=1)[[7\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-7)
- [Puppet](https://zh.wikipedia.org/w/index.php?title=Puppet&action=edit&redlink=1)[[8\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-8)
- [会话发起协议](https://zh.wikipedia.org/wiki/会话发起协议)
- [STUN](https://zh.wikipedia.org/wiki/STUN)
- [Teamspeak 3](https://zh.wikipedia.org/wiki/TeamSpeak)[[9\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-9)[[10\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-10)
- [XMPP](https://zh.wikipedia.org/wiki/可扩展消息与存在协议)[[11\]](https://zh.wikipedia.org/wiki/SRV记录#cite_note-11)

RFC 6335中定义[互联网号码分配局](https://zh.wikipedia.org/wiki/互联网号码分配局)（IANA）来维护SRV记录及协议的服务名记录表。



>[中文wiki](https://zh.wikipedia.org/wiki/SRV%E8%AE%B0%E5%BD%95)
>
>[rfc-2782](https://www.rfc-editor.org/rfc/pdfrfc/rfc2782.txt.pdf)
>
>[rfc-135](https://www.rfc-editor.org/rfc/pdfrfc/rfc1035.txt.pdf)

