# spring-cloud-scaffolding
spring cloud 脚手架集成Spring Cloud(Finchley版本)体系，整合了各微服务常用到的基础组件。
利用spring cloud生态圈里的各种组件，搭建一套完整的可实用的项目架构脚手架，只需稍加改造，就可以作为开发新项目的脚手架工程。

本脚手架目的：减少架构层次的搭建时间,提供一个脚手架可用于拓展，在此脚手架上可以进行业务代码的开发。

## 本项目会覆盖的技术点有：

| 技术 | 名称 | 官网 |
|:-: | :----- | :----- |
| Spring Cloud | 分布式微服务框架	| [https://projects.spring.io/spring-cloud/](https://projects.spring.io/spring-cloud/) |
| Spring Boot	 | 快速应用开发Spring框架	| [https://spring.io/projects/spring-boot/](https://spring.io/projects/spring-boot/) |
| OAuth2 | Oauth2认证服务	 | [https://spring.io/projects/spring-security-oauth/](https://spring.io/projects/spring-security-oauth/) |
| JPA | Java持久层API | [https://spring.io/projects/spring-data-jpa](https://spring.io/projects/spring-data-jpa) |
| Redis | 分布式缓存数据库 | [https://redis.io/](https://redis.io/) |
| Swagger2 | REST API 接口测试框架 | [http://swagger.io/](http://swagger.io/) |
| Maven	 | 项目构建管理	 | [http://maven.apache.org/](http://maven.apache.org/) |
|Spring Boot Admin| 分布式微服务监控中心	| [https://github.com/codecentric/spring-boot-admin/](https://github.com/codecentric/spring-boot-admin/)|
|Hystrix-dashboard|Hystrix的仪表盘组件|[https://github.com/spring-cloud-samples/hystrix-dashboard/](https://github.com/spring-cloud-samples/hystrix-dashboard/)
|Turbine|Hystrix熔断聚合组件|[https://github.com/spring-cloud-samples/turbine/](https://github.com/spring-cloud-samples/turbine/)|
|Zipkin	|分布式链路跟踪系统|[https://zipkin.io/](https://zipkin.io/)|
|RabbitMQ|消息中间件	|[https://www.rabbitmq.com/](https://www.rabbitmq.com/)|
|Ratelimit|网关限流框架|[https://github.com/marcosbarbero/spring-cloud-zuul-ratelimit/](https://github.com/marcosbarbero/spring-cloud-zuul-ratelimit/)|
|Kafka|分布式消息队列|[http://kafka.apache.org/](http://kafka.apache.org/)|
|Elasticsearch|搜索引擎|[https://www.elastic.co/cn/products/elasticsearch](https://www.elastic.co/cn/products/elasticsearch)|
|Kibana|ES分析和搜索仪表板|[https://www.elastic.co/cn/products/kibana](https://www.elastic.co/cn/products/kibana)|

PS：没有集成 spring cloud config 是因为实用性不好，我所了解到部分的开源的配置中心的功能比cloud config好很多，后续我会集成来自携程的apollo来作为配置中心。

* Spring Cloud Netflix Zuul网关服务器 <br>
* Spring Cloud Netflix Eureka发现服务器 <br>
* Spring Cloud Netflix Turbine断路器监控 <br>
* Spring Cloud Sleuth + Zipkin服务调用监控 <br>
* Sping Cloud Stream + RabbitMQ做异步消息 <br>
* Spring Data JPA做数据访问 <br>
* Spring Cloud Security + Oauth2.0做授权与资源保护 <br>


## 本项目使用的依赖版本是：
* Spring Cloud - Finchley.RELEASE <br>
* Spring Data - Lovelace-RELEASE <br>
* Spring Cloud Stream - Fishtown.M3 <br>
* Spring Boot - 2.0.3.RELEASE <br>



## 项目模块介绍：
* spring-cloud-eureka-server：平台服务注册与发现服务中心。 <br>
* spring-cloud-zuul-server:zuul网关服务 <br>
* spring-cloud-auth-server:授权服务。 <br>
* spring-cloud-turbine-server:断路器监控，用于汇总Hystrix服务断路器监控流。 <br>
* spring-cloud-turbine-mq:turbine的MQ模式<br>
* spring-cloud-admin-server:集成spring-boot-admin，用于对服务的监控，查看配置属性，日志的管理等，详见：[GITHUB:spring-boot-admin](https://github.com/codecentric/spring-boot-admin) <br>
* spring-cloud-common：接口共享方式实现的API项目，API项目不包含任何服务端实现，因此这里只是引入了feign组件。在API接口项目中，我们一般定义，一是服务接口定义，二是传输数据DTO定义,三是公共的基础类。 <br>
* spring-cloud-investservice-server:业务服务模块 <br>
* spring-cloud-projectservice-server:业务服务模块 <br>
* spring-cloud-userservice-server:业务服务模块<br>
* spring-cloud-payservice-server:支付平台服务模块 涉及支付相关逻辑代码放入其中，目前只集成了微信的jsApi方式调用支付的方式。<br>
* spring-cloud-projectservice-listener：业务服务模块，mq监听服务，可以和project-server合并，此处用于方便理解，进行拆分。 <br>
* zipkin，用于做服务调用监控、收集分布式追踪信息，spring cloud 升级到Finchley.RELEASE版本、spring boot升级到2.XX版本后，zipkin官网不建议自行集成，所以提供了下载jar包进行部署的方式。搭建方式：[GITHUB:zipkin](https://github.com/openzipkin/zipkin),jar包下载地址：[zipkinjar包下载](https://dl.bintray.com/openzipkin/maven/io/zipkin/java/zipkin-server/) 此处使用2.11.6版本。 <br>

=======================================================
### 一、平台服务注册与发现服务中心(spring-cloud-eureka-server)
详情链接：[详细说明](https://blog.csdn.net/qq_35551089/article/details/98076109)
```
eureka-service支持单点和集群模式
1、单点：http://localhost:8865/eureka/
2、集群：
因为在同一台机器上其中三个集群,所以需要配置下hosts文件
加入：
127.0.0.1 cluster1
127.0.0.1 cluster2
127.0.0.1 cluster3

集群cluster1启动： java -jar spring-cloud-eureka-server-1.0-SNAPSHOT.jar --spring.profiles.active=cluster1
对应服务：http://localhost:8861/eureka/

集群cluster2启动： java -jar spring-cloud-eureka-server-1.0-SNAPSHOT.jar --spring.profiles.active=cluster2
对应服务：http://localhost:8862/eureka/

集群cluster3启动： java -jar spring-cloud-eureka-server-1.0-SNAPSHOT.jar --spring.profiles.active=cluster3
对应服务：http://localhost:8863/eureka/
```
### 二、网关服务(spring-cloud-zuul-server)
详情链接：[zuul网关服务](https://blog.csdn.net/qq_35551089/article/details/98079739)

大体功能如下：
* 路由分发
* 动态路由配置与更新
* Ribbon均衡负载
* 统一的zuul网关层异常处理
* 请求限流
* 服务层级熔断降级
* Swagger API文档
* zuul过滤器
* zuul网关层作为资源服务器进行身份认证(oauth2.0)
### 三、授权服务(spring-cloud-auth-server)
详情链接:[oauth2.0授权服务](https://blog.csdn.net/qq_35551089/article/details/98081110)

利用Spring Security + oauth2.0 + JWT来实现的授权服务
* 客户端信息以及用户信息按照生产环境模拟，将其保存在了数据库中。
* 自定义了Token增强器，在其负荷部分加入自定义内容
* 使用JWT来实现token的生成
* 自定义根据手机号验证码和手机号密码的形式授权的grant_type，可基本满足于移动端的开发
### 四、hystrix监控服务(spring-cloud-turbine-server)
详情链接:[hystrix监控](https://blog.csdn.net/qq_35551089/article/details/98081452)

* 集成hystrix-dashboard+turbine来监控hystrix的熔断等情况
* 监控的服务为业务服务investservice,userservice,projectservice,projectservice-listener

### 五、hystrix监控服务MQ方式(spring-cloud-turbine-mq)
详情链接:[hystrixMQ监控](https://blog.csdn.net/qq_35551089/article/details/98081452)

* spring-cloud-turbine-server是监控端主动去各个被监控的服务获取 host:port/actuator/hystrix.stream接口数据，该监控服务使用的为各个被监控服务通过rabbitMQ主动上报数据，turbine通过从rabbitMQ中异步获取数据来展示监控页面。生产推荐使用该方式来保证数据的准确性。

###  六、全链路监控
详情链接：[全链路监控](https://blog.csdn.net/qq_35551089/article/details/99447071)

* Sleuth + Zipkin 实现链路监控
* Sleuth + Kafka + Zipkin + Elasticsearch + Kibana 实现链路监控

###  七、SBA服务监控
详情链接：[SBA服务监控](https://blog.csdn.net/qq_35551089/article/details/99840355)

* Spring Boot Admin 实现服务的监控
* 显示 name/id 和版本号
* 显示在线状态
* Logging 日志级别管理
* JMX beans 管理
* Threads 会话和线程管理
* Trace 应用请求跟踪
* 应用运行参数信息，如：Java 系统属性、Java 环境变量属性、内存信息、Spring 环境属性


## 表结构：
* user_auth表用于管理端oauth2的用户信息记录。<br>
* member_auth表用于移动端oauth2的会员信息记录。
* role_auth表，存放了用户的权限信息
* oauth_approvals授权批准表，存放了用户授权第三方服务器的批准情况
* oauth_client_details，客户端信息表，存放客户端的ID、密码、权限、允许访问的资源服务器ID以及允许使用的授权模式等信息
* oauth_code授权码表，存放了授权码
* user(会员表)业务表
* project(项目表)业务表
* invest(投资记录表)业务表
表结构如下：

```
CREATE TABLE `user_auth` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `password` varchar(255) DEFAULT NULL,
  `username` varchar(255) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `UK_sb8bbouer5wak8vyiiy4pf2bx` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='用户表';

CREATE TABLE `member` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `password` varchar(255) DEFAULT NULL,
  `username` varchar(255) NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `UK_sb8bbouer5wak8vyiiy4pf2bx` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=83 DEFAULT CHARSET=utf8 COMMENT='移动端会员表';


CREATE TABLE `role_auth` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `authority` varchar(255) NOT NULL,
  `user_id` bigint(20) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `UK_sb8bbouer5wak8vyiiy4pf2bx` (`authority`)
) ENGINE=InnoDB AUTO_INCREMENT=82 DEFAULT CHARSET=utf8 COMMENT='用户角色表';

CREATE TABLE `oauth_approvals` (
  `userId` varchar(256) DEFAULT NULL,
  `clientId` varchar(256) DEFAULT NULL,
  `partnerKey` varchar(32) DEFAULT NULL,
  `scope` varchar(256) DEFAULT NULL,
  `status` varchar(10) DEFAULT NULL,
  `expiresAt` datetime DEFAULT NULL,
  `lastModifiedAt` datetime DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `oauth_client_details` (
  `client_id` varchar(64) NOT NULL,
  `resource_ids` varchar(255) DEFAULT NULL,
  `client_secret` varchar(255) DEFAULT NULL,
  `scope` varchar(255) DEFAULT NULL,
  `authorized_grant_types` varchar(255) DEFAULT NULL,
  `web_server_redirect_uri` varchar(255) DEFAULT NULL,
  `authorities` varchar(255) DEFAULT NULL,
  `access_token_validity` int(11) DEFAULT NULL,
  `refresh_token_validity` int(11) DEFAULT NULL,
  `additional_information` varchar(1000) DEFAULT NULL,
  `autoapprove` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`client_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `oauth_code` (
  `code` varchar(255) DEFAULT NULL,
  `authentication` blob
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `user` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) NOT NULL,
  `available_balance` decimal(10,2) unsigned NOT NULL,
  `frozen_balance` decimal(10,2) unsigned NOT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE `project` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(50) CHARACTER SET utf8mb4 NOT NULL,
  `reason` varchar(50) CHARACTER SET utf8mb4 NOT NULL,
  `borrower_id` bigint(20) unsigned NOT NULL,
  `total_amount` decimal(10,0) unsigned NOT NULL,
  `remain_amount` decimal(10,0) unsigned NOT NULL,
  `status` tinyint(3) unsigned DEFAULT NULL,
  `created_at` datetime DEFAULT NULL,
  `updated_at` datetime DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE TABLE `invest` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `project_id` bigint(20) unsigned NOT NULL,
  `project_name` varchar(50) CHARACTER SET utf8mb4 NOT NULL,
  `investor_id` bigint(20) unsigned NOT NULL,
  `investor_name` varchar(50) CHARACTER SET utf8mb4 NOT NULL,
  `borrower_id` bigint(20) unsigned NOT NULL,
  `borrower_name` varchar(50) CHARACTER SET utf8mb4 NOT NULL,
  `amount` decimal(10,2) unsigned NOT NULL,
  `status` tinyint(4) NOT NULL,
  `created_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;



```
### 表结构数据初始化
定义客户端的授权方式：往oauth_client_details表插入一条名为userservice，密码123456(采用哈希)的password模式、client_credentials。

```
INSERT INTO `oauth_client_details` (`client_id`, `resource_ids`, `client_secret`, `scope`, `authorized_grant_types`, `web_server_redirect_uri`, `authorities`, `access_token_validity`, `refresh_token_validity`, `additional_information`, `autoapprove`) VALUES ('userservice', '', '$2a$10$ZRyWnA9PY8Wn.LPN0DtxKer4NF/COK7asCXOAemZSazliGhlIBVk.', 'service', 'password,client_credentials,refresh_token', NULL, NULL, '7200000', NULL, NULL, 'true');
```
### 业务表数据初始化
```
INSERT INTO `project` (`id`, `name`, `reason`, `borrower_id`, `total_amount`, `remain_amount`, `status`, `created_at`, `updated_at`) VALUES ('1', '投资项目', '借款人', '3', '1000', '1000', '1', '2017-05-31 17:24:15', '2017-06-03 16:56:47');
INSERT INTO `user` (`id`, `name`, `available_balance`, `frozen_balance`, `created_at`, `updated_at`) VALUES ('1', '投资人1', '10000.00', '0.00', '2017-05-31 17:25:02', '2017-05-31 17:25:02');
INSERT INTO `user` (`id`, `name`, `available_balance`, `frozen_balance`, `created_at`, `updated_at`) VALUES ('2', '投资人2', '10000.00', '0.00', '2017-05-31 17:25:02', '2017-05-31 17:25:02');
INSERT INTO `user` (`id`, `name`, `available_balance`, `frozen_balance`, `created_at`, `updated_at`) VALUES ('3', '借款人', '0.00', '0.00', '2017-05-31 17:25:02', '2017-05-31 17:25:02');

```

## 总结：
* Eureka服务注册发现<br>
* Feign服务远程调用<br>
* Hystrix服务断路器<br>
* Turbine断路器监控聚合<br>
* Stream做异步处理<br>
* sleuth和Zipkin服务调用链路监控<br>
* Zuul服务网关和自定义过滤器<br>
* JPA数据访问和Redisson分布式锁<br>
* security做授权与认证<br>
* swagger做为标准的REST API文档使用 详见:[集成Swagger](https://github.com/yipengcheng001/spring-cloud-scaffolding/blob/master/readme/swagger.md)<br>

后续会进行添加配置中心apollo与分布式事务管理中间件，后续也会集成当当的sharding-jdbc数据库中间件以Druid为数据源做分库分表与读写分离的构建。<br>
