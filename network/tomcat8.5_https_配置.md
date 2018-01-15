## server.xml
* JKS 格式
```xml

<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/test1.jks" certificateKeystorePassword="123456" certificateKeyPassword="888888" certificateKeyAlias="test_pri_key_entity"
                         type="RSA" />
        </SSLHostConfig>
</Connector>

```
* PKCS12 格式
```xml
<Connector port="443" protocol="org.apache.coyote.http11.Http11NioProtocol"
               maxThreads="150" SSLEnabled="true">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="conf/domain.pfx" 
            certificateKeystoreType="PKCS12"
             certificateKeystorePassword="123456" 
             <!-- certificateKeyAlias="domain"  可选 -->
                         type="RSA" />
        </SSLHostConfig>
</Connector>

```
