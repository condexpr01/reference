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


## 数据
> TLV(type-length-value)    
> BER(basic encoding ruls): [type 1B][length 1B][data]    
> DER(distinguished encoding ruls): [type 1B][length nB][data]    
> DER的length在最高位为0时1B，最高位为1时用低7位描述的长度    

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
> [Protocol:8bit] — Protocol（上层协议号，TCP=6, UDP=17, OSPF=89, ICMP=1）    
> [Checksum:16bit] — Header Checksum（首部校验和，仅校验首部）    
> [Src IP:32bit] — Source Address（源IP地址）    
> [Dst IP:32bit] — Destination Address（目的IP地址）    
> [Options:变长] — [type 1B][length 1B][data]（可选字段，0~40字节）    
> [Padding:变长] — Padding（填充至4字节对齐，Options不足时补0）    
> [Data:变长] — Data（上层协议数据，如TCP/UDP报文）    


> NAT(network address traslation): 对于IPv4同时更改ip:port，IPv6不需要NAT    

### 运输层封装:

> ICMP(internet control message protocol): [type 1B][code 1B][checksum 2B][message body]    
> ICMP type和code决定message body，是查询还是差错报文    
> ICMP需要再经过IP封装    


> IGMP (Internet Group Management Protocol): IP协议号 = 2，用于IPv4多播组成员管理    
> v1格式: [版本 4bit][类型 4bit][unused 4bit][checksum 16bit][组地址 32bit]    
>      
> v2格式: [类型 1B][最大回复时间 1B][checksum 2B][组地址 4B]    
> 类型: 0x11(成员查询), 0x16(成员报告,加入组和回复成员查询保活), 0x17(离开组)    
>      
> v3格式: [类型=0x11 1B][最大回复时间 1B][checksum 2B][保留 4B][鲁棒 4bit][查询间隔 4bit][保留 1B][组地址 4B]    
> v3格式: [类型=0x22 1B][保留 1B][checksum 2B][保留 4B][记录数 2B][组记录 12nB]    
> 组记录: [记录类型 1B][辅助数据长度 1B][源数量 2B][组播地址 4B][源 4nB][辅助数据]    
> 类型: 0x11(成员查询), 0x22(成员报告)    
>      
> IGMP需要再经过IP封装    


> OSPF(open shortest path first)    
> 格式: [版本 1B][类型 1B][包长度 2B][发送方路由器ID 4B][区域ID 4B][校验和 2B][认证类型 2B][认证数据 8B][报文数据]
>     
> OSPF版本字段: 2(OSPFv2 IPv4), 3(OSPFv3 IPv6)    
> OSPF报文数据类型字段: 1=hello, 2=DD, 3=LSR, 4=LSU, 5=LSA    
> OSPF包长度字段: OSPF帧长    
> OSPF认证类型字段: 0=无认证,1=明文,2=MD5    
> OSPF认证数据字段: 明文密码或MD5摘要    
>     
> 1Hello: [网络掩码 4B][Hello间隔 2B][选项 1B][优先级 1B][指定路由器 4B][备份指定路由器 4B][邻居路由器ID 4B×n]    
> 2DD(database desc): [接口MTU 2B][选项 1B][I/M/MS标志][DD序列号 4B][LSA头部列表 20B*n]    
> 3LSR(link state request): [LS类型 4B][链路状态ID 4B][通告路由器 4B]    
> 4LSU(link state update): [LSA数量 4B][LSA列表]    
> 5LSA(link state acknowledgment): [LS年龄(秒) 2B][选项 1B][LS类型 1B][链路状态ID 4B][通告路由器 4B][LS序列号 4B][LS校验和 2B][长度 2B][LS报文]    
> 通告路由器: 产生此LSA的路由器    
>     
> 1=Router LSA: [标志位 1B][链路数量 2B][链路列表]    
> 链路列表: [链路ID 4B][链路数据 4B][类型 1B][度量数 1B][TOS 1B][度量 2B]    
> 2=Network LSA: [网络掩码 4B][连接路由器列表 4B×n]    
> 3,4=Summary LSA: [网络掩码 4B][度量 4B]    
> 5=External LSA: [网络掩码 4B][度量 4B][转发地址 4B][外部路由标记 4B]    
>     
> cost: 参考带宽100M/接口带宽    
> 工作: 发送hello建立邻居, DD报文(交换LSA)->(发现缺失LSA时:LSR->LSU->LSA->更新LSDB), Dijkstra计算SPF, 更新路由表    


