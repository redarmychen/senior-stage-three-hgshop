# dubbo 与ssm 整的过程

## 1、创建一个父工程 贺三个子工程

   父工程 pom

  接口工程  =》jar

  服务提供者=》 jar

  服务消费者=》war 

## 2、引入依赖

### 处理父工程的依赖

```xml
   <!-- 定义主要版本号 -->
<properties>
	<spring.version>5.1.5.RELEASE</spring.version>
	<mybatis.version>3.4.2</mybatis.version>
	<mybatis.spring.version>1.3.0</mybatis.spring.version>
	<pagehelper-version>5.1.2</pagehelper-version>
	<druid.version>1.0.9</druid.version>
	<mysql.version>5.1.6</mysql.version>
	<aspectj-version>1.8.0</aspectj-version>
	
	<dubbo.version>2.7.3</dubbo.version>
	
	<log4j.version>1.2.17</log4j.version>
	<jackson.version>2.9.1</jackson.version>
	<jstl.version>1.2</jstl.version>
	<servlet-api.version>2.5</servlet-api.version>
	<jsp-api.version>2.2</jsp-api.version>
	
	<commons-lang3.version>3.3.2</commons-lang3.version>
	<commons-io.version>1.3.1</commons-io.version>
	<commons-net.version>3.3</commons-net.version>
	<commons-fileupload.version>1.3.1</commons-fileupload.version>
	
	<junit-version>4.12</junit-version>
	
	<ik.version>2012_u6</ik.version>
	<lucene.version>7.2.1</lucene.version>
	<spring-data-elasticsearch.version>3.1.5.RELEASE</spring-data-elasticsearch.version>
	<!-- <kafka.version>0.8.2.1</kafka.version> -->
	<kafka.version>2.1.0</kafka.version>
	<spring-kafka.version>2.2.4.RELEASE</spring-kafka.version>
	<jedis.version>2.9.0</jedis.version>
	<spring-data-redis.version>2.1.5.RELEASE</spring-data-redis.version>
</properties>

<dependencyManagement>

	<dependencies>
		<!-- 引入dubbo的依赖配置 -->
		<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo</artifactId>
			<version>${dubbo.version}</version>
			<exclusions>
				<exclusion>
					<artifactId>spring</artifactId>
					<groupId>org.springframework</groupId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.apache.dubbo</groupId>
			<artifactId>dubbo-dependencies-zookeeper</artifactId>
			<version>${dubbo.version}</version>
			<type>pom</type>
		</dependency>
		<!-- spring-webmvc依赖 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<!-- spring-jdbc依赖 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<!-- spring-tx 事务依赖 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<!-- 引入spring-test依赖 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<!-- mybatis核心包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>
		<!-- mybatis-spring 整合jar -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>${mybatis.spring.version}</version>
		</dependency>
		<!-- druid数据源 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>${druid.version}</version>
		</dependency>
		<!-- Mysql数据库驱动包 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${mysql.version}</version>
		</dependency>
		<!-- 日志文件管理包 -->
		<!-- log start -->
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>${log4j.version}</version>
		</dependency>
		<!-- 单元测试 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit-version}</version>
			<scope>test</scope>
		</dependency>
		<!-- 上传组件包 -->
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>${commons-fileupload.version}</version>
		</dependency>
		<!-- common-io依赖 -->
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>${commons-io.version}</version>
		</dependency>
		<!-- 引入jstl依赖 -->
		<dependency>
			<groupId>jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>${jstl.version}</version>
		</dependency>
		<!-- 引入jsp-api依赖 -->
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>${jsp-api.version}</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>${servlet-api.version}</version>
			<scope>provided</scope>
		</dependency>

		<!-- 引入jackson的依赖 -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>${jackson.version}</version>
		</dependency>

		<!-- 依赖的公共包 -->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-lang3</artifactId>
			<version>${commons-lang3.version}</version>
		</dependency>

		<!-- 引入org.aspectj依赖 -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>${aspectj-version}</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${aspectj-version}</version>
		</dependency>
		<!-- 引入mybaits pagehelper分页助手依赖 -->
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper</artifactId>
			<version>${pagehelper-version}</version>
		</dependency>
		
		<dependency>
			<groupId>org.apache.lucene</groupId>
			<artifactId>lucene-core</artifactId>
			<version>${lucene.version}</version>
		</dependency>
		<!-- 加入ik分词器 -->
		<dependency>
			<groupId>com.janeluo</groupId>
			<artifactId>ikanalyzer</artifactId>
			<version>${ik.version}</version>
			<!-- 排除自带的lucene操作 -->
			<exclusions>
				<exclusion>
					<groupId>org.apache.lucene</groupId>
					<artifactId>lucene-queries</artifactId>
				</exclusion>
				<exclusion>
					<groupId>org.apache.lucene</groupId>
					<artifactId>lucene-queryparser</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-elasticsearch</artifactId>
			<version>${spring-data-elasticsearch.version}</version>
		</dependency>
		
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>${jedis.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-redis</artifactId>
			<version>${spring-data-redis.version}</version>
		</dependency>
		
		<dependency>
			<groupId>org.apache.kafka</groupId>
			<artifactId>kafka_2.12</artifactId>
			<version>${kafka.version}</version>
			<exclusions>
				<exclusion>
					<artifactId>jmxri</artifactId>
					<groupId>com.sun.jmx</groupId>
				</exclusion>
				<exclusion>
					<artifactId>jms</artifactId>
					<groupId>javax.jms</groupId>
				</exclusion>
				<exclusion>
					<artifactId>jmxtools</artifactId>
					<groupId>com.sun.jdmk</groupId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework.kafka</groupId>
			<artifactId>spring-kafka</artifactId>
			<version>${spring-kafka.version}</version>
		</dependency>
		
	</dependencies>
</dependencyManagement>

<build>
	<pluginManagement>
		<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
		</plugin>
		
		<plugin>
			<groupId>org.eclipse.jetty</groupId>
			<artifactId>jetty-maven-plugin</artifactId>
			<version>9.4.20.v20190813</version>
			<configuration>
				<httpConnector>
					<!-- 端口号 -->
					<port>9093</port>
					<!-- 项目访问路径 -->
					<host>192.168.1.105</host>
				</httpConnector>
				<contextHandlers>
					<jettyWebAppContext>
						<!-- 虚拟路径 -->
						<contextPath>/pic</contextPath>
						<!-- 物理路径 -->
						<resourceBase>d:/pic/</resourceBase>
					</jettyWebAppContext>
				</contextHandlers>
				<scanIntervalSeconds>1</scanIntervalSeconds>
			</configuration>
		</plugin>
		
		</plugins>
	</pluginManagement>
	
</build>
<!-- 结束拷贝的位置 -->
```
### 接口的依赖

  <!-- 开始拷贝的位置 -->
  <dependencies>
		<dependency>
			<groupId>com.github.pagehelper</groupId>
			<artifactId>pagehelper</artifactId>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
		</dependency>
		

