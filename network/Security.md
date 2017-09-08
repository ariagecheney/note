##  [密码学的基本分类](http://blog.csdn.net/LonelyRoamer/article/details/7630594)
* 密码学在加密算法上大体可分为单向加密算法、对称加密算法、非对称加密算法。
* 单向加密算法: 散列函数广泛用于信息完整性的验证，是数据签名的核心技术，散列函数的常用算法有MD----消息摘要算法、SHA-----安全散列算法及MAC-----消息认证码算法。
* DES是典型的对称加密算法的代表，对称加密算法是数据存储加密的常用算法。
* 对称密码体制分为两种：一种是对明文的单个位（或字节）进行运算，称为流加密，也称序列加密。另一种是把明文信息划分成不同的组（或块）结构，分别对每个组（或块）进行加密和解密，称为分组加密（例如：DES）。
* RSA算法是非对称加密算法的代表，非对称加密算法是数据传输加密的常用算法。
* 在非对称密码体制中，公钥和私钥均可用于加密和解密操作，但它与对称密码体制不同。公钥与私钥分属通信双方，一份消息的加密和解密需要公钥与私钥共同参与。公钥加密则需要私钥解密，反之，私钥加密则需要公钥解密。
## The Public-Key Cryptography Standards (PKCS)[PKCS 发布的15 个标准](http://falchion.iteye.com/blog/1472453)
* 是由 RSA 实验室与其它安全系统开发商为促进公钥密码的发展而制订的一系列标准。
##  [Base64算法](http://blog.csdn.net/LonelyRoamer/article/details/7633183)
## [消息摘要算法简介](http://blog.csdn.net/LonelyRoamer/article/details/7638478)
## 加密技术包括两个元素：算法和密钥
## java 中 Security.getProviders() 返回结果：
* 每个 provider 有一个名称和一个版本号，并且在每个它装入运行时中进行配置。  
* `[SUN version 1.7, SunRsaSign version 1.7, SunEC version 1.7, SunJSSE version 1.7, SunJCE version 1.7, SunJGSS version 1.7, SunSASL version 1.7, XMLDSig version 1.0, SunPCSC version 1.7, SunMSCAPI version 1.7]`
## jdk 默认的 强加密随机数生成器类 使用的算法
`new SecureRandom().getAlgorithm()`  
`new SecureRandom().getProvider()`  
`SHA1PRNG`  // 算法  
`SUN version 1.7` // 提供者
## [理解AES加密解密的使用方法](http://blog.csdn.net/vieri_32/article/details/48345023)
## Diffie-Hellman密钥交换算法 [廖雪峰](https://zhuanlan.zhihu.com/p/21513964)
> 首先，小明先选一个素数和一个底数，例如，素数p=23，底数g=5（底数可以任选），再选择一个秘密整数a=6，计算A=g^a mod p=8，然后传纸条给女神：p=23，g=5，A=8；  
> 女神收到纸条后，也选一个秘密整数b=15，然后计算B=g^b mod p=19，并传纸条告诉小明：B=19；  
> 小明自己计算出密码s=B^a mod p=2，女神也自己计算出密码s=A^b mod p=2，因此，小明和女神最终协商的密码s为2。  
> 而这一切都发生在老王的眼皮底下，他却无法计算出小明和女神协商的密码：
## 对称加密与非对称加密 [原创](http://www.cnblogs.com/jfzhu/p/4020928.html)
（1） 对称加密加密与解密使用的是同样的密钥，所以速度快，但由于需要将密钥在网络传输，所以安全性不高。（选择了Rijndael算法作为新的高级加密标准（AES--Advanced Encryption Standard））

（2） 非对称加密使用了一对密钥，公钥与私钥，所以安全性高，但加密与解密速度慢。（非对称加密算法是RSA算法，是Rivest, Shamir, 和Adleman于1978年发明）

（3） 解决的办法是将对称加密的密钥使用非对称加密的公钥进行加密，然后发送出去，接收方使用私钥进行解密得到对称加密的密钥，然后双方可以使用对称加密来进行沟通。

## HTTPS
[https by 腾讯Bugly](https://mp.weixin.qq.com/s?__biz=MzA3NTYzODYzMg==&mid=402615812&idx=1&sn=b6dae639119bb66e7025321254b8d973&scene=1&srcid=122439MA3l7gRwfjgNOB76pA#rd)

## https双向认证，客户端使用与服务端协商好的自签证书
参考微信支付文档,和ClientCustomSSL.java, `../assets/下相关文件`

## [openssl](https://baike.baidu.com/item/openssl/5454803?fr=aladdin)
``