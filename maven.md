# maven
## 1. features
* 构建工具build  
* 依赖管理dependencies  
* 配置管理SCMs
* 发布管理Releases  
* 文档编制Documentation  
* 报告Reporting  

## 2. concepts
* 特点
> * 微内核  插件  -》 解析xml，管理生命周期，管理maven插件-》其他功能委派给maven插件-》编译源码，打包war文件，单元测试，发布站点--》mvn install 先下载核心插件
 * 约定优于配置,项目模型,核心概念
 * pom：项目的属性，依赖，构建配置被抽象成pom--》基本组成：项目基本信息，构建环境（开发，生产），pom关系（依赖，继承），构建设置（更改默认设置）
--》
* 项目坐标groupid：artifactid：packaging：vsrsion
* mvn help:effective-pom

* 插件与目标：
    1. 调用插件目标的两种方式:将插件目标与生命周期绑定，执行生命周期，
    2. 直接执行插件目标--》插件列表表http://maven.apache.org/plugins/index.html
http://www.mojohaus.org/plugins.html

* 项目的生命周期：指项目的构建过程，包含了一系列有序阶段，而一个阶段就是构建过程的一个步骤。很多种生命周期，maven默认生命周期：验证生命完整性，
一个生命周期阶段可以绑定多个插件目标
mvn clean--》mvn package   编译》打包

* 传递性依赖：scope 依赖的范围--》编译范围（compile），已提供范围（provided），运行时范围（runtime），测试范围（test），系统范围（system）

* maven仓库：一个存放了所有依赖的仓库，其通过依赖的坐标对其进行管理。--》mvn install --》pom里远程仓库配置，开源中国
http://search.maven.org/   maven central repo  maven.oschina.net

* 项目站点报告：在项目pom里配置 site插件
```xml
<plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-site-plugin</artifactId>
        <version>3.3</version>
        <configuration>
          <locales>zh_CN</locales>
        </configuration>
      </plugin>
      <!-- 使用mvn clean site -->
```

* cmd 构建maven项目  
mvn archetype:generate -->mvn package--》war包放在tomcat下webapps目录下，启动bin目录下的startup.bat 文件，浏览器访问地址是war包名
* packaging标签 jar war ear pom（父）modules>module 相对路径

* 更改setting的localrepo 位置后，cmd mvn help:system 下载的jar包

## 3. command

* Maven 参数  
-D 传入属性参数  
-P 使用pom中指定的配置   
-e 显示maven运行出错的信息     
-o 离线执行命令,即不去远程仓库更新包   
-X 显示maven允许的debug信息   
-U 强制去远程参考更新snapshot包   
例如 mvn install -Dmaven.test.skip=true -Poracle   
其他参数可以通过mvn help 获取  


* maven常用命令

mvn clean   
说明:清理项目生产的临时文件,一般是模块下的target目录   
mvn package   
说明: 项目打包工具,会在模块下的target目录生成jar或war等文件   
mvn test   
说明:测试命令,或执行src/test/java/下junit的测试用例.   
mvn install   
说明:模块安装命令,将打包的的jar/war文件复制到你的本地仓库中,
供其他模块使用`-Dmaven.test.skip=true` 跳过测试(同时会跳过test compile)  
mvn deploy   
说明: 发布命令 ，将打包的文件发布到远程参考,提供其他人员进行下载依赖

* otehr  

mvn dependency:resolve   
查看项目依赖情况   
mvn dependency:tree   
打印出项目的整个依赖树   
mvn dependency:analyze   
帮助你分析依赖关系, 用来取出无用, 重复依赖的好帮手   
```cmd
www
```
## 4. 实战
### maven多模块项目搭建

> spring-webmvc4.2.8 依赖以下jar包
spring-beans:
spring-context:
spring-aop:
spring-core:
spring-web:
spring-expression:
commons-logging:
aopalliance:


```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.spring.platform</groupId>
            <artifactId>platform-bom</artifactId>
            <version>2.0.5.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
<!-- 镜像 -->
<mirror>
        <id>CN</id>
        <name>OSChina Central</name>
        <url>http://maven.oschina.net/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>
      </mirror>
 <mirror>
        <id>repo2</id>
        <mirrorOf>central</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://repo2.maven.org/maven2/</url>
      </mirror>

 <mirror>
        <id>osc_thirdparty</id>
        <mirrorOf>thirdparty</mirrorOf>
        <url>http://maven.oschina.net/content/repositories/thirdparty/
        </url>
</mirror>
http://wx.banmahz.com:80/operate/invite?inviterId=13ae8cd622eb4ae9b5f287fcd84fa804&oid=fe4cc3a0e1514b6fac71af7a5ebde478&kid=55b78f12c5654b1ba48ab5851a3a98a7
```