```
	<dependency>
		<groupId>org.apache.lucene</groupId>
		<artifactId>lucene-core</artifactId>
	</dependency>

	<!-- 加入ik分词器 -->
	<dependency>
		<groupId>com.janeluo</groupId>
		<artifactId>ikanalyzer</artifactId>
		<!-- 排除自带的lucene操作 -->
		<exclusions>
			<exclusion>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-queries</artifactId>
			</exclusion>
			<exclusion>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-queryparser</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.springframework.data</groupId>
		<artifactId>spring-data-elasticsearch</artifactId>
	</dependency>
```
  </dependencies>
  <!-- <技术拷贝的位置> -->

### 服务提供者中添加依赖

#### 添加通用的依赖

<!-- https://mvnrepository.com/artifact/commons-beanutils/commons-beanutils -->
		<dependency>
		    <groupId>commons-beanutils</groupId>
		    <artifactId>commons-beanutils</artifactId>
		    <version>1.9.4</version>
		</dependency>
				
```xml
	<!-- 引入dubbo的依赖配置 -->
	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo</artifactId>
		<exclusions>
			<exclusion>
				<artifactId>spring-context</artifactId>
				<groupId>org.springframework</groupId>
			</exclusion>
			<exclusion>
				<artifactId>netty-all</artifactId>
				<groupId>io.netty</groupId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo-dependencies-zookeeper</artifactId>
		<type>pom</type>
	</dependency>
	<!-- spring-webmvc依赖 -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<exclusions>
			<exclusion>
				<artifactId>spring-context</artifactId>
				<groupId>org.springframework</groupId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
	</dependency>
	<!-- spring-jdbc依赖 -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-jdbc</artifactId>
	</dependency>
	<!-- spring-tx 事务依赖 -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-tx</artifactId>
	</dependency>
	<!-- 引入spring-test依赖 -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-test</artifactId>
	</dependency>

	<!-- mybatis核心包 -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis</artifactId>
	</dependency>
	<!-- mybatis-spring 整合jar -->
	<dependency>
		<groupId>org.mybatis</groupId>
		<artifactId>mybatis-spring</artifactId>
	</dependency>
	<!-- druid数据源 -->
	<dependency>
		<groupId>com.alibaba</groupId>
		<artifactId>druid</artifactId>
	</dependency>
	<!-- Mysql数据库驱动包 -->
	<dependency>
		<groupId>mysql</groupId>
		<artifactId>mysql-connector-java</artifactId>
	</dependency>
	<!-- 日志文件管理包 -->
	<!-- log start -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
	</dependency>
	<!-- 单元测试 -->
	<dependency>
		<groupId>junit</groupId>
		<artifactId>junit</artifactId>
		<scope>test</scope>
	</dependency>
	<!-- 依赖的公共包 -->
	<dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-lang3</artifactId>
	</dependency>

	<!-- 引入org.aspectj依赖 -->
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjweaver</artifactId>
	</dependency>
	<dependency>
		<groupId>org.aspectj</groupId>
		<artifactId>aspectjrt</artifactId>
	</dependency>
	<!-- 引入mybaits pagehelper分页助手依赖 -->
	<dependency>
		<groupId>com.github.pagehelper</groupId>
		<artifactId>pagehelper</artifactId>
	</dependency>

	<dependency>
		<groupId>org.apache.lucene</groupId>
		<artifactId>lucene-core</artifactId>
	</dependency>

	<!-- 加入ik分词器 -->
	<dependency>
		<groupId>com.janeluo</groupId>
		<artifactId>ikanalyzer</artifactId>
		<!-- 排除自带的lucene操作 -->
		<exclusions>
			<exclusion>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-queries</artifactId>
			</exclusion>
			<exclusion>
				<groupId>org.apache.lucene</groupId>
				<artifactId>lucene-queryparser</artifactId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.springframework.data</groupId>
		<artifactId>spring-data-elasticsearch</artifactId>
	</dependency>

	<dependency>
		<groupId>redis.clients</groupId>
		<artifactId>jedis</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.data</groupId>
		<artifactId>spring-data-redis</artifactId>
	</dependency>

	<dependency>
		<groupId>org.apache.kafka</groupId>
		<artifactId>kafka_2.12</artifactId>
		<exclusions>
			<exclusion>
				<artifactId>jmxri</artifactId>
				<groupId>com.sun.jmx</groupId>
			</exclusion>
			<exclusion>
				<artifactId>jms</artifactId>
				<groupId>javax.jms</groupId>
			</exclusion>
			<exclusion>
				<artifactId>jmxtools</artifactId>
				<groupId>com.sun.jdmk</groupId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.springframework.kafka</groupId>
		<artifactId>spring-kafka</artifactId>
		<!-- <version>1.0.0.RC1</version> -->
	</dependency>
```
#### 添加本项目中的接口的依赖

