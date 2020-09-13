> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)
----

在开发中，我们经常使用 `System.out.println()` 来打印一些信息，但是这样不好，因为大量的使用 `System.out` 会增加资源的消耗。我们实际项目中使用的是 slf4j 的 logback 来输出日志，效率挺高的，Spring Boot 提供了一套日志系统，logback 是最优的选择。

## 1. slf4j 介绍

引用百度百科里的一段话：
> SLF4J，即简单日志门面（Simple Logging Facade for Java），不是具体的日志解决方案，它只服务于各种各样的日志系统。按照官方的说法，SLF4J是一个用于日志系统的简单Facade，允许最终用户在部署其应用时使用其所希望的日志系统。

这段的大概意思是：你只需要按统一的方式写记录日志的代码，而无需关心日志是通过哪个日志系统，以什么风格输出的。因为它们取决于部署项目时绑定的日志系统。例如，在项目中使用了 slf4j 记录日志，并且绑定了 log4j（即导入相应的依赖），则日志会以 log4j 的风格输出；后期需要改为以 logback 的风格输出日志，只需要将 log4j 替换成 logback 即可，不用修改项目中的代码。这对于第三方组件的引入的不同日志系统来说几乎零学习成本，况且它的优点不仅仅这一个而已，还有简洁的占位符的使用和日志级别的判断。

正因为 sfl4j 有如此多的优点，阿里巴巴已经将 slf4j 作为他们的日志框架了。在《阿里巴巴Java开发手册(正式版)》中，日志规约一项第一条就强制要求使用 slf4j：
> 1.【强制】应用中不可直接使用日志系统（Log4j、Logback）中的API，而应依赖使用日志框架SLF4J中的API，使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。

“强制”两个字体现出了 slf4j 的优势，所以建议在实际项目中，使用 slf4j 作为自己的日志框架。使用 slf4j 记录日志非常简单，直接使用  LoggerFactory 创建即可。
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Test {
    private static final Logger logger = LoggerFactory.getLogger(Test.class);
    // ……
}
```

## 2. application.yml 中对日志的配置

Spring Boot 对 slf4j 支持的很好，内部已经集成了 slf4j，一般我们在使用的时候，会对slf4j 做一下配置。`application.yml` 文件是 Spring Boot 中唯一一个需要配置的文件，一开始创建工程的时候是 `application.properties` 文件，个人比较细化用 yml 文件，因为 yml 文件的层次感特别好，看起来更直观，但是 yml 文件对格式要求比较高，比如英文冒号后面必须要有个空格，否则项目估计无法启动，而且也不报错。用 properties 还是 yml 视个人习惯而定，都可以。本课程使用 yml。

我们看一下 application.yml 文件中对日志的配置：
```xml
logging:
  config: logback.xml
  level:
    com.itcodai.course03.dao: trace
```
`logging.config` 是用来指定项目启动的时候，读取哪个配置文件，这里指定的是日志配置文件是根路径下的 `logback.xml` 文件，关于日志的相关配置信息，都放在 `logback.xml` 文件中了。`logging.level` 是用来指定具体的 mapper 中日志的输出级别，上面的配置表示 `com.itcodai.course03.dao` 包下的所有 mapper 日志输出级别为 trace，会将操作数据库的 sql 打印出来，开发时设置成 trace 方便定位问题，在生产环境上，将这个日志级别再设置成 error 级别即可（本节课不讨论 mapper 层，在后面 Spring Boot 集成 MyBatis 时再详细讨论）。

常用的日志级别按照从高到低依次为：ERROR、WARN、INFO、DEBUG。

## 3. logback.xml 配置文件解析

在上面 `application.yml` 文件中，我们指定了日志配置文件 `logback.xml`，`logback.xml` 文件中主要用来做日志的相关配置。在 `logback.xml` 中，我们可以定义日志输出的格式、路径、控制台输出格式、文件大小、保存时长等等。下面来分析一下：

### 3.1 定义日志输出格式和存储路径

```xml
<configuration>
	<property name="LOG_PATTERN" value="%date{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n" />
	<property name="FILE_PATH" value="D:/logs/course03/demo.%d{yyyy-MM-dd}.%i.log" />
