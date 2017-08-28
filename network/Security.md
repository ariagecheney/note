## 对称加密与非对称加密 [原创](http://www.cnblogs.com/jfzhu/p/4020928.html)
（1） 对称加密加密与解密使用的是同样的密钥，所以速度快，但由于需要将密钥在网络传输，所以安全性不高。（选择了Rijndael算法作为新的高级加密标准（AES--Advanced Encryption Standard））

（2） 非对称加密使用了一对密钥，公钥与私钥，所以安全性高，但加密与解密速度慢。（非对称加密算法是RSA算法，是Rivest, Shamir, 和Adleman于1978年发明）

（3） 解决的办法是将对称加密的密钥使用非对称加密的公钥进行加密，然后发送出去，接收方使用私钥进行解密得到对称加密的密钥，然后双方可以使用对称加密来进行沟通。

## HTTPS
[https by 腾讯Bugly](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=402615812&idx=1&sn=b6dae639119bb66e7025321254b8d973&scene=1&srcid=122439MA3l7gRwfjgNOB76pA#rd)