[TOC]

# 第 7 章 Spring AOP 基础

## 7.2 基础知识

### 7.2.1 带有横切逻辑的实例

## 7.3 创建增强类

### 7.3.1 增强类型

N/A

### 7.3.2 前置增强

`chapter7\pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>me.demos</groupId>
    <artifactId>chapter7</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <file.encoding>UTF-8</file.encoding>
        <spring.version>4.2.2.RELEASE</spring.version>
        <testng.version>6.8.7</testng.version>
        <asm.version>4.0</asm.version>
        <cglib.version>3.0</cglib.version>
        <aopalliance.version>1.0</aopalliance.version>
        <commons-codec.version>1.9</commons-codec.version>
        <slf4j.version>1.7.5</slf4j.version>
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
            <groupId>org.ow2.asm</groupId>
            <artifactId>asm</artifactId>
            <version>${asm.version}</version>
        </dependency>
        <dependency>
            <groupId>org.ow2.asm</groupId>
            <artifactId>asm-util</artifactId>
            <version>${asm.version}</version>
        </dependency>
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
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <dependency>
            <groupId>aopalliance</groupId>
            <artifactId>aopalliance</artifactId>
            <version>${aopalliance.version}</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>${commons-codec.version}</version>
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
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.7.2</version>
                <configuration>
                    <forkMode>once</forkMode>
                    <threadCount>10</threadCount>
                    <argLine>-Dfile.encoding=UTF-8</argLine>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

`chapter7\src\main\java\com\smart\advice\Waiter.java`
```java
package com.smart.advice;

public interface Waiter {
    void greetTo(String name);
    void serveTo(String name);
}
```

`chapter7\src\main\java\com\smart\advice\NaiveWaiter.java`
```java
package com.smart.advice;

public class NaiveWaiter implements Waiter {
    public void greetTo(String name) {
        System.out.println("greet to " + name + "...");
    }

    public void serveTo(String name) {
        System.out.println("serving " + name + "...");
    }
}
```

`chapter7\src\main\java\com\smart\advice\GreetingBeforeAdvice.java`
```java
package com.smart.advice;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class GreetingBeforeAdvice implements MethodBeforeAdvice {
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        String clientName = (String) objects[0];
        System.out.println("How are you! Mr." + clientName + ".");
    }
}
```

**测试类（使用代码配置）：**

`chapter7\src\test\java\com\smart\advice\BeforeAdviceTest.java`
```java
package com.smart.advice;

import org.springframework.aop.BeforeAdvice;
import org.springframework.aop.framework.ProxyFactory;
import org.testng.annotations.Test;

public class BeforeAdviceTest {

    @Test
    public void before() {
        Waiter target = new NaiveWaiter();
        BeforeAdvice advice = new GreetingBeforeAdvice();
        ProxyFactory factory = new ProxyFactory();
        // 指定对接口进行代理
        factory.setInterfaces(target.getClass().getInterfaces());
        // 启用优化，也就是使用 CGLib 动态代理技术。
        factory.setOptimize(true);
        factory.setTarget(target);
        factory.addAdvice(advice);

        Waiter proxy = (Waiter) factory.getProxy();
        proxy.greetTo("John");
        proxy.serveTo("Tom");
    }
}
```

**测试类（使用 XML 配置）：**

`chapter7\src\main\resources\com\smart\advice\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="greetingAdvice" class="com.smart.advice.GreetingBeforeAdvice"/>
    <bean id="target" class="com.smart.advice.NaiveWaiter"/>
    <bean id="waiter" class="org.springframework.aop.framework.ProxyFactoryBean"
          p:proxyInterfaces="com.smart.advice.Waiter"
          p:interceptorNames="greetingAdvice"
          p:target-ref="target"
    />
</beans>
```

`chapter7\src\test\java\com\smart\advice\AdviceTest.java`
```java
package com.smart.advice;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;

