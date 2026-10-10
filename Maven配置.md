# Maven配置

[详细](https://maven.apache.org/pom.html#Maven_Coordinates)

父工程Pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!--聚合-->
    <!--打包形式pom|jar|war-->
    <packaging>pom</packaging>
    <modules>
        <module>MavenTest</module>
    </modules>
    <!--当前项目坐标-->
    <groupId>com.example</groupId>
    <artifactId>JAVApratice</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>JAVApratice</name>
    <description>JAVApratice</description>
    <!--定义属性(方便集中管理依赖版本)-->
    <properties>
        <java.version>17</java.version>
        <spring-boot-version>3.0.3</spring-boot-version>
    </properties>
   <!-- 放置公共依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--使用定义的属性-->
             <version>${spring-boot-version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
               <!--使用定义的属性-->
            <version>${spring-boot-version}</version>
            <scope>test</scope>
<!--            可选依赖 对外隐藏当前依赖资源（断开间接依赖）（自己的依赖不给其他人用）-->
         <optional>true</optional>
<!--            排除依赖 主动断开依赖中的部分资源，被排除的资源无需指定版本（自己用别人的依赖时，隐藏不需要的依赖）-->
          <exclusions>
                <exclusion>
                   <groupId></groupId>
                   <artifactId></artifactId>
              </exclusion>-->
           </exclusions>-->
        </dependency>

    </dependencies>

<!--    定义依赖管理 配置子工程可选依赖-->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
            <version>3.3.1</version>
        </dependency>
    </dependencies>
</dependencyManagement>
    <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.4.2</version>
                <configuration>
                    <!--mybatis的代码生成器的配置文件-->
                    <configurationFile>../JAVApratice/mybatis-generator-config.xml</configurationFile>
                </configuration>
                <!--给插件安装依赖 -->
                <dependencies>
                    <dependency>
                        <groupId>com.mysql</groupId>
                        <artifactId>mysql-connector-j</artifactId>
                        <version>8.4.0</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

</project>

```

子工程

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <!--继承父工程-->
    <parent>
        <artifactId>JAVApratice</artifactId>
        <groupId>com.example</groupId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>MavenTest</artifactId>
    <!--加入父工程的可选依赖-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
    </dependencies>
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
    </properties>

</project>
```

版本管理

- 工程版本
  - SNAPSHOT(快照版本)
    1. 项目开发过程中临时输出的版本
    2. 快照版本会随着开发的进展不断更新
  - RELEASE(发布版本)
    1.    稳定版本，对应的构建文件稳定，即使进行后续开发也不会改变当前发布的版本内容
- 发布版本
  - alpha版本
  - beta版本
  - 纯数字版本