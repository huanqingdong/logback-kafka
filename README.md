## 日志写入kafka通用模块

支持[springboot](#springboot项目使用) 和 [非springboot](#非springboot项目使用)两种类型的java项目


### springboot项目使用

#### 1. 引入maven依赖

在`pom.xml`中添加如下配置
```xml
<dependency>
    <groupId>tech.nosql</groupId>
    <artifactId>logback-kafka</artifactId>
    <version>1.0</version>
</dependency>
```

#### 2. 添加logback-spring.xml配置

在`src/main/resources`下添加logback-spring.xml文件,内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false" scan="true" scanPeriod="30 second">
    <include resource="logback-boot.xml"/>

    <logger name="org.apache.kafka" level="WARN"/>
    <logger name="com.weichai" level="DEBUG"/>

</configuration>
```

- `logback-base.xml`介绍
- root日志级别为INFO
    - 使用springboot中的属性控制日志输出
    
#### 3. 添加与环境对应的配置文件

- application-dev.properties格式
```properties
# 是关闭控制台日志输出[true|false],默认false
log.stdout.disable=true
# 文件日志根路径,只要配置了根路径就会开启文件日志
# 完整路径为 ${log.base-path}/${log.kafka.system}/{yyyy-mm-dd}.log
log.base-path=D://logs

# 日志写入kafka的服务器地址,只要配置此属性,便会开启日志写入kafka
log.kafka.bootstrap-servers=192.168.1.14:9092
# log.kafka.bootstrap-servers: kafka-prod01.nosql.tech:9092,kafka-prod02.nosql.tech:9092,kafka-prod03.nosql.tech:9092
# log.kafka.sasl-jaas-config: com.sun.security.auth.module.Krb5LoginModule required serviceName="kafka" useKeyTab=true keyTab="/etc/cert/kafkaUser.keytab" principal="kafkaUser";
# 日志写入kafka主题名
log.kafka.topic=log_kafka_dev
# 应用名称,用于标识文件路径以及es中的索引名称
log.kafka.system=logback-kafka-demo
```

- application-dev.yml格式
```yaml
log:
  stdout:
    # 是关闭控制台日志输出[true|false],默认false
    disable: true
  # 文件日志根路径,只要配置了根路径就会开启文件日志
  # 完整路径为 ${log.base-path}/${log.kafka.system}/{yyyy-mm-dd}.log
  base-path: D://logs

  kafka:
    # 日志写入kafka的服务器地址,只要配置此属性,便会开启日志写入kafka
    bootstrap-servers: 192.168.1.14:9092
    # bootstrap-servers:  kafka-prod01.nosql.tech:9092,kafka-prod02.nosql.tech:9092,kafka-prod03.nosql.tech:9092
    # sasl-jaas-config: com.sun.security.auth.module.Krb5LoginModule required serviceName="kafka" useKeyTab=true keyTab="/etc/cert/kafkaUser.keytab" principal="kafkaUser";
    # 日志写入kafka主题名
    topic: log_kafka_dev
    # 应用名称,用于标识文件路径以及es中的索引名称
    system: logback-kafka-demo
```

### 非springboot项目使用

#### 1. 引入maven依赖

在`pom.xml`中添加如下配置
```xml
<dependency>
    <groupId>tech.nosql</groupId>
    <artifactId>logback-kafka</artifactId>
    <version>1.0</version>
</dependency>
```

#### 2. 添加logback.xml配置

在`src/main/resources`下添加logback.xml文件,内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false" scan="true" scanPeriod="30 second">
    <!-- 指定配置文件 -->
    <property resource="application-${env}.properties"/>
    <include resource="logback-base.xml"/>

    <logger name="org.apache.kafka" level="WARN"/>
    <logger name="com.weichai" level="DEBUG"/>

</configuration>
```

- `logback-base.xml`介绍

    - root日志级别为INFO
    - 需要`application-${env}.properties`进行变量控制
    
#### 3. 添加与环境对应的配置文件

- application-dev.properties
```properties
# 是关闭控制台日志输出[true|false],默认false
log.stdout.disable=true
# 文件日志根路径,只要配置了根路径就会开启文件日志
# 完整路径为 ${log.base-path}/${log.kafka.system}/{yyyy-mm-dd}.log
log.base-path=D://logs

# 日志写入kafka的服务器地址,只要配置此属性,便会开启日志写入kafka
log.kafka.bootstrap-servers=192.168.1.14:9092
# log.kafka.bootstrap-servers: kafka-prod01.nosql.tech:9092,kafka-prod02.nosql.tech:9092,kafka-prod03.nosql.tech:9092
# log.kafka.sasl-jaas-config: com.sun.security.auth.module.Krb5LoginModule required serviceName="kafka" useKeyTab=true keyTab="/etc/cert/kafkaUser.keytab" principal="kafkaUser";
# 日志写入kafka主题名
log.kafka.topic=log_kafka_dev
# 应用名称,用于标识文件路径以及es中的索引名称
log.kafka.system=logback-kafka-demo
```



