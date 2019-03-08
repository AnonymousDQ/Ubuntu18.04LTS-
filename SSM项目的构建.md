# SSM项目的构建

## 1、创建一个maven工程 

![](images/maven_project.png)

eclipse创建maven工程后，pom.xml报错：web.xml is missing and <failOnMissingWebXml> is set to true

解决问题：右击项目-->java EE tools-->Generate  Deployment Descriptor Stub。

系统会自动创建一个web.xml文件放到src/main/webapp/WEB_INF下。

注：xml文件格式化快捷键，Ctrl+Shift+F,为了避免和搜狗拼音的简体繁体冲突，可以勾掉搜狗的快捷键。



## 2、引入项目依赖的jar包

- spring
- springmvc
- mybatis
- 数据库连接池，驱动包
- 其他

#### 1、引入Spring相关包的操作

##### 1、[maven中央仓库](https://mvnrepository.com/)

##### 2、引入springmvc的包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>4.3.7.RELEASE</version>
</dependency>
```

##### 3、引入Spring-jdbc的包

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>4.3.7.RELEASE</version>
</dependency>
```

##### 4、引入Spring面向切面编程模块Spring aspects

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>4.3.7.RELEASE</version>
</dependency>
```

#### 2、引入MyBatis相关包的操作

##### 1、引入mybatis的包

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.2</version>
</dependency>
```

##### 2、引入mybatis整合Spring的适配包

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.1</version>
</dependency>
```

#### 3、引入数据库连接池以及驱动

##### 1、引入c3p0数据库连接池的包

```xml
<!-- https://mvnrepository.com/artifact/c3p0/c3p0 -->
<dependency>
    <groupId>c3p0</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.1.2</version>
</dependency>

```

##### 2、引入mysql8.0.15的驱动包

查看本地mysql的版本，引入相应的驱动包

首先进入mysql

```shell
mysql -uroot -proot
status;
```

```xml
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
</dependency>
```

#### 4、其他模块

由于要开发web项目，先把常用的，jstl,servlet-api,junit先引入进来

##### 1、引入jstl包

```xml
<!-- https://mvnrepository.com/artifact/jstl/jstl -->
<dependency>
    <groupId>jstl</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```

##### 2、引入servlet-api包

```xml
<!-- https://mvnrepository.com/artifact/javax.servlet/servlet-api -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
```

我们知道tomcat服务器里面有servlet包，但是不引入的话jsp页面会报错，所以加入<scope>provided</scope>标签表示人家已经提供的。也即是当项目发布到服务器上的时候，这个包会被自动剔除掉。

##### 3、引入junit单元测试包

```xml
<!-- https://mvnrepository.com/artifact/junit/junit -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.2</version>
    <scope>test</scope>
