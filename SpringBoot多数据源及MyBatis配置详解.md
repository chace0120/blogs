---
title: SpringBoot多数据源及MyBatis配置详解
date: 2016-07-15 16:38:40
tags: [springboot,spring,mybatis,datasource]
categories: [开发笔记]
---

### 前言

最近迫于项目需要，笔者踏上了springboot多数据源的配置之旅。之前笔者配置过spring的动态多数据源切换，当时使用的是JDBC Template。

目前项目中持久化框架使用是mybatis，经过分析后不难发现，多数据源配置需要解决两个问题，一个是由原先的spring经典方式切换到了springboot方式下，多数据源如何配置？有无太大变化？另一个是怎样将多数据源与mybatis的配置关联起来？

不妨先来看下,单数据源下mybatis如何配置的？

### 单数据源示例

首先要声明一点，项目只是依赖单个数据源时，如果你不介意springboot帮你做事的话，那么恭喜你，你省事儿了！你只需要在项目的属性文件中添加数据源的相关属性配置，springboot会“免费”提供给你一个数据源使用，默认采用的是[tomcat jdbc connection pool](https://tomcat.apache.org/tomcat-7.0-doc/jdbc-pool.html)。

当然你可以拒绝springboot的好意，如果你依赖第三方的连接池技术，你可以配置自己的数据源，那么springboot检测到你自己定义了DataSource后，就不会自动配置数据源了。

笔者不能拒绝springboot的好意，所以仅在项目的application.properties中添加了如下属性：

``` Java
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.max-idle=10
spring.datasource.max-wait=10000
spring.datasource.min-idle=5
spring.datasource.initial-size=5
spring.datasource.validation-query=SELECT 1
spring.datasource.test-on-borrow=false
spring.datasource.test-while-idle=true
spring.datasource.time-between-eviction-runs-millis=18800
```

然后笔者创建了一个专门用于配置mybatis的类，如下:

``` Java
@Configuration
public class MybatisSpringConfig {
    @Bean(name = "sqlSessionFactory")
    public SqlSessionFactory sqlSessionFactory(DataSource dataSource) throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setTypeAliasesPackage("demo.model");

        return factoryBean.getObject();
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer() {
        MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
        mapperScannerConfigurer.setBasePackage("demo.repository");

        return mapperScannerConfigurer;
    }
```

没错，mybatis在spring中就是可以通过如此的简练配置进而正常工作起来。你无需刻意地去创建mybatis的配置文件，无需刻意地去注册mapper类及指定对应xml文件的位置，这完全得益于[mybatis-spring](http://www.mybatis.org/spring/)，它就像一个“粘合剂”，可以很方便地将mybatis和spring“粘合”在一起。

### 多数据源示例

#### MyBatis-Spring相关配置

### 总结