</configuration>
```
我们来看一下这个定义的含义：首先定义一个格式，命名为 “LOG_PATTERN”，该格式中 `%date` 表示日期，`%thread` 表示线程名，`%-5level` 表示级别从左显示5个字符宽度，`%logger{36}`  表示 logger 名字最长36个字符，`%msg` 表示日志消息，`%n` 是换行符。

然后再定义一下名为 “FILE_PATH” 文件路径，日志都会存储在该路径下。`%i` 表示第 i 个文件，当日志文件达到指定大小时，会将日志生成到新的文件里，这里的 i 就是文件索引，日志文件允许的大小可以设置，下面会讲解。这里需要注意的是，不管是 windows 系统还是 Linux 系统，日志存储的路径必须要是绝对路径。

### 3.2 定义控制台输出

```xml
<configuration>
	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
            <!-- 按照上面配置的LOG_PATTERN来打印日志 -->
			<pattern>${LOG_PATTERN}</pattern>
		</encoder>
	</appender>
</configuration>
```
使用 `<appender>` 节点设置个控制台输出（`class="ch.qos.logback.core.ConsoleAppender"`）的配置，定义为 “CONSOLE”。使用上面定义好的输出格式（LOG_PATTERN）来输出，使用 `${}` 引用进来即可。

### 3.3 定义日志文件的相关参数

```xml
<configuration>
	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<!-- 按照上面配置的FILE_PATH路径来保存日志 -->
			<fileNamePattern>${FILE_PATH}</fileNamePattern>
			<!-- 日志保存15天 -->
			<maxHistory>15</maxHistory>
			<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
				<!-- 单个日志文件的最大，超过则新建日志文件存储 -->
				<maxFileSize>10MB</maxFileSize>
			</timeBasedFileNamingAndTriggeringPolicy>
		</rollingPolicy>

		<encoder>
			<!-- 按照上面配置的LOG_PATTERN来打印日志 -->
			<pattern>${LOG_PATTERN}</pattern>
		</encoder>
	</appender>
</configuration>
```
使用 `<appender>` 定义一个名为 “FILE” 的文件配置，主要是配置日志文件保存的时间、单个日志文件存储的大小、以及文件保存的路径和日志的输出格式。

### 3.4 定义日志输出级别

```xml
<configuration>
	<logger name="com.itcodai.course03" level="INFO" />
	<root level="INFO">
		<appender-ref ref="CONSOLE" />
		<appender-ref ref="FILE" />
	</root>
</configuration>
```
有了上面那些定义后，最后我们使用 `<logger>` 来定义一下项目中默认的日志输出级别，这里定义级别为 INFO，然后针对 INFO 级别的日志，使用 `<root>` 引用上面定义好的控制台日志输出和日志文件的参数。这样 logback.xml 文件中的配置就设置完了。

## 4. 使用Logger在项目中打印日志

在代码中，我们一般使用 Logger 对象来打印出一些 log 信息，可以指定打印出的日志级别，也支持占位符，很方便。
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/test")
public class TestController {

    private final static Logger logger = LoggerFactory.getLogger(TestController.class);

    @RequestMapping("/log")
    public String testLog() {
        logger.debug("=====测试日志debug级别打印====");
        logger.info("======测试日志info级别打印=====");
        logger.error("=====测试日志error级别打印====");
        logger.warn("======测试日志warn级别打印=====");

        // 可以使用占位符打印出一些参数信息
        String str1 = "武哥聊编程";
        String str2 = "blog.csdn.net/eson_15";
        logger.info("======武哥的公众号：{}；武哥的CSDN博客：{}", str1, str2);

        return "success";
    }
}
```
启动该项目，在浏览器中输入 `localhost:8080/test/log` 后可以看到控制台的日志记录：
>1. ======测试日志info级别打印=====
>2. =====测试日志error级别打印====
>3. ======测试日志warn级别打印=====
>4. ======武哥的公众号：武哥聊编程；武哥的CSDN博客：blog.csdn.net/eson_15

因为 INFO 级别比 DEBUG 级别高，所以 debug 这条没有打印出来，如果将 logback.xml 中的日志级别设置成 DEBUG，那么四条语句都会打印出来，这个大家自己去测试了。同时可以打开 D:\logs\course03\ 目录，里面有刚刚项目启动，以后后面生成的所有日志记录。在项目部署后，我们大部分都是通过查看日志文件来定位问题。

## 5. 总结

本节课主要对 slf4j 做了一个简单的介绍，并且对 Spring Boot 中如何使用 slf4j 输出日志做了详细的说明，着重分析了 `logback.xml` 文件中对日志相关信息的配置，包括日志的不同级别。最后针对这些配置，在代码中使用 Logger 打印出一些进行测试。在实际项目中，这些日志都是排查问题的过程中非常重要的资料。
课程源代码下载地址：[戳我下载](https://gitee.com/eson15/springboot_study)

-----

> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)