[TOC] 

##1 Spring Boot知识
###1.1 Spring Boot的核心
- 自动配置：Sprin的应用程序常见的应用功能，Spring Boot自动提供相关配置
- 起步依赖：告诉Spring Boot要干什么，它自动引入需要的库
只需要org.springframework.boot:spring-boot-starter-web根据依赖传递把其他所需依赖引入项目。只需要引用相关的起步依赖如：jpa、security  
另外，起步依赖不需关心用哪个版本  
- 命令行界面：Spring Boot的可选特性，只需写代码就能完成完整应用程序，无需传统项目构建  
Spring Boot CLI是Spring Boot非必要组成部分
- Actuator：可以深入运行中的Spring Boot应用程序，观察内部情况
提供在运行时


