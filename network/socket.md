* [TCP 协议简介](http://www.ruanyifeng.com/blog/2017/06/tcp-protocol.html)
* [TCP协议的三次握手和四次分手](https://github.com/jawil/blog/issues/14)

![](../assets/tcp_close.jpg)

* MSL时间  
MSL就是maximum segment lifetime(最大分节生命期），这是一个IP数据包能在互联网上生存的最长时间，超过这个时间IP数据包将在网络中消失 。MSL在RFC 1122上建议是2分钟，而源自berkeley的TCP实现传统上使用30秒。
* TIME_WAIT状态维持时间  
TIME_WAIT状态维持时间是两个MSL时间长度，也就是在1-4分钟。Windows操作系统就是4分钟。
## 长连接
* HTTP长连接的数据传输完成识别  
使用长连接之后，客户端、服务端怎么知道本次传输结束呢？两部分：1是判断传输数据是否达到了Content-Length指示的大小；2动态生成的文件没有Content-Length，它是分块传输（chunked），这时候就要根据chunked编码来判断，chunked编码的数据在最后有一个空chunked块，表明本次传输数据结束。

* HTTP/1.1起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头有加入这行代码：
Connection:keep-alive  
Keep-Alive: timeout=20