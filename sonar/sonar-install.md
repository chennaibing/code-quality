sonar 部署安装文档
==============
## 创建数据库信息
```
mysql> CREATE USER 'sonar' IDENTIFIED BY 'sonar'; 
mysql> GRANT ALL ON sonar.* TO 'sonar'@'localhost' IDENTIFIED BY 'sonar';
mysql> GRANT ALL ON sonar.* TO 'sonar'@'%' IDENTIFIED BY 'sonar';
mysql> FLUSH PRIVILEGES;
mysql> CREATE DATABASE sonar CHARACTER SET utf8 COLLATE utf8_general_ci;
```
## 创建linux 环境信息
```
useradd -d /home/sonar/ -m sonar  -U
mkdir /home/sonar/conf/
mkdir /home/sonar/data/
mkdir /home/sonar/logs/
mkdir /home/sonar/extensions/
chown -R sonar:sonar /home/sonar/
chmod -R 777 /home/sonar/
```
## docker-compose.yml
```
version: "3"
services:
    sonar:
        image: sonarqube:7.5-community
        container_name: dev_sonar
        ports:
           - "172.18.154.136:9100:9000"
           - "172.18.154.136:9092:9092"
        volumes:
            - "/home/sonar/conf:/opt/sonarqube/conf"
            - "/home/sonar/extensions:/opt/sonarqube/extensions"
            - "/home/sonar/logs:/opt/sonarqube/logs"
            - "/home/sonar/data:/opt/sonarqube/data"
        environment:
          sonar.jdbc.username: sonar #用户密码
          sonar.jdbc.password: sonar   #创建用户
          sonar.jdbc.url: "jdbc:mysql://x.x.x.x:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance&useSSL=false"
        restart: always

```