![1587610442695](D:\八维教学\senior3\教案\senior-stage-three-hgshop\第六单元 Dubbo理论技能扩展\dubbo 与ssm 整的过程.assets\1587610442695.png) 



### 处理服务消费者的依赖

#### 	添加开通用的依赖

```xml
<!-- 引入dubbo的依赖配置 -->
	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo</artifactId>
		<exclusions>
			<exclusion>
				<artifactId>spring-context</artifactId>
				<groupId>org.springframework</groupId>
			</exclusion>
			<exclusion>
				<artifactId>netty-all</artifactId>
				<groupId>io.netty</groupId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.apache.dubbo</groupId>
		<artifactId>dubbo-dependencies-zookeeper</artifactId>
		<type>pom</type>
	</dependency>
	<!-- spring-webmvc依赖 -->
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
	</dependency>
	<!-- 日志文件管理包 -->
	<!-- log start -->
	<dependency>
		<groupId>log4j</groupId>
		<artifactId>log4j</artifactId>
	</dependency>
	<!-- 上传组件包 -->
	<dependency>
		<groupId>commons-fileupload</groupId>
		<artifactId>commons-fileupload</artifactId>
	</dependency>
	<!-- common-io依赖 -->
	<dependency>
		<groupId>commons-io</groupId>
		<artifactId>commons-io</artifactId>
	</dependency>
	<!-- 引入jstl依赖 -->
	<dependency>
		<groupId>jstl</groupId>
		<artifactId>jstl</artifactId>
	</dependency>
	<!-- 引入jsp-api依赖 -->
	<dependency>
		<groupId>javax.servlet.jsp</groupId>
		<artifactId>jsp-api</artifactId>
		<scope>provided</scope>
	</dependency>
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<scope>provided</scope>
	</dependency>

	<!-- 引入jackson的依赖 -->
	<dependency>
		<groupId>com.fasterxml.jackson.core</groupId>
		<artifactId>jackson-databind</artifactId>
	</dependency>

	<!-- 依赖的公共包 -->
	<dependency>
		<groupId>org.apache.commons</groupId>
		<artifactId>commons-lang3</artifactId>
	</dependency>
	
	<dependency>
		<groupId>org.apache.kafka</groupId>
		<artifactId>kafka_2.12</artifactId>
		<exclusions>
			<exclusion>
				<artifactId>jmxri</artifactId>
				<groupId>com.sun.jmx</groupId>
			</exclusion>
			<exclusion>
				<artifactId>jms</artifactId>
				<groupId>javax.jms</groupId>
			</exclusion>
			<exclusion>
				<artifactId>jmxtools</artifactId>
				<groupId>com.sun.jdmk</groupId>
			</exclusion>
		</exclusions>
	</dependency>
	<dependency>
		<groupId>org.springframework.kafka</groupId>
		<artifactId>spring-kafka</artifactId>
		<!-- <version>1.0.0.RC1</version> -->
	</dependency>
```


