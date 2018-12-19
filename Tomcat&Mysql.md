# 1.	安装Docker(略)
# 2.	下载Docker镜像
## 下载镜像(以MySQL镜像为例)
    docker pull mysql
## 查看镜像
    docker images
## 启动容器
    docker run  -p 3306:3306 -p 443:443 -p 80:8080 --name mysql02 -v $PWD/opt:/opt -e MYSQL_ROOT_PASSWORD=123456 -d mysql-tomcat:1.0
## 查看容器信息
    docker ps -a 
## 进入容器
    docker exec -it mysql bash
# 3.	安装JDK
## 下载JDK
    https://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.tar.gz?AuthParam=1545201344_455211fda9b1f199a77307207f45b6d5
## 进入容器，安装JDK
    cd /usr/
    mkdir java 
    tar -zxvf jdk-8u191-linux-x64.tar.gz
## 配置环境变量，在 /etc/profile 末尾添加
    #java#
    export JAVA_HOME=/usr/java/jdk1.8.0_191
    export PATH=$JAVA_HOME/bin:$PATH
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib 
    java -version 查看版本
    java version "1.8.0_191"
    Java(TM) SE Runtime Environment (build 1.8.0_191-b12)
    Java HotSpot(TM) 64-Bit Server VM (build 25.191-b12, mixed mode)
# 4.	安装Tomcat
    cd apache-tomcat-7.0.77/
    ./bin/startup.sh
    ./bin/shutdown.sh
# 5.	登陆配置MySQL数据库
## 登陆MySQL测试
    mysql –uroot –p
    Enter password:******
## 连接数据报错 2509
## 登陆mysql 执行下面语句可解决
    ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码';
# 6.	Centos 防火墙开放端口
    firewall-cmd --state 
    systemctl start firewalld.service
    firewall-cmd --zone=public --add-port=80/tcp –permanent
    firewall-cmd --zone=public --add-port=443/tcp --permanent
    firewall-cmd --zone=public --add-port=3306/tcp --permanent
    systemctl restart firewalld.service
    firewall-cmd --reload
## 验证Tomcat Http页面
    http://ip:80
# 7.	生成SSL 证书
## ./keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore /root/https.keystore

    [root@test1 bin]# ./keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore /root/https.keystore 
    输入密钥库口令:  
    再次输入新口令: 
    您的名字与姓氏是什么?
    [Unknown]:  test1.com
    您的组织单位名称是什么?
    [Unknown]:  test1.com
    您的组织名称是什么?
    [Unknown]:  test1.com
    您所在的城市或区域名称是什么?
    [Unknown]:  CD
    您所在的省/市/自治区名称是什么?
    [Unknown]:  SC
    该单位的双字母国家/地区代码是什么?
    [Unknown]:  CN
    CN=test1.com, OU=test1.com, O=test1.com, L=CD, ST=SC, C=CN是否正确?
    [否]:  y
    输入tomcat的密钥口令
    注意：您的名字与姓氏的填写：这里可以填写域名，要是填写域名，需要修改本机配置把域名和本机ip绑定
    <Connector port="443" protocol="org.apache.coyote.http11.Http11Protocol"
         maxThreads="150" 
         SSLEnabled="true" 
         scheme="https" 
         secure="true" 
         clientAuth="false" 
         sslProtocol="TLS"
         keystoreFile="/root/https.keystore" keystorePass="hnga1219@"/>
# 8.	Http自动跳转Https
## 编辑conf/web.xml文件，在web.xml末尾加上如下配置：
    <security-constraint>
    <web-resource-collection >
              <web-resource-name >SSL</web-resource-name>
              <url-pattern>/*</url-pattern>
       </web-resource-collection>
       <user-data-constraint>
       <transport-guarantee>CONFIDENTIAL</transport-guarantee>
       </user-data-constraint>
    </security-constraint>
## 重启Tomcat服务

