# TestNG参数化测试

TestNG中的另一个有趣的功能是参数化测试。 在大多数情况下，您会遇到业务逻辑需要大量测试的场景。 参数化测试允许开发人员使用不同的值一次又一次地运行相同的测试。

TestNG可以通过两种不同的方式将参数直接传递给测试方法：

- 使用`testng.xml`
- 使用数据提供者

在本教程中，我们将向您展示如何通过XML `@Parameters`或`@DataProvider`将参数传递给`@Test`方法。

为了方便演示，这里创建一个名称为：**ParameterTest** 的 Maven 工程，其结构如下所示 -
![img](http://www.yiibai.com/uploads/images/201705/0205/816110510_51197.png)

## 1. 使用XML传递参数

在此示例中，`filename`属性从`testng.xml`传递，并通过`@Parameters`注入到该方法中。

依懒文件：`pom.xml` 文件代码如下所示 -

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yiibai</groupId>
    <artifactId>IgnoreTest</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>IgnoreTest</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>6.8.7</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.2</version>
        </dependency>
    </dependencies>
    <build>
        <defaultGoal>compile</defaultGoal>
    </build>

</project>


XML
```

创建一个名称为：*TestParameterXML.java*，其代码如下所示 -

```
package com.yiibai;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.URL;
import java.sql.Connection;
import java.sql.DriverManager;
import java.util.Properties;

import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

public class TestParameterXML {

    Connection con;

    @Test
    @Parameters({ "dbconfig", "poolsize" })
    public void createConnection(String dbconfig, int poolsize) {

        System.out.println("dbconfig : " + dbconfig);
        System.out.println("poolsize : " + poolsize);

        Properties prop = new Properties();
        InputStream input = null;

        try {
            // get properties file from project classpath
            String path = System.getProperty("user.dir")+"\\"+dbconfig;

            System.out.println("path => "+path);
            //input = getClass().getClassLoader().getResourceAsStream(path);

            //prop.load(input);
            prop.load(new FileInputStream(dbconfig));  

            String drivers = prop.getProperty("jdbc.driver");
            String connectionURL = prop.getProperty("jdbc.url");
            String username = prop.getProperty("jdbc.username");
            String password = prop.getProperty("jdbc.password");

            System.out.println("drivers : " + drivers);
            System.out.println("connectionURL : " + connectionURL);
            System.out.println("username : " + username);
            System.out.println("password : " + password);

            Class.forName(drivers);
            con = DriverManager.getConnection(connectionURL, username, password);

        } catch (Exception e) {
            //e.printStackTrace();
        } finally {
            if (input != null) {
                try {
                    input.close();
                } catch (IOException e) {
                    //e.printStackTrace();
                }
            }
        }

    }

}


Java
```

创建一个名称为：*db.properties* 的文件， 其代码如下所示 -

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=123456


Shell
```

创建一个名称为：*testng.xml* 的文件， 其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="test-parameter">

    <test name="example1">

        <parameter name="dbconfig" value="db.properties" />
        <parameter name="poolsize" value="10" />

        <classes>
            <class name="com.yiibai.TestParameterXML" />
        </classes>

    </test>

</suite>


XML
```

执行上面测试类代码，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\ParameterTest\src\main\java\com\yiibai\testng.xml

dbconfig : db.properties
poolsize : 10
path => F:\worksp\testng\ParameterTest\db.properties
drivers : com.mysql.jdbc.Driver
connectionURL : jdbc:mysql://localhost:3306/test
username : root
password : 123456
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
Tue May 02 23:11:05 CST 2017 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

===============================================
test-parameter
Total tests run: 1, Failures: 0, Skips: 0
===============================================


Shell
```

## 2. 通过[@DataProvider](https://github.com/DataProvider)传递参数

2.1. 查看一个简单的`@DataProvider`示例，传递一个`int`参数。

创建一个名称为：*TestParameterDataProvider.java* 的文件， 其代码如下所示 -

```
package com.yiibai;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider {

    @Test(dataProvider = "provideNumbers")
    public void test(int number, int expected) {
        Assert.assertEquals(number + 10, expected);
    }

    @DataProvider(name = "provideNumbers")
    public Object[][] provideData() {

        return new Object[][] { { 10, 20 }, { 100, 110 }, { 200, 210 } };
    }

}


Java
```

执行上面测试类代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--1925148879\testng-customsuite.xml

PASSED: test(10, 20)
PASSED: test(100, 110)
PASSED: test(200, 210)

===============================================
    Default test
    Tests run: 3, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 3, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 13 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 34 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 7 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 69 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 4 ms


Shell
```

2.2. `@DataProvider`支持传递一个对象参数。 下面的例子显示了如何传递一个Map对象作为参数。

创建一个名称为：*TestParameterDataProvider2.java* 的文件， 其代码如下所示 -

```
package com.yiibai;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider2 {

    @Test(dataProvider = "dbconfig")
    public void testConnection(Map<String, String> map) {

        for (Map.Entry<String, String> entry : map.entrySet()) {
            System.out.println("[Key] : " + entry.getKey() + " [Value] : " + entry.getValue());
        }

    }

    @DataProvider(name = "dbconfig")
    public Object[][] provideDbConfig() {
        Map<String, String> map = readDbConfig();
        return new Object[][] { { map } };
    }

    public Map<String, String> readDbConfig() {

        Properties prop = new Properties();
        InputStream input = null;
        Map<String, String> map = new HashMap<String, String>();

        try {
            input = getClass().getClassLoader().getResourceAsStream("db.properties");

            prop.load(input);

            map.put("jdbc.driver", prop.getProperty("jdbc.driver"));
            map.put("jdbc.url", prop.getProperty("jdbc.url"));
            map.put("jdbc.username", prop.getProperty("jdbc.username"));
            map.put("jdbc.password", prop.getProperty("jdbc.password"));

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            if (input != null) {
                try {
                    input.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

        return map;

    }

}


Java
```

执行上面测试类代码，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\ParameterTest\src\main\java\com\yiibai\testng.xml

dbconfig : db.properties
poolsize : 10
path => F:\worksp\testng\ParameterTest\db.properties
drivers : com.mysql.jdbc.Driver
connectionURL : jdbc:mysql://localhost:3306/test
username : root
password : 123456
Loading class `com.mysql.jdbc.Driver'. This is deprecated. The new driver class is `com.mysql.cj.jdbc.Driver'. The driver is automatically registered via the SPI and manual loading of the driver class is generally unnecessary.
Tue May 02 23:15:52 CST 2017 WARN: Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.

===============================================
test-parameter
Total tests run: 1, Failures: 0, Skips: 0
===============================================


Shell
```

## 3. [@DataProvider](https://github.com/DataProvider) + 方法

此示例显示如何根据测试方法名称传递不同的参数。

创建一个名称为：*TestParameterDataProvider3.java* 的文件， 其代码如下所示 -

```
package com.yiibai;

import java.lang.reflect.Method;
import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider3 {

    @Test(dataProvider = "dataProvider")
    public void test1(int number, int expected) {
        Assert.assertEquals(number, expected);
    }

    @Test(dataProvider = "dataProvider")
    public void test2(String email, String expected) {
        Assert.assertEquals(email, expected);
    }

    @DataProvider(name = "dataProvider")
    public Object[][] provideData(Method method) {

        Object[][] result = null;

        if (method.getName().equals("test1")) {
            result = new Object[][] {
                { 1, 1 }, { 200, 200 }
            };
        } else if (method.getName().equals("test2")) {
            result = new Object[][] {
                { "test@gmail.com", "test@gmail.com" },
                { "test@yahoo.com", "test@yahoo.com" }
            };
        }

        return result;

    }

}


Java
```

执行上面测试类代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-817554174\testng-customsuite.xml

PASSED: test1(1, 1)
PASSED: test1(200, 200)
PASSED: test2("test@gmail.com", "test@gmail.com")
PASSED: test2("test@yahoo.com", "test@yahoo.com")

===============================================
    Default test
    Tests run: 4, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 4, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 17 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 51 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 9 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 93 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 5 ms


Shell
```

## 4. [@DataProvider](https://github.com/DataProvider) + ITestContext

在TestNG中，我们可以使用`org.testng.ITestContext`来确定调用当前测试方法的运行时参数。 在最后一个例子中，我们将演示如何根据包含的分组名称传递参数。

创建一个名称为：*TestParameterDataProvider4.java* 的文件， 其代码如下所示 -

```
package com.yiibai;

import org.testng.Assert;
import org.testng.ITestContext;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class TestParameterDataProvider4 {

    @Test(dataProvider = "dataProvider", groups = {"groupA"})
    public void test1(int number) {
        Assert.assertEquals(number, 1);
    }

    @Test(dataProvider = "dataProvider", groups = "groupB")
    public void test2(int number) {
        Assert.assertEquals(number, 2);
    }

    @DataProvider(name = "dataProvider")
    public Object[][] provideData(ITestContext context) {

        Object[][] result = null;

        //get test name
        //System.out.println(context.getName());

        for (String group : context.getIncludedGroups()) {

            System.out.println("group : " + group);

            if ("groupA".equals(group)) {
                result = new Object[][] { { 1 } };
                break;
            }

        }

        if (result == null) {
            result = new Object[][] { { 2 } };
        }
        return result;

    }

}


Java
```

创建一个名称为：*testng4.xml* 的文件， 其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="test-parameter">

    <test name="example1">

        <groups>
            <run>
                <include name="groupA" />
            </run>
        </groups>

        <classes>
            <class name="com.yiibai.TestParameterDataProvider4" />
        </classes>

    </test>

</suite>


XML
```

执行上面测试类代码，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\ParameterTest\src\main\java\com\yiibai\testng4.xml

group : groupA

===============================================
test-parameter
Total tests run: 1, Failures: 0, Skips: 0
===============================================

```