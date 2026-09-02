## TCP/IP分层
> 链路层(Driver), 网络层(IP, ICMP, IGMP), 运输层(TCP, UDP), 应用层(Telnet, FTP, e-mail)    

## 标准化
> 局域网/城域网: IEEE(institute of electrical and electronics engineers) 802系列    
> 互联网IETF(internet engineering task force): 所有Internet的正式标准都以RFC(request for comment)文档出版    
> 广域网/电信网: ITU-T(international telecommunication union - telecommunication std sector)的G系列等    

## 地址

> 网络字节序为`大端序(big endian)`    
> 网络号：同一网络号可以直接通信，不同网络号必须通过路由器    
> 子网掩码mask： a&mask == b&mask, 那么是同一网络号，网络号=IP&mask    
> 主机号: IP&(~mask)，广播地址和网段地址无主机号    
> 广播地址: IP|(~mask)    
> 网段地址: IP&mask    
> 端口: 区分具体软件, 2byte的端口号表示, 类unix在/etc/services里有熟知的    
> MAC(media access control)地址: 局域网内物理硬件识别，6byte表示    
> Internet Protocol(IPv4, IPv6): 分别为4byte和16byte表示    

> IP地址分类(按起始位)：A(0), B(10), C(110), D(1110), E(1110)    
> IP地址分类(按播数)：单播地址，多播地址，广播地址    

> IP地址分类(按功能)：    
> 0.0.0.0/8地址块，    
> 127.0.0.0/8环回地址    
> 169.254.0.0/16链路本地地址    
> 224.0.0.0/4组播地址    
> 240.0.0.0/4保留地址    
> 10.0.0.0/8,172.16.0.0/12,192.168.0.0/16局域网私有地址    
> 192.0.2.0/24, 198.51.100.0/24, 203.0.113.0/24 文档/示例地址    
> 其余大部分为公有地址    

> CIDR(classless inter-domain routing): 缩短mask以聚合地址    
> VLSM(variable length subnet mask): 变长mask以分散地址    

> DNS(Domain Name System): 保存主机名和IP映射关系    

> TCP(transmission control protocol): 三次握手(请求回复，收到回复，确认收到回复)    
> UDP(user datagram protocol): 无连接的不可靠的传输层协议    

## 封装/分用
> 封装data到以太网帧：应用程序添加首部-> TCP/UDP添加首部-> IP添加首部 -> 数据链路添加首部和尾部    
> 分用：在每个阶段逆处理首部尾部信息    

### 数据链路封装：
> FCS(frame check sequence): CRC(cyclic redundancy check)算法算出的余数校验值    
> DSAP/SSAP(destination/source service access point)    
> OUI(organizationally unique identifier)    
> LLC(logical link control): [DSAP 1B] [SSAP 1B] [control 1B]    
> SNAP(SubNetwork Access Protocol): [OUI 3B] [protocol type 2B]    
> TPID(tag protocol identifier): 0x8100    
> VLAN(virtual local area network)    
> TCI(tag control information): [PCP(priority code point) 3bit] [DEI(drop eligible indicator) 1bit] [VID(vlan id) 12bit]    
> (RFC 894) Ethernet II:       [目的MAC 6B] [源MAC 6B] [类型 2B] [数据+填充 46~1500B] [FCS 4B]    
> (RFC 1042) IEEE 802.3/802.2: [目的MAC 6B] [源MAC 6B] [长度 2B] [LLC 3B] [SNAP 5B] [数据+填充 38~1492B] [FCS 4B]    
> (IEEE 802.1Q) VLAN:          [目的MAC 6B] [源MAC 6B] [TPID=0x8100] [TCI 2B] [类型 2B] [数据+填充 46~1500B] [FCS 4B]    

> SLIP(serial line internet protocol)/CSLIP(compressed SLIP): END(0xc0/0xdbdc转义),0xdb转义为0xdbdd    
> PPP(point to point protocol): 
> - 帧结构: [Flag 0x7E][Address 0xFF][Control 0x03][Protocol 1~2B][Information 0~1500B][FCS 2~4B][Flag 0x7E]
> - Protocol字段取值: 0x0021(IP数据报), 0xC021(LCP(link control protocol)), 0x8021(NCP(network control protocol))    
> - 透明传输（防止数据中出现0x7E误判帧边界）:    
>   - 异步链路（串口/Modem）: 字节填充，0x7E转义为0x7D 0x5E，0x7D转义为0x7D 0x5D    
>   - 同步链路（SDH(synchronous digital hierarchy)/SONET(synchronous optical network)）: 零比特填充，连续5个"1"后插入"0"    

