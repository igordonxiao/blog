---
title: "Spring Cloud Gateway响应式网关"
date: "2019-12-26"
categories: [微服务]
tags: [
    "Spring",
]
---

# 前言

随着Zuul 1.0的不再维护，Spring官方开发了基于WebFlux编程模型的Spring Cloud Gateway来作为微服务的网关。

WebFlux是随着Spring5推出的响应式编程框架，底层是基于Netty、Reactor Core、Reactive Streams等技术，同时也大量使用到了函数式编程范式。

Spring Cloud Gateway里有`三个`核心的概念：

1. 过滤器
2. 断言
3. 路由

## 过滤器

Spring Cloud Gateway做为网关，是通过一系列的过滤器对请求和响应进行处理，这些过滤器形成了一个链条，称之为过滤器链。

Spring Cloud Gateway的过滤器分为两种：   
     
+ 全局过滤器 

+ 局部过滤器 

顾名思义，全局过滤器回作用到每一个请求上，而局部过滤器只作用于配置了的路由上。

## 断言

Spring Cloud Gateway是通过predicate(断言)来匹配请求的，如可以根据请求的URL进行匹配，请求的动词进行匹配，请求头进行匹配。

> 这里所说的断言其实就是Java8 java.util.function包下的Predicate<T>，它提供一个泛型参数，返回一个Boolean类型，表示传入的参数是否能匹配上某种条件。
    
    
## 路由

一个路由可以粗略地理解为一个请求的表现方式。它的类是在`org.springframework.cloud.gateway.route`包下的`RouteDefinition`定义的。它包含了
一个由`UUID`来表示的ID，一系列的断言、一系列的过滤器、uri地址、顺序和用户可自定义的元数据组成。
```java
public class RouteDefinition {

	@NotEmpty
	private String id = UUID.randomUUID().toString();
	@NotEmpty
	@Valid
	private List<PredicateDefinition> predicates = new ArrayList<>();
	@Valid
	private List<FilterDefinition> filters = new ArrayList<>();
	@NotNull
	private URI uri;
	private Map<String, Object> metadata = new HashMap<>();
	private int order = 0;
    // .....
}

```

# 实践

## 引入相关依赖

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
            <version>2.2.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-pool2</artifactId>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.9</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
```

## 自定义一个新的路由

```yaml
server:
  port: 8080 # 监听端口
spring:
  cloud:
    gateway:
      routes: # 路由集合
        - id: test # 路由的ID
          uri: http://abc.com # 转发到后端的服务
          predicates: # 断言集合
            - Path=/api/orders # 根据path进行匹配
            - Method=GET # 根据请求方法进行匹配
```
根据上面的配置，网关启动后监听到`8080`端口。当前端以`GET`方法访问`http://localhost:8080/api/orders`的时候，网关就会将这个请求转发到`http://abc.com/api/orders`上进行处理。

## 跨域配置

```yaml
spring:
  cloud:
    gateway:
      globalcors:
        corsConfigurations:
          '[/**]':
            allowed-origins: # Access-Control-Allow-Origin头中携带了服务器端验证后的允许的跨域请求域名，可以是一个具体的域名或是一个*（表示任意域名）。
              - "http://localhost:8081"
            allowed-headers: "*"
            exposed-headers: # Access-Control-Expose-Headers头用于允许返回给跨域请求的响应头列表，在列表中的响应头的内容，才可以被浏览器访问。
              - "X-Powered-By"
            allow-credentials: true
            allowed-methods:  # Access-Control-Allow-Methods用于告知浏览器可以在实际发送跨域请求时，可以支持的请求方法，可以是一个具体的方法列表或是一个*（表示任意方法）。
              - GET
              - POST
              - OPTIONS
            max-age: 3600 # Access-Control-Max-Age用于告知浏览器可以将预先检查请求返回结果缓存的时间，在缓存有效期内，浏览器会使用缓存的预先检查结果判断是否发送跨域请求。

```

## 自定义一个断言

我们自定义一个路由是否启动的断言。及当该断言开启时才向后端服务进行转发，否则直接返回4o4。
在Spring Cloud Gateway中自定一断言（或过滤器）都非常简单，只要按照固定的命名格式即可。
我们创建一个名为`RouteEnableRoutePredicateFactory`的断言工厂。
注意：类的命名必须是`XXXRoutePredicateFactory`, 在类中继承自`AbstractRoutePredicateFactory`即可，如果断言有自己的配置，可以在泛型参数中定义自己的配置。如：
```java
public class RouteEnableRoutePredicateFactory extends AbstractRoutePredicateFactory<RouteEnableRoutePredicateFactory.Config> {
    private static final Logger log = LoggerFactory.getLogger(RouteEnableRoutePredicateFactory.class);
    // 在路由中配置断言时以该字段作为断言名称
    public static final String ROUTE_ENABLE = "RouteEnable"; 

    public RouteEnableRoutePredicateFactory() {
        super(Config.class);
    }
    @Override
    public List<String> shortcutFieldOrder() {
        return Collections.singletonList(ROUTE_ENABLE);
    }
    @Override
    public Predicate<ServerWebExchange> apply(Config config) {
        return new GatewayPredicate() {
            private ServerWebExchange exchange;

            @Override
            public boolean test(ServerWebExchange exchange) {
                this.exchange = exchange;
                if (log.isDebugEnabled()) {
                    log.debug(this.toString());
                }
                return config.getRouteEnable();
            }
            @Override
            public String toString() {
                return String.format("%s enable status: %s", this.exchange.getRequest().getPath().value(), config.getRouteEnable());
            }
        };
    }
    /**
     * 路由是否生效配置
     */
    public static class Config {
        private Boolean routeEnable;
        public Boolean getRouteEnable() {
            return routeEnable;
        }
        public void setRouteEnable(Boolean routeEnable) {
            this.routeEnable = routeEnable;
        }
    }
}
```

