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
提供在运行时检视应用程序内部情况的能力，如：
■Spring应用程序上下文装配的Bean  
■Spring Boot自动配置做的决策  
■应用程序取到的环境变量、系统属性、配置属性和命令行参数  
■应用程序里线程的当前状态  
■应用程序最近处理过的HTTP请求的追踪情况  
##2 Spring Boot实操入门
###2.1 Spring Boot CLI
在【http://repo.spring.io/snapshot/org/springframework/boot/spring-boot-cli】可以下载到各个版本的Spring Boot CLI  
下载解压后，将Spring Boot CLI的bin目录配置到环境变量中，执行Spring --version测试是否配置成功  
![](./img/001.png)  

###2.2 Spring Initializr
Spring Initializr本质上是个Web应用，能生成Spring Boot项目结构，提供基本的项目结构，以及一个pom  
Spring Initializr的几种用法：  
- 通过Web界面使用
访问【start.spring.io】进入Initializr配置页面，进行配置并选择相应的依赖后便可以下载一个工程骨架  
![](./img/002.png)  
生成的工程下的路径符合maven工程的结构。而且工程的结构都是按照group和artifact生成  
![](./img/003.png)  
在工程的根目录下有pom、maven的脚本mvnw.cmd  
![](./img/004.png)  
- 通过Spring Tool Suite使用
- 通过IntelliJ IDEA使用
- 通过Spring Boot CLI使用