> ARP(address resolution protocol): 类型0x0806, 根据IP地址解析出MAC    
> RARP(reverse ARP): 类型0x8035,根据MAC解析出IP地址    
> 格式: [硬件类型 2B][协议类型 2B][硬件长度 1B][协议长度 1B][操作码 2B][发送方MAC 6B][发送方IP 4B][目标MAC 6B][目标IP 4B]    
> 操作码：ARP请求1/应答2, RARP请求3/应答4    
> ARP/RARP的数据需要再经过链路封装    

> MTU(maximum transmission unit): 
> PPPoE (PPP over Ethernet) — 1480-1492    
> PPTP (Point-to-Point Tunneling Protocol) — 1400    
> IPsec (Internet Protocol Security) — 1400-1460    
> WireGuard — 1420    
> OpenVPN — 1428    
> SLIP (Serial Line Internet Protocol) — 1006    
> FDDI (Fiber Distributed Data Interface) — 4352    
> X.25 — 576    
> Token Ring — 4464 (4M) / 17914 (16M)    
> Hyperchannel — 65535    
> Loopback — 65536 / 16384 (depends on OS)    
> Ethernet — 1500    
> IEEE 802.3/802.2 — 1492    

### 网络层封装:

> IP数据报（IPv4）格式（[]内为字段，括号内为比特位数，按顺序排列）：    
> [Version:4bit] — Version（版本号，IPv4 = 4）    
> [IHL:4bit] — Internet Header Length（首部长度，单位4字节，最小5，即20字节）    
> [ToS:8bit] — Type of Service（服务类型，优先级、延迟、吞吐量等）    
> [Total Length:16bit] — Total Length（总长度，首部+数据，单位字节）    
> [ID:16bit] — Identification（标识符，分片重组用）    
> [Flags:3bit] — Flags（标志位，DF=不分片，MF=更多片）    
> [Frag Offset:13bit] — Fragment Offset（片偏移，单位8字节）    
> [TTL:8bit] — Time to Live（生存时间，跳数限制）    
> [Protocol:8bit] — Protocol（上层协议号，TCP=6，UDP=17，ICMP=1）    
> [Checksum:16bit] — Header Checksum（首部校验和，仅校验首部）    
> [Src IP:32bit] — Source Address（源IP地址）    
> [Dst IP:32bit] — Destination Address（目的IP地址）    
> [Options:变长] — [type 1B][length 1B][data]（可选字段，0~40字节）    
> [Padding:变长] — Padding（填充至4字节对齐，Options不足时补0）    
> [Data:变长] — Data（上层协议数据，如TCP/UDP报文）    

> ICMP(internet control message protocol): [type 1B][code 1B][checksum 2B][message body]    
> ICMP type和code决定message body，是查询还是差错报文    
> ICMP需要再经过IP封装    

> IGMP (Internet Group Management Protocol): IP协议号 = 2，用于IPv4多播组成员管理    
> v1格式: [版本 4bit][类型 4bit][unused 4bit][checksum 16bit][组地址 32bit]    
> v2格式: [类型 1B][最大回复时间 1B][checksum 2B][组地址 4B]    
> IGMP需要再经过IP封装    

### 运输层封装:

> UDP(user datagram protocol):    
> 格式:[src_port 2B][dst_port 2B][length 2B][checksum 2B][data][padding]    

> TCP(transmission control protocol):    
> 格式:[src_port 2B][dst_port 2B][seq 4B][ack 4B][data_offset 4bit][reserved 3bit][flags 9bit][window 2B][checksum 2B][urgent_ptr 2B][options 0~40B][data]    


### 应用层封装:


## 加密
> 对称加密：双方都持有同一密钥进行通信    
> 非对称加密：(加密: 数据保密)公钥加密私钥解密，(签名: 防篡改)私钥加密公钥解密    
> CA(certificate authority): 用CA的私钥签名服务端的公钥，客户端使用CA的公钥验证服务端的公钥    


