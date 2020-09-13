> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)
----

## 1. Spring Boot是什么

我们知道，从 2002 年开始，Spring 一直在飞速的发展，如今已经成为了在Java EE（Java Enterprise Edition）开发中真正意义上的标准，但是随着技术的发展，Java EE使用 Spring 逐渐变得笨重起来，大量的 XML 文件存在于项目之中。**繁琐的配置，整合第三方框架的配置问题，导致了开发和部署效率的降低**。  

2012 年 10 月，Mike Youngstrom 在 Spring jira 中创建了一个功能请求，要求**在 Spring 框架中支持无容器 Web 应用程序体系结构**。他谈到了在主容器引导 spring 容器内配置 Web 容器服务。这是 jira 请求的摘录：

> 我认为 Spring 的 Web 应用体系结构可以大大简化，如果它提供了从上到下利用 Spring 组件和配置模型的工具和参考体系结构。在简单的 `main()`方法引导的 Spring 容器内嵌入和统一这些常用Web 容器服务的配置。 

这一要求促使了 2013 年初开始的 Spring Boot 项目的研发，到今天，Spring Boot 的版本已经到了 2.0.3 RELEASE。Spring Boot 并不是用来替代 Spring 的解决方案，而**是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具**。

它集成了大量常用的第三方库配置，Spring Boot应用中这些第三方库几乎可以是零配置的开箱即用（out-of-the-box），大部分的 Spring Boot 应用都只需要非常少量的配置代码（基于 Java 的配置），开发者能够更加专注于业务逻辑。   

## 2. 为什么学习Spring Boot

### 2.1 从Spring官方来看

