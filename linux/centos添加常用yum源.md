YUM是RedHat Linux在线安装更新及软件的工具，但是这是RHEL5的收费功能，如果没有购买Redhat的服务时不能使用RHEL的更新源的，会提示注册。

由于CentOS是从RedHat演化而来的免费Linux版本，因此可以利用CentOS的yum更新源来实现RHEL5的YUM功能
# 删除centos自带yum 包
`rpm -aq | grep yum |xargs rpm -e --nodeps`
# 下载、安装yum

访问 http://mirrors.163.com/centos/6/os/x86_64/Packages 搜索相应的包文件
* python-iniparse
* yum-metadata-parser
* yum-plugin-fastestmirror
* yum
按照以上顺序安装，注意最后两个rpm 包，一起安装  
`rpm -ivh python-iniparse.rpm`  
`rpm -ivh yum-metadata-parser.rpm`  
`rpm -ivh yum.rpm yum-plugin-fastestmirror.rpm`
#### 我这服务器在安装yum的时候，提示 python-urlgrabber rpm包版本较低，所以需要先卸载，再重新下载相应的包（还是从上面的连接里找），安装即可
# 添加阿里云的yum源
`wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo`
## 修改配置文件，并更新缓存
* `sed -i 's/\$releasever/6/g' CentOS-Base.repo`
* `yum clean all` // 清空 yum 缓存
* `yum makecache` //将服务器上的软件包信息缓存到本地

# 配置第三方源 

* [参考：CentOS 添加常用 yum 源](https://blog.itnmg.net/2012/09/17/centos-yum-source/)  

```sh
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
rpm -ivh remi-release-6.rpm
rpm -ivh epel-release-6-8.noarch.rpm
```
### 安装完运行yum报错：

Error: Cannot retrieve metalink for repository: epel. Please verify its path and try again

### 解决办法 [参考：在Centos 5.x或6.x上安装RHEL EPEL Repo](https://teddysun.com/153.html)
* `vi /etc/yum.repos.d/epel.repo`  
编辑[epel]下的baseurl前的#号去掉，mirrorlist前添加#号。

* 再运行 `yum makecache`
## 参考
[CentOS 添加常用 yum 源](https://blog.itnmg.net/2012/09/17/centos-yum-source/)  
[RHEL6配置yum源为网易镜像](http://www.jianshu.com/p/446e3fe7d710)  
[rhel源更换为centos源](https://www.cnblogs.com/blackchain/p/4877132.html)  