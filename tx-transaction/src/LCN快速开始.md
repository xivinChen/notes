# LCN快速开始

> LCN分为TX—Client（tc) 和TX-Manager (tm)
>
>

## 搭建tm(分布式事务服务器)
1. 引入依赖
```$xslt
    <dependency>
      <groupId>com.codingapi.txlcn</groupId>
      <artifactId>txlcn-tm</artifactId>
    </dependency>
```
2. 添加配置
> 这个服务会启动两个端口，一个是为服务的端口号7970，另一个是分布式事务监听端口 8070
```$xslt
spring.application.name=tx-manager
server.port=7970

spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/tx-manager?characterEncoding=UTF-8
spring.datasource.username=root
spring.datasource.password=root

mybatis.configuration.map-underscore-to-camel-case=true
mybatis.configuration.use-generated-keys=true

#tx-lcn.logger.enabled=true
# TxManager Host Ip
#tx-lcn.manager.host=127.0.0.1
# TxClient连接请求端口
#tx-lcn.manager.port=8070
# 心跳检测时间(ms)
#tx-lcn.manager.heart-time=15000
# 分布式事务执行总时间
#tx-lcn.manager.dtx-time=30000
#参数延迟删除时间单位ms
#tx-lcn.message.netty.attr-delay-time=10000
#tx-lcn.manager.concurrent-level=128
# 开启日志
#tx-lcn.logger.enabled=true
#logging.level.com.codingapi=debug
#redis 主机
#spring.redis.host=127.0.0.1
#redis 端口
#spring.redis.port=6379
#redis 密码
#spring.redis.password=
```

3. 启动类加入注解
```$xslt
@SpringBootApplication
@EnableTransactionManagerServer
public class TxApp
{
    public static void main( String[] args )
    {
        SpringApplication.run(TxApp.class,args);
    }
}
```


## 搭建tc(即在为服务引入分布式事务服务并使用事务)
1. 引入依赖
```$xslt
        <dependency>
            <groupId>com.codingapi.txlcn</groupId>
            <artifactId>txlcn-tc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>com.codingapi.txlcn</groupId>
            <artifactId>txlcn-txmsg-netty</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
```

2. 添加配置
```$xslt
spring:
  application:
    name: bs-ccr-provider-combo
  main:
    allow-bean-definition-overriding: true
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
  transaction:
    rollback-on-commit-failure: true

tx-lcn:
  client:
    manager-address: 127.0.0.1:8070
```

3. 在启动类加入注解
```$xslt
@EnableDistributedTransaction
```

4. 在代码中加入事务

```$xslt
@LcnTransaction
```

### 注意，加入A服务引用了B服务，配置和TC一样，但在代码中加入注解小有不同
```$xslt
//被调用服务的方法声明加多了(propagation = DTXPropagation.SUPPORTS)
@LcnTransaction(propagation = DTXPropagation.SUPPORTS)
```

#官方教程：http://www.txlcn.org/zh-cn/docs/start.html