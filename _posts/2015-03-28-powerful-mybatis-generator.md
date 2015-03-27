---
layout: post
title: 用mybatisGenerator解放你的生产力
category: java
description: "在写代码时繁杂的dao，domain，mapper写这种代码又无聊又容易出错，尤其是在mapper里出了错，还不容易发现，有了mybatisGenerator，原来生活可以如此美好。。。。"
modified: 2015-03-18
tags: [java,xml,mybatis]
image:
    background: witewall_3.png
---

我的人生分为两个阶段，没用mybatisGenerator的阶段和用了的阶段。

因为这个简直好用到死啊。。。这个数据库逆向框架代码生成工具就是为我这种懒人准备的啊。

进入正题，怎么用呢？用着方便，配置起来也非常简单。

最关键的文件就是*generator.xml*这个文件是用于指定数据库jar包位置，数据库连接信息，生成包的位置，表名等信息。是指导*mybatis-generator-core*这个jar包的生成工具。
先贴一份xml，再慢慢道来。

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>
  <properties url="file:///Users/test/dev/testproject/src/main/webapp/WEB-INF/conf/database.properties"/>
  
  <classPathEntry
        location="/Users/test/.m2/repository/mysql/mysql-connector-java/5.1.15/mysql-connector-java-5.1.15.jar" />
   
  <!-- 一个数据库一个context -->
  <context id="waimai" targetRuntime="MyBatis3">
  	<!-- 引入配置文件 -->  
  	
  	<!-- 注释 -->  
    <commentGenerator >  
        <property name="suppressAllComments" value="true"/>	<!-- 是否取消注释 -->  
        <property name="suppressDate" value="true" /> 			<!-- 是否取消注释时间戳-->  
    </commentGenerator> 
  	
	<!-- jdbc连接 -->  
    <jdbcConnection 
    	driverClass="${database.driverClassName}"  
        connectionURL="${database.dbWmWrite.url}" 
        userId="${database.dbWmWrite.username}"  
        password="${database.dbWmWrite.password}" 
    />  
      
    <!-- 类型转换 -->  
    <javaTypeResolver>  
        <!-- 是否使用bigDecimal， false可自动转化以下类型（Long, Integer, Short, etc.） -->  
        <property name="forceBigDecimals" value="false"/>  
    </javaTypeResolver>  
      
    <!-- 生成实体类 -->    
    <javaModelGenerator 
    	targetPackage="com.test.domain"  
        targetProject="testproject/src/main/java" >
        <!-- 是否在当前路径下新加一层schema, eg：false:com.oop.eksp.user.model，true:com.oop.eksp.user.model.[schemaName] -->  
        <property name="enableSubPackages" value="false"/>  
        <!-- 是否针对string类型的字段在set的时候进行trim调用 -->  
        <property name="trimStrings" value="true"/>  
    </javaModelGenerator>  
      
    <!-- 生成map xml文件 -->  
   <sqlMapGenerator targetPackage="mybatis"
                    targetProject="testproject/src/main/resources" >
		<!--是否在当前路径下新加一层schema,eg：false:com.oop.eksp.user.model， true:com.oop.eksp.user.model.[schemaName]  --> 
        <property name="enableSubPackages" value="false" />  
    </sqlMapGenerator>
      
    <!-- 生成map xml对应client，也就是接口dao -->      
    <javaClientGenerator targetPackage="com.test.dao"  
        targetProject="testproject/src/main/java" type="XMLMAPPER" >
		<!--是否在当前路径下新加一层schema,eg：fase路径com.oop.eksp.user.model， true:com.oop.eksp.user.model.[schemaName] -->
        <property name="enableSubPackages" value="false" />  
    </javaClientGenerator> 
   
      
    <!-- 配置表信息 -->      
    <!-- tableName为对应的数据库表 domainObjectName是要生成的实体类 enable*ByExample是否生成example类   -->  
    <table tableName="test"   domainObjectName="Test"
    	   enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false">  
      <!--忽略列，不生成改字段 -->  
        <ignoreColumn column="FRED" />   
       	<!-- 指定列的java数据类型   -->
	<columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />  
     </table>

      
    
  </context>
</generatorConfiguration>
{% endhighlight %}