> UDP(user datagram protocol):    
> 格式:[src_port 2B][dst_port 2B][length 2B][checksum 2B][data][padding]    


> TCP(transmission control protocol):    
> 格式:[src_port 2B][dst_port 2B][seq 4B][ack 4B][data_offset 4bit][reserved 3bit][flags 9bit][window 2B][checksum 2B][urgent_ptr 2B][options 0~40B][data]    
> seq: 序列号    
> ack: 确认号    
> data_offset: 单位4字节，头部长度表示的数据偏移    
> flags: [NS 1bit][CWR 1bit][ECE 1bit][URG 1bit][ACK 1bit][PSH 1bit][RST 1bit][SYN 1bit][FIN 1bit]    
> window: 窗口大小    
> urgent_ptr: URG=1时指向紧急数据的末尾位置    
> options: 可选项    
> ECN:explicit congestion notification, NS:nonce sum, CWR:congestion window reduced, ECE:ECN-echo,    
> URG:urgent, ACK:acknowledgment, PSH:push, RST:reset, SYN:synchronize, FIN:finish    
>     
> 工作: S(server), C(client)    
> 建立连接(三次握手到established): C:SYN, S:SYN+ACK, C:ACK    
> 断开连接(四次挥手): C:FIN, S:ACK, S:FIN, C:ACK, 或S:FIN, C:ACK, C:FIN, S:ACK    
> 传输数据(停止等待, 累计确认, 滑动窗口, 拥塞窗口, 超时重传): S:[seq_n, seq_nx], C:ack_n    
> nagle(小包传送): 当没有未确认已发数据或收到ACK清空已发数据，就直接发，否则攒够MSS(max segment size)后再发    


### 应用层封装:

> RIP(routing information protocol)基于udp 520端口    
> RIPv2: [命令 1B][版本 1B][路由域 2B][不超过24个路由条目]    
> 路由条目: [地址类 2B][路由标记 2B][目的地址][掩码][下一跳][度量]    
>     
> RIPv2命令字段: 1请求,2响应    
> RIPv2版本字段: 1(v1),2(v2)    
> RIPv2路由域字段: RIP进程的逻辑编号    
> RIPv2地址类字段: 0(完整路由表交换), 2(IPv4), 0xffff(认证条目)    
> RIPv2路由标记字段: 自定义的如用于AS号等    
> RIPv2度量字段: 到达目标的跳数, 16表示不可达    
> 工作: RIP通过定期广播rip请求, 更新更小的度量的值的路由项    


> BGP(border gateway protocol)基于tcp 179端口    
> BGP: [Marker 16B][Length 2B][Type 1B][报文内容]    
>     
> OPEN（类型1）:[版本 1B][AS号 2B][保持时间 2B][BGP ID 4B][可选参数长度 1B][可选参数]    
> UPDATE（类型2）:[撤销路由长度 2B][撤销路由][路径属性长度 2B][路径属性][可达路由]    
> 撤销路由:[前缀长度 1B][IP前缀 变长] × n    
> 路径属性:[属性类型 2B][属性长度 1/2B][属性值 变长] × n    
> 可达路由:[前缀长度 1B][IP前缀 变长] × n    
> NOTIFICATION（类型3）:[错误码 1B][错误子码 1B][错误数据]    
> KEEPALIVE（类型4）:无数据字段    
> ROUTE-REFRESH（类型5）:[地址族 2B][保留 1B]    
>     
> 工作: 交换OPEN报文建立邻居->发送UPDATE路由通告更新路由表    
> KEEPALIVE保持连接, NOTIFICATION报告错误, ROUTE_REFRESH报文请求刷新路由信息    


