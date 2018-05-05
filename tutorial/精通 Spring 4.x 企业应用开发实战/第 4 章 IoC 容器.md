[TOC]

# 第 4 章 IoC 容器

## 4.3 资源访问利器

### 4.3.1 资源抽象接口

[Interface Resource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/Resource.html)

[Interface WritableResource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/WritableResource.html)

[Class PathResource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/PathResource.html)

[Class ClassPathResource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/ClassPathResource.html)

`E:\Git\spring\spring4x-demos\chapter4\pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>me.demos</groupId>
    <artifactId>chapter4</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>chapter4 Maven Webapp</name>
    <url>http://maven.apache.org</url>

    <properties>
        <file.encoding>UTF-8</file.encoding>
        <spring.version>4.2.2.RELEASE</spring.version>
        <groovy.version>2.3.6</groovy.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
            <version>${groovy.version}</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>chapter4</finalName>
    </build>
</project>
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\resource\FileSourceExample.java`
```java
package com.smart.resource;

import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.PathResource;
import org.springframework.core.io.Resource;
import org.springframework.core.io.WritableResource;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.util.Date;

public class FileSourceExample {

    public static void main(String[] args) {

        try {
            String filePath = "E:\\Git\\spring\\spring4x-demos\\chapter4\\src\\main\\resources\\conf\\file1.txt";
            WritableResource resource1 = new PathResource(filePath);
            System.out.println("res1: " + resource1.getFilename());

            OutputStream outputStream = resource1.getOutputStream();
            outputStream.write((new Date()).toString().getBytes());
            outputStream.close();

            InputStream inputStream = resource1.getInputStream();
            ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
            int i;
            while ((i = inputStream.read()) != -1) {
                byteArrayOutputStream.write(i);
            }
            System.out.println(byteArrayOutputStream.toString());

            Resource resource2 = new ClassPathResource("conf/file1.txt");
            System.out.println("resource2: " + resource2.getFilename());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

[Class EncodedResource](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/support/EncodedResource.html)

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\resource\EncodedResourceExample.java`
```
package com.smart.resource;

import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.EncodedResource;
import org.springframework.util.FileCopyUtils;

public class EncodedResourceExample {
    public static void main(String[] args) throws Throwable {
        Resource resource = new ClassPathResource("conf/file1.txt");
        EncodedResource encodedResource = new EncodedResource(resource, "UTF-8");
        String content = FileCopyUtils.copyToString(encodedResource.getReader());
        System.out.println(content);
    }
}
```

### 4.3.2 资源加载

[Class PathMatchingResourcePatternResolver](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/core/io/support/PathMatchingResourcePatternResolver.html)

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\resource\PatternResolverExample.java`
```java
package com.smart.resource;

import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;

public class PatternResolverExample {
    public static void main(String[] args) throws Throwable {
        ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
        Resource resources[] = resolver.getResources("classpath*:conf/*.txt");
        for (Resource resource :
                resources) {
            System.out.println(resource.getDescription());
        }
    }
}
```

## 4.4 BeanFactory 和 ApplicationContext

### 4.4.1 BeanFactory 介绍

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\Car.java`
```java
package com.smart;

public class Car {
    private String brand;
    private String color;
    private int maxSpeed;

    public Car() {
    }

    public Car(String brand, String color, int maxSpeed) {
        this.brand = brand;
        this.color = color;
        this.maxSpeed = maxSpeed;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", color='" + color + '\'' +
                ", maxSpeed=" + maxSpeed +
                '}';
    }
}
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\resources\com\smart\beanfactory\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="car" class="com.smart.Car"
          p:brand="红旗CA72"
          p:color="黑色"
          p:maxSpeed="200"
    />

</beans>
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\beanfactory\BeanFactoryExample.java`
```java
package com.smart.beanfactory;

import com.smart.Car;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;
import org.springframework.core.io.Resource;
import org.springframework.core.io.support.PathMatchingResourcePatternResolver;
import org.springframework.core.io.support.ResourcePatternResolver;

/**
 * 4.4.1 节的示例 第 93 页
 */
public class BeanFactoryExample {

    public static void main(String[] args) throws Throwable {

        ResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
        Resource resource = resolver.getResource("classpath:com/smart/beanfactory/beans.xml");
        System.out.println(resource.getURL());

        DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
        reader.loadBeanDefinitions(resource);

        Car car = factory.getBean("car", Car.class);
        System.out.println(car.toString());
    }
}

```

