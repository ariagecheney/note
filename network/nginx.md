### [教程](https://linux.cn/article-4279-1.html)
### 1. 优点
* 可以实现高并发
* 部署简单
* 内存消耗少
* 跨平台

### 2. 缺点
* rewrite功能不够强大
* 模块少

### 3. Nginx启动关闭
```shell
# /usr/local/nginx-1.0.6/sbin/nginx  //启动nginx
# /usr/local/nginx-1.0.6/sbin/nginx –t //测试nginx配置文件的准确性
# /usr/local/nginx-1.0.6/sbin/nginx –s reload //重载nginx
# /usr/local/nginx-1.0.6/sbin/nginx –s stop //关闭nginx
```