public class AdviceTest {

    @Test
    public void advice() {
        String configPath = "com/smart/advice/beans.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configPath);
        Waiter waiter = (Waiter) context.getBean("waiter");
        waiter.greetTo("John");
    }
}
```

### 7.3.3 后置增强

`chapter7\src\main\java\com\smart\advice\GreetingAfterAdvice.java`
```java
package com.smart.advice;

import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class GreetingAfterAdvice implements AfterReturningAdvice {
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("Please enjoy yourself!");
    }
}
```

`chapter7\src\main\resources\com\smart\advice\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="greetingBefore" class="com.smart.advice.GreetingBeforeAdvice"/>
    <bean id="greetingAfter" class="com.smart.advice.GreetingAfterAdvice"/>
    <bean id="target" class="com.smart.advice.NaiveWaiter"/>
    <bean id="waiter" class="org.springframework.aop.framework.ProxyFactoryBean"
          p:proxyInterfaces="com.smart.advice.Waiter"
          p:interceptorNames="greetingBefore,greetingAfter"
          p:target-ref="target"
    />
</beans>
```

**测试类：**

同上

### 7.3.4 环绕增强

```java
package com.smart.advice;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;

public class GreetingInterceptor implements MethodInterceptor {
    public Object invoke(MethodInvocation invocation) throws Throwable {

        Object[] args = invocation.getArguments();
        String clientName = (String) args[0];
        System.out.println("How are you！Mr." + clientName + ".");

        Object obj = invocation.proceed();

        System.out.println("Please enjoy yourself!");

        return obj;
    }
}
```

`chapter7\src\main\resources\com\smart\advice\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
       
    <bean id="greetingAround" class="com.smart.advice.GreetingInterceptor"/>
    <bean id="target" class="com.smart.advice.NaiveWaiter"/>
    <bean id="waiter" class="org.springframework.aop.framework.ProxyFactoryBean"
          p:proxyInterfaces="com.smart.advice.Waiter"
          p:interceptorNames="greetingAround"
          p:target-ref="target"
    />

</beans>
```

**测试类：**

同上

### 7.3.5 异常抛出增强

`chapter7\src\main\java\com\smart\advice\Forum.java`
```java
package com.smart.advice;

public class Forum {
    private int forumId;

    public int getForumId() {
        return forumId;
    }

    public void setForumId(int forumId) {
        this.forumId = forumId;
    }
}
```

`chapter7\src\main\java\com\smart\advice\ForumService.java`
```java
package com.smart.advice;

import java.sql.SQLException;

public class ForumService {
    public void removeForum(int forumId) {
        throw new RuntimeException("运行异常。");
    }

    public void updateForum(Forum forum) throws Exception {
        throw new SQLException("数据更新操作异常。");
    }
}
```

`chapter7\src\main\java\com\smart\advice\TransactionManager.java`
```java
package com.smart.advice;

import org.springframework.aop.ThrowsAdvice;

import java.lang.reflect.Method;

public class TransactionManager implements ThrowsAdvice {
    public void afterThrowing(Method method, Object[] args, Object target,
                              Exception ex) throws Throwable {
        System.out.println("-----------");
        System.out.println("method: " + method.getName());
        System.out.println("抛出异常：" + ex.getMessage());
        System.out.println("成功回滚事务。");
    }
}
```

`chapter7\src\main\resources\com\smart\advice\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="forumServiceTarget" class="com.smart.advice.ForumService"/>
    <bean id="transactionManager" class="com.smart.advice.TransactionManager"/>
    <bean id="forumService" class="org.springframework.aop.framework.ProxyFactoryBean"
          p:interceptorNames="transactionManager"
          p:target-ref="forumServiceTarget"
          p:proxyTargetClass="true"
    />

</beans>
```

**测试类：**

`chapter7\src\test\java\com\smart\advice\ThrowAdviceTest.java`
```java
package com.smart.advice;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;