> DNS(domain name system): 域名是树状的,用`.`分隔标签,标签最长63,总长最长255,从右到左表示树根到节点层级     
> FQDN(full qualified domain name): 包含末点表示根域的域名    
> [ID 2B][Flags 2B][QDCOUNT 2B][ANCOUNT 2B][NSCOUNT 2B][ARCOUNT 2B][Question][Answer][Authority][Additional]    
> - ID：标识符，用于匹配请求和响应    
> - Flags：标志位，包含QR、Opcode、AA、TC、RD、RA、Z、Rcode    
>> - QR：查询/响应标志（0查询，1响应）    
>> - Opcode：操作码（0标准查询，1反向查询，2服务器状态）    
>> - AA：权威回答标志    
>> - TC：截断标志    
>> - RD：期望递归标志    
>> - RA：可用递归标志    
>> - Z：保留位    
>> - Rcode：响应码（0成功，1格式错误，2服务器错误等）    
> - QDCOUNT：问题数    
> - ANCOUNT：回答数    
> - NSCOUNT：权威记录数    
> - ARCOUNT：附加记录数    
> - Question：查询部分，包含查询名、查询类型、查询类    
> - Answer：回答部分，包含资源记录    
> - Authority：权威部分，包含权威服务器记录    
> - Additional：附加部分，包含额外信息    
> ---    
> question字段：[QNAME variable][QTYPE 2B][QCLASS 2B]    
> answer,auth,addition字段：[NAME variable][TYPE 2B][CLASS 2B][TTL 4B][RDLENGTH 2B][RDATA variable]    
> TTL表示缓存中生存时间(秒), 避免频繁查询    
> - NAME由标签长度和标签组成到0时表示根标签    
> - CLASS: 表示DNS所属的类别或协议族，通常是IN    
> - TYPE:    
>> A(addr 32)：IPv4地址    
>> AAAA(addr 32*4=128)：IPv6地址    
>> MX(mail exchanger)：邮件交换器    
>> CNAME(canonical name)：别名    
>> NS(name server)：名称服务器    
>> TXT：文本记录    
>> SOA(start of authority)：起始授权机构    
>> PTR(PoinTeR)：指针记录    
>> SRV(SeRVice)：服务定位    
>> ANY：所有记录    
> ---    
> 解析n级域名:    
> (根服务器: 返回1级域名服务器NS), 没有glue A时,本地再递归解析NS获取A    
> (1级域名服务器: 返回2级域名服务器NS), 没有glue A时, 本地再递归解析NS获取A,    
> ...    
> (n-1级域名服务器: 返回n级域名对应A记录)    


> TFTP(trivial file transfer protocol): UDP/69控制，但是数据不从69端口走    
> RRQ/WRQ（读/写请求，Opcode=1/2, 模式=netascii,octet,mail）    
> [Opcode][文件名][0][模式][0]    
> DATA（数据，Opcode=3）    
> [Opcode][块号][数据...]    
> ACK（确认，Opcode=4）    
> [Opcode][块号]    
> ERROR（错误，Opcode=5，错误码1=未找到文件，2=访问违规，3=磁盘满，4=非法操作，5=未知传输 ID，6=文件已存在，7=无此用户）    
> [Opcode][错误码][错误信息][0]    
> OACK（选项确认，Opcode=6，RFC 2347）    
> [Opcode][选项1][0][值1][0]...    


> BOOTP 报文格式（固定 236 字节头 + 64 字节 vend 区）：客户端udp/68,服务器udp/67    
> [op 1B]    :请求=1, 回复=2    
> [htype 1B] :硬件类型, 以太网=1    
> [hlen 1B]  :硬件地址长度, 以太网=6    
> [hops 1B]  :中继跳数, 客户端=0    
> [xid 4B]   :事务ID, 客户端随机, 服务器原样返回    
> [secs 2B]  :启动后经过的秒数    
> [flags 2B] :bit0=1 要求广播回复    
> [ciaddr 4B]:客户端已知IP    
> [yiaddr 4B]:服务器分配的客户端IP    
> [siaddr 4B]:引导服务器IP    
> [giaddr 4B]:中继代理IP    
> [chaddr 16B]:客户端MAC    
> [sname 64B]:服务器名    
> [file 128B]:引导文件名    
> [vend 64B] :可选项区: [magic cookie 4B][选项...]    


