+++
title = "Spring boot 2 Kotlin WebFlux DSL"
description = ""
categories = [
    "Web"
]
tags = [
    "Spring","Kotlin",
]
date = "2018-03-02 10:23:23"
+++
> Spring Boot + Kotlin + Flux Web + Kotlin DSL

`SpringbootdemoApplication.kt`

```shell
package io.igordonxiao.springbootdemo

import org.springframework.boot.Banner.Mode.OFF
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication
import org.springframework.context.annotation.Bean
import org.springframework.context.annotation.Configuration
import org.springframework.data.annotation.Id
import org.springframework.data.mongodb.core.mapping.Document
import org.springframework.data.mongodb.repository.ReactiveMongoRepository
import org.springframework.http.HttpMethod
import org.springframework.http.HttpStatus
import org.springframework.stereotype.Component
import org.springframework.web.reactive.function.server.ServerResponse.ok
import org.springframework.web.reactive.function.server.router
import org.springframework.web.server.ServerWebExchange
import org.springframework.web.server.WebFilter
import org.springframework.web.server.WebFilterChain
import reactor.core.publisher.Mono
import reactor.core.publisher.toMono


@SpringBootApplication
class SpringbootdemoApplication

@Document
data class User(@Id val id: String, val name: String)

interface IUserRepository : ReactiveMongoRepository<User, String>

@Configuration
class ApiRoutes(private val userRepository: IUserRepository) {

    @Bean
    fun apiRouter() = router {

        "/users".nest {
            GET("/") {
                ok().body(userRepository.findAll(), User::class.java)
            }
            POST("/") {
                ok().body(userRepository.insert(it.bodyToMono(User::class.java)).toMono(), User::class.java)
            }
        }

    }
}

fun main(args: Array<String>) {
    runApplication<SpringbootdemoApplication>(*args) {
        setBannerMode(OFF)
    }
}

```
Kotlin的扩展及DSL让代码显示非常简洁，少了很多模板代码。

[完整示例](https://github.com/igordonxiao/springboot2demo)
