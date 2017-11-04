# 简介
**最新使用Maven搭建springMVC+spring+mybatis（SSM）的详细过程Demo。**<br>
#### 另附上本人基于SSM搭建的高效便捷开发框架，详情参考[https://github.com/micyo202](https://github.com/micyo202/yan_demo)

# 环境
* MacOS Sierra / Windows 7
* MySql 5.7
* JDK 1.8
* Eclipse 4.6.1
* Maven 3.3.9
* Jetty 9.4.6.v20170531 / Tomcat 9.0

# 技术选型
名称 | 描述 | 版本号 | 网址
--- | --- | --- | ---
Spring MVC| MVC框架 | 4.3.11.RELEASE  | [https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc)
Spring Framework | 容器 | 4.3.10.RELEASE  | [http://projects.spring.io/spring-framework/](http://projects.spring.io/spring-framework/)
MyBatis| ORM/持久层框架 | 3.4.5 | [http://www.mybatis.org/mybatis-3/zh/index.html](http://www.mybatis.org/mybatis-3/zh/index.html)
AspectJ| 面向切面的框架 | 1.8.10 | [http://www.eclipse.org/aspectj/](http://www.eclipse.org/aspectj/)
Druid| 数据库连接池 | 1.1.3 | [https://github.com/alibaba/druid](https://github.com/alibaba/druid)
Jackson| json解析器 | 2.9.1 | [https://github.com/FasterXML/jackson](https://github.com/FasterXML/jackson)
Logback| 日志组件 | 1.2.3 | [https://logback.qos.ch](https://logback.qos.ch)
Maven| 项目构建管理 | 3.3.9 | [http://maven.apache.org/](http://maven.apache.org/)

# 搭建步骤
## 一、创建Maven项目
##### 具体操作如下图<br>
![1.png](http://upload-images.jianshu.io/upload_images/8015461-c17ba12874caec50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![2.png](http://upload-images.jianshu.io/upload_images/8015461-63b386013cb776ea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![3.png](http://upload-images.jianshu.io/upload_images/8015461-58c0e09a3326a455.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 新建好的项目会有报错提示，别着急，在项目上右键选择 Java EE Tools > Generate Deployment Descriptor Stub
![4.png](http://upload-images.jianshu.io/upload_images/8015461-96b62cc81ecc329d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 接下来在项目上右键选择最后一项 **Properties > Project Facets**
![5.png](http://upload-images.jianshu.io/upload_images/8015461-2eb6c1851eb7e0e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 到这里一个Maven的Web项目就创建好了

## 二、在pom.xml中添加依赖包
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.frame</groupId>
	<artifactId>ssm</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<spring.framework.version>4.3.11.RELEASE</spring.framework.version>
		<aspectj.version>1.8.10</aspectj.version>
		<jackson.version>2.9.1</jackson.version>
		<logback.version>1.2.3</logback.version>
	</properties>

	<dependencies>
		<!-- 添加 commons 依赖包 -->
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.3.3</version>
		</dependency>
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.5</version>
		</dependency>
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>3.6</version>
		</dependency>

		<!-- 添加 spring 依赖包 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>${spring.framework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.framework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.framework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.framework.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${spring.framework.version}</version>
		</dependency>

		<!-- 添加 aspectJ 依赖包 -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>${aspectj.version}</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${aspectj.version}</version>
		</dependency>

		<!-- 添加 mybatis 依赖包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.5</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.1</version>
		</dependency>

		<!-- 添加 druid 依赖包 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.1.3</version>
		</dependency>

		<!-- 添加 mysql 驱动 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.44</version>
		</dependency>

		<!-- 添加 jackson 依赖包 -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>${jackson.version}</version>
		</dependency>

		<!-- 添加 servlet 依赖包 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
		</dependency>

		<!-- 添加 jstl 依赖包 -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- 添加 logback 依赖包 -->
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>${logback.version}</version>
		</dependency>

	</dependencies>

</project>
```
## 三、配置web.xml文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	id="WebApp_ID" version="3.1">
	<display-name>ssm</display-name>
	
	<!-- 过滤器解决中文乱码问题、强制编码 UTF-8 -->
	<filter>
		<filter-name>characterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceRequestEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>characterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

	<!-- 配置 springMVC 基本配置 -->
	<servlet>
		<servlet-name>springMVC</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>springMVC</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
	<!-- 配置 spring 监听器 -->
	<!-- needed for ContextLoaderListener -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
	<!-- Bootstraps the root web application context before servlet initialization -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>

	<!-- 配置欢迎页 -->
	<welcome-file-list>
		<welcome-file>views/index.jsp</welcome-file>
	</welcome-file-list>

</web-app>
```
## 四、在src/main/resources下创建spring配置文件
##### 创建applicationContext.xml配置如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">

	<!-- 自动扫描指定目录，将控制器加载到bean -->
	<context:component-scan base-package="com.frame" />

	<!-- 配置 druid 数据源 -->
	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
		<!-- 基本属性 url、user、password、driverClass -->
		<property name="username" value="root" />
		<property name="password" value="root" />
		<property name="driverClassName" value="com.mysql.jdbc.Driver" />
		<property name="url" value="jdbc:mysql://192.168.0.200:3306/test?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false" />
	</bean>
	
		<!-- 配置 sessionFactory -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="mapperLocations">
			<list>
				<value>classpath:com/frame/**/*Mapper.xml</value>
				<value>classpath:mybatis/**/*Mapper.xml</value>
			</list>
		</property>
	</bean>
	
	<!-- 配置 sqlSessionTemplate 持久化模板 -->
	<bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg ref="sqlSessionFactory" />
	</bean>

	<!-- 配置事物管理器 -->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource" />
	</bean>

	<!-- 配置事物传播行为 -->
	<tx:advice id="txAdvice" transaction-manager="transactionManager">
		<tx:attributes>
			<tx:method name="get*" read-only="true" />
			<tx:method name="find*" read-only="true" />
			<tx:method name="select*" read-only="true" />
			<tx:method name="*" propagation="REQUIRED" />
		</tx:attributes>
	</tx:advice>

	<!-- 配置事物切入点 -->
	<aop:config>
		<aop:pointcut expression="execution(* com.frame.service.*.*(..))" id="pointCut" />
		<aop:advisor advice-ref="txAdvice" pointcut-ref="pointCut" />
	</aop:config>

	<!-- 开始声明式事务（事物注解） -->
	<tx:annotation-driven transaction-manager="transactionManager" />
	
</beans>
```
##### 创建spring-mvc.xml配置如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">
		
	<!-- 自动将控制器加载到bean -->
	<context:component-scan base-package="com.frame.controller" />
	
	<!-- 配置视图解析器 -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/views/" />
		<property name="suffix" value=".jsp" />
		<!-- 可为空,方便实现自已的依据扩展名来选择视图解释类的逻辑 -->
		<property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
	</bean>

	<!-- 返回json 需导入 jackson-annotations.jar，jackson-core.jar，jackson-databind.jar -->
	<!-- 通过处理器映射DefaultAnnotationHandlerMapping来开启支持@Controller注解 -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping" />
	<!-- 通过处理器适配器AnnotationMethodHandlerAdapter来开启支持@RequestMapping注解 -->
	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
		<property name="messageConverters">
			<list>
				<!-- 配置返回字符串编码 -->
				<bean
					class="org.springframework.http.converter.StringHttpMessageConverter">
					<property name="supportedMediaTypes">
						<list>
							<value>text/html; charset=UTF-8</value>
							<value>application/json;charset=UTF-8</value>
						</list>
					</property>
				</bean>
				<!-- 配置 json 转换器 -->
				<bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
					<property name="supportedMediaTypes">
						<list>
							<value>text/html; charset=UTF-8</value>
							<value>application/json;charset=UTF-8</value>
						</list>
					</property>
				</bean>
			</list>
		</property>
	</bean>
	
</beans>
```
## 五、在src/main/resources下创建mybatis/\*\*/\*Mapper.xml模板
##### 用于撰写mybatis执行sql的语句
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.frame.mapper.DemoMapper">
	
	<select id="getTest" resultType="java.util.Map">
		SELECT * FROM TB_TEST
	</select>
	
</mapper>
```
## 五、在src/main/java下创建com.frame.\*测试类
##### 创建service接口
```java
package com.frame.service;

public interface DemoService {
	void test();
}
```
##### 创建service业务层实现类
```java
package com.frame.service.impl;

import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.frame.service.DemoService;

@Service("demoService")
public class DemoServiceImpl implements DemoService {

	@Autowired
	private SqlSessionTemplate sqlSessionTemplate;

	// mybatis sql模板的命名空间
	private static final String NAMESPACE = "com.frame.mapper.DemoMapper";

	@Override
	public void test() {
		System.out.println("返回查询结果集 -> " + sqlSessionTemplate.selectList(NAMESPACE + ".getTest")); // 查询结果集
	}
}
```
##### 创建controller控制器
```java
package com.frame.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import com.frame.service.DemoService;

@Controller
public class DemoController {

	@Autowired
	private DemoService demoService;

	@RequestMapping("/hello")
	public String hello() {
		System.out.println("执行hello控制器方法");
		// 调用业务层执行查询操作
		demoService.test();
		return "hello";
	}
}
```
## 六、在webapp下创建views/\*.jsp页面
##### 创建index.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	This is index page.
</body>
</html>
```
##### 创建hello.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Hello World!</h1>
</body>
</html>
```
## 七、部署项目并启动服务（Jetty/Tomcat）
![6.png](http://upload-images.jianshu.io/upload_images/8015461-b0e716c6d3ca6449.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 服务启动成功后输入地址[http://localhost:8888/ssm/hello](http://localhost:8888/ssm/hello)进行测试，页面将展示**Hello World！**字样，控制台(console)将输出如下信息
```console
执行hello控制器方法
返回查询结果集 -> [{ID=1ba6d11d2639401ebf63c00c5ae7c2a0, NAME=SSM, TYPE=FRAME}]
```
##### 到此整个框架最基本的搭建就完成了，接下来即可进入开发阶段，如果感觉不错请分享出去。
#### 这里推荐本人自己基于SSM搭建的高效便捷开发框架，上手就能用，详情参考[https://github.com/micyo202](https://github.com/micyo202/yan_demo)
