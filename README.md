Cas-server服务器搭建3.3.1
作者：zhaojf
创建日期：2016-05-04
版本：1.1
制作并导入证书
生成密钥

 
 
注意：现在使用的是java version "1.7.0_79"
Java(TM) SE Runtime Environment (build 1.7.0_79-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.79-b02, mixed mode)
 
E:/cas/cas-server-3.3.1>keytool -genkey -alias tomcat -keystore ./mydestore
 -keyalg RSA -validity 2000
输入keystore密码：
再次输入新密码:
您的名字与姓氏是什么？
  [Unknown]：  localhost
您的组织单位名称是什么？
  [Unknown]：  mulechina
您的组织名称是什么？
  [Unknown]：  mulechina
您所在的城市或区域名称是什么？
  [Unknown]：  beijing
您所在的州或省份名称是什么？
  [Unknown]：  beijing
该单位的两字母国家代码是什么
  [Unknown]：  zh
CN=localhot, OU=mulechina, O=mulechina, L=beijing, ST=beijing,
C=zh 正确吗？
  [否]：  y
 
输入<tomcat>的主密码
        （如果和 keystore 密码相同，按回车）：
 
E:/cas/cas-server-3.3.1>
 
导出认证
 
E:/cas/cas-server-3.3.1>keytool -export -alias tomcat -keystore ./mydestore
 -file server.crt
输入keystore密码：
保存在文件中的认证 <server.crt>
注意：必须手工输入命令行
E:/cas/cas-server-3.3.1>
导入到jdk的密钥库
E:/cas/cas-server-3.3.1>keytool -import -alias tomcat -file ./server.crt -keystore D:/tools/Java/jdk1.7.0_79/jre/lib/security/cacerts
输入keystore密码：
所有者:CN=localhot, OU=mulechina, O=mulechina, L=beijing, ST=beijing, C=zh
签发人:CN=localhot, OU=mulechina, O=mulechina, L=beijing, ST=beijing, C=zh
序列号:4a4d52cf
有效期: Fri Jul 03 08:37:35 CST 2016 至Wed Dec 24 08:37:35 CST 2020
证书指纹:
         MD5:C2:6E:E6:BC:21:E3:53:57:42:3F:B1:58:6D:4B:B9:AF
         SHA1:E5:9D:81:6E:2C:77:0F:7E:14:6A:3D:74:1E:FD:DD:02:11:F2:1C:FF
         签名算法名称:SHA1withRSA
         版本: 3
信任这个认证？ [否]：  y
认证已添加至keystore中
 
E:/cas/cas-server-3.3.1>
删除重复的证书
E:/cas/cas-server-3.3.1>keytool -import -alias tomcat -file ./server.crt -keystore d:/tools/Java/jdk1.7.0_79/jre/lib/security/cacerts
输入keystore密码：
keytool错误： java.lang.Exception: 认证未输入，别名 <tomcat> 已经存在
 
E:/cas/cas-server-3.3.1>keytool -delete -alias tomcat -file ./server.crt -keystore d:/tools/Java/jdk1.7.0_79/jre/lib/security/cacerts
输入keystore密码：
 
配置JBOSS4.2.3 (server.xml)
D:/jboss-4.2.3.GA/server/default/deploy/jboss-web.deployer/server.xml
 
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
       maxThreads="150" scheme="https" secure="true"
       clientAuth="false" sslProtocol="TLS" 
       keystoreFile="e:/cas/cas-server-3.3.1/mydestore " 
       keystorePass="changeit" />
 
配置cas服务器
将E:/cas/cas-server-3.3.1/modules/cas-server-webapp-3.3.1.war
拷贝到D:/jboss-4.2.3.GA/server/default/deploy/下面，并且更名为cas-server.war
 
启动jboss4.2.3,注意jboss使用的jdk必须是刚才将认证导入的jdk，我们使用的是d:/jdk1.5.0
注意：现在使用的是tools/Java/jdk1.7.0_79
 
在IE浏览器中录入：http://localhost:8080/cas-server
登陆，录入的用户名和密码相同即可。

 
tools/Java/jdk1.7.0_79命令keytool用法
 
E:/cas/cas-server-3.3.1>keytool
keytool 用法：
 
-certreq     [-v] [-protected]
             [-alias <别名>] [-sigalg <sigalg>]
             [-file <csr_file>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-changealias [-v] [-protected] -alias <别名> -destalias <目标别名>
             [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-delete      [-v] [-protected] -alias <别名>
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-exportcert  [-v] [-rfc] [-protected]
             [-alias <别名>] [-file <认证文件>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-genkeypair  [-v] [-protected]
             [-alias <别名>]
             [-keyalg <keyalg>] [-keysize <密钥大小>]
             [-sigalg <sigalg>] [-dname <dname>]
             [-validity <valDays>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-genseckey   [-v] [-protected]
             [-alias <别名>] [-keypass <密钥库口令>]
             [-keyalg <keyalg>] [-keysize <密钥大小>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-help
 
-importcert  [-v] [-noprompt] [-trustcacerts] [-protected]
             [-alias <别名>]
             [-file <认证文件>] [-keypass <密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-importkeystore [-v]
             [-srckeystore <源密钥库>] [-destkeystore <目标密钥库>]
             [-srcstoretype <源存储类型>] [-deststoretype <目标存储类型>]
             [-srcstorepass <源存储库口令>] [-deststorepass <目标存储库口令>]
             [-srcprotected] [-destprotected]
             [-srcprovidername <源提供方名称>]
             [-destprovidername <目标提供方名称>]
             [-srcalias <源别名> [-destalias <目标别名>]
               [-srckeypass <源密钥库口令>] [-destkeypass <目标密钥库口令>]]
             [-noprompt]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-keypasswd   [-v] [-alias <别名>]
             [-keypass <旧密钥库口令>] [-new <新密钥库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-list        [-v | -rfc] [-protected]
             [-alias <别名>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
-printcert   [-v] [-file <认证文件>]
 
-storepasswd [-v] [-new <新存储库口令>]
             [-keystore <密钥库>] [-storepass <存储库口令>]
             [-storetype <存储类型>] [-providername <名称>]
             [-providerclass <提供方类名称> [-providerarg <参数>]] ...
             [-providerpath <路径列表>]
 
E:/cas/cas-server-3.3.1>