> SNMP里的每个字段用BER TLV表示, 请求响应在UDP/161, Trap和Inform:UDP/162    
> SNMP(simple network management protocl): [version type:02][community type:04][PDU type:A0-A8]    
> PDU(protocl data type):    
> A0 GetRequest           :[request-id][error-status][error-index][variable-bindings]    
> A1 GetNextRequest       :[request-id][error-status][error-index][variable-bindings]    
> A2 Response             :[request-id][error-status][error-index][variable-bindings]    
> A3 SetRequest           :[request-id][error-status][error-index][variable-bindings]    
> A4 v1 Trap(仅v1)        :[enterprise][agent-addr][generic-trap][specific-trap][time-stamp][variable-bindings]    
> A5 GetBulkRequest(v2c+) :[request-id][non-repeaters][max-repetitions][variable-bindings]    
> A6 InformRequest(v2c+)  :[request-id][variable-bindings]    
> A7 SNMPv2-Trap(v2c+)    :[request-id][variable-bindings]    
> A8 Report(仅v3)         :[request-id][variable-bindings]    
> 
> type comment:    
> [request-id Type=02]    
> [error-status Type=02]    
> [error-index Type=02]    
> [variable-bindings Type=30]    
> [non-repeaters Type=02]    
> [max-repetitions Type=02]    
> [enterprise Type=06]    
> [agent-addr Type=40]    
> [generic-trap Type=02]    
> [specific-trap Type=02]    
> [time-stamp Type=43]    
>    
> 02 INTEGER    
> 03 BIT STRING    
> 04 OCTET STRING    
> 05 NULL    
> 06 OBJECT IDENTIFIER    
> 30 SEQUENCE    
> 40 IpAddress    
> 41 Counter32    
> 42 Gauge32 / Unsigned32    
> 43 TimeTicks    
> 44 Opaque    
> 46 Counter64    
> 80 noSuchObject    
> 81 noSuchInstance    
> 82 endOfMibView    



> FTP(file transfer protocol): TCP/21控制, TCP/20主动模式服务器数据连接    
> SMTP(simple mail transfer protocol): TCP/25默认(服务器间), 587(客户端提交,STARTTLS), 465(SSL), 单连接    
> POP3(post office protocol v3): TCP/110(明文), 995(SSL), 单连接    
> 命令: [4个大写ASCII][参数][\r\n]    
> FTP/SMTP响应: [3位数字][文本][\r\n]    
> POP3响应: [+OK/-ERR][文本][\r\n]    
>     
> ######FTP command######
>  USER:USER NAME,args:用户名    
>  PASS:PASSWORD,args:密码    
>  ACCT:ACCOUNT,args:账户信息    
>  CWD:CHANGE WORKING DIRECTORY,args:目录路径    
>  XCWD:CHANGE WORKING DIRECTORY (compat),args:目录路径    
>  CDUP:CHANGE TO PARENT DIRECTORY
>  XCUP:CHANGE TO PARENT DIRECTORY (compat)
>  PWD:PRINT WORKING DIRECTORY
>  XPWD:PRINT WORKING DIRECTORY (compat)
>  MKD:MAKE DIRECTORY,args:目录名    
>  XMKD:MAKE DIRECTORY (compat),args:目录名    
>  RMD:REMOVE DIRECTORY,args:目录名    
>  XRMD:REMOVE DIRECTORY (compat),args:目录名    
>  LIST:LIST,args:路径(可选)    
>  NLST:NAME LIST,args:路径(可选)    
>  RETR:RETRIEVE,args:远端文件名    
>  STOR:STORE,args:远端文件名    
>  STOU:STORE UNIQUE
>  APPE:APPEND,args:远端文件名    
>  DELE:DELETE,args:远端文件名    
>  RNFR:RENAME FROM,args:源文件名    
>  RNTO:RENAME TO,args:目标文件名    
>  SITE:SITE PARAMETERS,args:服务器专用命令    
>  SYST:SYSTEM
>  STAT:STATUS,args:路径/文件(可选)    
>  HELP:HELP,args:命令名(可选)    
>  NOOP:NO OPERATION
>  REST:RESTART,args:偏移字节数    
>  ALLO:ALLOCATE,args:字节数(多数预留)    
>  PORT:PORT,args:h1,h2,h3,h4,p1,p2(h1.h2.h3.h4:p1*256+p2)    
>  PASV:PASSIVE
>  EPSV:EXTENDED PASSIVE,args:协议号(可选)    
>  EPRT:EXTENDED PORT,args:|af|addr|port|    
>  LPRT:LONG PORT,args:af,len,addr,len,port
>  LPSV:LONG PASSIVE    
>  TYPE:TYPE,args:A=ASCII,I=Image / E=EBCDIC,可分带I    
>  STRU:STRUCTURE,args:F=File / R=Record / P=Page    
>  MODE:MODE,args:S=Stream / B=Block / C=Compressed    
>  ABOR:ABORT
>  SMNT:STRUCTURE MOUNT,args:路径    
>  STPC:STORE PATH CONTROL,args:路径    
>  REIN:REINITIALIZE    
>  QUIT:QUIT    
>     
> ######SMTP command######
> HELO:HELLO,args:域名    
> EHLO:EXTENDED HELLO,args:域名    
> MAIL FROM:MAIL FROM,args:<发件人地址>    
> RCPT TO:RCPT TO,args:<收件人地址>    
> DATA:DATA    
> RSET:RESET    
> VRFY:VERIFY,args:用户/邮箱地址    
> EXPN:EXPAND,args:邮件列表名    
> HELP:HELP,args:命令名(可选)    
> NOOP:NO OPERATION    
> QUIT:QUIT    
> SEND FROM:SEND FROM,args:<地址>    
> SOML FROM:SEND OR MAIL,args:<地址>    
> SAML FROM:SEND AND MAIL,args:<地址>    
> TURN:TURN    
> SIZE:SIZE,args:字节数(MAIL FROM扩展参数)    
> AUTH:AUTHENTICATE,args:LOGIN/PLAIN/CRAM-MD5    
> STARTTLS:START TLS    
> BDAT:BINARY DATA,args:块长度    
> DSN:DELIVERY STATUS NOTIFICATION    
> PIPELINING:PIPELINING    
> 8BITMIME:8-BIT MIME    
> SMTPUTF8:SMTP UTF-8    
> ENHANCEDSTATUSCODES:ENHANCED STATUS CODES    
> CHUNKING:CHUNKING    
>     
> ######POP3 command######
> USER:USER,args:用户名    
> PASS:PASS,args:密码    
> APOP:AUTHENTICATED POP,args:邮箱名 时间戳MD5摘要    
> STAT:STATUS    
> LIST:LIST,args:邮件编号(可选)    
> RETR:RETRIEVE,args:邮件编号    
> DELE:DELETE,args:邮件编号    
> TOP:TOP,args:邮件编号 行数    
> UIDL:UNIQUE IDENTIFIER LISTING,args:邮件编号(可选)    
> NOOP:NO OPERATION    
> RSET:RESET    
> QUIT:QUIT    
> CAPA:CAPABILITIES    
> STLS:START TLS    