​		
```xml
	<dependency>
		<groupId>redis.clients</groupId>
		<artifactId>jedis</artifactId>
	</dependency>
	<dependency>
		<groupId>org.springframework.data</groupId>
		<artifactId>spring-data-redis</artifactId>
	</dependency>
```


#### 	添加本项目接口的依赖

​	    添加方式参考服务提供者。



#### 	引入web容器插件

```xml
		<build>
	<plugins>
		
		<plugin>
			<groupId>org.eclipse.jetty</groupId>
			<artifactId>jetty-maven-plugin</artifactId>
			<version>9.4.20.v20190813</version>
			<configuration>
				<httpConnector>
				
					<!-- 端口号 -->
					<port>9094</port>
					<!-- 项目访问路径 -->
					<host>0.0.0.0</host>
				</httpConnector>
				<contextHandlers>
					<jettyWebAppContext>
						<!-- 虚拟路径 -->
						<contextPath>/pic</contextPath>
						<!-- 物理路径 -->
						<resourceBase>d:/pic/</resourceBase>
					</jettyWebAppContext>
				</contextHandlers>
				<scanIntervalSeconds>1</scanIntervalSeconds>
			</configuration>
		</plugin>
		
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<configuration>
				<port>8080</port>
				<path>/</path>
				<uriEncoding>UTF-8</uriEncoding>
				<contextReloadable>true</contextReloadable>
				<staticContextDocbase>G://pic</staticContextDocbase>
				<staticContextPath>/pic</staticContextPath>
				<!-- 自动部署开始 -->
				    <url>http://192.168.110.134:8880/manager/text</url>
    				<username>boss</username>
			        <password>123456</password>
			        <update>true</update>
			        <path>/stu</path>
			       <!-- 自动部署结束 --> 
			</configuration>
		</plugin>
	</plugins>
</build>

```
## 编写代码

