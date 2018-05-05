# 第2章　Spring常用配置

## 2.1　Bean的Scope

[Annotation Type Scope](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Scope.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/scope/DemoSingletonService.java`
```java
package com.wisely.highlight_spring4.ch2.scope;

import org.springframework.stereotype.Service;

@Service
public class DemoSingletonService {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/scope/DemoPrototypeService.java`
```java
package com.wisely.highlight_spring4.ch2.scope;

import org.springframework.context.annotation.Scope;
import org.springframework.stereotype.Service;

@Service
@Scope("prototype")
public class DemoPrototypeService {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/scope/ScopeConfig.java`
```java
package com.wisely.highlight_spring4.ch2.scope;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch2.scope")
public class ScopeConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/scope/Main.java`
```java
package com.wisely.highlight_spring4.ch2.scope;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScopeConfig.class);
        DemoSingletonService s1 = context.getBean(DemoSingletonService.class);
        DemoSingletonService s2 = context.getBean(DemoSingletonService.class);
        System.out.println(s1 == s2);

        DemoPrototypeService p1 = context.getBean(DemoPrototypeService.class);
        DemoPrototypeService p2 = context.getBean(DemoPrototypeService.class);
        System.out.println(p1 == p2);
    }
}
```

## 2.2　Spring EL和资源调用

[Annotation Type Value](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/annotation/Value.html)

[Annotation Type PropertySource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/PropertySource.html)

[Interface Environment](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/env/Environment.html)

[Class PropertySourcesPlaceholderConfigurer](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/support/PropertySourcesPlaceholderConfigurer.html)

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
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>2.3</version>
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

`highlight_spring4/src/main/resources/test.txt`
```
测试文件
```

`highlight_spring4/src/main/resources/test.properties`
```
book.author=wangyunfei
book.name=spring boot
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/el/DemoService.java`
```java
package com.wisely.highlight_spring4.ch2.el;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;

@Service
public class DemoService {
    @Value("其他类的属性")
    private String another;

    public String getAnother() {
        return another;
    }

