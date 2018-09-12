## aware
spring中提供了一些Aware结尾的接口，比如：BeanFactoryAware、BeanNameAware、ApplicationContextAware、ResourceLoaderAware、ServletContextAware等，这些接口的作用是：实现这些接口的Bean被实例化后，可以取得一些相对的资源
因为ApplicationContext 接口集成了MessageSource 接口、ApplicationEventPublisher 接口和ResourceLoader 接口，所以Bean 继承ApplicationContextAware 可以获得Spring 容器的所有服务，原则上用到什么接口，就实现什么接口

## [@Import](http://weiqingfei.iteye.com/blog/2361152)

@Import 是被用来整合所有在@Configuration注解中定义的bean配置


## BeanPostProcessor、BeanFactoryPostProcessor、BeanValidationPostProcessor，ConfigurationClassPostProcessor等一系列后处理器
## [Configuration和@Component](https://blog.csdn.net/isea533/article/details/78072133)
配置类不能是 final 类（没法动态代理）
配置类不能是本地化的，亦即不能将配置类定义在其他类的方法内部；
配置类必须有一个无参构造函数
(1)、@Bean注解在返回实例的方法上，如果未通过@Bean指定bean的名称，则默认与标注的方法名相同； 

(2)、@Bean注解默认作用域为单例singleton作用域，可通过@Scope(“prototype”)设置为原型作用域； 

(3)、既然@Bean的作用是注册bean对象，那么完全可以使用@Component、@Controller、@Service、@Ripository等注解注册bean，当然需要配置@ComponentScan注解进行自动扫描。
* [Spring @Configuration 使用](https://www.jianshu.com/p/06b2f181d799)

## spring-boot 热部署 
* jvm 参数`-javaagent:D:/code/springloaded-1.2.8.RELEASE.jar -noverify`
* -Ddebug 或者在application.properties文件这添加属性debug= true。这样，当我们以调试模式启动应用程序时，SpringBoot就可以帮助我们创建自动配置的运行报告。
* https://blog.csdn.net/fxbin123/article/details/80518664
* java -Ddebug -jar emample.jar --server.port=8081 该命令行参数，将会覆盖application.properties中的端口配置; -D传入系统参数（System.getProperty()）
* mvn spring-boot:run -Dspring-boot.run.profiles=local -Ddebug 
* Arguments that should be passed to the application. On command line use
      commas to separate multiple arguments.
      User property: spring-boot.run.arguments

mvn spring-boot:run -Dspring-boot.run.arguments="--server.port=8111"

## mvn spring-boot:help -Dgoal=run -Ddetail=true

## servlet URL
```java
basePath:http://localhost:8080/test/

getContextPath:/test 
getServletPath:/test.jsp 
getRequestURI:/test/test.jsp 
getRequestURL:http://localhost:8080/test/test.jsp 
getRealPath:D:\Tomcat 6.0\webapps\test\ 
getServletContext().getRealPath:D:\Tomcat 6.0\webapps\test\ 
getQueryString:p=fuck

在一些应用中，未登录用户请求了必须登录的资源时，提示用户登录，此时要记住用户访问的当前页面的URL，当他登录成功后根据记住的URL跳回用户最后访问的页面：

String lastAccessUrl = request.getRequestURL() + "?" + request.getQueryString();


```