### 编写接口层

#### 	编写实体bean

​			注意要点：一定要加序列化。

#### 	编写接口service接口、

​		接口中的方法绝对不可以返回空值（void）

​	

### 编写服务层

#### 	实现接口工程中所有的接口。

​	实现类上加Service注解，这个service 注解必须是下图中所示的，不能使用spring的。	

​	![1587612379885](D:\八维教学\senior3\教案\senior-stage-three-hgshop\第六单元 Dubbo理论技能扩展\dubbo 与ssm 整的过程.assets\1587612379885.png) 



####   编写dao层

####  引入配置文件

  applicationContext-dubbo-provider.xml	

```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
xmlns="http://www.springframework.org/schema/beans"
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans 
http://www.springframework.org/schema/beans/spring-beans.xsd
   http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd
   http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
   http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
   http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
```


```xml
<!-- 应用程序 -->
<dubbo:application name="dubbo-service-demo"  />
<!-- 注册中心 采用zookeeper 必须修改  -->
<dubbo:registry
	address="zookeeper://192.168.110.134:2181" /> 
<!-- 直连 -->
<!-- <dubbo:registry address="N/A" /> -->
<!-- 通讯协议 必须修改 -->
<dubbo:protocol name="dubbo" port="20881" />
<!-- 注解扫描配置 指定了扫描的包   必须修改  -->
<dubbo:annotation
	package="com.mmcor.pregnant.service.impl" />
```
</beans>

​	



applicationContext-dao.xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:p="http://www.springframework.org/schema/p"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">


```xml
<!--指定外部属性文件的位置 -->
<context:property-placeholder location="classpath:db.properties" />

<!-- 使用druid数据源 连接池 -->
<bean id="dataSource"
	class="com.alibaba.druid.pool.DruidDataSource">
	<property name="driverClassName" value="${jdbc.driver}">
	</property>
	<property name="url" value="${jdbc.url}">
	</property>
	<property name="username" value="${jdbc.username}">
	</property>
	<property name="password" value="${jdbc.password}">
	</property>
</bean>
```


```xml
<!--4、 配置mybatis SqlSessionFactory -->
<bean id="sqlSessionFactory"
	class="org.mybatis.spring.SqlSessionFactoryBean" scope="singleton"
	autowire="default">
	<!-- 注入数据源 -->
	<property name="dataSource" ref="dataSource" />
	<!-- 关联mybatis配置文件 -->
	<property name="configLocation" value="classpath:mybatis.xml" />
	<!-- mapper加载的位置   -->
	<property name="mapperLocations" value="classpath:mappers/*" />
	<!-- 必须修改 xxxxxx-->
	<property name="typeAliasesPackage" value="com.mmcor.pregnant.pojo"/>
</bean>

<!--8 扫描mapper -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
	<!-- basePackage :mapper接口所在的包    必须修改 -->
	<property name="basePackage" value="com.mmcor.pregnant.dao" />
</bean>
```