> HTTP(hyper text transfer protocol): TCP/80    
> 帧: [起始行/状态行][若干头][空行][体]    
> 起始行: [方法][空格][URL][空格][HTTP版本][CRLF]    
> 状态行: [HTTP版本][空格][状态码][空格][原因短语][CRLF]    
> 头格式: [字段名]:[空格][字段值][CRLF]    
> URL(uniform resource locator): [协议]://[主机][:端口][/路径][?查询串][#锚点]    
> URN(uniform resource name): urn:[命名空间]:[特定名字]    
> URI(uniform resource id): URL, URN等的统称    
>     
> 起始行方法:    
> GET:读取资源(只读, 不改变服务器)    
> HEAD:读取资源但只返回响应头, 不返回响应体    
> POST:提交数据, 在服务器上创建资源(登录/发帖/下单)    
> PUT:上传/整体替换一个资源    
> PATCH:部分修改资源    
> DELETE:删除资源    
> OPTIONS:询问服务器支持哪些方法(CORS预检)    
> TRACE:回显请求(调试用, 现代禁用)    
> CONNECT:建立隧道(HTTPS代理)    
> PROPFIND:查看资源属性和信息(WebDAV)    
> LOCK:锁定资源, 防止他人改(WebDAV)    
> UNLOCK:解锁资源(WebDAV)    
> MOVE:移动/重命名资源(WebDAV)    
> COPY:复制资源(WebDAV)    
>     
> 状态行状态码:    
> 1xx: 信息性    
> 100: Continue 继续发送    
> 101: Switching Protocols 切换协议    
> 102: Processing 处理中    
> 2xx: 成功    
> 200: OK 成功    
> 201: Created 已创建    
> 202: Accepted 已接受(处理中)    
> 204: No Content 成功但无内容    
> 206: Partial Content 部分内容(断点续传)    
> 3xx: 重定向    
> 301: Moved Permanently 永久移动    
> 302: Found 临时移动    
> 303: See Other 请用GET重新访问    
> 304: Not Modified 未修改(走缓存)    
> 307: Temporary Redirect 临时重定向    
> 308: Permanent Redirect 永久重定向    
> 4xx: 客户端错误    
> 400: Bad Request 请求语法错    
> 401: Unauthorized 未认证(没登录)    
> 403: Forbidden 禁止访问(没权限)    
> 404: Not Found 资源不存在    
> 405: Method Not Allowed 方法不允许    
> 408: Request Timeout 请求超时    
> 409: Conflict 冲突    
> 410: Gone 资源已删除(永久不存在)    
> 413: Payload Too Large 请求体太大    
> 429: Too Many Requests 请求太频繁(限流)    
> 5xx: 服务器错误    
> 500: Internal Server Error 内部错误    
> 501: Not Implemented 未实现    
> 502: Bad Gateway 网关/上游错误    
> 503: Service Unavailable 服务暂不可用    
> 504: Gateway Timeout 上游超时    
> 505: HTTP Version Not Supported 版本不支持    
>     
> 头字段名:    
> Cache-Control:缓存控制, 如max-age=60    
> Connection:连接管理, keep-alive / close    
> Date:日期时间    
> Transfer-Encoding:传输编码, 分块chunked    
> Upgrade:协议升级    
> Host:目标主机, 必须    
> User-Agent:客户端标识    
> Accept:能接受的数据类型    
> Accept-Language:能接受的语言    
> Accept-Encoding:能接受的压缩    
> Authorization:认证凭证    
> Cookie:携带的cookie    
> Referer:来源页面    
> Origin:来源站, 跨域判断用    
> Content-Type:请求/响应体类型    
> Content-Length:请求/响应体长度    
> If-Modified-Since:条件请求, 缓存用    
> If-None-Match:ETag条件请求    
> Range:断点请求, bytes=0-1023    
> Content-Encoding:压缩方式, gzip    
> Content-Disposition:下载附件, attachment; filename=x    
> Location:重定向地址, 301/302用    
> Set-Cookie:设置cookie    
> Server:服务器软件    
> ETag:资源版本指纹, 缓存    
> Last-Modified:最后修改时间, 缓存    
> Expires:过期时间, 旧缓存    
> Allow:允许的方法, 405配合    
> Retry-After:多久后重试, 503/429配合    
> WWW-Authenticate:认证质询, 401配合    
> Access-Control-Allow-Origin:CORS允许的源    
> Strict-Transport-Security:强制HTTPS    
> X-Content-Type-Options:防MIME嗅探    
> X-Frame-Options:防点击劫持    
> Content-Security-Policy:内容安全策略CSP    


