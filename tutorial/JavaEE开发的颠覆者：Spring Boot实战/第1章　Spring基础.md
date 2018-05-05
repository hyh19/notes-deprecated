# 第1章　Spring基础

## 1.3　Spring基础配置

### 1.3.1　依赖注入

[Annotation Type Component](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/stereotype/Component.html)

[Annotation Type Controller](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/stereotype/Controller.html)

[Annotation Type Repository](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/stereotype/Repository.html)

[Annotation Type Service](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/stereotype/Service.html)

[Annotation Type Autowired](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/Autowired.html)

[Annotation Type ComponentScan](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/ComponentScan.html)

[Annotation Type Configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Configuration.html)

[Class AnnotationConfigApplicationContext](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/AnnotationConfigApplicationContext.html)

`highlight_spring4/pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.wisely</groupId>
    <artifactId>highlight_spring4</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>1.7</java.version>
        <spring-framework.version>4.1.5.RELEASE</spring-framework.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring-framework.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <configuration>
                    <source>${java.version}</source>
                    <target>${java.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/di/FunctionService.java`
```java
package com.wisely.highlight_spring4.ch1.di;

import org.springframework.stereotype.Service;

@Service
public class FunctionService {
    public String sayHello(String word) {
        return "Hello " + word +" !";
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/di/UseFunctionService.java`
```java
package com.wisely.highlight_spring4.ch1.di;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UseFunctionService {
    @Autowired
    FunctionService functionService;

    public String SayHello(String word) {
        return functionService.sayHello(word);
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/di/DiConfig.java`
```java
package com.wisely.highlight_spring4.ch1.di;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch1.di")
public class DiConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/di/Main.java`
```java
package com.wisely.highlight_spring4.ch1.di;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(DiConfig.class);
        UseFunctionService useFunctionService = context.getBean(UseFunctionService.class);
        System.out.println(useFunctionService.SayHello("world"));
        context.close();
    }
}
```

### 1.3.2　Java配置

[Annotation Type Bean](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Bean.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/javaconfig/FunctionService.java`
```java
package com.wisely.highlight_spring4.ch1.javaconfig;

public class FunctionService {
    public String sayHello(String word){
        return "Hello " + word +" !";
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/javaconfig/UseFunctionService.java`
```java
package com.wisely.highlight_spring4.ch1.javaconfig;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JavaConfig {

    @Bean
    public FunctionService functionService() {
        return new FunctionService();
    }

//    @Bean
//    public UseFunctionService useFunctionService() {
//        UseFunctionService useFunctionService = new UseFunctionService();
//        useFunctionService.setFunctionService(functionService());
//        return useFunctionService;
//    }

    @Bean
    public UseFunctionService useFunctionService(FunctionService functionService) {
        UseFunctionService useFunctionService = new UseFunctionService();
        useFunctionService.setFunctionService(functionService);
        return useFunctionService;
    }

}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/javaconfig/JavaConfig.java`
```java
package com.wisely.highlight_spring4.ch1.javaconfig;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class JavaConfig {

    @Bean
    public FunctionService functionService() {
        return new FunctionService();
    }

    @Bean
    public UseFunctionService useFunctionService() {
        UseFunctionService useFunctionService = new UseFunctionService();
        useFunctionService.setFunctionService(functionService());
        return useFunctionService;
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch1/javaconfig/Main.java`
```java
package com.wisely.highlight_spring4.ch1.javaconfig;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context =
                new AnnotationConfigApplicationContext(JavaConfig.class);
        UseFunctionService useFunctionService = context.getBean(UseFunctionService.class);
        System.out.println(useFunctionService.SayHello("java config"));
        context.close();
    }
}
```

### 1.3.3　AOP

跳过

