## linux 学习记录 2015.12

### 软件篇

1. atom   
download: [官网deb/rpm包](https://atom.io/) 有点慢  
install:
```
sudo dpkg -i atom-amd64.deb
// 复制快捷方式到桌面
cp /usr/share/applications/atom.desktop ~/桌面
// 更改快捷方式权限，以显示正常
sudo chmod 755 atom.desktop
// 还可以拖动图标文件到dash 启动器栏
```
2. teamviewer  
download: [官网](https://www.teamviewer.com/zhCN/download/linux.aspx)  
install:  
```
sudo dpkg -i teamviewer.deb
//依赖问题
sudo apt-get -f install
// 后台运行
teamviewer 
```
