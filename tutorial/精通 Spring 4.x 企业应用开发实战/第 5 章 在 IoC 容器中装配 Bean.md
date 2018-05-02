[TOC]

# 第 5 章 在 IoC 容器中装配 Bean

`chapter5\pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>me.demos</groupId>
    <artifactId>chapter5</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>chapter5 Maven Webapp</name>
    <url>http://maven.apache.org</url>

    <properties>
        <file.encoding>UTF-8</file.encoding>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>

        <spring.version>4.2.2.RELEASE</spring.version>
        <groovy.version>2.3.6</groovy.version>
        <mockito.version>1.10.19</mockito.version>
        <testng.version>6.8.7</testng.version>
        <asm.version>4.0</asm.version>
        <cglib.version>3.0</cglib.version>
        <aspectj.version>1.8.1</aspectj.version>
        <aopalliance.version>1.0</aopalliance.version>
        <commons-codec.version>1.9</commons-codec.version>
    </properties>

    <dependencies>

        <!-- spring 依赖-->
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


        <!-- asm/cglib依赖（spring依赖） -->
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>${cglib.version}</version>
            <exclusions>
                <exclusion>
                    <artifactId>asm</artifactId>
                    <groupId>org.ow2.asm</groupId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>${aspectj.version}</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>${aspectj.version}</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>${commons-codec.version}</version>
        </dependency>

        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
            <version>${groovy.version}</version>
        </dependency>

        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <build>
        <finalName>chapter5</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.7.2</version>
                <configuration>
                    <parallel>methods</parallel>
                    <threadCount>10</threadCount>
                    <argLine>-Dfile.encoding=UTF-8</argLine>
                    <!-- <skip>true</skip> -->
                    <!-- <testFailureIgnore>true</testFailureIgnore> -->
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

## 5.1 Spring 配置概述

### 5.1.1 Spring 容器高层视图

N/A

### 5.1.2 基于 XML 配置

N/A

## 5.2 Bean 基本配置

### 5.2.1 装配一个 Bean

`chapter5\src\main\java\com\smart\simple\Car.java`
```java
package com.smart.simple;

/**
 * 5.2.1 节 第 120 页
 */
public class Car {
}
```

`chapter5\src\main\java\com\smart\simple\Boss.java`
```java
package com.smart.simple;

/**
 * 5.2.1 节 第 120 页
 */
public class Boss {
}
```

`chapter5\src\main\resources\com\smart\simple\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.2.1 节 第 120 页-->
    <bean id="car" class="com.smart.simple.Car"/>
    <bean id="boss" class="com.smart.simple.Boss"/>
</beans>
```

`chapter5\src\test\java\com\smart\simple\TestBeanRetrieve.java`
```java
package com.smart.simple;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.2.1 节 第 120 页
 */
public class TestBeanRetrieve {
    public ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/simple/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testBeanRetrieve() {
        Car car = context.getBean("car", Car.class);
        assertNotNull(car);
    }
}
```

### 5.2.2 Bean 的命名

N/A

## 5.3 依赖注入

### 5.3.1 属性注入

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.1 节 第 122 页
 */
public class Car {

    private String brand;
    private int maxSpeed;
    private double price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

`chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.3.1 节 第 122 页-->
    <bean id="car" class="com.smart.ditype.Car">
        <property name="brand" value="红旗CA72"/>
        <property name="maxSpeed" value="200"/>
        <property name="price" value="20000.00"/>
    </bean>
</beans>
```

`chapter5\src\test\java\com\smart\ditype\TestDiType.java`
```java
package com.smart.ditype;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.3.1 节
 */
public class TestDiType {
    public ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/ditype/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testCar() {
        Car car = context.getBean("car", Car.class);
        assertNotNull(car);
        System.out.println(car);
    }
}
```

### 5.3.2 构造函数注入

#### 1. 按类型匹配入参

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 第 124 页
 */
public class Car {

    private String brand;
    private int maxSpeed;
    private double price;

    public Car() {
    }

    public Car(String brand, int maxSpeed, double price) {
        this.brand = brand;
        this.maxSpeed = maxSpeed;
        this.price = price;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getMaxSpeed() {
        return maxSpeed;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", maxSpeed=" + maxSpeed +
                ", price=" + price +
                '}';
    }
}
```

`chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.3.2 节 第 125 页-->
    <bean id="car" class="com.smart.ditype.Car">
        <constructor-arg type="java.lang.String">
            <value>红旗CA72</value>
        </constructor-arg>
        <constructor-arg type="int">
            <value>200</value>
        </constructor-arg>
        <constructor-arg type="double">
            <value>20000.00</value>
        </constructor-arg>
    </bean>
</beans>
```

#### 2. 按索引匹配入参

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 按索引匹配入参
 */
public class Car {

    private String brand;
    private String corp;
    private double price;

    public Car() {
    }

    public Car(String brand, String corp, double price) {
        this.brand = brand;
        this.corp = corp;
        this.price = price;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getCorp() {
        return corp;
    }

    public void setCorp(String corp) {
        this.corp = corp;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", corp='" + corp + '\'' +
                ", price=" + price +
                '}';
    }
}
```

`chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.3.2 节 按索引匹配入参-->
    <bean id="car" class="com.smart.ditype.Car">
        <constructor-arg index="0" value="红旗CA72"/>
        <constructor-arg index="1" value="中国一汽"/>
        <constructor-arg index="2" value="20000.00"/>
    </bean>
</beans>
```

#### 3. 联合使用类型和索引匹配入参

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 联合使用类型和索引匹配入参
 */
public class Car {

    private String brand;
    private String corp;
    private double price;
    private int maxSpeed;

    public Car() {
    }

    public Car(String brand, String corp, double price) {
        this.brand = brand;
        this.corp = corp;
        this.price = price;
    }

    public Car(String brand, String corp, int maxSpeed) {
        this.brand = brand;
        this.corp = corp;
        this.maxSpeed = maxSpeed;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getCorp() {
        return corp;
    }

    public void setCorp(String corp) {
        this.corp = corp;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
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
                ", corp='" + corp + '\'' +
                ", maxSpeed=" + maxSpeed +
                '}';
    }
}
```

`chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.3.2 节 联合使用类型和索引匹配入参-->
    <bean id="car" class="com.smart.ditype.Car">
        <constructor-arg index="0" type="java.lang.String" value="红旗CA72"/>
        <constructor-arg index="1" type="java.lang.String" value="中国一汽"/>
        <constructor-arg index="2" type="int" value="200"/>
    </bean>
</beans>
```

#### 4. 通过自身类型反射匹配入参

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 通过自身类型反射匹配入参
 */
public class Car {
    private String brand;
    private int maxSpeed;
    private double price;

    public Car() {
    }

    public Car(String brand, int maxSpeed, double price) {
        this.brand = brand;
        this.maxSpeed = maxSpeed;
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", maxSpeed=" + maxSpeed +
                ", price=" + price +
                '}';
    }
}
```

`chapter5\src\main\java\com\smart\ditype\Office.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 通过自身类型反射匹配入参
 */
public class Office {
}
```

`chapter5\src\main\java\com\smart\ditype\Boss.java`
```java
package com.smart.ditype;

/**
 * 5.3.2 节 通过自身类型反射匹配入参
 */
public class Boss {
    private String name;
    private Car car;
    private Office office;

    public Boss() {
    }

    public Boss(String name, Car car, Office office) {
        this.name = name;
        this.car = car;
        this.office = office;
    }

    public Boss(String name, Car car) {
        this.name = name;
        this.car = car;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "name='" + name + '\'' +
                ", car=" + car +
                ", office=" + office +
                '}';
    }
}
```


`chapter5\src\test\java\com\smart\ditype\TestDiType.java`
```java
package com.smart.ditype;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

public class TestDiType {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/ditype/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testBoss() {
        Boss boss = context.getBean("boss", Boss.class);
        assertNotNull(boss);
        System.out.println(boss);
    }
}
```

### 5.3.3 工厂方法注入

`chapter5\src\main\java\com\smart\ditype\Car.java`
```java
package com.smart.ditype;

/**
 * 5.3.3 节 工厂方法注入
 */
public class Car {
    private String brand;

