## spring-boot 热部署 
* jvm 参数`-javaagent:D:/code/springloaded-1.2.8.RELEASE.jar -noverify`

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