> 对称加密：双方都持有同一密钥进行通信    
> 非对称加密：(加密: 数据保密)公钥加密私钥解密，(签名: 防篡改)私钥加密公钥解密    
> CA(certificate authority): 用CA的私钥签名服务端的公钥，客户端使用CA的公钥验证服务端的公钥    
>    
> RSA(rivest-shamir-adleman)    
> DH(diffie-hellman)    
>     
> TLS(transport layer security): [类型 1B][版本 2B][长度 2B][数据 变长]    
> 类型:    
> 20:ChangeCipherSpec, 数据=1字节 01    
> 21:Alert, 数据=[级别1B][描述1B]    
> 22:Handshake, 数据=[握手类型1B][长度3B][内容...]    
> 23:ApplicationData, 数据=[密文/明文 变长]    
>     
> 在22Handshake握手类型的数据格式：    
> 01:ClientHello, 数据=[版本2B][随机数32B][会话ID][加密套件列表][扩展...]    
> 02:ServerHello, 数据=[版本2B][随机数32B][会话ID][选定的加密套件][扩展...]    
> 0B:Certificate, 数据=[证书长度3B][DER证书...]    
> 0C:ServerKeyExchange, 数据=[DH参数...][签名...]    
> 0E:ServerHelloDone, 数据=空    
> 10:ClientKeyExchange, 数据=[密钥交换数据...]    
> 14:Finished, 数据=[校验值12B]    
>    
> 版本:    
> 03 01: TLS 1.0    
> 03 02: TLS 1.1    
> 03 03: TLS 1.2    
> 03 04: TLS 1.3    
>     
> 工作: ClientHello->ServerHello->Certificate,ServerKeyExchange,ServerHelloDone->ClientKeyExchange,ChangeCipherSpec,Finished    
> 确认一致后，用HTTP在23ApplicationData进行通信，即HTTPS(port: 443)    



## 互连
> OSI(open systems interconnection): 物理层,数据链路层,网络层,传输层,会话层,表示层,应用层    

> CDN(content delivery network): 内容分发网    