    public void setBrand(String brand) {
        this.brand = brand;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                '}';
    }
}
```

`chapter5\src\main\java\com\smart\ditype\CarFactory.java`
```java
package com.smart.ditype;

/**
 * 5.3.3 节 工厂方法注入
 */
public class CarFactory {
    public Car createHongQiCar() {
        Car car = new Car();
        car.setBrand("红旗CA72");
        return car;
    }

    public static Car createQiRuiCar() {
        Car car = new Car();
        car.setBrand("奇瑞");
        return car;
    }
}
```

`chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.3.3 节 工厂方法注入-->
    <bean id="carFactory" class="com.smart.ditype.CarFactory"/>
    <bean id="car1" factory-bean="carFactory" factory-method="createHongQiCar"/>
    <bean id="car2" class="com.smart.ditype.CarFactory" factory-method="createQiRuiCar"/>
</beans>
```

`chapter5\src\test\java\com\smart\ditype\TestDiType.java`
```java
package com.smart.ditype;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.3.3 节 工厂方法注入
 */
public class TestDiType {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/ditype/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testCar() {
        Car car1 = context.getBean("car1", Car.class);
        assertNotNull(car1);
        System.out.println(car1);

        Car car2 = context.getBean("car2", Car.class);
        assertNotNull(car2);
        System.out.println(car2);
    }
}
```

## 5.4 注入参数详解

### 5.4.1 字面值

`chapter5\src\main\java\com\smart\attr\Car.java`
```java
package com.smart.attr;

/**
 * 5.4.1 节 字面值
 * 5.4.2 节 引用其他 Bean
 */
public class Car {
    private String brand;
    private int maxSpeed;
    private double price;

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", maxSpeed=" + maxSpeed +
                ", price=" + price +
                '}';
    }
}
```

`chapter5\src\main\resources\com\smart\attr\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.1 节 字面值-->
    <bean id="car" class="com.smart.attr.Car">
        <property name="maxSpeed" value="200"/>
        <property name="brand">
            <value><![CDATA[红旗&CA72]]></value>
        </property>
    </bean>
</beans>
```

`chapter5\src\test\java\com\smart\attr\TestBeanAttrDI.java`
```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;
import static org.testng.Assert.assertTrue;

/**
 * 5.4.1 节 字面值
 */
public class TestBeanAttrDI {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/attr/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testBeanRetrieveCar() {
        Car car = context.getBean("car", Car.class);
        assertNotNull(car);
        System.out.println("" + car);
    }
}
```

### 5.4.2 引用其他 Bean

`chapter5\src\main\java\com\smart\attr\Boss.java`
```java
package com.smart.attr;

/**
 * 5.4.2 节 引用其他 Bean
 */
public class Boss {
    private Car car;

    public void setCar(Car car) {
        this.car = car;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "car=" + car +
                '}';
    }
}
```

`chapter5\src\main\resources\com\smart\attr\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.2 节 引用其他 Bean-->
    <bean id="car" class="com.smart.attr.Car">
        <property name="maxSpeed" value="200"/>
        <property name="brand">
            <value><![CDATA[红旗&CA72]]></value>
        </property>
    </bean>

    <bean id="boss" class="com.smart.attr.Boss">
        <property name="car">
            <ref bean="car"/>
        </property>
    </bean>
</beans>
```

`chapter5\src\test\java\com\smart\attr\TestBeanAttrDI.java`
```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;
import static org.testng.Assert.assertTrue;

/**
 * 5.4.2 节 引用其他 Bean
 */
public class TestBeanAttrDI {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/attr/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testBoss() {
        Boss boss = context.getBean("boss", Boss.class);
        assertNotNull(boss);
        System.out.println(boss);
    }
}
```

**子容器对父容器中 Bean 的引用**

`E:\Git\spring\spring4x-demos\chapter5\src\main\resources\com\smart\attr\beans-parent.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.2 子容器对父容器中 Bean 的引用-->
    <bean id="car" class="com.smart.attr.Car">
        <property name="brand" value="红旗（父容器）"/>
        <property name="maxSpeed" value="200"/>
        <property name="price" value="20000.00"/>
    </bean>
