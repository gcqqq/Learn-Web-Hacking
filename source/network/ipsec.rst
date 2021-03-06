IPsec
========================================

简介
----------------------------------------
IPsec（IP Security）是IETF制定的三层隧道加密协议，它为Internet上传输的数据提供了高质量的、可互操作的、基于密码学的安全保证。特定的通信方之间在IP层通过加密与数据源认证等方式，提供了以下的安全服务：

- 数据机密性（Confidentiality）
    - IPsec发送方在通过网络传输包前对包进行加密。
- 数据完整性（Data Integrity）
    - IPsec接收方对发送方发送来的包进行认证，以确保数据在传输过程中没有被篡改。
- 数据来源认证（Data Authentication）
    - IPsec在接收端可以认证发送IPsec报文的发送端是否合法。
- 防重放（Anti-Replay）
    - IPsec接收方可检测并拒绝接收过时或重复的报文。

优点
----------------------------------------
IPsec具有以下优点：

- 支持IKE（Internet Key Exchange，因特网密钥交换），可实现密钥的自动协商功能，减少了密钥协商的开销。可以通过IKE建立和维护SA的服务，简化了IPsec的使用和管理。
- 所有使用IP协议进行数据传输的应用系统和服务都可以使用IPsec，而不必对这些应用系统和服务本身做任何修改。
- 对数据的加密是以数据包为单位的，而不是以整个数据流为单位，这不仅灵活而且有助于进一步提高IP数据包的安全性，可以有效防范网络攻击。

构成
----------------------------------------
IPsec由四部分内容构成：

- 负责密钥管理的Internet密钥交换协议IKE（Internet Key Exchange Protocol）
- 负责将安全服务与使用该服务的通信流相联系的安全关联SA（Security Associations）
- 直接操作数据包的认证头协议AH（IP Authentication Header）和安全载荷协议ESP（IP Encapsulating Security Payload）
- 若干用于加密和认证的算法

安全联盟（Security Association，SA）
----------------------------------------
IPsec在两个端点之间提供安全通信，端点被称为IPsec对等体。

SA是IPsec的基础，也是IPsec的本质。SA是通信对等体间对某些要素的约定，例如，使用哪种协议（AH、ESP还是两者结合使用）、协议的封装模式（传输模式和隧道模式）、加密算法（DES、3DES和AES）、特定流中保护数据的共享密钥以及密钥的生存周期等。建立SA的方式有手工配置和IKE自动协商两种。

SA是单向的，在两个对等体之间的双向通信，最少需要两个SA来分别对两个方向的数据流进行安全保护。同时，如果两个对等体希望同时使用AH和ESP来进行安全通信，则每个对等体都会针对每一种协议来构建一个独立的SA。

SA由一个三元组来唯一标识，这个三元组包括SPI（Security Parameter Index，安全参数索引）、目的IP地址、安全协议号（AH或ESP）。

SPI是用于唯一标识SA的一个32比特数值，它在AH和ESP头中传输。在手工配置SA时，需要手工指定SPI的取值。使用IKE协商产生SA时，SPI将随机生成。

IKE
----------------------------------------
IKE（RFC2407，RFC2408、RFC2409）属于一种混合型协议，由Internet安全关联和密钥管理协议（ISAKMP）和两种密钥交换协议OAKLEY与SKEME组成。IKE创建在由ISAKMP定义的框架上，沿用了OAKLEY的密钥交换模式以及SKEME的共享和密钥更新技术，还定义了它自己的两种密钥交换方式。

IKE使用了两个阶段的ISAKMP：

第一阶段，协商创建一个通信信道（IKE SA），并对该信道进行验证，为双方进一步的IKE通信提供机密性、消息完整性以及消息源验证服务；
第二阶段，使用已建立的IKE SA建立IPsec SA（V2中叫Child SA）。
