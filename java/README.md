此Dockerfile文件适用于构建SpringBoot应用

+ 基于JDK8 构建

## 使用方式
1. 将你的Jar文件拷贝到此目录下,重命名为app.jar 
2. 执行脚本 install.sh 看到Termial输入如下内容，表示Docker项目部署完成

```sh
Start exec docker-compose command....
java_spring_app_1   java -jar /java/app.jar   Up      8080/tcp, 0.0.0.0:8888->9090/tcp
exist old service ,and will be stop it ....
Going to remove java_spring_app_1
Are you sure? [yN] Step 1/5 : FROM openjdk:8-jre-slim
 ---> 2cf9c5fa7ea0
Step 2/5 : MAINTAINER zhoutao825638@vip.qq.com
 ---> Using cache
 ---> 9e9849b76ed5
Step 3/5 : EXPOSE 8080
 ---> Using cache
 ---> 7c00e20786be
Step 4/5 : COPY ./app.jar /java/app.jar
 ---> Using cache
 ---> d8acb59ce85e
Step 5/5 : CMD [ "java","-jar","/java/app.jar" ]
 ---> Using cache
 ---> 4b2bfcc64015
Successfully built 4b2bfcc64015
Successfully tagged java_spring_app:latest
command execute complete,please visit http://localhost:8888

```

## SpringBoot Application
1. 使用Docker构建Spring应用,需要注意的在Spring的配置文件中，其运行的端口使用的是环境变量，如下:

```yml
server:
  port: ${SPRING_APPLICATION_PORT}

spring:
  application:
    name: ${SPRING_APPLICATION_NAME}
```

2. 在访问http://localhost:8888的时候，其Controller代码如下：

```java

package com.seven.springboot.demo;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class DemoApplication {

  @Value("${spring.application.name}")
  String applicationName;

  public static void main(String[] args) {
    SpringApplication.run(DemoApplication.class, args);
  }


  @GetMapping()
  public String indexPage() {
    StringBuffer buffer = new StringBuffer("Docker for Java 项目部署成功 ");
    buffer.append("名称为:").append(applicationName);
    return buffer.toString();
  }
}
```


