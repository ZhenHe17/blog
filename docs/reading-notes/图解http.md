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

