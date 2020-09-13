> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)
----

[上一节](https://github.com/nxJava/nx_java/blob/master/SpringBoot/%E6%88%91%E4%BB%AC%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%AD%A6%E4%B9%A0SpringBoot.md)对 SpringBoot 的特性做了一个介绍，本节主要对 **jdk 的配置、Spring Boot工程的构建和项目的启动、Spring Boot 项目工程的结构做一下讲解和分析**。

## 1. jdk 的配置

本课程是使用 IDEA 进行开发，在IDEA 中配置 jdk 的方式很简单，打开`File->Project Structure`，如下图所：

![IDEA中配置jdk](https://images.gitbook.cn/9befde80-8659-11e8-9b0d-95de449dc107)

1. 选择 SDKs
2. 在 JDK home path 中选择本地 jdk 的安装目录
3. 在 Name 中为 jdk 自定义名字

通过以上三步骤，即可导入本地安装的 jdk。如果是使用 STS 或者 eclipse 的朋友，可以通过两步骤添加：
* `window->preference->java->Instralled JRES`来添加本地 jdk。
* `window-->preference-->java-->Compiler`选择 jre，和 jdk 保持一致。

## 2. Spring Boot 工程的构建

### 2.1 IDEA 快速构建

IDEA 中可以通过`File->New->Project`来快速构建 Spring Boot 工程。如下，选择 Spring Initializr，在 Project SDK 中选择刚刚我们导入的 jdk，点击 Next，到了项目的配置信息。

* Group：填企业域名，本课程使用com.itcodai
* Artifact：填项目名称，本课程中每一课的工程名以`course+课号`命令，这里使用 course01
* Dependencies：可以添加我们项目中所需要的依赖信息，根据实际情况来添加，本课程只需要选择 Web 即可。

### 2.2 官方构建

第二种方式可以通过官方构建，步骤如下：
* 访问 http://start.spring.io/。
* 在页面上输入相应的 Spring Boot 版本、Group 和 Artifact 信息以及项目依赖，然后创建项目。
* ![创建Spring Boot工程](https://images.gitbook.cn/014a1ba0-865b-11e8-956e-f528114b28bd)
* 解压后，使用 IDEA 导入该 maven 工程：`File->New->Model from Existing Source`，然后选择解压后的项目文件夹即可。如果是使用 eclipse 的朋友，可以通过`Import->Existing Maven Projects->Next`，然后选择解压后的项目文件夹即可。

### 2.3 maven配置

创建了 Spring Boot 项目之后，需要进行 maven 配置。打开`File->settings`，搜索 maven，配置一下本地的 maven 信息。如下：

![maven配置](https://images.gitbook.cn/2ff7b930-865b-11e8-8675-5537a701ae7d)

在 Maven home directory 中选择本地 Maven 的安装路径；在 User settings file 中选择本地 Maven 的配置文件所在路径。在配置文件中，我们配置一下国内阿里的镜像，这样在下载 maven 依赖时，速度很快。
```xml
<mirror>
	<id>nexus-aliyun</id>
	<mirrorOf>*</mirrorOf>
	<name>Nexus aliyun</name>
	<url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```
如果是使用 eclipse 的朋友，可以通过`window-->preference-->Maven-->User Settings`来配置，配置方式和上面一致。

### 2.4 编码配置

同样地，新建项目后，我们一般都需要配置编码，这点非常重要，很多初学者都会忘记这一步，所以要养成良好的习惯。

IDEA 中，仍然是打开`File->settings`，搜索 encoding，配置一下本地的编码信息。如下：

![编码配置](https://images.gitbook.cn/57564e60-865b-11e8-8a91-d70bc2d847c5)

如果是使用 eclipse 的朋友，有两个地方需要设置一下编码：
* window--> perferences-->General-->Workspace，将Text file encoding改成utf-8
* window-->perferences-->General-->content types，选中Text，将Default encoding填入utf-8

OK，编码设置完成即可启动项目工程了。

## 3. Spring Boot 项目工程结构

Spring Boot 项目总共有三个模块，如下图所示：

![Spring Boot项目工程结构](https://images.gitbook.cn/8b32fa80-865b-11e8-9d13-03ea4b4d8504)

* src/main/java路径：主要编写业务程序
* src/main/resources路径：存放静态文件和配置文件
* src/test/java路径：主要编写测试程序

默认情况下，如上图所示会创建一个启动类 Course01Application，该类上面有个`@SpringBootApplication`注解，该启动类中有个 main 方法，没错，Spring Boot 启动只要运行该 main 方法即可，非常方便。另外，Spring Boot 内部集成了 tomcat，不需要我们人为手动去配置 tomcat，开发者只需要关注具体的业务逻辑即可。

到此为止，Spring Boot 就启动成功了，为了比较清楚的看到效果，我们写一个 Controller 来测试一下，如下：
```java
package com.itcodai.course01.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/start")
public class StartController {

    @RequestMapping("/springboot")
    public String startSpringBoot() {
        return "Welcome to the world of Spring Boot!";
    }
}
```
重新运行 main 方法启动项目，在浏览器中输入 `localhost:8080/start/springboot`，如果看到 `“Welcome to the world of Spring Boot!”`，那么恭喜你项目启动成功！Spring Boot 就是这么简单方便！端口号默认是8080，如果想要修改，可以在 application.yml 文件中使用 `server.port` 来人为指定端口，如8001端口：
```xml
server:
  port: 8001
```

## 4. 总结

本节我们快速学习了如何在 IDEA 中导入 jdk，以及使用 IDEA 如何配置 maven 和编码，如何快速的创建和启动 Spring Boot 工程。IDEA 对 Spring Boot 的支持非常友好，建议大家使用 IDEA 进行 Spring Boot 的开发，从下一课开始，我们真正进入 Spring Boot 的学习中。
课程源代码下载地址：[戳我下载](https://gitee.com/eson15/springboot_study)

----

> 文章可以白嫖，公众号不能不关注，手动滑稽🤣🤣 &nbsp;
>
> 欢迎大家关注：**武哥聊编程**、**Java学习指南**，您的支持，是我们创作的持续动力！&nbsp;&nbsp;

![武哥聊编程](https://img-blog.csdnimg.cn/202002150421550.jpg)![Java学习指南](https://img-blog.csdnimg.cn/20200601113720522.png)