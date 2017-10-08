#Spring实操、实例
[TOC]
##1 Spring配置
###1.1 依赖注入
####1.1.1 基于注解配置依赖注入
1. 功能类
FunctionService.java  
将该功能声明为@Service，让它成为由Spring管理的类  
~~~java
package com.wuji1626.spring.annotation_di.service;
import org.springframework.stereotype.Service;
@Service
public class FunctionService {
    public String sayHello(String word){
        return "Hello " + word + " !";
    }
}
~~~
2. 使用功能类
UseFunctionService.java  
■这个类是使用功能类提供服务的类，将它也声明为Service，由Spring进行管理  
■使用@Autowired将FunctionService Bean注入到UseFunctionService中，让UseFunctionService具备FunctionService的功能  
~~~java
package com.wuji1626.spring.annotation_di.service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service
public class UseFunctionService {
    @Autowired
    FunctionService functionService;

    public String SayHello(String word){
        return functionService.sayHello(word);
    }
}
~~~
3. 配置类
DiConfig.java  
■@Configuration.java声明该类是一个配置类  
■@ComponentScan指定自动扫描注解的包的根  
~~~java
package com.wuji1626.spring.annotation_di.config;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
/**
 * Created by Administrator on 2017/9/29.
 */
@Configuration
@ComponentScan("com.wuji1626.spring.annotation_di")
public class DiConfig {
}
~~~
4. 运行类
~~~java
package com.wuji1626.spring.annotation_di;
import com.wuji1626.spring.annotation_di.config.DiConfig;
import com.wuji1626.spring.annotation_di.service.UseFunctionService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
/**
 * Created by Administrator on 2017/9/29.
 */
public class Main {
    public static void main(String[] args){
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DiConfig.class);

        UseFunctionService useFunctionService = context.getBean(UseFunctionService.class);
        System.out.println(useFunctionService.SayHello("Di"));

        context.close();
    }
}
~~~

####1.1.2 基于Java配置的依赖注入 
1. 功能类Bean
FunctionService.java  
与1.1.1中的区别是该类，没有了@Service的Bean的注解  
~~~java
package com.wuji1626.spring.javaconfig_di.service;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/9/28.
 */
public class FunctionService {
    public String sayHello(String word){
        return "Hello " + word + " !";
    }
}
~~~
2.功能使用类
UseFunctionService.java  
该类的逻辑与1.1.1相同，只是将之前@Service与@Autowired注解去掉  
~~~java
package com.wuji1626.spring.javaconfig_di.service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/9/29.
 */
public class UseFunctionService {
    FunctionService functionService;
    public void setFunctionService(FunctionService functionService) {
        this.functionService = functionService;
    }
    public String SayHello(String word){
        return functionService.sayHello(word);
    }
}
~~~
3. 配置类
JavaConfig.java  
~~~java
package com.wuji1626.spring.javaconfig_di.config;
import com.wuji1626.spring.javaconfig_di.service.FunctionService;
import com.wuji1626.spring.javaconfig_di.service.UseFunctionService;
import org.springframework.context.annotation.Bean;
/**
 * Created by Administrator on 2017/9/29.
 */
public class JavaConfig {
    @Bean
    public FunctionService functionService(){
        return new FunctionService();
    }
    @Bean
    public UseFunctionService useFunctionService(){
        UseFunctionService useFunctionService = new UseFunctionService();
        useFunctionService.setFunctionService(functionService());
        return useFunctionService;
    }
}
~~~
4. 运行类
Main.java
~~~java
package com.wuji1626.spring.javaconfig_di;
import com.wuji1626.spring.javaconfig_di.config.JavaConfig;
import com.wuji1626.spring.javaconfig_di.service.UseFunctionService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
/**
 * Created by Administrator on 2017/9/30.
 */
public class Main {
    public static void main(String[] args){
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(JavaConfig.class);
        UseFunctionService useFunctionService = context.getBean(UseFunctionService.class);
        System.out.println(useFunctionService.SayHello("java config"));
        context.close();
    }
}
~~~
运行结果：  
![](img/R001.png)  

###1.2 AOP
AOP面向切面编程，相对于OOP面向对象编程。让一组类共享相同的行为。在OOP中通过继承类和实现接口实现，同时增加了耦合度，而且Java的继承都是单继承，阻碍更多行为添加到类上  
Spring支持AspectJ注解式切面：  
1. @Aspect：声明一个切面
2. @After、@Before、@Around定义建言（advice），直接将拦截规则（切点）作为参数
3. @After、@Before、@Around参数的拦截规则为切点（PointCut），为了使切点复用，可使用@PointCut专门定义拦截规则，在@After、@Before、@Around的参数中调用
4. 符合条件的每隔被拦截处为连接点（JoinPoint）

