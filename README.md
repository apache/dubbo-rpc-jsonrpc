[![Build Status](https://travis-ci.org/wuwen5/dubbo-rpc-jsonrpc.svg)](https://travis-ci.org/wuwen5/dubbo-rpc-jsonrpc)

dubbo-rpc-jsonrpc
=====================

maven依赖：
```xml
<dependency>
  <groupId>com.ofpay</groupId>
  <artifactId>dubbo-rpc-jsonrpc</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>

```

配置：
Define jsonrpc protocol:
```xml
 <dubbo:protocol name="jsonrpc" port="8080" server="jetty" />
```

Set default protocol:
```xml
<dubbo:provider protocol="jsonrpc" />
```

Set service protocol:
```xml
<dubbo:service protocol="jsonrpc" />
```

Multi port:
```xml
<dubbo:protocol id="jsonrpc1" name="jsonrpc" port="8080" />
<dubbo:protocol id="jsonrpc2" name="jsonrpc" port="8081" />
```


Jetty Server: (default)
```xml
<dubbo:protocol ... server="jetty" />
```
Maven:
```xml
<dependency>
  <groupId>org.mortbay.jetty</groupId>
  <artifactId>jetty</artifactId>
  <version>6.1.26</version>
</dependency>
```

Servlet Bridge Server: (recommend)
```xml
<dubbo:protocol ... server="servlet" />
```

web.xml：
```xml
<servlet>
         <servlet-name>dubbo</servlet-name>
         <servlet-class>com.alibaba.dubbo.remoting.http.servlet.DispatcherServlet</servlet-class>
         <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
         <servlet-name>dubbo</servlet-name>
         <url-pattern>/*</url-pattern>
</servlet-mapping>
```
注意，如果使用servlet派发请求：

协议的端口<dubbo:protocol port="8080" />必须与servlet容器的端口相同，
协议的上下文路径<dubbo:protocol contextpath="foo" />必须与servlet应用的上下文路径相同。