public class ThrowAdviceTest {
    @Test
    public void throwAdvice() {
        String configPath = "com/smart/advice/beans.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configPath);
        ForumService forumService = (ForumService) context.getBean("forumService");

        try {
            forumService.removeForum(10);
        } catch (Exception e) {
            //
        }

        try {
            forumService.updateForum(new Forum());
        } catch (Exception e) {
            //
        }
    }
}
```

### 7.3.6 引介增强

`chapter7\src\main\java\com\smart\introduce\MethodPerformace.java`
```java
package com.smart.introduce;

public class MethodPerformace {
    private long begin;
    private long end;
    private String serviceMethod;

    public MethodPerformace(String serviceMethod) {
        reset(serviceMethod);
    }

    public void printPerformace() {
        end = System.currentTimeMillis();
        long elapse = end - begin;
        System.out.println(serviceMethod + "花费" + elapse + "毫秒。");
    }

    public void reset(String serviceMethod) {
        this.serviceMethod = serviceMethod;
        this.begin = System.currentTimeMillis();
    }
}
```

`chapter7\src\main\java\com\smart\introduce\PerformanceMonitor.java`
```java
package com.smart.introduce;

public class PerformanceMonitor {
    private static ThreadLocal<MethodPerformace> performaceRecord = new ThreadLocal<MethodPerformace>();

    public static void begin(String method) {
        System.out.println("begin monitor...");
        MethodPerformace mp = performaceRecord.get();
        if (mp == null) {
            mp = new MethodPerformace(method);
            performaceRecord.set(mp);
        } else {
            mp.reset(method);
        }
    }

    public static void end() {
        System.out.println("end monitor...");
        MethodPerformace mp = performaceRecord.get();
        mp.printPerformace();
    }
}
```

`chapter7\src\main\java\com\smart\introduce\ForumService.java`
```java
package com.smart.introduce;

public class ForumService {

    public void removeTopic(int topicId) {
        System.out.println("模拟删除Topic记录:" + topicId);
        try {
            Thread.currentThread().sleep(20);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }

    }