[Class XmlBeanDefinitionReader](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/xml/XmlBeanDefinitionReader.html)

[Class DefaultListableBeanFactory](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/support/DefaultListableBeanFactory.html)

### 4.4.2 ApplicationContext 介绍

#### 1. ApplicationContext 类体系结构

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\context\Beans.java`
```java
package com.smart.context;

import com.smart.Car;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 4.4.2 节示例 第 96 页
 */

@Configuration
public class Beans {
    @Bean(name = "car")
    public Car buildCar() {
        Car car = new Car();
        car.setBrand("红旗CA72");
        car.setColor("黑色");
        car.setMaxSpeed(200);
        return car;
    }
}
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\context\AnnotationApplicationContextExample.java`
```java
package com.smart.context;

import com.smart.Car;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

/**
 * 4.4.2 节示例 第 97 页
 */
public class AnnotationApplicationContextExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(Beans.class);
        Car car = context.getBean("car", Car.class);
        System.out.println(car.toString());
    }
}
```

## 4.5 Bean 的生命周期

### 4.5.1 BeanFactory 中 Bean 的生命周期

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\Car.java`
```java
package com.smart;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.*;

/**
 * 4.5.1 节 第 106 页
 */
public class Car implements BeanFactoryAware, BeanNameAware, InitializingBean, DisposableBean {

    private String brand;
    private String color;
    private int maxSpeed;
    private BeanFactory beanFactory;
    private String beanName;

    public Car() {
        System.out.println("调用Car()构造函数。");
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        System.out.println("调用setBrand()设置属性。");
        this.brand = brand;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", color='" + color + '\'' +
                ", maxSpeed=" + maxSpeed +
                '}';
    }

    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("调用BeanFactoryAware.setBeanFactory()。");
        this.beanFactory = beanFactory;
    }

    public void setBeanName(String s) {
        System.out.println("调用BeanNameAware.setBeanName()。");
        this.beanName = beanName;
    }

    public void afterPropertiesSet() throws Exception {
        System.out.println("调用InitializingBean.afterPropertiesSet()。");
    }

    public void destroy() throws Exception {
        System.out.println("调用DisposableBean.destory()。");
    }

    public void myInit() {
        System.out.println("调用myInit()，将maxSpeed设置为240。");
        this.maxSpeed = 240;
    }

    public void myDestory() {
        System.out.println("调用myDestroy()。");
    }
}
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\resources\com\smart\beanfactory\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--4.5.1 节 第 109 页-->
    <bean id="car" class="com.smart.Car"
          init-method="myInit"
          destroy-method="myDestory"
          p:brand="红旗CA72"
          p:maxSpeed="200"
    />

</beans>
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\beanfactory\MyBeanPostProcessor.java`
```java
package com.smart.beanfactory;

import com.smart.Car;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

/**
 * 4.5.1 节 第 108 页
 */
public class MyBeanPostProcessor implements BeanPostProcessor {
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("car")) {
            Car car = (Car) bean;
            if (car.getColor() == null) {
                System.out.println("调用MyBeanPostProcessor.postProcessBeforeInitialization()，color为空，设置为默认黑色。");
                car.setColor("黑色");
            }
        }
        return bean;
    }

    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        if (beanName.equals("car")) {
            Car car = (Car) bean;
            if (car.getMaxSpeed() >= 200) {
                System.out.println("调用MyBeanPostProcessor.postProcessAfterInitialization()，将maxSpeed调整为200。");
                car.setMaxSpeed(200);
            }
        }
        return bean;
    }
}
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\beanfactory\MyInstantiationAwareBeanPostProcessor.java`
```java
package com.smart.beanfactory;

import org.springframework.beans.BeansException;
import org.springframework.beans.PropertyValues;
import org.springframework.beans.factory.config.InstantiationAwareBeanPostProcessorAdapter;

import java.beans.PropertyDescriptor;

/**
 * 4.5.1 节 第 107 页
 */
public class MyInstantiationAwareBeanPostProcessor extends InstantiationAwareBeanPostProcessorAdapter {
    @Override
    public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
        if("car".equals(beanName)){
            System.out.println("MyInstantiationAwareBeanPostProcessor.postProcessBeforeInstantiation");
        }
        return null;
    }

    @Override
    public boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
        if("car".equals(beanName)){
            System.out.println("InstantiationAwareBeanPostProcessor.postProcessAfterInstantiation");
        }
        return true;
    }

    @Override
    public PropertyValues postProcessPropertyValues(PropertyValues pvs, PropertyDescriptor[] pds, Object bean, String beanName) throws BeansException {
        if("car".equals(beanName)){
            System.out.println("InstantiationAwareBeanPostProcessor.postProcessPropertyValues");
        }
        return pvs;
    }

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }
}
```


