---
layout: post
title: 用mybatisGenerator解放你的生产力
category: java
description: "在写代码时繁杂的dao，domain，mapper写这种代码又无聊又容易出错，尤其是在mapper里出了错不易发现，有了mybatisGenerator，原来生活可以如此美好..."
modified: 2015-03-28
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
        connectionURL="${database.url}" 
        userId="${database.username}"  
        password="${database.password}" 
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
        <!--忽略列，不生成该字段 -->  
        <ignoreColumn column="test_column" />   
       	<!-- 指定列的java数据类型   -->
	<columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />  
     </table>    
  </context>
</generatorConfiguration>
{% endhighlight %}

其实上面的代码里面已经有一些注释了，大家应该也能看明白，我就简单的讲一下每个配置是做什么的。

* properties 可以用来存放一些配置信息，这样可以不在这个config文件里面存放数据库的信息
* classPathEntr 用于指明进行数据库连接所需要使用的jar包的地址，按照jar包的位置填写就可以
* jdbcConnection 用于指明数据库的配置信息，我在上面xml的配置里是使用的property里的信息，所有不用写出来
* javaTypeResolver 进行类型转换的一些信息*forceBigDecimals*为false则JDBC DECIMAL、NUMERIC类型解析为Integer，若为true则解析为java.math.BigDecimal，一般我们都会把这个设为false
* javaModelGenerator 生成的数据表对应的类存放的位置以及一些转换信息*enableSubPackages*意思是是不是要在你给定的路径下再生成一级对象名的文件夹一般会设为false。 *trimStrings*意思是是不是要把数据库里返回的数据trim一下去除空格。
* sqlMapGenerator 生成的mapper文件存放的位置信息等，与上一个类似
* javaClientGenerator 生成的DAO文件的存放位置信息等，与上一个类似
* table 这个是需要转换的表的信息，*enableDeleteByExample*这一类的意思是是不是要生成例子，如果需要可以设置为true。*ignoreColumn*是需要忽略掉的列名，这一列不要出现在生成的文件中。*columnOverride*意思是无论数据库字段是何类型，生成的类属性都是varchar

以上就是最有用的配置信息，有了这些就可以简单的生成代码了。

生成只要一行代码
{% highlight sh %}
java -jar mybatis-generator-core-1.3.2.jar -configfile testproject/src/main/resources/generatorConfig.xml -overwrite
//根据自己的配置情况来更改信息，大概就是这么个意思。
{% endhighlight %}

现在去检查下，是不是你指定的文件夹里面有所有的需要的java文件？

***巴适***

ps:真是美好的一天啊