样例：  
1. POM依赖
为使用Aspectj，需要增加Aspectj依赖：  
~~~xml
<dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjrt</artifactId>
      <version>1.8.5</version>
    </dependency>
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.8.5</version>
    </dependency>
~~~
2. 拦截规则注解
~~~java
package com.wuji1626.spring.aop;
import java.lang.annotation.*;
/**
 * Created by Administrator on 2017/10/6.
 */
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Action {
    String name();
}
~~~
3. 使用注解的被拦截类
~~~java
package com.wuji1626.spring.aop.service;
import com.wuji1626.spring.aop.Action;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/10/6.
 */
@Service
public class DemoAnnotationService {
    @Action(name="注解式拦截的add操作")
    public void add(){}
}
~~~
4. 使用方法规则被拦截类
~~~java
package com.wuji1626.spring.aop.service;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/10/6.
 */
@Service
public class DemoMethodService {
    public void add(){}
}
~~~
5. 切面
~~~java
package com.wuji1626.spring.aop.aspect;
import com.wuji1626.spring.aop.Action;
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.stereotype.Component;
import java.lang.reflect.Method;
/**
 * Created by Administrator on 2017/10/6.
 */
@Aspect
@Component
public class LogAspect {
    @Pointcut("@com.wuji1626.spring.aop.Action")
    public void annotationPointCut(){};
    @After("annotationPointCut()")
    public void after(JoinPoint joinPoint){
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        Action action = method.getAnnotation(Action.class);
        System.out.println("注解式拦截" + action.name());
    }
    @Before("execution(*com.wuji1626.spring.aop.service.DemoMethodService.*(..))")
    public void before(JoinPoint joinPoint){
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        System.out.println("方法规则式拦截" + method.getName());
    }
}
~~~
6. 配置类
通过注解@EnableAspectJAutoProxy开启Spring对AspectJ代理支持  
~~~java
package com.wuji1626.spring.aop.config;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;
import org.springframework.stereotype.Component;
/**
 * Created by Administrator on 2017/10/7.
 */
@Configuration
@Component("com.wuji1626.spring.aop")
@EnableAspectJAutoProxy
public class AopConfig {
}
~~~
7. 运行
~~~java
package com.wuji1626.spring.aop;
import com.wuji1626.spring.aop.config.AopConfig;
import com.wuji1626.spring.aop.service.DemoAnnotationService;
import com.wuji1626.spring.aop.service.DemoMethodService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.test.context.support.AnnotationConfigContextLoader;
/**
 * Created by Administrator on 2017/10/7.
 */
public class Main {
    public static void main(String[] args){
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AopConfig.class);
        DemoAnnotationService demoAnnotationService = context.getBean(DemoAnnotationService.class);
        DemoMethodService demoMethodService = context.getBean(DemoMethodService.class);
        demoAnnotationService.add();
        demoMethodService.add();
        context.close();
    }
}
~~~
运行结果：  
![](img/R002.png)  

###1.3 Bean的Scope
Spring的Scope：  
1. Singleton：每个Spring容器中只有一个Bean实例，Spring的默认配置，全容器共享一个实例
2. Prototype：每次调用新建一个Bean实例
3. Request：Web项目中，每个http request新建一个Bean实例
4. Session：Web项目中，每个http session新建一个Bean实例
5. GlobalSession：只在Portal应用中有用，给每个global http session新建一个Bean实例
6. @StepScope：Spring Batch中的选项

Bean Scope案例：  
1. 创建Singleton服务  
~~~java
package com.wuji1626.spring.scope.service;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/10/7.
 */
@Service
public class DemoSingletonService {
}
~~~
2. 创建prototype服务
~~~java
package com.wuji1626.spring.scope.service;
import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/10/7.
 */
@Service
@Scope("prototype")
public class DemoPrototypeService {
}
~~~
3. 配置类
~~~java
package com.wuji1626.spring.scope.config;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
/**
 * Created by Administrator on 2017/10/7.
 */