</beans>
```

`E:\Git\spring\spring4x-demos\chapter5\src\main\resources\com\smart\attr\beans-child.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.2 子容器对父容器中 Bean 的引用-->
    <bean id="car" class="com.smart.attr.Car">
        <property name="brand" value="吉利（子容器）"/>
        <property name="maxSpeed" value="100"/>
        <property name="price" value="2000.00"/>
    </bean>

    <bean id="boss" class="com.smart.attr.Boss">
        <property name="car">
            <ref parent="car"/>
        </property>
    </bean>
</beans>
```

`E:\Git\spring\spring4x-demos\chapter5\src\test\java\com\smart\attr\ParentContainerBeanTest.java`
```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.4.2 子容器对父容器中 Bean 的引用
 */
public class ParentContainerBeanTest {
    @Test
    public void parent() {
        ApplicationContext parentContext = new ClassPathXmlApplicationContext(new String[]{"com/smart/attr/beans-parent.xml"});
        ApplicationContext childContext = new ClassPathXmlApplicationContext(new String[]{"com/smart/attr/beans-child.xml"}, parentContext);
        Boss boss = childContext.getBean("boss", Boss.class);
        assertNotNull(boss);
        System.out.println(boss.getCar().toString());
    }
}
```

### 5.4.3 内部 Bean


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.3 内部 Bean-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="car">
            <bean class="com.smart.attr.Car">
                <property name="maxSpeed" value="200"/>
                <property name="price" value="20000.00"/>
                <property name="brand" value="吉利"/>
            </bean>
        </property>
    </bean>
</beans>
```


```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.4.3 内部 Bean
 */
public class MyTest {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/attr/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void test() {
        Boss boss = context.getBean("boss", Boss.class);
        assertNotNull(boss);
        System.out.println(boss.getCar());
    }
}
```

### 5.4.4 null 值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.4 null 值-->
    <bean id="car" class="com.smart.attr.Car">
        <property name="brand">
            <null></null>
        </property>
        <property name="price" value="20000.00"/>
        <property name="maxSpeed" value="200"/>
    </bean>
</beans>
```

```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.4.4 null 值
 */
public class MyTest {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/attr/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void test() {
        Car car = context.getBean("car", Car.class);
        assertNotNull(car);
        System.out.println(car);
    }
}
```

### 5.4.5 级联属性

```java
package com.smart.attr;

/**
 * 5.4.5 级联属性
 */
public class Boss {
    private Car car = new Car();

    public void setCar(Car car) {
        this.car = car;
    }

