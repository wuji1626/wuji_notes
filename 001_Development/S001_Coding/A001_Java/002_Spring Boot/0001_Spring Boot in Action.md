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
如果选择gradle，生成的工程将是gradle风格的，带有build.gradle等文件  
- 通过Spring Tool Suite使用
- 通过IntelliJ IDEA使用
IntelliJ中，新建工程的选项中有Spring Initializr选项  
![](./img/005.png)  
选择Default:https://start.spring.io，下一步  
![](./img/006.png)  
填写项目相关的信息，下一步  
![](./img/007.png)  
选择工程中依赖的jar包  
![](./img/008.png)  
选择工程存放的物理路径，Finish
![](./img/009.png)  
工程创建完成  
- 通过Spring Boot CLI使用
Spring Boot CLI的init命令可以创建项目基线：
■spring init：下载一个demo.zip工程  
■spring init -dweb,jpa,security：工程依赖web、jpa、security包，同样会下载一个demo.zip。其中-d可以换成--dependencies  
■spring init -dweb,jpa,security --build gradle：工程构建类型是gradle，如果不指定默认是maven  
■spring init -dweb,jpa,security --build gradle -p war：最终发布的包是war包，如果不指定默认是打成jar包  
■spring init -dweb,jpa,security --build gradle -p war myapp：最后一个参数说明希望把项目解压到myapp目录  
■spring init -dweb,jpa,security --build gradle -p jar -x：最后一个参数说明要将羡慕解压到当前目录，-x可以替换为--extract  
■spring help init：查看init命令的参数  
■spring init -l：查看init命令的可选项，-l可替换成--list
##3 应用开发
###3.1 创建一个样例工程framework
1. 依赖：web、jpa、H2、thymeleaf，采用gradle构建  
![](img/010.png)  
2. XXXApplication有两个作用：配置、启动引导  
■@SpringBootApplication：开启组件扫描和自动配置。该注解将@Configuration、@ComponentScan、@EnableAutoConfiguration三个注解组合在一起  
■@Configuration：该类使用Spring基于Java的配置，而不是XML配置  
■@ComponentScan：启用组件扫描，Web控制类和其他组件才能被自动发信并主持为Spring应用程序上下文里的Bean  
■@EnableAutoConfiguration：开启Spring Boot自动配置，可以不用再写大量的配置  
■XXXApplication（在我创建的样例工程中，由于Artifact为framework，因此XXXApplication为FrameworkApplication）：是一个启动类，要运行Spring Boot有多种方式。①可以打成war包，部署；②在main()方法中可以在命令行中把该应用程序当成jar文件运行，向SpringApplication.run()穿衣第一XXXApplication类引用以及一个命令行参数来启动应用程序  
3. XXXApplicationTests测试类
Initializr测试类骨架，可以编写测试用例  
4. 配置应用程序属性  
Initializr生成的工程中有一个application.properties文件，是个空文件。可以在该文件中添加配置。
如添加：server.port=8000  
![](img/016.png)  
5. 工程试运行  
■build工程  
![](img/011.png)  
■在编译工程后，Run工程。由于编译完成，在run菜单中可以选择XXXApplication（FrameworkApplication）  
![](img/012.png)  
在弹出的菜单中，可以找到XXXApplication（FrameworkApplication）  
![](img/013.png)  
控制台输出正常输出  
![](img/014.png)  
在浏览器中输入`http://localhost:8080/framework` 由于工程中没有控制类，都是404错误，不过可见容器已经运行  
![](img/015.png)  

