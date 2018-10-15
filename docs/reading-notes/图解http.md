### III.图解http

1.3 ：
> TCP/IP 协议族按层次分别分为以下 4 层：应用层、传输层、网络层和数据链路层。
>
> * 应用层决定了向用户提供应用服务时通信的活动。
> * 传输层对上层应用层，提供处于网络连接中的两台计算机之间的数据传输。
> * 网络层用来处理在网络上流动的数据包。
> * 链路层用来处理连接网络的硬件部分。
> 
> 利用 TCP/IP 协议族进行网络通信时，会通过分层顺序与对方进行通信。发送端从应用层往下走，接收端则往应用层往上走。
> 
> 发送端在层与层之间传输数据时，每经过一层时必定会被打上一个该层所属的首部信息。反之，接收端在层与层传输数据时，每经过一层时会把对应的首部消去。

1.4 与 HTTP 关系密切的协议 : IP、TCP 和 DNS：
> * IP（Internet Protocol）网际协议的作用是把各种数据包传送给对方。而要保证确实传送到对方那里，则需要满足各类条件。其中两个重要的条件是 IP 地址和 MAC地址（Media Access Control Address）。
> * TCP 位于传输层，提供可靠的字节流服务。为了准确无误地将数据送达目标处，TCP 协议采用了三次握手（three-way handshaking）策略。握手过程中使用了 TCP 的标志（flag） —— SYN（synchronize） 和ACK（acknowledgement）。
> * DNS（Domain Name System）服务是和 HTTP 协议一样位于应用层的协议。它提供域名到 IP 地址之间的解析服务。

1.7：
> * URI 是 Uniform Resource Identifier 的缩写。
> * URL 是 Uniform Resource Locator 的缩写。
> * URI 用字符串标识某一互联网资源，而 URL表示资源的地点（互联网上所处的位置）。可见 URL是 URI 的子集。

2.3：
> HTTP 是一种不保存状态，即无状态（stateless）协议。在 HTTP 这个级别，协议对于发送过的请求或响应都不做持久化处理。

2.7：
> 持久连接（HTTP Persistent Connections，也称为 HTTP keep-alive 或HTTP connection reuse）的特点是，只要任意一端没有明确提出断开连接，则保持 TCP 连接状态。
> 在 HTTP/1.1 中，所有的连接默认都是持久连接，但在 HTTP/1.0 内并未标准化。

7.2：
> HTTPS 并非是应用层的一种新协议。只是 HTTP 通信接口部分用SSL（Secure Socket Layer）和 TLS（Transport Layer Security）协议代替而已。

> 为了更好地理解 HTTPS，我们来观察一下 HTTPS 的通信步骤：
> * 步骤 1： 客户端通过发送 Client Hello 报文开始 SSL通信。报文中包含客户端支持的 SSL的指定版本、加密组件（Cipher Suite）列表（所使用的加密算法及密钥长度等）。
> * 步骤 2： 服务器可进行 SSL通信时，会以 Server Hello 报文作为应答。和客户端一样，在报文中包含 SSL版本以及加密组件。服务器的加密组件内容是从接收到的客户端加密组件内筛选出来的。
> * 步骤 3： 之后服务器发送 Certificate 报文。报文中包含公开密钥证书。
> * 步骤 4： 最后服务器发送 Server Hello Done 报文通知客户端，最初阶段的 SSL握手协商部分结束。
> * 步骤 5： SSL第一次握手结束之后，客户端以 Client Key Exchange 报文作为回应。报文中包含通信加密中使用的一种被称为 Pre-mastersecret 的随机密码串。该报文已用步骤 3 中的公开密钥进行加密。
> * 步骤 6： 接着客户端继续发送 Change Cipher Spec 报文。该报文会提示服务器，在此报文之后的通信会采用 Pre-master secret 密钥加密。
> * 步骤 7： 客户端发送 Finished 报文。该报文包含连接至今全部报文的整体校验值。这次握手协商是否能够成功，要以服务器是否能够正确解密该报文作为判定标准。
> * 步骤 8： 服务器同样发送 Change Cipher Spec 报文。
> * 步骤 9： 服务器同样发送 Finished 报文。
> * 步骤 10： 服务器和客户端的 Finished 报文交换完毕之后，SSL连接就算建立完成。当然，通信会受到 SSL的保护。从此处开始进行应用层协议的通信，即发送 HTTP 请求。
> * 步骤 11： 应用层协议通信，即发送 HTTP 响应。
> * 步骤 12： 最后由客户端断开连接。断开连接时，发送 close_notify 报文。这步之后再发送 TCP FIN 报文来关闭与 TCP的通信。