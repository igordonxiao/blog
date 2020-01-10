---
title: "从pom文件看Spring Boot用到了哪些技术"
date: "2020-01-10"
categories: [Spring]
tags: [
    "Spring"
]
---
Spring Boot越来越流行了，来通过它的`pom`文件看看都使用到了哪些技术。依赖技术的子依赖并没有在本文列出。
通过查看这些依赖的技术，以后在有单独需要的时候也可以有一些思路。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"
         xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath>../spring-boot-parent</relativePath>
    </parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot</artifactId>
    <version>2.2.2.RELEASE</version>
    <name>Spring Boot</name>
    <description>Spring Boot</description>
    <url>https://projects.spring.io/spring-boot/#/spring-boot-parent/spring-boot</url>
    <organization>
        <!--开发Spring的公司名称-->
        <name>Pivotal Software, Inc.</name>
        <!--Spring官网-->
        <url>https://spring.io</url>
    </organization>
    <licenses>
        <license>
            <!--开源协议-->
            <name>Apache License, Version 2.0</name>
            <url>https://www.apache.org/licenses/LICENSE-2.0</url>
        </license>
    </licenses>
    <developers>
        <!-- 开发者信息，还是Pivotal-->
        <developer>
            <name>Pivotal</name>
            <email>info@pivotal.io</email>
            <organization>Pivotal Software, Inc.</organization>
            <organizationUrl>https://www.spring.io</organizationUrl>
        </developer>
    </developers>
    <scm>
        <!--        Spring代码仓库-->
        <connection>scm:git:git://github.com/spring-projects/spring-boot.git</connection>
        <developerConnection>scm:git:ssh://git@github.com/spring-projects/spring-boot.git</developerConnection>
        <url>https://github.com/spring-projects/spring-boot</url>
    </scm>
    <issueManagement>
        <!--        提交bug的地方-->
        <system>Github</system>
        <url>https://github.com/spring-projects/spring-boot/issues</url>
    </issueManagement>
    <dependencies>
        <!--        依赖列表-->
        <dependency>
            <!--            Spirng核心库-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <!--            Spring上下文库-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
        </dependency>
        <dependency>
            <!--            logback日志框架-->
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            多数据源JDBC分布式事物管理-->
            <groupId>com.atomikos</groupId>
            <artifactId>transactions-jdbc</artifactId>
            <version>4.0.6</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            辅助多数据源分布式事物管理消息确认-->
            <groupId>com.atomikos</groupId>
            <artifactId>transactions-jms</artifactId>
            <version>4.0.6</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--         分布式事物管理   -->
            <groupId>com.atomikos</groupId>
            <artifactId>transactions-jta</artifactId>
            <version>4.0.6</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            jackson JSON序列化与反序列化支持-->
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.10.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Google JSON序列化与反序列化支持-->
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.6</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Oracle JDBC驱动连接-->
            <groupId>com.oracle.ojdbc</groupId>
            <artifactId>ojdbc8</artifactId>
            <version>19.3.0.0</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--       jmustache模版支持     -->
            <groupId>com.samskivert</groupId>
            <artifactId>jmustache</artifactId>
            <version>1.15</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            发送邮件-->
            <groupId>com.sendgrid</groupId>
            <artifactId>sendgrid-java</artifactId>
            <version>4.4.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--           HikariCP数据库连接池 -->
            <groupId>com.zaxxer</groupId>
            <artifactId>HikariCP</artifactId>
            <version>3.4.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--         响应式Netty, 作为WebFlux的底层   -->
            <groupId>io.projectreactor.netty</groupId>
            <artifactId>reactor-netty</artifactId>
            <version>0.9.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--          Netty SSL部署支持  -->
            <groupId>io.netty</groupId>
            <artifactId>netty-tcnative-boringssl-static</artifactId>
            <version>2.0.28.Final</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--      rsocket相关依赖      -->
            <groupId>io.rsocket</groupId>
            <artifactId>rsocket-core</artifactId>
            <version>1.0.0-RC5</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>io.rsocket</groupId>
            <artifactId>rsocket-transport-netty</artifactId>
            <version>1.0.0-RC5</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--      Servlet undertow 容器      -->
            <groupId>io.undertow</groupId>
            <artifactId>undertow-servlet</artifactId>
            <version>2.0.28.Final</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>jakarta.jms</groupId>
            <artifactId>jakarta.jms-api</artifactId>
            <version>2.0.3</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        JavaEE jakartaxv相关依赖 -->
        <dependency>
            <groupId>jakarta.servlet</groupId>
            <artifactId>jakarta.servlet-api</artifactId>
            <version>4.0.3</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>jakarta.validation</groupId>
            <artifactId>jakarta.validation-api</artifactId>
            <version>2.0.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            单元测试-->
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>hamcrest-core</artifactId>
                    <groupId>org.hamcrest</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Apache commons 连接池-->
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-dbcp2</artifactId>
            <version>2.7.0</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>commons-logging</artifactId>
                    <groupId>commons-logging</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Apache HTTP Client工具-->
            <groupId>org.apache.httpcomponents</groupId>
            <artifactId>httpclient</artifactId>
            <version>4.5.10</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>commons-logging</artifactId>
                    <groupId>commons-logging</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <!--            日志相关依赖-->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.12.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.12.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        内嵌Tomcat 相关依赖-->
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-core</artifactId>
            <version>9.0.29</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-jasper</artifactId>
            <version>9.0.29</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.apache.tomcat</groupId>
            <artifactId>tomcat-jdbc</artifactId>
            <version>9.0.29</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            又一个做单元测试的-->
            <groupId>org.assertj</groupId>
            <artifactId>assertj-core</artifactId>
            <version>3.13.2</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            又一个做事物管理的-->
            <groupId>org.codehaus.btm</groupId>
            <artifactId>btm</artifactId>
            <version>2.1.4</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>jta</artifactId>
                    <groupId>javax.transaction</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <!--        Groovy脚本语言支持-->
        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy</artifactId>
            <version>2.5.8</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-xml</artifactId>
            <version>2.5.8</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        Jetty容器相关依赖-->
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-servlets</artifactId>
            <version>9.4.24.v20191120</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-util</artifactId>
            <version>9.4.24.v20191120</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-webapp</artifactId>
            <version>9.4.24.v20191120</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty</groupId>
            <artifactId>jetty-alpn-conscrypt-server</artifactId>
            <version>9.4.24.v20191120</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.eclipse.jetty.http2</groupId>
            <artifactId>http2-server</artifactId>
            <version>9.4.24.v20191120</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>javax.servlet-api</artifactId>
                    <groupId>javax.servlet</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            用于做自动化测试的-->
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest</artifactId>
            <version>2.1</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Hibernate ORM框架-->
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.4.9.Final</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>javax.activation-api</artifactId>
                    <groupId>javax.activation</groupId>
                </exclusion>
                <exclusion>
                    <artifactId>javax.persistence-api</artifactId>
                    <groupId>javax.persistence</groupId>
                </exclusion>
                <exclusion>
                    <artifactId>jaxb-api</artifactId>
                    <groupId>javax.xml.bind</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.hibernate.validator</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.0.18.Final</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>validation-api</artifactId>
                    <groupId>javax.validation</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Jboss事物管理相关-->
            <groupId>org.jboss</groupId>
            <artifactId>jboss-transaction-spi</artifactId>
            <version>7.6.0.Final</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            做数据库重构和迁移的工具-->
            <groupId>org.liquibase</groupId>
            <artifactId>liquibase-core</artifactId>
            <version>3.8.2</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>logback-classic</artifactId>
                    <groupId>ch.qos.logback</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            neo4j图形数据库支持-->
            <groupId>org.neo4j</groupId>
            <artifactId>neo4j-ogm-core</artifactId>
            <version>3.2.3</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        SLF4J日志框架-->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jul-to-slf4j</artifactId>
            <version>1.7.29</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.29</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring 消息-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-messaging</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            ORM支持-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            转XML相关功能-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-oxm</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring测试相关-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring Web开发基础包-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring 响应式 Web开发支持-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-webflux</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring Servlet 开发支持-->
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring Web 安全支持-->
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>5.2.1.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            Spring WebSocket开发支持-->
            <groupId>org.springframework.ws</groupId>
            <artifactId>spring-ws-core</artifactId>
            <version>3.0.8.RELEASE</version>
            <scope>compile</scope>
            <exclusions>
                <exclusion>
                    <artifactId>commons-logging</artifactId>
                    <groupId>commons-logging</groupId>
                </exclusion>
            </exclusions>
            <optional>true</optional>
        </dependency>
        <dependency>
            <!--            yaml格式文件解析-->
            <groupId>org.yaml</groupId>
            <artifactId>snakeyaml</artifactId>
            <version>1.25</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        Jetbrains Kotlin语言支持，Spring已经使用Kotlin写了一些代码了，如DSL配置的支持，某些测试代码-->
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-reflect</artifactId>
            <version>1.3.61</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-stdlib</artifactId>
            <version>1.3.61</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
        <!--        用于生成配置文件的提示信息，打包时会扫描用户通过@ConfigurationProperties自定义的配置，引用包后在properties文件或yaml文件中就能又提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <version>2.2.2.RELEASE</version>
            <scope>compile</scope>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>

```