配置：
```yaml
spring:
  cloud:
    gateway:
      routes: # 路由集合
        - id: test # 路由的ID
          uri: http://abc.com # 转发到后端的服务
          predicates: # 断言集合
            - RouteEnable=true # 路由是否生效的断言
```

## 自定义一个过滤器

我们来定义一个记录请求耗时的一个过滤器。
先定义一个名为`TimeSpentFilter`的类，这个类需要实现`GatewayFilter`类和`Ordered`类。
```java
@Slf4j
public class TimeSpentFilter implements GatewayFilter, Ordered {
    private static final String TIME_SPENT = "timeSpent";

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        exchange.getAttributes().put(TIME_SPENT, System.currentTimeMillis());
        return chain.filter(exchange).then(
                Mono.fromRunnable(() -> {
                    Long startTime = (Long) exchange.getAttributes().get(TIME_SPENT);
                    log.info("spent time : {}", System.currentTimeMillis() - startTime);
                })
        );
    }
    @Override
    public int getOrder() {
        return Ordered.LOWEST_PRECEDENCE;
    }
}
```

再通过一个过滤器工厂将其注入到Spring环境中。类的命名要以`GatewayFilterFactory`结尾并实现`GatewayFilterFactory`类。
```java
@Component
public class TimeSpentGatewayFilterFactory implements GatewayFilterFactory {
    @Override
    public GatewayFilter apply(Object config) {
        return new TimeSpentFilter();
    }
    @Override
    public Class getConfigClass() {
        return this.getClass();
    }
}

```

配置使用：
```yaml
spring:
  cloud:
    gateway:
      routes: # 路由集合
        - id: test # 路由的ID
          uri: http://abc.com # 转发到后端的服务
          predicates: # 断言集合
            - RouteEnable=true # 路由是否生效的断言
          filters:
            - name: TimeSpent # 耗时统计过滤器
```

## 动态路由配置

刚才我们的配置都是存放在`application.yml`文件中的，每次配置了路由都要重启网关，这对于网关来说是不可接受的。我们可以引入配置中心来将配置动态化。
将携程的Apollo作为配置中心是个不错的选择。简化起见，这里我们以Spring Config Server作为配置中心。我们不使用Git来存储而是使用`MySQL`数据库来存储配置信息。

新建一个Config Server项目并引入一下依赖：
```xml
 <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

```
配置：
```yaml
server:
  port: 9090
spring:
  cloud:
    config:
      server:
        jdbc:
          sql: SELECT `key`, `value` from `properties` where application=? and profile=? and label=?
      label: master
  profiles:
    active: jdbc
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/spring-config-server?useUnicode=true&characterEncoding=UTF-8
    username: admin
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
logging:
  level:
    root: error
```

数据库中需要有一张表来存储配置信息：
```mysql
DROP TABLE IF EXISTS `properties`;
CREATE TABLE `properties`  (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `key` text CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL,
  `value` text CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL,
  `application` text CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL,
  `profile` text CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL,
  `label` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NULL DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB AUTO_INCREMENT = 221 CHARACTER SET = utf8mb4 COLLATE = utf8mb4_bin ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;

```

在网关处引入spring-cloud-starter-config和spring-boot-starter-actuator：
```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-config</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
            <version>2.2.1.RELEASE</version>
        </dependency>
```
我们需要使用spring-cloud-starter-config来连接Config Server， 使用spring-boot-starter-actuator来达到动态刷新配置。

网关配置：
```yaml
spring:
  cloud:
    config: # 配置中心地址配置
      uri: http://localhost:9090
      label: master
      name: dev
      profile: dev

management: # 使用management来动态刷新配置
  server:
    port: 9008
  endpoint:
    health:
      show-details: always
  endpoints:
    web:
      exposure:
        include: "*"
```
> 注意：上面的配置需要配置在`bootstrap.yml`文件中。    

启动网关，连接上配置中心。修改数据库中的配置信息，使用PostMan以Post方法访问`http://localhost:9008/actuator/refresh`接口就实现了动态刷新。