    public Car getCar() {
        return car;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "car=" + car +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.5 级联属性-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="car.brand" value="吉利"/>
    </bean>
</beans>
```

### 5.4.6 集合类型属性

#### 1. List

```java
package com.smart.attr;

import java.util.ArrayList;
import java.util.List;

/**
 * 5.4.6 集合类型属性
 */
public class Boss {
    private List favorites = new ArrayList();

    public List getFavorites() {
        return favorites;
    }

    public void setFavorites(List favorites) {
        this.favorites = favorites;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "favorites=" + favorites +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="favorites">
            <list>
                <value>看报</value>
                <value>赛车</value>
                <value>高尔夫</value>
            </list>
        </property>
    </bean>
</beans>
```

```java
package com.smart.attr;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeClass;
import org.testng.annotations.Test;

import static org.testng.Assert.assertNotNull;

/**
 * 5.4.6 集合类型属性
 */
public class MyTest {
    private ApplicationContext context = null;
    private static String[] CONFIG_FILES = {"com/smart/attr/beans.xml"};

    @BeforeClass
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void test() {
        Boss boss = context.getBean("boss", Boss.class);
        assertNotNull(boss);
        System.out.println(boss);
    }
}
```

#### 2. Set

```java
package com.smart.attr;

import java.util.HashSet;
import java.util.Set;

/**
 * 5.4.6 集合类型属性 Set
 */
public class Boss {
    private Set favorites = new HashSet();

    public Set getFavorites() {
        return favorites;
    }

    public void setFavorites(Set favorites) {
        this.favorites = favorites;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "favorites=" + favorites +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性 Set-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="favorites">
            <set>
                <value>看报</value>
                <value>赛车</value>
                <value>高尔夫</value>
            </set>
        </property>
    </bean>
</beans>
```

#### 3. Map

```java
package com.smart.attr;

import java.util.HashMap;
import java.util.Map;

/**
 * 5.4.6 集合类型属性 Map
 */
public class Boss {
    private Map jobs = new HashMap();

    public Map getJobs() {
        return jobs;
    }

    public void setJobs(Map jobs) {
        this.jobs = jobs;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "jobs=" + jobs +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性 Map-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="jobs">
            <map>
                <entry key="AM" value="会见客户"/>
                <entry key="PM" value="公司内部会议"/>
            </map>
        </property>
    </bean>
</beans>
```

#### 4. Properties

```java
package com.smart.attr;

import java.util.Properties;

/**
 * 5.4.6 集合类型属性 Properties
 */
public class Boss {
    private Properties mails = new Properties();

    public Properties getMails() {
        return mails;
    }

    public void setMails(Properties mails) {
        this.mails = mails;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "mails=" + mails +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性 Properties-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="mails">
            <props>
                <prop key="jobMail">john-office@smart.com</prop>
                <prop key="lifeMail">john-life@smart.com</prop>
            </props>
        </property>
    </bean>
</beans>
```
#### 5. 强类型集合

```java
package com.smart.attr;

import java.util.HashMap;
import java.util.Map;

/**
 * 5.4.6 集合类型属性 强类型集合
 */
public class Boss {
    private Map<String, Integer> jobTime = new HashMap<>();

    public Map<String, Integer> getJobTime() {
        return jobTime;
    }

    public void setJobTime(Map<String, Integer> jobTime) {
        this.jobTime = jobTime;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "jobTime=" + jobTime +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性 强类型集合-->
    <bean id="boss" class="com.smart.attr.Boss">
        <property name="jobTime">
            <map>
                <entry key="会见客户" value="124"/>
            </map>
        </property>
    </bean>
</beans>
```

#### 6. 集合合并

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.6 集合类型属性 集合合并-->
    <bean id="parentBoss" abstract="true" class="com.smart.attr.Boss">
        <property name="favorites">
            <set>
                <value>看报</value>
                <value>赛车</value>
                <value>高尔夫</value>
            </set>
        </property>
    </bean>
    <bean id="childBoss" parent="parentBoss">
        <property name="favorites">
            <set merge="true">
                <value>爬山</value>
                <value>游泳</value>
            </set>
        </property>
    </bean>
</beans>
```

### 5.4.7 简化配置方式

#### 3. 使用 p 命名空间

`E:\Git\spring\spring4x-demos\chapter5\src\main\resources\com\smart\ditype\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.4.7 使用 p 命名空间-->
    <bean id="car" class="com.smart.ditype.Car"
          p:brand="红旗"
          p:maxSpeed="200"
          p:price="20000.00"/>
    <bean id="boss" class="com.smart.ditype.Boss"
          p:car-ref="car"/>
</beans>
```

### 5.4.8 自动装配

```java
package com.smart;

/**
 * 5.4.8 自动装配
 */
public class Car {
    private String brand;
    private String corp;
    private double price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getCorp() {
        return corp;
    }

    public void setCorp(String corp) {
        this.corp = corp;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", corp='" + corp + '\'' +
                ", price=" + price +
                '}';
    }
}
```

```java
package com.smart;

/**
 * 5.4.8 自动装配
 */
public class Office {
    private String city;

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    @Override
    public String toString() {
        return "Office{" +
                "city='" + city + '\'' +
                '}';
    }
}
```

```java
package com.smart;

/**
 * 5.4.8 自动装配
 */
public class Boss {
    private String name;
    private Car car;
    private Office office;

    public Boss() {
    }

    public Boss(String name, Car car, Office office) {
        this.name = name;
        this.car = car;
        this.office = office;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Car getCar() {
        return car;
    }

    public void setCar(Car car) {
        this.car = car;
    }

    public Office getOffice() {
        return office;
    }

    public void setOffice(Office office) {
        this.office = office;
    }

    @Override
    public String toString() {
        return "Boss{" +
                "name='" + name + '\'' +
                ", car=" + car +
                ", office=" + office +
                '}';
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--5.4.8 自动装配-->
    <bean id="car" class="com.smart.Car" scope="singleton">
        <property name="brand" value="红旗"/>
        <property name="price" value="20000.00"/>
    </bean>

    <bean id="office" class="com.smart.Office" autowire-candidate="false">
        <property name="city" value="Guangzhou"/>
    </bean>
    <bean id="office1" class="com.smart.Office" autowire-candidate="true">
        <property name="city" value="Huizhou"/>
    </bean>

    <bean id="boss" class="com.smart.Boss" autowire="constructor">
        <constructor-arg index="0" value="John"/>
    </bean>

</beans>
```

## 5.5 方法注入

### 5.5.1 lookup 方法注入

```java
package com.smart.injectfun;

/**
 * 5.5 方法注入
 */
public class Car {
    private String brand;
    private String corp;
    private double price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public String getCorp() {
        return corp;
    }

    public void setCorp(String corp) {
        this.corp = corp;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", corp='" + corp + '\'' +
                ", price=" + price +
                '}';
    }
}
```

```java
package com.smart.injectfun;

/**
 * 5.5 方法注入
 */
public interface MagicBoss {
    Car getCar();
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="car" class="com.smart.injectfun.Car"
          p:brand="红旗"
          p:price="20000.00"
          scope="prototype"/>

    <bean id="magicBoss" class="com.smart.injectfun.MagicBoss">
        <lookup-method name="getCar" bean="car"/>
    </bean>
</beans>
```

```java
package com.smart.injectfun;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import static org.testng.Assert.*;

/**
 * 5.5.1 lookup 方法注入
 */
public class InjectFunTest {
    private ApplicationContext context = null;

    private static String[] CONFIG_FILES = {"com/smart/injectfun/beans.xml"};

    @BeforeMethod
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testLookup() {
        MagicBoss magicBoss = context.getBean("magicBoss", MagicBoss.class);
        assertNotSame(magicBoss.getCar(), magicBoss.getCar());
    }
}
```

### 5.5.2 方法替换

```java
package com.smart.injectfun;

/**
 * 5.5.2 方法替换
 */
public class Boss1 implements MagicBoss {
    @Override
    public Car getCar() {
        Car car = new Car();
        car.setBrand("宝马Z4");
        return car;
    }
}
```

```java
package com.smart.injectfun;

import org.springframework.beans.factory.support.MethodReplacer;

import java.lang.reflect.Method;

/**
 * 5.5.2 方法替换
 */
public class Boss2 implements MethodReplacer {
    @Override
    public Object reimplement(Object o, Method method, Object[] objects) throws Throwable {
        Car car = new Car();
        car.setBrand("美人豹");
        return car;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--5.5.2 方法替换-->
    <bean id="boss2" class="com.smart.injectfun.Boss2"/>
    <bean id="boss1" class="com.smart.injectfun.Boss1">
        <replaced-method name="getCar" replacer="boss2"/>
    </bean>
</beans>
```

```java
package com.smart.injectfun;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import static org.testng.Assert.*;

/**
 * 5.5.2 方法替换
 */
public class InjectFunTest {
    private ApplicationContext context = null;

    private static String[] CONFIG_FILES = {"com/smart/injectfun/beans.xml"};

    @BeforeMethod
    public void setUp() throws Exception {
        context = new ClassPathXmlApplicationContext(CONFIG_FILES);
    }

    @Test
    public void testReplace() {
        MagicBoss magicBoss = context.getBean("boss1", Boss1.class);
        assertEquals(magicBoss.getCar().getBrand(), "美人豹");
    }
}
```

[Interface MethodReplacer](https://docs.spring.io/spring/docs/4.3.14.RELEASE/javadoc-api/org/springframework/beans/factory/support/MethodReplacer.html)