</dependency>
```

## 3、引入BootStrap前端框架

[bootstrap中文官网](https://github.com/twbs/bootstrap/releases/download/v3.3.7/bootstrap-3.3.7-dist.zip)

#### 1、下载bootstrap然后解压。

#### 2、在webapp目录下建立一个folder命名为叫static文件夹

![](../../桌面/ssm项目构建/images/webapp下建立static文件夹.png)

#### 3、把解压的bootstrap-3.3.7-dist复制到新建的static目录下

#### 4、新建一个index.jsp引入bootstrap样式和基本文件

[bootstrap官网](https://www.bilibili.com/video/av35988777/?p=5)

```jsp
<link href="static/bootstrap-3.3.7-dist/css/bootstrap.min.css" rel="stylesheet">
<script src="static/bootstrap-3.3.7-dist/js/bootstrap.min.js"></script>
```

## 4、编写ssm整合的关键配置文件

#### 1、web.xml配置文件的编写

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	id="WebApp_ID"
	version="2.5">
	<display-name>ssm-crud</display-name>

	<!-- 1、启动spring的容器(快捷键：alt+/) -->
	<!-- needed for ContextLoaderListener -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>

	<!-- Bootstraps the root web application context before servlet initialization -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- 2、springmvc的前端控制器，拦截所有请求 -->
	<!-- The front controller of this Spring Web application, responsible for 
		handling all application requests -->
	<servlet>
		<servlet-name>dispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 可以不指定配置文件路径，但是必须要在与web.xml同级目录下 有一个springmvc配置文件，与<servlet-name>标签属性名-servlet.xml的配置文件 
			eg:dispatcherServlet-servlet.xml <init-param> <param-name>contextConfigLocation</param-name> 
			<param-value>location</param-value> </init-param> -->
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map all requests to the DispatcherServlet for handling -->
	<servlet-mapping>
		<servlet-name>dispatcherServlet</servlet-name>
		<!-- 让它拦截所有请求 -->
		<url-pattern>/</url-pattern>
	</servlet-mapping>

	<!-- 3、配上springmvc中带的字符编码过滤器 Ctrl+Shift+T，
	输入CharacterEncodingFilter,打开对应的class文件。 
	当过滤器有多个，这个肯定有先后顺序的。
	******注意：一定要把字符编码过滤器放在所有过滤器之前。******
	-->
	<filter>
		<filter-name>CharacterEncodingFilter</filter-name>
		<!-- 右键选择，copy qualified name复制全类名 -->
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<!-- 在初始化参数的时候，给它指定有一个属性，这个属性在CharacterEncodingFilter 叫encoding的属性，指定我们要用的字符编码集用utf-8 -->
		<init-param>
			<param-name>encoding</param-name>
			<param-value>utf-8</param-value>
		</init-param>

		<!-- springmvc4中的CharacterEncodingFilter， 多了两个属性：forceRequestEncoding，forceResponseEncoding -->
		<!-- 设置这下面两个属性都为true，让它强制请求和响应编码为utf-8 -->
		<init-param>
			<param-name>forceRequestEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
		<init-param>
			<param-name>forceResponseEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CharacterEncodingFilter</filter-name>
		<!-- 过滤所有请求 -->
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
	<!-- 4、使用Rest风格的URI，而提交页面的请求，
	是发不出PUT，DELETE这样的请求的。所以还需要一个过滤器HiddenHttpMethodFilter
	它会将页面的post请求转为指定的delete或者put请求。
	-->
	<filter>
		<filter-name>HiddenHttpMethodFilter</filter-name>
		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>HiddenHttpMethodFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
</web-app>
```

#### 2、springmvc配置文件的编写

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

	<!--SpringMVC的配置文件，包含网站跳转逻辑的控制配置 -->
	
	<!-- 1、配置包扫描，让它只扫描@Controller注解的 -->
	<context:component-scan base-package="com.victor" use-default-filters="false">
		<!-- 让它只扫描控制器 -->
		<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
	
	<!-- 2、配置视图解析器，方便页面返回解析 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	
	<!-- 两个标准配置  -->
	<!-- 3、将springmvc不能处理的请求交给tomcat 
		这样实现了动态，静态资源都能访问成功了
     -->
	<mvc:default-servlet-handler/>
	<!-- 4、能支持springmvc更高级的一些功能，比如JSR303教研，
	快捷的ajax，更重要的是来映射动态请求-->
	<mvc:annotation-driven></mvc:annotation-driven>