</beans>

 

#### 编写测试类

```java

package com.mmcor.pregnant.service.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import com.mmcor.pregnant.pojo.User;
import com.mmcor.pregnant.service.UserService;

@RunWith(SpringRunner.class)
@ContextConfiguration({"classpath:applicationContext-dao.xml",
	"classpath:applicationContext-dubbo-provider.xml"})
public class TestUser {
	

@Autowired
UserService us;

@Test
public void testLogin() {
	
	User user = new User();
	user.setUsername("zhangsan");
	user.setPassword("123456");
	User login = us.login(user);
	System.err.println("等路成功 ?  " + (login!=null) );

}

}
```



### 完成消费者

#### 编写controller

​	小心referenc注解

```java
package com.mmcor.pregnant.controller;

import javax.servlet.http.HttpServletRequest;

import org.apache.dubbo.config.annotation.Reference;
import org.springframework.stereotype.Controller;

import com.mmcor.pregnant.pojo.User;
import com.mmcor.pregnant.service.UserService;

@Controller
public class UserController {

// 特别注意    需要使用apache.dubbo的注解
@Reference
UserService us;

public String login(HttpServletRequest request,User user) {
	User loginUser = us.login(user);
	if(loginUser==null) {
		//登录失败  保持在登录的页面
		return "login";
	}
	
	//登录成功
	request.getSession().setAttribute("sessionuser", user);
	return "index";
}
    
}




```

#### 完成jsp

#### 	

#### 完成配置文件

​	web.xml

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">
	
```xml
<welcome-file-list>
	<welcome-file>login.jsp</welcome-file>
</welcome-file-list>

<!-- <context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:applicationContext*.xml</param-value>
</context-param>
 -->

<servlet>
	<servlet-name>springDispatcherServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring-mvc.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>springDispatcherServlet</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>

<filter>
	<filter-name>CharacterEncodingFilter</filter-name>
	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<init-param>
		<param-name>encoding</param-name>
		<param-value>utf-8</param-value>
	</init-param>
</filter>
<filter-mapping>
	<filter-name>CharacterEncodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

</web-app>

#### 		

spring-mvc.xml



```xml
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:dubbo="http://dubbo.apache.org/schema/dubbo"
	xmlns="http://www.springframework.org/schema/beans"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	    http://www.springframework.org/schema/beans/spring-beans.xsd
       http://dubbo.apache.org/schema/dubbo 
       http://dubbo.apache.org/schema/dubbo/dubbo.xsd
       http://www.springframework.org/schema/context 
       http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/mvc 
		http://www.springframework.org/schema/mvc/spring-mvc.xsd">
```



```xml
<!-- 必须修改   -->
 <!-- <import resource="classpath:applicationContext-elasticSearch.xml"/> -->
<!-- <import resource="classpath:applicationContext-kafka-consumer.xml"/>  -->
<!-- 扫描器   必须修改 -->
<context:component-scan
	base-package="com.mmcor.pregnant.controller" />
<!-- 视图解析图 -->
<bean
	class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<!-- 配置前缀 -->
	<property name="prefix" value="/WEB-INF/view/"></property>
	<!-- 配置后缀 -->
	<property name="suffix" value=".jsp"></property>
</bean>
<!-- 不拦截静态资源 -->
<mvc:default-servlet-handler />
<!-- mvc注解驱动 -->
<mvc:annotation-driven/>
```


​	
```xml
 <!-- 文件上传的处理类 -->
<bean id="multipartResolver"
	class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
	
<dubbo:application name="pregnant-web"  >
	<dubbo:parameter key="qos.enable" value="false"/>
</dubbo:application>
```


​	
```xml
<!-- 注册中心  必须修改 -->
<dubbo:registry
	address="zookeeper://192.168.110.134:2181"  />
<!-- 配置扫描包的路径 -->	
<dubbo:annotation package="com.mmcor.pregnant.controller"/> 
```
</beans>