```java
package com.smart.beanfactory;

import com.smart.Car;
import org.springframework.beans.factory.BeanFactory;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;

/**
 * 4.5.1 节 第 110 页
 */
public class BeanLifeCycleExample {

    public static void main(String[] args) {

        Resource resource = new ClassPathResource("com/smart/beanfactory/beans.xml");
        BeanFactory factory = new DefaultListableBeanFactory();
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader((DefaultListableBeanFactory) factory);
        reader.loadBeanDefinitions(resource);

        // ((DefaultListableBeanFactory) factory).addBeanPostProcessor(new MyBeanPostProcessor());
        // ((DefaultListableBeanFactory) factory).addBeanPostProcessor(new MyInstantiationAwareBeanPostProcessor());

        Car car1 = factory.getBean("car", Car.class);
        Car car2 = factory.getBean("car", Car.class);
        System.out.println(car1);
        System.out.println(car2);
        // 查看 car1 和 car2 是否指向同一引用
        System.out.println("car1 == car2: " + (car1 == car2));

        ((DefaultListableBeanFactory) factory).destroySingletons();
        System.out.println(car1);
        System.out.println(car2);
    }
}
```

### 4.5.2 ApplicationContext 中 Bean 的生命周期

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\context\MyBeanFactoryPostProcessor.java`
```java
package com.smart.context;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.config.BeanFactoryPostProcessor;
import org.springframework.beans.factory.config.ConfigurableListableBeanFactory;

/**
 * 4.5.2 节 第 113 页
 */
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        BeanDefinition beanDefinition = configurableListableBeanFactory.getBeanDefinition("car");
        beanDefinition.getPropertyValues().addPropertyValue("brand", "奇瑞QQ");
        System.out.println("调用MyBeanFactoryPostProcessor.postProcessBeanFactory()！");
    }
}
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\resources\com\smart\context\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
    <!--4.5.2 节 第 114 页-->
    <bean id="car" class="com.smart.Car"
          p:brand="红旗CA72"
          p:maxSpeed="200"
    />
    <bean id="myBeanFactoryPostProcessor" class="com.smart.context.MyBeanFactoryPostProcessor"/>
</beans>
```

`E:\Git\spring\spring4x-demos\chapter4\src\main\java\com\smart\context\TestBeanFactoryPostProcessor.java`
```java
package com.smart.context;

import com.smart.Car;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 4.5.2 节
 */
public class TestBeanFactoryPostProcessor {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("com/smart/context/beans.xml");
        Car car = context.getBean("car", Car.class);
        System.out.println(car);
    }
}
```