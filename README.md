# AXIS 1.4以下版本（含）开启远程管理导致命令执行漏洞环境

## 1.将axis文件夹移动到tomcat webapps目录
## 2.修改WEB-INF里的deploy.wsdd文件，增加以下内容
 <service name="AdminService" provider="java:MSG">
  <namespace>http://xml.apache.org/axis/wsdd/</namespace>
  <parameter name="allowedMethods" value="AdminService"/>
  <parameter name="enableRemoteAdmin" value="true"/>
  <parameter name="className" value="org.apache.axis.utils.Admin"/>
 </service>
## 3.在WEB-INF下执行以下命令，完成漏洞环境部署。

```
java -Djava.ext.dirs=/export/servers/tomcat8.5.41/apache-tomcat-8.5.41/webapps/axis/WEB-INF/lib org.apache.axis.client.AdminClient ./deploy.wsdd

```

## 4.黑盒检测PoC：

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:wsdd="http://xml.apache.org/axis/wsdd/">
   <soapenv:Header/>
   <soapenv:Body>
      <wsdd:deployment>anyType</wsdd:deployment>
   </soapenv:Body>
</soapenv:Envelope>
```