@Configuration
@ComponentScan("com.wuji1626.spring.scope")
public class ScopeConfig {
}
~~~
4. 运行类
~~~java
package com.wuji1626.spring.scope;
import com.wuji1626.spring.scope.config.ScopeConfig;
import com.wuji1626.spring.scope.service.DemoPrototypeService;
import com.wuji1626.spring.scope.service.DemoSingletonService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
/**
 * Created by Administrator on 2017/10/7.
 */
public class Main {
    public static void main(String[] args){
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScopeConfig.class);
        DemoSingletonService s1 = context.getBean(DemoSingletonService.class);
        DemoPrototypeService p1 = context.getBean(DemoPrototypeService.class);
        DemoSingletonService s2 = context.getBean(DemoSingletonService.class);
        DemoPrototypeService p2 = context.getBean(DemoPrototypeService.class);
        System.out.println("s1和s2是否相等：" + s1.equals(s2));
        System.out.println("p1和p2是否相等：" + p1.equals(p2));
    }
}
~~~
结果：  
![](img/R003.png)  

###1.4 Spring EL和资源调用
1. POM中增加依赖
需要commons-io引用  
~~~xml
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.3</version>
    </dependency>
~~~
2. 创建test.properties文件及text文件
el.properties  
~~~
book.auther=zhangwenhe
book.name=Che in Action
~~~
el_text.txt 
~~~
This is a text File content.
~~~
3. 创建服务类
~~~java
package com.wuji1626.spring.el.service;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
/**
 * Created by Administrator on 2017/10/7.
 */
@Service
public class DemoService {
    @Value("其他属性")
    private String another;
    public String getAnother() {
        return another;
    }
    public void setAnother(String another) {
        this.another = another;
    }
}
~~~
4. 配置类
~~~java
package com.wuji1626.spring.el.config;
import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
import org.springframework.core.env.Environment;
import org.springframework.core.io.Resource;
import org.springframework.stereotype.Component;
import java.io.IOException;
/**
 * Created by Administrator on 2017/10/7.
 */
@Component
@ComponentScan("com.wuji1626.spring.el")
@PropertySource("classpath:el.properties")
public class ElConfig {
    @Value("I Love You")
    private String normal;
    @Value("#{systemProperties['os.name']}")
    private String osName;
    @Value("#{T(java.lang.Math).random()*100.0}")
    private double randomNumber;
    @Value("#{demoService.another}")
    private String fromAnohter;
    @Value("classpath:el_text.txt")
    private Resource testFile;
    @Value("http://www.baidu.com")
    private Resource testUrl;
    @Value("${book.name}")
    private String bookName;
    @Autowired
    private Environment environment;
	// 为保证${book.name}基于配置文件的生效，需要注入PropertySourcesPlaceholderConfigurer
    @Bean //注入配置文件,
    public static PropertySourcesPlaceholderConfigurer propertySourcesPlaceholderConfigurer(){
        return new PropertySourcesPlaceholderConfigurer();
    }
    public void outputResource(){
        System.out.println("正常输出：" + normal);
        System.out.println("系统变量：" + osName);
        System.out.println("数学公式：" + randomNumber);
        System.out.println("从其他类属性：" + fromAnohter);
        try {
            System.out.print("文件输出：");
            System.out.println(IOUtils.toString(testFile.getInputStream()));
            System.out.print("互联网返回：");
            System.out.println(IOUtils.toString(testUrl.getInputStream()));
        } catch (IOException e) {
            e.printStackTrace();
        }
        System.out.println("注入配置文件：" + bookName);
        System.out.println("注入配置文件：" + environment.getProperty("book.author"));
    }
}
~~~
5. 运行类
~~~java
package com.wuji1626.spring.el;
import com.wuji1626.spring.el.config.ElConfig;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
/**
 * Created by Administrator on 2017/10/7.
 */
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ElConfig.class);
        ElConfig resourceService = context.getBean(ElConfig.class);
        resourceService.outputResource();
        context.close();
    }
}
~~~
结果：  
![](img/R004.png)  

###1.5 Bean初始化与销毁
Bean的生命周期管理：  
1. Java配置：使用@Bean的initMethod、destroyMethod（相当于xml配置中的init-method、destroy-method）
2. 注解方式：利用JSR-250的@PostConstruct、@PreDestroy

案例：
1. 增加JSR-250的支持
~~~xml
    <dependency>
      <groupId>javax.annotation</groupId>
      <artifactId>jsr250-api</artifactId>
      <version>1.0</version>
    </dependency>
