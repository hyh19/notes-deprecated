[TOC]

# 第17章　Spring MVC介绍

## 17.1　采用Spring MVC的好处

N/A

## 17.2　Spring MVC的DispatcherServlet

`web.xml`
```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!-- map all requests to the DispatcherServlet -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

Spring MVC 的配置文件默认在 `WEB-INF` 目录下，命名规则为：`servletName-servlet.xml`，如果在其他目录，则需要在 `web.xml` 中指定所在位置。
```xml
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/config/simple-config.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>
```

## 17.3　Controller接口

N/A

## 17.4　第一个Spring MVC应用

`app17a/pom.xml`
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.app</groupId>
  <artifactId>app17a</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>app17a Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>2.3.1</version>
      <scope>provided</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.3.14.RELEASE</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>app17a</finalName>
  </build>
</project>
```

`app17a/src/main/webapp/WEB-INF/web.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
      org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.action</url-pattern>
  </servlet-mapping>

</web-app>
```

`app17a/src/main/webapp/WEB-INF/springmvc-servlet.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--路径映射-->
    <bean name="/product_input.action" class="app17a.controller.InputProductController"/>
    <bean name="/product_save.action" class="app17a.controller.SaveProductController"/>
</beans>
```

`app17a/src/main/java/app17a/domain/Product.java`
```java
package app17a.domain;
import java.io.Serializable;

public class Product implements Serializable {
    private static final long serialVersionUID = 748392348L;
    private String name;
    private String description;
    private float price;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getDescription() {
        return description;
    }
    public void setDescription(String description) {
        this.description = description;
    }
    public float getPrice() {
        return price;
    }
    public void setPrice(float price) {
        this.price = price;
    }
}
```

`app17a/src/main/java/app17a/form/ProductForm.java`
```java
package app17a.form;

public class ProductForm {
    private String name;
    private String description;
    private String price;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getDescription() {
        return description;
    }
    public void setDescription(String description) {
        this.description = description;
    }
    public String getPrice() {
        return price;
    }
    public void setPrice(String price) {
        this.price = price;
    }
}
```

`app17a/src/main/java/app17a/controller/InputProductController.java`
```java
package app17a.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class InputProductController implements Controller {

    private static final Log logger = LogFactory.getLog(InputProductController.class);

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        logger.info("InputProductController called");
        return new ModelAndView("/WEB-INF/jsp/ProductForm.jsp");
    }
}
```

`app17a/src/main/java/app17a/controller/SaveProductController.java`
```java
package app17a.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import app17a.domain.Product;
import app17a.form.ProductForm;

public class SaveProductController implements Controller {

    private static final Log logger = LogFactory.getLog(SaveProductController.class);

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {

        logger.info("SaveProductController called");

        ProductForm productForm = new ProductForm();
        // populate action properties
        productForm.setName(httpServletRequest.getParameter("name"));
        productForm.setDescription(httpServletRequest.getParameter("description"));
        productForm.setPrice(httpServletRequest.getParameter("price"));

        // create model
        Product product = new Product();
        product.setName(productForm.getName());
        product.setDescription(productForm.getDescription());
        try {
            product.setPrice(Float.parseFloat(productForm.getPrice()));
        } catch (NumberFormatException e) {
        }
        return new ModelAndView("/WEB-INF/jsp/ProductDetails.jsp", "product", product);
    }
}
```

`app17a/src/main/webapp/WEB-INF/jsp/ProductForm.jsp`
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Add Product Form</title>
    <style type="text/css">@import url(../../css/main.css);</style>
</head>
<body>
<div id="global">
    <form action="product_save.action" method="post">
        <fieldset>
            <legend>Add a product</legend>
            <p>
                <label for="name">Product Name: </label>
                <input type="text" id="name" name="name"
                       tabindex="1">
            </p>
            <p>
                <label for="description">Description: </label>
                <input type="text" id="description"
                       name="description" tabindex="2">
            </p>
            <p>
                <label for="price">Price: </label>
                <input type="text" id="price" name="price"
                       tabindex="3">
            </p>
            <p id="buttons">
                <input id="reset" type="reset" tabindex="4">
                <input id="submit" type="submit" tabindex="5"
                       value="Add Product">
            </p>
        </fieldset>
    </form>
</div>
</body>
</html>
```

`app17a/src/main/webapp/WEB-INF/jsp/ProductDetails.jsp`
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page isELIgnored="false" %>
<html>
<head>
    <title>Save Product</title>
    <style type="text/css">@import url(../../css/main.css);</style>
</head>
<body>
<div id="global">
    <h4>The product has been saved.</h4>
    <p>
    <h5>Details:</h5>
    Product Name: ${product.name}<br/>
    Description: ${product.description}<br/>
    Price: $${product.price}
    </p>
</div>
</body>
</html>
```

`app17a/src/main/webapp/css/main.css`
```css
#global {
    text-align: left;
    border: 1px solid #dedede;
    background: #efefef;
    width: 560px;
    padding: 20px;
    margin: 100px auto;
}

form {
    font:100% verdana;
    min-width: 500px;
    max-width: 600px;
    width: 560px;
}

form fieldset {
    border-color: #bdbebf;
    border-width: 3px;
    margin: 0;
}

legend {
    font-size: 1.3em;
}

form label {
    width: 250px;
    display: block;
    float: left;
    text-align: right;
    padding: 2px;
}

#buttons {
    text-align: right;
}
```

## 17.5　View Resolver

重构上一节的代码

`app17b/src/main/webapp/WEB-INF/web.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
      org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <!--Spring MVC 的配置文件放在其他目录时，需要在部署描述符文件指明。-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/config/springmvc-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.action</url-pattern>
  </servlet-mapping>

</web-app>
```

`app17b/src/main/webapp/WEB-INF/config/springmvc-servlet.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--路径映射-->
    <bean name="/product_input.action" class="app17b.controller.InputProductController"/>
    <bean name="/product_save.action" class="app17b.controller.SaveProductController"/>

    <!--简化 JSP 页面在 Java 代码中的加载，只需要使用文件名就行，路径和扩展名会自动加上去。-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

`app17b/src/main/java/app17b/controller/InputProductController.java`
```java
package app17b.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class InputProductController implements Controller {

    private static final Log logger = LogFactory.getLog(InputProductController.class);

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
        logger.info("InputProductController called");
        // 直接写 JSP 页面的文件名，路径和扩展名自动会加上去。
        return new ModelAndView("ProductForm");
    }
}
```

`app17b/src/main/java/app17b/controller/SaveProductController.java`
```java
package app17b.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import app17b.domain.Product;
import app17b.form.ProductForm;

public class SaveProductController implements Controller {

    private static final Log logger = LogFactory.getLog(SaveProductController.class);

    public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {

        logger.info("SaveProductController called");

        ProductForm productForm = new ProductForm();
        // populate action properties
        productForm.setName(httpServletRequest.getParameter("name"));
        productForm.setDescription(httpServletRequest.getParameter("description"));
        productForm.setPrice(httpServletRequest.getParameter("price"));

        // create model
        Product product = new Product();
        product.setName(productForm.getName());
        product.setDescription(productForm.getDescription());
        try {
            product.setPrice(Float.parseFloat(productForm.getPrice()));
        } catch (NumberFormatException e) {
        }
        // 直接写 JSP 页面的文件名，路径和扩展名自动会加上去。
        return new ModelAndView("ProductDetails", "product", product);
    }
}
```

学习结束