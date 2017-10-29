## 文件共享
- [参考](http://www.cnblogs.com/xing901022/p/5774677.html)
- `sudo mount -t vboxsf winshare /home/pmz/share/` 注意winshare是共享文件名称，不是具体win系统文件路径
- 如果没有自动挂载，可以在 /etc/fstab 添加一行配置
`winshare /home/pmz/share vboxsf rw,gid=100,uid=1000,auto 0 0`
“设备文件 挂载目录 格式类型 权限选项 自检 优先级”
## 网络配置
* 双网卡 
   1. nat
   2. host-only
* /etc/sysconfig/network-scripts/ifcfg-eth0  
onboot=yes
* 开启 `ifup eth0`
* 重启网络 `service network restart`

