# TestNG+Spring集成测试

在本教程中，我们将演示如何使用TestNG测试Spring的组件。

**使用的工具 ：**

- TestNG 6.8.7
- Spring 3.2.2.RELEASE
- Maven 3
- Eclipse IDE

## 1. 项目依赖

为了演示，首先创建一个名称为：**TestngSpringIntegration** 的 Maven 项目。

要将Spring与TestNG集成，您需要`spring-test.jar`包依懒，添加以下内容：

创建文件：*pom.xml* 配置代码如下 -

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yiibai</groupId>
    <artifactId>TestngSpringIntegration</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>TestngSpringIntegration</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <spring.version>4.0.6.RELEASE</spring.version>
        <testng.version>6.8.7</testng.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <defaultGoal>compile</defaultGoal>
    </build>

</project>


XML
```

## 2. Spring组件

创建一个简单的Spring组件，稍后将使用TestNG来测试这个组件。

创建一个Java接口文件：*EmailGenerator.java* ，代码如下 -

```
package com.yiibai;

public interface EmailGenerator {

    public String generate();

}


Java
```

创建一个Java类文件：*RandomEmailGenerator.java* ，代码如下 -

```
package com.yiibai;

import org.springframework.stereotype.Service;

@Service
public class RandomEmailGenerator implements EmailGenerator {

    public String generate() {
        return "feedback@yiibai.com";
    }

}


Java
```

## 3. TestNG + Spring

在 `test` 文件夹中创建一个Spring配置文件，用于Spring组件扫描。

创建一个XML文件：*${project}/src/test/resources/spring-test-config.xml* ，代码如下 -

```
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.2.xsd
    ">

    <context:component-scan base-package="com.yiibai" />

</beans>


XML
```

要访问TestNG中的Spring组件，扩展`AbstractTestNGSpringContextTests`，请参阅以下示例：

创建一个Java文件：*${project}/src/test/java/com/yiibai/TestSpring.java* ，代码如下 -

```
package com.yiibai;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.testng.AbstractTestNGSpringContextTests;
import org.testng.Assert;
import org.testng.annotations.Test;
import com.yiibai.EmailGenerator;

@Test
@ContextConfiguration(locations = { "classpath:spring-test-config.xml" })
public class TestSpring extends AbstractTestNGSpringContextTests {

    @Autowired
    EmailGenerator emailGenerator;

    @Test()
    void testEmailGenerator() {

        String email = emailGenerator.generate();
        System.out.println(email);

        Assert.assertNotNull(email);
        Assert.assertEquals(email, "feedback@yiibai.com");


    }

}


Java
```

执行上面测试代码，输出以下结果 -

```
五月 04, 2017 00:26:07 上午 org.springframework.test.context.TestContextManager retrieveTestExecutionListeners
信息: Could not instantiate TestExecutionListener [org.springframework.test.context.web.ServletTestExecutionListener]. Specify custom listener classes or make the default listener classes (and their required dependencies) available. Offending class: [javax/servlet/ServletContext]
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--1855856090\testng-customsuite.xml

五月 04, 2017 00:26:07 上午 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
信息: Loading XML bean definitions from class path resource [spring-test-config.xml]
五月 04, 2017 00:26:07 上午 org.springframework.context.support.GenericApplicationContext prepareRefresh
信息: Refreshing org.springframework.context.support.GenericApplicationContext@262b2c86: startup date [Thu May 04 00:26:07 CST 2017]; root of context hierarchy
feedback@yiibai.com
PASSED: testEmailGenerator

===============================================
    Default test
    Tests run: 1, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 1 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@49e4cb85: 12 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@5594a1b5: 6 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@5a39699c: 80 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@7d417077: 34 ms
[TestNG] Time taken by org.testng.reporters.XMLReporter@7c75222b: 13 ms


Shell
```