###3.2 代码编写
####3.2.1 编写领域模型  
~~~java
package com.wuji1626.domain;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
@Entity
public class Book {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    private String reader;
    private String isbn;
    private String title;
    private String author;
    private String description;
    public Long getId() {
        return id;
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getReader() {
        return reader;
    }
    public void setReader(String reader) {
        this.reader = reader;
    }
    public String getIsbn() {
        return isbn;
    }
    public void setIsbn(String isbn) {
        this.isbn = isbn;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getAuthor() {
        return author;
    }
    public void setAuthor(String author) {
        this.author = author;
    }
    public String getDescription() {
        return description;
    }
    public void setDescription(String description) {
        this.description = description;
    }
}
~~~
####3.2.2 编写仓库接口（Dao）
~~~java
package com.wuji1626.dao;

import com.wuji1626.domain.Book;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.List;

/**
 * Created by Administrator on 2017/7/17.
 */
public interface ReadingListRepository extends JpaRepository<Book, Long> {
    List<Book> findByReader(String reader);
}

}
~~~
####3.2.3 Web控制器（Controller）
~~~java
package com.wuji1626.controller;

import com.wuji1626.dao.ReadingListRepository;
import com.wuji1626.domain.Book;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.List;

/**
 * Created by Administrator on 2017/7/17.
 */
@Controller
@RequestMapping("/")
public class ReadingListController {
    private ReadingListRepository readingListRepository;
    @Autowired
    public ReadingListController(
            ReadingListRepository readingListRepository) {
        this.readingListRepository = readingListRepository;
    }
    @RequestMapping(value="/{reader}", method= RequestMethod.GET)
    public String readersBooks(
            @PathVariable("reader") String reader,
            Model model) {
        List<Book> readingList =
                readingListRepository.findByReader(reader);
        if (readingList != null) {
            model.addAttribute("books", readingList);
        }
        return "readingList";
    }
    @RequestMapping(value="/{reader}", method=RequestMethod.POST)
    public String addToReadingList(
            @PathVariable("reader") String reader, Book book) {
        book.setReader(reader);
        readingListRepository.save(book);
        return "redirect:/{reader}";
    }
}

~~~
####3.2.4 页面模板
~~~HTML
<html>
<head>
    <title>Reading List</title>
    <link rel="stylesheet" th:href="@{/style.css}"></link>
</head>
<body>
<h2>Your Reading List</h2>
<div th:unless="${#lists.isEmpty(books)}">
    <dl th:each="book : ${books}">
        <dt class="bookHeadline">
            <span th:text="${book.title}">Title</span> by
            <span th:text="${book.author}">Author</span>
            (ISBN: <span th:text="${book.isbn}">ISBN</span>)
        </dt>
        <dd class="bookDescription">
<span th:if="${book.description}"
      th:text="${book.description}">Description</span>
            <span th:if="${book.description eq null}">
No description available</span>
        </dd>
    </dl>
</div>
<div th:if="${#lists.isEmpty(books)}">
    <p>You have no books in your book list</p>
</div>
<hr/>
<h3>Add a book</h3>
<form method="POST">
    <label for="title">Title:</label>
    <input type="text" name="title" size="50"></input><br/>
    <label for="author">Author:</label>
    <input type="text" name="author" size="50"></input><br/>
    <label for="isbn">ISBN:</label>
    <input type="text" name="isbn" size="15"></input><br/>
    <label for="description">Description:</label><br/>
    <textarea name="description" cols="80" rows="5">
</textarea><br/>
    <input type="submit"></input>
</form>
</body>
</html>
~~~
####3.2.5 静态资源
静态资源包括css  
~~~css
body {
    background-color: #cccccc;
    font-family: arial,helvetica,sans-serif;
}
.bookHeadline {
    font-size: 12pt;
    font-weight: bold;
}
.bookDescription {
    font-size: 10pt;
}
label {
    font-weight: bold;
}
~~~
####3.2.6 测试运行
上述代码编写完成后，运行应用即可运行。由于controller中未指定的地址为"/"，则在浏览器中按如下方式运行  
![](img/017.png)  
如浏览器中url所示，111代表用户，表明111 reader有3本书  
###3.3 Spring Boot的运行原理
应用程序加入Spring Boot时，应用中会增加一个spring-boot-autoconfigure的jar包，其中包括了Thymeleaf、JPA、SpringMVC等组件的默认配置。每个配置类都放在Classpath里。Spring 4.0提供条件化配置，在满足某些特定条件前忽略默认配置  
只需要实现Condition接口，覆盖match()方法。当某些特定配置信息存在Classpath下，某些Bean才被创建  
自动配置中使用的条件化注解：  
条件化注解|配置生效条件
---|----
@ConditionalOnBean|配置了某个特定Bean
@ConditionalOnMissingBean|没有配置特定的Bean
@ConditionalOnClass|Classpath里有指定的类
@ConditionalOnMissingClass|Classpath里缺少指定的类
@ConditionalOnExpression|给定的Spring Expression Language（ SpEL）表达式计算结果为true
@ConditionalOnJava|Java的版本匹配特定值或者一个范围值
@ConditionalOnJndi|参数中给定的JNDI位置必须存在一个，如果没有给参数，则要有JNDI InitialContext
@ConditionalOnProperty|指定的配置属性要有一个明确的值
@ConditionalOnResource|Classpath里有指定的资源
@ConditionalOnWebApplication|这是一个Web应用程序
@ConditionalOnNotWebApplication|这不是一个Web应用程序

Spring Boot完成了自动配置Spring的责任  

##4 自定义配置
###4.1 覆盖配置
■如果在工程中增加一个security组件的依赖，编译器将spring security组件
gradle dependencies中增加`compile("org.springframework.boot:spring-boot-starter-security")`  
■运行工程后，在日志中会出现一段随机生成的密码日志  
![](img/018.png)  
■再次进入样例工程，有一个简陋的输入用户名、密码的对话框，默认的用户名为user、密码为日志输出的密码。  
![](img/019.png)  
验证后，与之前页面效果一致  
■要自定义安全配置，只需要忽视默认配置，编写自己的配置即可  
自定义配置类：  
~~~java
package com.wuji1626.config;

import com.wuji1626.dao.ReaderRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.web.HttpSecurityBuilder;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfiguration;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;

/**
 * Created by Administrator on 2017/7/23.
 */
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Autowired
    private ReaderRepository readerRepository;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .authorizeRequests()
                .antMatchers("/")
                .access("hasRole('READER')")
                .antMatchers("/**").permitAll()
                .and()
                .formLogin()
                .loginPage("/login")
                .failureUrl("login?error=true");

    }
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception{
        auth.userDetailsService(new UserDetailsService() {
            @Override
            public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
                return readerRepository.findOne(username);
            }
        });
    }
}
~~~
增加实体类，继承了Security中UserDetails  
~~~java
package com.wuji1626.domain;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import javax.persistence.Entity;
import javax.persistence.Id;
import java.io.Serializable;
import java.util.Arrays;
import java.util.Collection;