</beans>
```

#### 3、spring的配置文件

##### 1、applicationContext.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">
	
	<!-- 1、在spring的配置文件中，让Spring不扫控制器。 -->
	<context:component-scan base-package="com.victor">
		<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
	</context:component-scan>
	<!-- spring的配置文件：这里主要配置和业务逻辑有关的 /ssm-crud/src/main/resources/dbconfig.properties-->
	<!-- 数据源，事务控制 ，xxx-->
	
	<!-- ================================一、数据源配置============================================== -->	
	<!--2、context:property-placeholder用来引入外部配置文件 -->
	<context:property-placeholder location="dbconfig.properties"/>
	
	<!--3、配置c3p0数据源(用alt+/快捷键进行补全) -->
	<bean id="pooledDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<!--这些数据库设置，我们都不是写死的，
		因此创建一个dbconfig.properties文件
		为了不跟其他配置文件混乱，跟数据源有关的都加上jdbc.的前缀，
		 -->
		<property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
		<property name="driverClass" value="${jdbc.driverClass}"></property>
		<property name="user" value="${jdbc.user}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>
	<!-- ========================================================================================== -->
	
	
	<!-- =========================二、MyBatis整合配置====================================================== -->
	<!-- 4、配置mybatis的整合 -->
	<!-- 可以帮我们创建sqlSessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 指定mybatis全局配置文件的位置-->
		<property name="configLocation" value="mybatis-config.xml"></property>
		<!-- 指定mybatis的数据源引用文件 -->
		<property name="dataSource" ref="pooledDataSource"></property>
		<!-- 指定mybatis的mapper文件的位置 -->
		<property name="mapperLocations" value="classpath:mapper/*.xml"></property>
	</bean>
	
	<!-- 5、配置扫描器，将mybatis接口的实现加入到ioc容器中
	因为我们知道mybatis的接口的实现是一个代理对象，需要加到ioc容器中
	 -->
	 <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	 	<!-- 扫描所有的dao接口的实现，加入到ioc容器中-->
	 	<property name="basePackage" value="com.victor.crud.dao"></property>
	 </bean>
	 <!-- ========================================================================================== -->
	 
	 
	 <!-- ===============================三、事务控制的配置（非常重要）======================================================= -->	 
	 <!-- 6、事务控制的配置 -->
	 <!-- 数据源的开启关闭回滚操作，我们用事务管理器来做 -->
	 <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	 	<!-- 控制住数据源 -->
	 	<!-- 告诉项目中用的数据源是哪个 -->
	 	<property name="dataSource" ref="pooledDataSource"></property>
	 </bean>
	 
	 <!-- 7、开启基于注解的事务 ，或者使用xml配置形式的事务
	 一般我们推荐：比较重要的事务都是使用配置xml文件的形式。
	 -->
	 <aop:config>
	 	<!-- 切入点表达式（也就是想要切入到哪些里面进行事务控制）
	 	首先是返回值，返回值类型：*为所有
	 	com.victor.crud.service..*(..):表示service包下的所有类，所有方法都来控制事务,括号里面的双点：表示这个方法里的参数任意多也行
	 	..：表示即使这个包下还有子包也行
	 	 -->
	 	<aop:pointcut expression="execution(* com.victor.crud.service..*(..))" id="txPoint"/>
	 	<!-- 配置事务增强(需要引入tx名称空间) -->
	 	<aop:advisor advice-ref="txAdvice" pointcut-ref="txPoint"/>
	 </aop:config>
	 
	 <!--8、配置事务增强(也就是事务如何切入) -->
	 <!-- 关键是跟事务管理器又产生什么联系的呢？
	 	其实是我们事务增强配置的时候有一个属性transaction-manager，
	 	它的值默认取值叫transactionManager，而我们的事务管理器的id正好是transactionManager
	 	如果事务管理器的id改了，也一定要把id复制粘贴到事务增强处。
	 	意思就是我们用这个事务管理器来控制事务，控制事务的细节，切哪些方法在<aop:point>标签配置处指定。
	 	哪些方法切入以后该怎么办，在<tx:advice>标签配置处指定。
	  -->
	 <tx:advice id="txAdvice" transaction-manager="transactionManager">
	 	<tx:attributes>
	 		<!-- *表示这个切入点切入的所有方法，都是事务方法 -->
	 		<tx:method name="*"/>
	 		<!-- 表示以get开始的所有方法(我们可以认为以get开始的方法都是查询，设置一个属性read-only=true,来进行优化)-->
	 		<tx:method name="get*" read-only="true"/>
	 	</tx:attributes>
	 </tx:advice>
	 <!-- ========================================================================================== -->
	 
	 
	 <!--Spring配置文件的核心点：
	 	数据源，与mybatis的整合，事务控制
	   -->
</beans>
```