    public void setAnother(String another) {
        this.another = another;
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/el/ElConfig.java`
```java
package com.wisely.highlight_spring4.ch2.el;

import org.apache.commons.io.IOUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.PropertySource;
import org.springframework.context.support.PropertySourcesPlaceholderConfigurer;
import org.springframework.core.env.Environment;
import org.springframework.core.io.Resource;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch2.el")
@PropertySource("test.properties")
public class ElConfig {
    @Value("I Love You!")
    private String normal;

    @Value("#{systemProperties['os.name']}")
    private String osName;

    @Value("#{ T(java.lang.Math).random() * 100.0 }")
    private double randomNumber;

    @Value("#{demoService.another}")
    private String fromAnother;

    @Value("test.txt")
    private Resource testFile;

    @Value("http://www.baidu.com")
    private Resource testUrl;

    @Value("${book.name}")
    private String bookName;

    @Autowired
    private Environment environment;

    @Bean
    public static PropertySourcesPlaceholderConfigurer propertyConfigure() {
        return new PropertySourcesPlaceholderConfigurer();
    }

    public void outputResource() {
        try {
            System.out.println(normal);
            System.out.println(osName);
            System.out.println(randomNumber);
            System.out.println(fromAnother);

            System.out.println(IOUtils.toString(testFile.getInputStream()));
            System.out.println(IOUtils.toString(testUrl.getInputStream()));
            System.out.println(bookName);
            System.out.println(environment.getProperty("book.author"));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/el/Main.java`
```java
package com.wisely.highlight_spring4.ch2.el;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ElConfig.class);
        ElConfig resourceService = context.getBean(ElConfig.class);
        resourceService.outputResource();
        context.close();
    }
}
```

## 2.3　Bean的初始化和销毁

```xml
<dependency>
    <groupId>javax.annotation</groupId>
    <artifactId>jsr250-api</artifactId>
    <version>1.0</version>
</dependency>
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/prepost/BeanWayService.java`
```java
package com.wisely.highlight_spring4.ch2.prepost;

public class BeanWayService {

    public BeanWayService() {
        super();
        System.out.println("初始化构造函数-BeanWayService");
    }

    public void init() {
        System.out.println("@Bean-init-method");
    }

    public void destroy() {
        System.out.println("@Bean-destory-method");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/prepost/JSR250WayService.java`
```java
package com.wisely.highlight_spring4.ch2.prepost;

import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;

public class JSR250WayService {

    public JSR250WayService() {
        super();
        System.out.println("初始化构造函数-JSR250WayService");
    }

    @PostConstruct
    public void init() {
        System.out.println("jsr250-init-method");
    }

    @PreDestroy
    public void destroy() {
        System.out.println("jsr250-destory-method");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/prepost/PrePostConfig.java`
```java
package com.wisely.highlight_spring4.ch2.prepost;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch2.prepost")
public class PrePostConfig {

    @Bean(initMethod = "init", destroyMethod = "destroy")
    BeanWayService beanWayService() {
        return new BeanWayService();
    }

    @Bean
    JSR250WayService jsr250WayService() {
        return new JSR250WayService();
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/prepost/Main.java`
```java
package com.wisely.highlight_spring4.ch2.prepost;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(PrePostConfig.class);
        BeanWayService beanWayService = context.getBean(BeanWayService.class);
        JSR250WayService jsr250WayService = context.getBean(JSR250WayService.class);
        context.close();
    }
}
```

## 2.4　Profile

[Annotation Type Profile](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/annotation/Profile.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/profile/DemoBean.java`
```java
package com.wisely.highlight_spring4.ch2.profile;

public class DemoBean {

    private String content;

    public DemoBean(String content) {
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
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/profile/ProfileConfig.java`
```java
package com.wisely.highlight_spring4.ch2.profile;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Profile;

@Configuration
public class ProfileConfig {

    @Bean
    @Profile("dev")
    public DemoBean devDemoBean() {
        return new DemoBean("from development profile");
    }

    @Bean
    @Profile(("prod"))
    public DemoBean prodDemoBean() {
        return new DemoBean("from production profile");
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/profile/Main.java`
```java
package com.wisely.highlight_spring4.ch2.profile;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {

        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        // context.getEnvironment().setActiveProfiles("dev");
        context.getEnvironment().setActiveProfiles("prod");
        context.register(ProfileConfig.class);
        context.refresh();

        DemoBean demoBean = context.getBean(DemoBean.class);
        System.out.println(demoBean.getContent());

        context.close();
    }
}
```

## 2.5　事件（Application Event）

[Class ApplicationEvent](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/ApplicationEvent.html)

[Interface ApplicationListener<E extends ApplicationEvent>](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/context/ApplicationListener.html)

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/event/DemoEvent.java`
```java
package com.wisely.highlight_spring4.ch2.event;

import org.springframework.context.ApplicationEvent;

public class DemoEvent extends ApplicationEvent {

    private static final long serialVersionUID = 1L;
    private String msg;

    public DemoEvent(Object source, String msg) {
        super(source);
        this.msg = msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/event/DemoListener.java`
```java
package com.wisely.highlight_spring4.ch2.event;

import org.springframework.context.ApplicationListener;
import org.springframework.stereotype.Component;

@Component
public class DemoListener implements ApplicationListener<DemoEvent> {

    @Override
    public void onApplicationEvent(DemoEvent demoEvent) {
        String msg = demoEvent.getMsg();
        System.out.println("DemoListener: "+ msg);
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/event/DemoPublisher.java`
```java
package com.wisely.highlight_spring4.ch2.event;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class DemoPublisher {

    @Autowired
    ApplicationContext applicationContext;

    public void publish(String msg) {
        applicationContext.publishEvent(new DemoEvent(this, msg));
    }
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/event/EventConfig.java`
```java
package com.wisely.highlight_spring4.ch2.event;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.wisely.highlight_spring4.ch2.event")
public class EventConfig {
}
```

`highlight_spring4/src/main/java/com/wisely/highlight_spring4/ch2/event/Main.java`
```java
package com.wisely.highlight_spring4.ch2.event;

import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(EventConfig.class);
        DemoPublisher demoPublisher = context.getBean(DemoPublisher.class);
        demoPublisher.publish("Hello World");
        context.close();
    }
}
```