> PON(passive optical network): 无电源光网络    
> ONU/ONT(optical network unit/terminal): 光网络单元/终端, 光猫调制解调器     
> ODN(optical distribution network): 光分配网，从OLT的PON口分光到ONU/ONT,分光数不超过允许值    
> OLT(optical line terminal): 光线路终端, 二层交换机和光猫管理    
> AGG(aggregation switch): 二层汇聚交换机    
> BNG(broadband network gateway): 宽带网络网关,分配ip,认证限速计费,本质路由器    
> BRAS(broadband remote access server): 宽带远程接入服务器, BNG传统叫法    
> SR(server router): 业务路由，城域网内汇聚bras    
> CR(core router): 核心路由器，城域网核心，省网核心，国家核心    
> ASBR(autonomous system boundary router): 自治系统边界路由器,电信AS4134,联通AS4837,移动AS9808    > IXP(internet exchange point): 互联网交换中心, 汇聚各运营商ASBR    > 同网段两机通信: 网线直连，通过arp广播获取mac+单播通信    > 同网段多机通信: 借CAM(content addr mem table, 交换机学习生成mac和vlan的映射)，通过arp广播获取mac+单播通信    > 不同网段通信：通过逐跳查路由表(路由器上)，转发到不同目的网段的网关    > []内表示非路由    > 访问到另一个运营商下：router<-[ONU/ONT(光电转换)<-ODN(分光)<-OLT<-AGG]<-BRAS/BNG<-SR链<-CR链<-ASBR<-[IXP]<-(其他ASBR及同样的逻辑)    # pcap.h(packet capture)    