##### 2、dbconfig.properties文件的写法

由于mysql8，driverClass=com.mysql.jdbc.Driver会报错

错误解决：Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdb

mysql其他版本:driverClass=com.mysql.jdbc.Driver

mysql8以上版本:driverClass=com.mysql.cj.jdbc.Driver

```properties
jdbc.jdbcUrl=jdbc:mysql://localhost:3306/java?useSSL=false
jdbc.driverClass=com.mysql.cj.jdbc.Driver
jdbc.user=victor
jdbc.password=root
```

#### 4、mybatis的配置文件

##### 1、mybatis-cfg.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 开启驼峰命名规则 -->
	<settings>
		<setting name="mapUnderscoreToCamelCase" value="true"/>
	</settings>
	<!-- 类型别名的配置 -->
	<typeAliases>
		<package name="com.victor.crud.bean"/>
	</typeAliases>
</configuration>
```

##### 2、利用MyBatis Generator逆向工程

[MyBatis Generator的官网](http://www.mybatis.org/generator/configreference/xmlconfig.html)

- 引入mybatis generator的依赖

  ```xml
  <!-- https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core -->
  <dependency>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-core</artifactId>
      <version>1.3.5</version>
  </dependency>
  ```

- 给当前工程中创建一个mbg.xml文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE generatorConfiguration
    PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
  
  <generatorConfiguration>
  
    <context id="DB2Tables" targetRuntime="MyBatis3">
      <!-- 配置数据库连接 -->
      <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
          connectionURL="jdbc:mysql://localhost:3306/java?useSSL=false"
          userId="victor"
          password="root">
      </jdbcConnection>
      
  	<!-- Java的大数据类型解析 -->
      <javaTypeResolver >
        <property name="forceBigDecimals" value="false" />
      </javaTypeResolver>
  	
  	<!-- 指定javaBean生成的位置
  	windows系统的写法：targetProject=".\src\main\java":表示当前工程下的main的java工程下。
  macOS、linux系统的写法：targetProject="./src/main/java"
  	 -->
      <javaModelGenerator targetPackage="com.victor.crud.bean" targetProject="./src/main/java">
        <property name="enableSubPackages" value="true" />
        <property name="trimStrings" value="true" />
      </javaModelGenerator>
  
  	<!-- 指定sql映射文件生成的位置 -->
      <sqlMapGenerator targetPackage="mapper"  targetProject="./src/main/resources">
        <property name="enableSubPackages" value="true" />
      </sqlMapGenerator>
  
  	<!-- 指定dao接口生成的位置，mapper接口 -->
      <javaClientGenerator type="XMLMAPPER" targetPackage="com.victor.crud.dao"  targetProject="./src/main/java">
        <property name="enableSubPackages" value="true" />
      </javaClientGenerator>
  
  	<!-- table标签指定每个表的生成策略 
  	tableName="tbl_emp":指定表名，必须的属性。
  	domainObjectName="Employee"：指定生成的javabean的类名
  	-->
      <table tableName="tbl_emp" domainObjectName="Employee"></table>
      <table tableName="tbl_dept" domainObjectName="Department"></table>
  
    </context>
  </generatorConfiguration>
  ```

- 如何生成呢

  因为我们是java程序+xml配置文件的生成方式

  [生成逆向工程](http://www.mybatis.org/generator/running/runningWithJava.html)

  ![](../../桌面/ssm项目构建/images/建立一个test类.png)

- 运行这个测试类main方法生成就行。

```java
package com.victor.crud.test;

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.exception.XMLParserException;
import org.mybatis.generator.internal.DefaultShellCallback;

public class MBGTest {

	public static void main(String[] args) throws IOException, XMLParserException, Exception {
		// TODO Auto-generated method stub
		List<String> warnings = new ArrayList<String>();
		boolean overwrite = true;
		//指定当前项目下的mybatis-generator的配置文件
		File configFile = new File("mbg.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
		myBatisGenerator.generate(null);
		System.out.println("mybatis-generator successfully!");

	}

}
```