    public void removeForum(int forumId) {
        System.out.println("模拟删除Forum记录:" + forumId);
        try {
            Thread.currentThread().sleep(40);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

`chapter7\src\main\java\com\smart\introduce\Monitorable.java`
```java
package com.smart.introduce;

public interface Monitorable {
    void setMonitorActive(boolean active);
}
```

`chapter7\src\main\java\com\smart\introduce\ControllablePerformaceMonitor.java`
```java
package com.smart.introduce;

import org.aopalliance.intercept.MethodInvocation;
import org.springframework.aop.support.DelegatingIntroductionInterceptor;

public class ControllablePerformaceMonitor extends DelegatingIntroductionInterceptor implements Monitorable {

    private ThreadLocal<Boolean> MonitorStatusMap = new ThreadLocal<Boolean>();

    public void setMonitorActive(boolean active) {
        MonitorStatusMap.set(active);
    }

    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        Object obj = null;
        if (MonitorStatusMap.get() != null && MonitorStatusMap.get()) {
            PerformanceMonitor.begin(methodInvocation.getClass().getName() + "." + methodInvocation.getMethod().getName());
            obj = super.invoke(methodInvocation);
            PerformanceMonitor.end();
        } else {
            obj = super.invoke(methodInvocation);
        }
        return obj;
    }
}
```

`chapter7\src\main\resources\com\smart\introduce\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="pmonitor" class="com.smart.introduce.ControllablePerformaceMonitor"/>
    <bean id="forumServiceTarget" class="com.smart.introduce.ForumService"/>
    <bean id="forumService" class="org.springframework.aop.framework.ProxyFactoryBean"
          p:interfaces="com.smart.introduce.Monitorable"
          p:target-ref="forumServiceTarget"
          p:interceptorNames="pmonitor"
          p:proxyTargetClass="true"/>

</beans>
```

**测试类：**

`chapter7\src\test\java\com\smart\introduce\IntroduceTest.java`
```java
package com.smart.introduce;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;

public class IntroduceTest {

    @Test
    public void introduce() {

        String configPath = "com/smart/introduce/beans.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configPath);

        ForumService forumService = (ForumService) context.getBean("forumService");
        forumService.removeForum(10);
        forumService.removeTopic(1022);

        Monitorable monitorable = (Monitorable) forumService;
        monitorable.setMonitorActive(true);
        forumService.removeForum(10);
        forumService.removeTopic(1022);
    }
}
```

## 7.4 创建切面

### 7.4.3 静态普通方法名匹配切面

`chapter7\src\main\java\com\smart\advisor\Waiter.java`
```java
package com.smart.advisor;

public class Waiter {
    public void greetTo(String name) {
        System.out.println("waiter greet to " + name + "...");
    }

    public void serveTo(String name) {
        System.out.println("waiter serving " + name + "...");
    }
}
```

`chapter7\src\main\java\com\smart\advisor\Seller.java`
```java
package com.smart.advisor;

public class Seller {
    public void greetTo(String name) {
        System.out.println("seller greet to " + name + "...");
    }
}
```

`chapter7\src\main\java\com\smart\advisor\GreetingAdvisor.java`
```java
package com.smart.advisor;

import org.springframework.aop.ClassFilter;
import org.springframework.aop.support.StaticMethodMatcherPointcutAdvisor;

import java.lang.reflect.Method;

public class GreetingAdvisor extends StaticMethodMatcherPointcutAdvisor {
    @Override
    public ClassFilter getClassFilter() {
        return new ClassFilter() {
            public boolean matches(Class<?> aClass) {
                return Waiter.class.isAssignableFrom(aClass);
            }
        };
    }

    public boolean matches(Method method, Class<?> aClass) {
        return "greetTo".equals(method.getName());
    }
}
```

`chapter7\src\main\java\com\smart\advisor\GreetingBeforeAdvice.java`
```java
package com.smart.advisor;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class GreetingBeforeAdvice implements MethodBeforeAdvice {
    public void before(Method method, Object[] args, Object obj) throws Throwable {
        String clientName = (String) args[0];
        System.out.println(obj.getClass().getName() + "." + method.getName());
        System.out.println("How are you! Mr." + clientName + ".");
    }
}
```

`chapter7\src\main\resources\com\smart\advisor\beans.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/util
	http://www.springframework.org/schema/util/spring-util-4.0.xsd">

    <bean id="waiterTarget" class="com.smart.advisor.Waiter"/>
    <bean id="sellerTarget" class="com.smart.advisor.Seller"/>
    <bean id="greetingAdvice" class="com.smart.advisor.GreetingBeforeAdvice"/>
    <bean id="greetingAdvisor" class="com.smart.advisor.GreetingAdvisor" p:advice-ref="greetingAdvice"/>

    <bean id="parent" abstract="true"
          class="org.springframework.aop.framework.ProxyFactoryBean"
          p:interceptorNames="greetingAdvisor" p:proxyTargetClass="true"/>
    <bean id="waiter" parent="parent" p:target-ref="waiterTarget" />
    <bean id="seller" parent="parent" p:target-ref="sellerTarget" />

</beans>
```

**测试类：**

`chapter7\src\test\java\com\smart\advisor\StaticMethodAdvisorTest.java`
```java
package com.smart.advisor;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.testng.annotations.Test;

public class StaticMethodAdvisorTest {

    @Test
    public void staticMethod() {
        String configPath = "com/smart/advisor/beans.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configPath);

        Waiter waiter = (Waiter) context.getBean("waiter");
        Seller seller = (Seller) context.getBean("seller");
        waiter.greetTo("John");
        waiter.serveTo("John");
        seller.greetTo("John");
    }
}
```