我们打开 Spring 的[官方网站](http://spring.io/)，可以看到下图：  

![Spring官网首图](https://images.gitbook.cn/98fda7f0-8656-11e8-8675-5537a701ae7d)

我们可以看到图中官方对 Spring Boot 的定位：*Build Anything*， Build任何东西。Spring Boot旨在尽可能快地启动和运行，并且只需最少的 Spring 前期配置。  同时我们也来看一下官方对后面两个的定位：  

SpringCloud：*Coordinate Anything*，协调任何事情；
SpringCloud Data Flow：*Connect everything*，连接任何东西。

仔细品味一下，Spring 官网对 Spring Boot、SpringCloud 和 SpringCloud Data Flow三者定位的措辞非常有味道，同时也可以看出，Spring 官方对这三个技术非常重视，是现在以及今后学习的重点（SpringCloud 相关达人课课程届时也会上线）。

### 2.2 从Spring Boot的优点来看

Spring Boot 有哪些优点？主要给我们解决了哪些问题呢？我们以下图来说明：

![Spring Boot的优点](https://images.gitbook.cn/e17b8c40-8656-11e8-8a91-d70bc2d847c5)

#### 2.2.1 良好的基因

Spring Boot 是伴随着 Spring 4.0 诞生的，从字面理解，Boot是引导的意思，因此 Spring Boot 旨在帮助开发者快速搭建 Spring 框架。Spring Boot 继承了原有 Spring 框架的优秀基因，使 Spring 在使用中更加方便快捷。

![Spring Boot与Spring](https://images.gitbook.cn/00078650-8657-11e8-87de-d910a3ee087e)

#### 2.2.2 简化编码

举个例子，比如我们要创建一个 web 项目，使用 Spring 的朋友都知道，在使用 Spring 的时候，需要在 pom 文件中添加多个依赖，而 Spring Boot 则会帮助开发着快速启动一个 web 容器，在 Spring Boot 中，我们只需要在 pom 文件中添加如下一个 starter-web 依赖即可。

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

我们点击进入该依赖后可以看到，Spring Boot 这个 starter-web 已经包含了多个依赖，包括之前在 Spring 工程中需要导入的依赖，我们看一下其中的一部分，如下：

```xml
<!-- .....省略其他依赖 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.0.7.RELEASE</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.0.7.RELEASE</version>
    <scope>compile</scope>
</dependency>
```
由此可以看出，Spring Boot 大大简化了我们的编码，我们不用一个个导入依赖，直接一个依赖即可。

#### 2.2.3 简化配置

Spring 虽然使Java EE轻量级框架，但由于其繁琐的配置，一度被人认为是“配置地狱”。各种XML、Annotation配置会让人眼花缭乱，而且配置多的话，如果出错了也很难找出原因。Spring Boot更多的是采用 Java Config 的方式，对 Spring 进行配置。举个例子：

我新建一个类，但是我不用 `@Service`注解，也就是说，它是个普通的类，那么我们如何使它也成为一个 Bean 让 Spring 去管理呢？只需要`@Configuration` 和`@Bean`两个注解即可，如下：
```java
public class TestService {
    public String sayHello () {
        return "Hello Spring Boot!";
    }
}
```
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JavaConfig {
    @Bean
    public TestService getTestService() {
        return new TestService();
    }
}
```
`@Configuration`表示该类是个配置类，`@Bean`表示该方法返回一个 Bean。这样就把`TestService`作为 Bean 让 Spring 去管理了，在其他地方，我们如果需要使用该 Bean，和原来一样，直接使用`@Resource`注解注入进来即可使用，非常方便。
```java
@Resource
private TestService testService;
```

另外，部署配置方面，原来 Spring 有多个 xml 和 properties配置，在 Spring Boot 中只需要个 application.yml即可。

#### 2.2.4 简化部署

在使用 Spring 时，项目部署时需要我们在服务器上部署 tomcat，然后把项目打成 war 包扔到 tomcat里，在使用 Spring Boot 后，我们不需要在服务器上去部署 tomcat，因为 Spring Boot 内嵌了 tomcat，我们只需要将项目打成 jar 包，使用 `java -jar xxx.jar`一键式启动项目。

另外，也降低对运行环境的基本要求，环境变量中有JDK即可。

#### 2.2.5 简化监控

我们可以引入 spring-boot-start-actuator 依赖，直接使用 REST 方式来获取进程的运行期性能参数，从而达到监控的目的，比较方便。但是 Spring Boot 只是个微框架，没有提供相应的服务发现与注册的配套功能，没有外围监控集成方案，没有外围安全管理方案，所以在微服务架构中，还需要 Spring Cloud 来配合一起使用。

### 2.3 从未来发展的趋势来看

微服务是未来发展的趋势，项目会从传统架构慢慢转向微服务架构，因为微服务可以使不同的团队专注于更小范围的工作职责、使用独立的技术、更安全更频繁地部署。而 继承了 Spring 的优良特性，与 Spring 一脉相承，而且 支持各种REST API 的实现方式。Spring Boot 也是官方大力推荐的技术，可以看出，Spring Boot 是未来发展的一个大趋势。

## 3. 本节能学到什么

本节使用目前 Spring Boot 最新版本2.0.3 RELEASE，文章均为作者在实际项目中剥离出来的场景和demo，目标是带领学习者快速上手 Spring Boot，将 Spring Boot 相关技术点快速运用在微服务项目中。全篇分为两部分：基础篇和进阶篇。  

基础篇主要介绍 Spring Boot 在项目中最常使用的一些功能点，旨在带领学习者快速掌握 Spring Boot 在开发时需要的知识点，能够把 Spring Boot 相关技术运用到实际项目架构中去。该部分以 Spring Boot 框架为主线，内容包括Json数据封装、日志记录、属性配置、MVC支持、在线文档、模板引擎、异常处理、AOP 处理、持久层集成等等。  

进阶篇主要是介绍 Spring Boot 在项目中拔高一些的技术点，包括集成的一些组件，旨在带领学习者在项目中遇到具体的场景时能够快速集成，完成对应的功能。该部分以 Spring Boot 框架为主线，内容包括拦截器、监听器、缓存、安全认证、分词插件、消息队列等等。  

认真读完该系列文章之后，学习者会快速了解并掌握 Spring Boot 在项目中最常用的技术点，作者课程的最后，会基于课程内容搭建一个 Spring Boot 项目的空架构，该架构也是从实际项目中剥离出来，学习者可以运用该架构于实际项目中，具备使用 Spring Boot 进行实际项目开发的能力。  

课程所有源码提供免费下载：[下载地址](https://gitee.com/eson15/springboot_study)。

-----

> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)