~~~
2. 用@Bean形式的Bean
BeanWayService.java
~~~java
package com.wuji1626.spring.bean_lifecycle.service;
/**
 * Created by Administrator on 2017/10/7.
 */
public class BeanWayService {
    public void init(){
        System.out.println("@Bean-init-method");
    }
    public BeanWayService(){
        super();
        System.out.println("初始化构造函数-BeanWayService");
    }
    public void destroy(){
        System.out.println("@Bean-destroy-method");
    }
}
~~~
3. 用JSR250形式的Bean
JSR250WayService.java
~~~java
package com.wuji1626.spring.bean_lifecycle.service;
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
/**
 * Created by Administrator on 2017/10/8.
 */
public class JSR250WayService {
    @PostConstruct
    public void init(){
        System.out.println("jsr250-init-method");
    }
    public JSR250WayService(){
        super();
        System.out.println("初始化构造函数-JSR250WayService");
    }
    @PreDestroy
    public void destroy(){
        System.out.println("jsr250-destroy-method");
    }
}
~~~
4. 配置
~~~java
package com.wuji1626.spring.bean_lifecycle.config;
import com.wuji1626.spring.bean_lifecycle.service.BeanWayService;
import com.wuji1626.spring.bean_lifecycle.service.JSR250WayService;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
/**
 * Created by Administrator on 2017/10/8.
 */
@Configuration
@ComponentScan("com.wuji1626.spring.bean_lifecycle")
public class PrePostConfig {
    @Bean(initMethod = "init", destroyMethod = "destroy")
    BeanWayService beanWayService(){
        return new BeanWayService();
    }
    @Bean
    JSR250WayService jsr250WayService(){
        return new JSR250WayService();
    }
}
~~~
5. 运行类
~~~java
package com.wuji1626.spring.bean_lifecycle;
import com.wuji1626.spring.bean_lifecycle.config.PrePostConfig;
import com.wuji1626.spring.bean_lifecycle.service.BeanWayService;
import com.wuji1626.spring.bean_lifecycle.service.JSR250WayService;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import javax.annotation.PreDestroy;
/**
 * Created by Administrator on 2017/10/7.
 */
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(PrePostConfig.class);
        BeanWayService beanWayService = context.getBean(BeanWayService.class);
        JSR250WayService jsr250WayService = context.getBean(JSR250WayService.class);
        context.close();
    }
}
~~~
6. 结果
![](img/R005.png)  

###1.6 Profile
Profile：在不同环境下使用不同配置（如：开发环境、生产环境的不同配置）  
使用Profile的方法：  
1. 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境。在开发中使用@Profile注解类，达到不同情况下，选择实例化不同的Bean
2. 通过设定jvm的spring.profiles.active参数来设置环境
3. Web项目在Servlet的context parameter中选择环境

案例：
1. DemoBean.java
~~~java
package com.wuji1626.spring.profile.bean;
/**
 * Created by Administrator on 2017/10/8.
 */
public class DemoBean {
    private String content;
    public DemoBean(String content){
        super();
        this.content = content;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
}
~~~
2. 配置类
~~~java
package com.wuji1626.spring.profile.config;
import com.wuji1626.spring.profile.bean.DemoBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;
/**
 * Created by Administrator on 2017/10/8.
 */
@Configuration
public class ProfileConfig {
    @Bean
    @Profile("dev")
    public DemoBean devDemoBean(){
        return new DemoBean("from development profile");
    }
    @Bean
    @Profile("prod")
    public DemoBean prodDemoBean(){
        return new DemoBean("from production profile");
    }
}
~~~
3. 运行类
~~~java
package com.wuji1626.spring.profile;
import com.wuji1626.spring.profile.bean.DemoBean;
import com.wuji1626.spring.profile.config.ProfileConfig;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
/**
 * Created by Administrator on 2017/10/8.
 */
public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        context.getEnvironment().setActiveProfiles("prod");
        context.register(ProfileConfig.class);
        context.refresh();
        DemoBean demoBean = context.getBean(DemoBean.class);
        System.out.println(demoBean.getContent());
        context.close();
    }
}
~~~
4. 结果
![](img/R006.png)  
如果将环境改为"dev"，则结果：
![](img/R007.png)  

###1.7 事件
spring的事件，支持Bean与Bean之间消息通信，流程如下：
1. 自定义事件，继承ApplicationEvent
2. 定义事件监听器，实现ApplicationListener
3. 使用容器发布事件

案例：
1. 事件类
DemoEvent.java
~~~java

~~~