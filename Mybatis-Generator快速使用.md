# Mybatis-Generator快速使用

[](https://mybatis.org/generator/configreference/xmlconfig.html)

## 1.pom.xml同级创建generator-config.xml文件：

方式1

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!--context 生成上下文 配置生成规则
           id 随便写
           targetRuntime 生成策略
                 MyBatis3DynamicSql 默认的，会生成带动态Sql的CRUD
                 MyBatis3 生成通用的查询，可以指定动态where条件
                 MyBatis3Simple 只生成简单的CRUD
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple||MyBatis3">
        
        <!-- 注释 -->
        <commentGenerator >
            <property name="suppressAllComments" value="true"/><!-- 是否取消注释 -->
            <property name="suppressDate" value="true" /> <!-- 是否生成注释代时间戳-->
        </commentGenerator>
        <!--        连接数据库-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/medical"
                        userId="root"
                        password="root">
        </jdbcConnection>

        <!-- java类型处理器
                用于处理DB中的类型到Java中的类型，默认使用JavaTypeResolverDefaultImpl；
                注意一点，默认会先尝试使用Integer，Long，Short等来对应DECIMAL和 NUMERIC数据类型；
        -->

        <javaTypeResolver>
            <property name="forceBigDecimals" value="true"/>
        </javaTypeResolver>
        <!--生成po的包名和项目位置-->
        <javaModelGenerator targetPackage="com.example.demo.PO" targetProject="src/main/java">
            <!-- 在targetPackage的基础上，根据数据库的schema再生成一层package，最终生成的类放在这个package下，默认为             false -->
            <property name="enableSubPackages" value="true"/>

            <!-- 设置是否在getter方法中，对String类型字段调用trim()方法 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成Mapper.xml文件的包名和位置 -->
        <sqlMapGenerator targetPackage="com.example.demo.Mapper" targetProject="src/main/resources">
            <!--<property name="enableSubPackages" value="false"/>-->
        </sqlMapGenerator>


        <!--生成Mapper接口的包名和位置-->
        <!--ANNOTATEDMAPPER(基于注解写的sql) XMLMAPPER（基于映射） -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.example.demo.Mapper"                              targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>


        <table  tableName="pe" domainObjectName="PE"

               enableCountByExample="true"
               enableDeleteByExample="true" enableSelectByExample="true"
               enableUpdateByExample="true"
        >

        </table>

    </context>
</generatorConfiguration>

```





方式2

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>
	<!-- 引入配置文件 -->
	<properties resource="init.properties"/>
	
	<!-- 指定数据连接驱动jar地址（如果你使用的是 Maven 或 Gradle 这样的构建工具，不需要手动指定）指定了 JDBC 驱动 JAR 文件的位置 -->
	<classPathEntry location="${classPath}" />
	
	<!-- 一个数据库一个context -->
	<context id="infoGuardian" targetRuntime="MyBatis3Simple||MyBatis3">
		<!-- 注释 -->
		<commentGenerator >
			<property name="suppressAllComments" value="false"/><!-- 是否取消注释 -->
			<property name="suppressDate" value="true" /> <!-- 是否生成注释代时间戳-->
		</commentGenerator>
		
		<!-- jdbc连接 -->
		<jdbcConnection 
            driverClass="${jdbc_driver}"
			connectionURL="${jdbc_url}" 
            userId="${jdbc_user}"
			password="${jdbc_password}" />
		
		<!-- 类型转换 -->
		<javaTypeResolver>
			<!-- 是否使用bigDecimal， false可自动转化以下类型（Long, Integer, Short, etc.） -->
			<property name="forceBigDecimals" value="false"/>
		</javaTypeResolver>
		
		<!-- 生成实体类的包名和地址 -->	
		<javaModelGenerator targetPackage="com.oop.eksp.user.model"
			targetProject="${project}" >
			<!-- 是否在当前路径下新加一层schema,eg：fase路径com.oop.eksp.user.model，                      
            true:com.oop.eksp.user.model.[schemaName] -->
			<property name="enableSubPackages" value="false"/>
			<!-- 是否针对string类型的字段在set的时候进行trim调用 -->
			<property name="trimStrings" value="true"/>
		</javaModelGenerator>
		
		<!-- 生成Map.xml文件的包名和地址 -->
		<sqlMapGenerator targetPackage="com.oop.eksp.user.data"
			targetProject="${project}" >
			<!-- 是否在当前路径下新加一层schema,eg：fase路径com.oop.eksp.user.model，           
            true:com.oop.eksp.user.model.[schemaName] -->
			<property name="enableSubPackages" value="false" />
		</sqlMapGenerator>
		
		<!-- 生成Mapper，也就是接口dao 的包名和地址 -->	
		<javaClientGenerator targetPackage="com.oop.eksp.user.data"
			targetProject="${project}" type="XMLMAPPER" >
			<!-- 是否在当前路径下新加一层schema,eg：fase路径com.oop.eksp.user.model，                       
             true:com.oop.eksp.user.model.[schemaName] -->
			<property name="enableSubPackages" value="false" />
		</javaClientGenerator>
		
		<!-- 配置表信息 -->	
		<table schema="${jdbc_database}" tableName="s_user"
			domainObjectName="UserEntity" enableCountByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			enableUpdateByExample="false">
			<!-- schema即为数据库名 tableName为对应的数据库表 domainObjectName是要生成的实体类 enable*ByExample 
				是否生成 example类   -->
			
			<!-- 忽略列，不生成bean 字段 -->
			<ignoreColumn column="FRED" />
			<!-- 指定列的java数据类型 -->
	      	<columnOverride column="LONG_VARCHAR_FIELD" jdbcType="VARCHAR" />
		</table>
 
	</context>
</generatorConfiguration>
```

init.properties

```properties
#Mybatis Generator configuration
project = 文件地址
classPath=E:/workplace/EKSP/WebContent/WEB-INF/lib/ojdbc14.jar（如果你使用的是 Maven 或 Gradle 这样的构建工具，不需要手动指定）
jdbc_driver = com.mysql.cj.jdbc.Driver
jdbc_url=jdbc:mysql://localhost:3306
jdbc_database=medical
jdbc_user=username
jdbc_password=password
```

## 2.pom.xml中配置插件

```xml
   <project ...>
     ...
     <build>
       ...
       <plugins>
        ...
        <plugin>
          <groupId>org.mybatis.generator</groupId>
          <artifactId>mybatis-generator-maven-plugin</artifactId>
          <version>1.4.2</version>
        <configuration>
           <verbose>true</verbose> <!-- 开启详细日志输出 -->
           <overwrite>true</overwrite> <!-- 每次运行时允许覆盖现有文件 -->
           <!--mybatis的代码生成器的配置文件位置-->
           <configurationFile>../demo/src/main/resources/mybatis-generator-config.xml</configurationFile>
        </configuration>
        <dependencies>
               <!-- 添加你的JDBC驱动依赖 -->
                <dependency>
                    <groupId>com.mysql</groupId>
                    <artifactId>mysql-connector-j</artifactId>
                    <version>8.4.0</version>
               </dependency>
        </dependencies>
        </plugin>
        ...
      </plugins>
      ...
    </build>
    ...
  </project>

```

## 3.在maven中双击mybatis-generator:generate插件运行

![plugin.png](./plugin.png)