```cpp
//地址族
/* Address families.  */
#define AF_UNSPEC	PF_UNSPEC
#define AF_LOCAL	PF_LOCAL
#define AF_UNIX		PF_UNIX
#define AF_FILE		PF_FILE
#define AF_INET		PF_INET
#define AF_AX25		PF_AX25
#define AF_IPX		PF_IPX
#define AF_APPLETALK	PF_APPLETALK
#define AF_NETROM	PF_NETROM
#define AF_BRIDGE	PF_BRIDGE
#define AF_ATMPVC	PF_ATMPVC
#define AF_X25		PF_X25
#define AF_INET6	PF_INET6
#define AF_ROSE		PF_ROSE
#define AF_DECnet	PF_DECnet
#define AF_NETBEUI	PF_NETBEUI
#define AF_SECURITY	PF_SECURITY
#define AF_KEY		PF_KEY
#define AF_NETLINK	PF_NETLINK
#define AF_ROUTE	PF_ROUTE
#define AF_PACKET	PF_PACKET
#define AF_ASH		PF_ASH
#define AF_ECONET	PF_ECONET
#define AF_ATMSVC	PF_ATMSVC
#define AF_RDS		PF_RDS
#define AF_SNA		PF_SNA
#define AF_IRDA		PF_IRDA
#define AF_PPPOX	PF_PPPOX
#define AF_WANPIPE	PF_WANPIPE
#define AF_LLC		PF_LLC
#define AF_IB		PF_IB
#define AF_MPLS		PF_MPLS
#define AF_CAN		PF_CAN
#define AF_TIPC		PF_TIPC
#define AF_BLUETOOTH	PF_BLUETOOTH
#define AF_IUCV		PF_IUCV
#define AF_RXRPC	PF_RXRPC
#define AF_ISDN		PF_ISDN
#define AF_PHONET	PF_PHONET
#define AF_IEEE802154	PF_IEEE802154
#define AF_CAIF		PF_CAIF
#define AF_ALG		PF_ALG
#define AF_NFC		PF_NFC
#define AF_VSOCK	PF_VSOCK
#define AF_KCM		PF_KCM
#define AF_QIPCRTR	PF_QIPCRTR
#define AF_SMC		PF_SMC
#define AF_XDP		PF_XDP
#define AF_MCTP		PF_MCTP
#define AF_MAX		PF_MAX

typedef unsigned short int sa_family_t;
#define	__SOCKADDR_COMMON(sa_prefix) sa_family_t sa_prefix##family

//地址
struct __attribute_struct_may_alias__ sockaddr{
	//这里会是sa_family_t sa_family;
	__SOCKADDR_COMMON (sa_);	/* Common data: address family and length.  */

	char sa_data[14];		/* Address data.  */
};

//地址链表
struct pcap_addr {
	struct pcap_addr *next;
	struct sockaddr *addr;		/* address */
	struct sockaddr *netmask;	/* netmask for that address */
	struct sockaddr *broadaddr;	/* broadcast address for that address */
	struct sockaddr *dstaddr;	/* P2P destination address for that address */
};

//pcap interface, 链表
struct pcap_if {
	struct pcap_if *next;
	char *name;		/* name to hand to "pcap_open_live()" */
	char *description;	/* textual description of interface, or NULL */
	struct pcap_addr *addresses;
	bpf_u_int32 flags;	/* PCAP_IF_ interface flags */
};
typedef struct pcap_if pcap_if_t


//opts: PCAP_CHAR_ENC_LOCAL, PCAP_CHAR_ENC_UTF_8
//errbuf: large enough to hold at least PCAP_ERRBUF_SIZE
//
//ret: 0 on success and PCAP_ERROR on failure
int pcap_init(unsigned int opts, char *errbuf);

//alldevsp: 链表头指针
//errbuf: large enough to hold at least PCAP_ERRBUF_SIZE
//
//ret: 0 on success and PCAP_ERROR on failure
int pcap_findalldevs(pcap_if_t **alldevsp, char *errbuf);
void pcap_freealldevs(pcap_if_t *);

//creat a live capture handle
//source: dev name
//errbuf: buf at least PCAP_ERRBUF_SIZE
//
//ret: a live capture handle
pcap_t	*pcap_create(const char *source, char *errbuf);

//close a capture device or savefile
void pcap_close(pcap_t *p);

//get capture statistics
//p:  handle
//ps: struct pcap_stat
//
//ret: 0 on success, PCAP_ERROR_NOT_ACTIVATED, PCAP_ERROR on error
int pcap_stats(pcap_t *p, struct pcap_stat *ps);

//get or print libpcap error message text
char *pcap_geterr(pcap_t *p);

//handle: 句柄
//
//ret: 0 on success, ret>0 on warning, ret<0 on error
int pcap_activate(pcap_t *handle);

//user: 自定义的
//h: pkthdr
//bytes: datas
//p: handle
//cnt: count, 捕获数量
//
//pcap_loop ret: 0, or PCAP_ERROR_BREAK(pcap_breakloop), PCAP_ERROR_NOT_ACTIVATED, PCAP_ERROR
//pcap_dispatch ret: 本次捕获的包数量, or PCAP_ERROR_BREAK(pcap_breakloop导致), PCAP_ERROR_NOT_ACTIVATED, PCAP_ERROR
typedef void (*pcap_handler)(u_char *user, const struct pcap_pkthdr *h, const u_char *bytes);
int pcap_loop(pcap_t *p, int cnt, pcap_handler callback, u_char *user);
int pcap_dispatch(pcap_t *p, int cnt, pcap_handler callback, u_char *user);

//pcap_dispatch的单包模式封装, 但返回0表示超时或无包
int pcap_next_ex(pcap_t *p, struct pcap_pkthdr **pkt_header, const u_char **pkt_data);

//ret: 0表示阻塞或savefile，1非阻塞，错误有PCAP_ERROR，PCAP_ERROR_NOT_ACTIVATED
int pcap_getnonblock(pcap_t *p, char *errbuf);

//call from the other thread, force a pcap_dispatch() or pcap_loop() call to return
void pcap_breakloop(pcap_t *handle);

//p: handle
//buf: inject or send buf
//size: bufsize
//
//pcap_inject ret: the number of bytes written, or PCAP_ERROR_NOT_ACTIVATED, PCAP_ERROR
//pcap_sendpacket ret: 0 on success, or PCAP_ERROR_NOT_ACTIVATED, PCAP_ERROR
int pcap_inject(pcap_t *p, const void *buf, size_t size);
int pcap_sendpacket(pcap_t *p, const u_char *buf, int size);
```

# inet.h
```cpp
//af: address family，地址族
//cp: copy数据源
//buf: 缓冲
//len: 缓冲长度，INET6_ADDRSTRLEN是最大的
//
//ret: buf指针
const char *inet_ntop (int __af, const void *__restrict __cp, char *__restrict __buf, socklen_t __len)__THROW;

//af: address family，地址族
//cp: copy数据源
//buf: 目的缓冲, struct in_addr, struct in6_addr
//
//ret: -1 on error,0 on invalid, 1 on success
int inet_pton (int __af, const char *__restrict __cp, void *__restrict __buf) __THROW;
```

# in.h
```cpp
//htos//host to network short
//ntohs//network to host short
//htonl//host to network long
//ntohl//network to host long
```
