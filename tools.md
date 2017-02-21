## 1. atom
### setting
1. tab length 设置为4


### Packages

1. atom-beautify
2. vim-mode and ex-mode (可以添加esc键映射)  
```
'atom-text-editor.vim-mode':
        'escape': 'vim-mode:reset-normal-mode'
```
3. terminal-plus
4. emmet  `tab` `ctrl+e`
5. autocomplete-plus `ctrl+space` 已内置，主插件
6. linter `语法检查主插件`
7. linter-jshint `javascript+json`
8. git-plus
9. copy-as-rtf 转换成富文本格式到剪切板

### thems
1. atom-material-ui
2. seti-ui

## 2. win softwire
* dev  
    1.tomcat  
    2.Git  
    3.eclipse  
    4.idea  
        config  
    5.pycharm  
    6.emacs  
    7.xshell  
    8.hbuilder  
    9.jdk  
    10.maven  
    11.mongodb  
    12.mysql  
    13.node  
    14.ps   
    15.python  
    16.svn  
    18.lantern  
    19.altrun  
    20.atom sublime  
    21.filezilla  

* user  
    1.cclear  
    2.compare  
    3.fscapcher  
    4.wiz evernote  
    5.mychrom  
    6.potplayer  
    9.sougou  
    10.qq  
    11.teamviewer  
    12.youdao  
    13.pdf  
    14.office wps  
    15.kwifi  
    16.kms  
    18.VC REDIST INSTALLER  
    19aliqintao

## 3. idea
* 坚果云配置（${idea.home}/bin/idea.properties）
* acejump Ctrl+ALT+; target word
* Find Action Ctrl+Shift+A
    1. Search everywhere   跳转到任意地方  
    2. java class 新建java类
    3. fix doc 添加java注释
* Postfix
    1. sout: println
    2. psvm: main
    3. collections.for: forEach

Ctrl+E，可以显示最近编辑的文件列表  
Ctrl+F12，可以显示当前文件的结构  
Ctrl+N，可以快速打开类  
Ctrl+Shift+N，可以快速打开文件  
Ctrl+W可以选择单词继而语句继而行继而函数  
Ctrl+P，可以显示参数信息  
CTRL+SHIFT+ALT+N 查找类中的方法或变量  
CTRL+G 定位行  
CTRL+R 在 当前窗口替换文本  
F3 向下查找关键字出现位置  
SHIFT+F3 向上一个关键字出现位置  
F4 查找变量来源  
CTRL+D 复制行  
CTRL+X 剪切,删除行  
CTRL+/ 注释//  
CTRL+SHIFT+/ 注释/*...*/  
CTRL+H 显示类结构图  
CTRL+Q 显示注释文档  
CTRL+SHIFT+UP/DOWN 代码向上/下移动。  
ESC 光标返回编辑框  
F11 mark  

## 4. cmd commond
以端口8080为例：
1. 查找对应的端口占用的进程：netstat  -aon|findstr  "8080"，找到占用8080端口对应的程序的PID号：
2. 根据PID号找到对应的程序：tasklist|findstr "PID号"    ，找到对应的程序名
3. 结束该进程：taskkill /f /t /im 程序名
