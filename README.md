[![Build Status](https://travis-ci.org/QianmiOpen/dubbo-rpc-jsonrpc.svg)](https://travis-ci.org/QianmiOpen/dubbo-rpc-jsonrpc)



## Why HTTP
With the rapid iteration of the Internet, more and more companies choose quick script frameworks such as nodejs, Django and rails to develop web-side applications.

Java is the most suitable for back-end service, which resulting in a lot of cross-language invocation requirements.

While HTTP and JSON are naturally suitable as cross-language standards, all languages have mature class libraries for them.

Although Dubbo's asynchronous long connection protocol is efficient, in scripting languages this loss of efficiency is not important.  


## Why Not RESTful
Dubbox has made an attempt on the RESTful interface, but there is some difference between the REST architecture and the original RPC architecture of Dubbo. 

The difference is that the REST architecture needs the definition of resources and we should operate the resources using the basic HTTP methods —— GET, POST, PUT, DELETE.  

Dubbox needs to redefine the properties of the interface, which is a big burden on the original interface migration of Dubbox.

In contrast, RESTful is more appropriate for calls between Internet systems, and RPC is more appropriate for calls within a system, so we used JsonRPC which is more consistent with the Dubbo concept.


dubbo-rpc-jsonrpc
=====================

## maven依赖：
```xml
<dependency>
    <groupId>com.qianmi</groupId>
    <artifactId>dubbo-rpc-jsonrpc</artifactId>
    <version>1.0.1</version>
</dependency>

```

## 配置：
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
Multi protocol:
```xml
<dubbo:protocol name="dubbo" port="20880" />
<dubbo:protocol name="jsonrpc" port="8080" />
```
<!-- 使用多个协议暴露服务 -->
```xml
<dubbo:service id="helloService" interface="com.alibaba.hello.api.HelloService" version="1.0.0" protocol="dubbo,jsonrpc" />
```


Jetty Server: (default)
```xml
<dubbo:protocol ... server="jetty" />

或jetty的最新版：
<dubbo:protocol ... server="jetty9" />

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

协议的端口```<dubbo:protocol port="8080" />```必须与servlet容器的端口相同，
协议的上下文路径```<dubbo:protocol contextpath="foo" />```必须与servlet应用的上下文路径相同。

CORS跨源支持:
```xml
<dubbo:protocol name="jsonrpc" ...  />
	<dubbo:parameter key="cors" value="true" />
</dubbo:protocol>
```
--------------
## Example

JAVA API
```java
public interface PhoneNoCheckProvider {
    /**
     * 校验号码是否受限
     * @param operators 运营商
     * @param no 号码
     * @param userid 用户编号
     * */
    boolean isPhoneNoLimit(Operators operators, String no, String userid);
}
```
Client
```shell
curl -i -H 'content-type: application/json' -X POST -d '{"jsonrpc": "2.0", "method": "isPhoneNoLimit", "params": [ "MOBILE", "130000", "A001"],
         "id": 1 }' 'http://127.0.0.1:18080/com.qianmi.api.PhoneNoCheckProvider'
```

Python Client Example
```python
import httplib
import json

__author__ = 'caozupeng'


def raw_client(app_params):
    headers = {"Content-type": "application/json-rpc",
               "Accept": "text/json"}
    h1 = httplib.HTTPConnection('172.19.32.135', port=18080)
    h1.request("POST", '/com.qianmi.ofdc.api.phone.PhoneNoCheckProvider', json.dumps(app_params), headers)
    response = h1.getresponse()
    return response.read()


if __name__ == '__main__':
    app_params = {
        "jsonrpc": "2.0",
        "method": "isPhoneNoLimit",
        "params": ["MOBILE", "130000", "A001"],
        "id": 1
    }
    print json.loads(raw_client(app_params), encoding='utf-8')
```

## Python客户端
https://github.com/QianmiOpen/dubbo-client-py

## Nodejs客户端
https://github.com/QianmiOpen/dubbo-node-client

## 客户端服务端Example  
https://github.com/JoeCao/dubbo_jsonrpc_example  
使用docker运行

## 浏览器调用
需按前述开启CORS支持, 可使用 https://github.com/datagraph/jquery-jsonrpc

## 文档资料

[JSON-RPC 2.0 规范](http://www.jsonrpc.org/specification) 
 
[jsonrpc4j](https://github.com/briandilley/jsonrpc4j) 
 
[dubbo procotol](http://www.dubbo.io/Protocol+Reference-zh.htm) 