/**
 * Created by Administrator on 2017/7/23.
 */
@Entity
public class Reader implements UserDetails {
    private static final long serialVersionUID = 1L;
    @Id
    private String username;
    private String fullname;
    private String password;
    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return Arrays.asList(new SimpleGrantedAuthority("READER"));
    }

    @Override
    public String getPassword() {
        return null;
    }

    @Override
    public String getUsername() {
        return null;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }

    public String getFullname() {
        return fullname;
    }

    public void setFullname(String fullname) {
        this.fullname = fullname;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}

~~~

Dao层的Repository的实现  
~~~java
package com.wuji1626.dao;

import com.wuji1626.domain.Reader;
import org.springframework.data.jpa.repository.JpaRepository;

/**
 * Created by Administrator on 2017/7/23.
 */
public interface ReaderRepository extends JpaRepository<Reader,String> {
}
~~~

■Spring Boot多种设置行为的途径，Spring Boot能从多重属性源获得属性：  
1. 命令行参数  
2. java:comp/env里的JNDI属性  
3. JVM系统属性  
4. 操作系统环境变量  
5. 随机生成的带random.*前缀的属性（在设置其他属性时，可以引用它们，如${random.long}）  
6. 应用程序外的application.properties或application.yml文件  
7. 打包在应用程序内的application.properties或application.yml文件  
8. 通过@PropertySource标注的属性源  
9. 默认属性  

上述的顺序是按照优先级从高到低  

■application.properties或application.yml文件放置的位置：  
1. 外置，在相对应用程序运行目录的/config子目录  
2. 外置，在应用程序运行目录  
3. 内置，在config包内  
4. 内置，在Classpath根目录  

###4.2 排除Logback使用Log4j
在build.gradle中添加：  
~~~
configurations {
	all*.exclude group:'org.springframework.boot',
	module:'spring-boot-starter-logging'
}
~~~
在buidl.gradle的dependencies中添加：  
~~~
compile("org.springframework.boot:spring-boot-starter-log4j")
~~~
重新运行工程，日志输出由Logback改为Log4j，但是由于slf4j没有配置完成报异常与警告  
![](img/021.png)  
将log4j的依赖改为log4j2：
~~~
compile("org.springframework.boot:spring-boot-starter-log4j2")
~~~
日志输出与logback模式一致  
![](img/022.png)  

###4.3 Logback日志配置的
■在resources目录下创建logback.xml配置日志的输出格式：  
~~~xml
<configuration>
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>
                %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
            </pattern>
        </encoder>
    </appender>
    <logger name="root" level="INFO"/>
    <root level="INFO">
        <appender-ref ref="STDOUT" />
    </root>
</configuration>
~~~

##5 Spring Boot核心技术
###5.1 基本配置
####5.1.1 入口类和@SpringBootApplication
SpringBoot都包含一个*Application的入口类，其中包括main方法  
@SpringBootApplication是SpringBoot核心注解。组合了@Configuration、@EnableAutoConfiguration、@ComponentScan。




