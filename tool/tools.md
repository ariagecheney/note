## Listory
* 启动  
    1. 文件管理器模式：直接输入 双击
    2. 启动器模式： ct 两下  
* 快捷键  
    ↓ 键 或 Ctrl + N：选择下一项；

    ↑ 键 或 Ctrl + P：选择上一项；

    → 键 或 Ctrl + O：打开选中项的动作菜单。

## 1. atom
### setting
1. Editor tab length 设置为4


### Packages

1. atom-beautify
2. vim-mode-plus and ex-mode (可以添加esc键映射)  
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

### 快捷键（vim插件环境下）
* 注释 ctrl+/

## 2. win softwire
* dev  
    1. Git  
    2. vscode or atom or sublime  
    3. jdk  
    4. tomcat  
    5. maven  
    6. idea  
        config  
    7. mysql  
    8. navicat -百度网盘  
    9. xshell  
    10. lantern  
    11. altrun - 坚果云  
    12. eclipse  
    13. node  
    14. svn  
    15. smartgit

* user  
    1. zip
    2. compare  
    3. fscapcher  
    4. wiz evernote  
    5. mychrom  
    6. potplayer  
    9. sougou  
    10. qq  
    11. teamviewer  
    12. youdao  
    13. pdf  
    14. office wps  
    18. VC REDIST INSTALLER  
    19. clower
    20. alt run

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
* TranslationPlugin 翻译
* jrebel [参考](http://blog.csdn.net/garfielder007/article/details/53591480)
    
    1. [plugin download](https://plugins.jetbrains.com/plugin/4441-jrebel-for-intellij)
    2. 激活
        * `http://idea.qinxi1992.cn/pmz` (idea.goxz.gq)加 邮箱
        * 切换离线模式和关闭日志反馈  
    3. 使用
        * http://blog.lanyus.com/archives/133.html
        * 设置 on frame deactivation 为 Update classes and resources
        * [usage1](https://zeroturnaround.com/software/jrebel/quickstart/intellij/?run=ide#!/project-configuration)  
        * [usage2](http://manuals.zeroturnaround.com/license-server/install/index.html#getting-started)  
        * [maven project-configuration](http://manuals.zeroturnaround.com/jrebel/standalone/maven.html)     
    4. tomcat server.xml 的配置
    <Context path="/helloapp" docBase="helloapp" reloadable="true"/> 
* 注意
一般Jrebel有15天免费试用期，不过Jrebel对于个人是免费的，在Google上搜索myJrebel然后注册就会送个人免费注册码，   
传送带：https://my.jrebel.com/
* spring boot
http://www.jianshu.com/p/97dd8978482f
* 破解
    1. `license-server http://phxism.top:41017/`

* keymap
alt + insert ： getter and setter  
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

## 4 vsc
### 快捷键
* 格式化 c+k c+f
* getter，setter alt+insert
* go to line c+g

## chrome plugins
* 划词翻译   ctrl+q
* colorZilla   提取css颜色 ctrl + alt + z  与qq冲突
* pageRuler  
 General Shortcuts
ctrl + P: Enable / Disable Page Ruler  与chrome冲突  需单独设置
Toolbar Inputs
Up Key: Increase the value by 1
+ Shift: Increase the value by 10
Down Key: Decrease the value by 1
+ Shift: Decrease the value by 10
Ruler
Arrow keys: Move the ruler by 1 pixel in any direction
+ Shift: Move the ruler by 10 pixels
+ Ctrl (Command): Expand the ruler outwards by 1 pixel
+ Shift + Ctrl (Command): Expand the ruler outwards by 10 pixels
+ Ctrl (Command) + Alt: Shrink the ruler inwards by 1 pixel
+ Shift + Ctrl (Command) + Alt: Shrink the ruler inwards by 10 pixels
* cssviewer    chrome单独设置  alt+v

## window path
```dos
Path=D:\soft\install\python\;  
D:\soft\install\python\Scripts;  
C:\Windows\system32;  
C:\Windows;C:\Windows\System32\Wbem;   
C:\Windows\System32\WindowsPowerShell\v1.0\;  
D:\soft\install\Vim\vim74;  
C:\Program Files (x86)\Spoon\Cmd;D:\soft\install\apache-maven-3.3.9\bin;
D:\soft\install\SVN\bin;
D:\soft\install\java\jdk\bin;
C:\Android;
D:\soft\install\Git\cmd;D:\soft\package\nodejs\;
D:\soft\install\gradle-2.10\bin;
D:\soft\install\Tesseract-OCR;
D:\me\redis;
C:\Windows\system32;
C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;
D:\soft\install\Vim\vim74;  
C:\Program Files (x86)\Spoon\Cmd;
D:\soft\install\SVN\bin;
D:\soft\package\mysql-5.5\bin;
C:\Program Files (x86)\QuickTime\QTSystem\;
D:\soft\install\svn\bin;
C:\Users\Administrator\AppData\Local\atom\bin;
C:\Users\Administrator\AppData\Roaming\npm;C:\Program Files (x86)\Microsoft VS Code\bin
```
## win10
* win+shift+s 和 ct + sh printscreen
* win+空格 切换输入法
* Ct + Sh + b 表情 ct + 方向键 切换
* cmd 支持 ct+v
* win+g 录屏
* Win + Ctrl + D 虚拟桌面
* win+tab 任务视图
* Win+A 操作中心
* Win+S cortana
* Win+I 设置面板
* Win+X 
* Win+K 无线设备搜索
* Win+P 投影
* Win+↑/↓/←/→ 固定并移动窗口
* Alt+↑/←/→ 文件导航
* Alt+空格+N 最小化 r 还原 x最大化
