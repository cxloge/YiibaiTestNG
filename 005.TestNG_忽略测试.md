# TestNG忽略测试

有时，我们编写的代码并没有准备就绪，并且测试用例要测试该方法/代码是否失败(或成功)。 在本示例中，注释`@Test(enabled = false)`有助于禁用此测试用例。

如果使用`@Test(enabled = false)`注释在测试方法上，则会绕过这个未准备好测试的测试用例。

在本教程中，我们将演示如何使用`@Test(enabled = false)`来忽略测试方法。

创建一个Maven项目，其结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/204150511_34557.png)

*pom.xml* 依懒包配置 -

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
    </dependencies>

</project>


XML
```

创建一个测试类：*TestIgnore.java*，其代码如下所示 -

```
package com.yiibai;

import org.testng.Assert;
import org.testng.annotations.Test;

public class TestIgnore {

    @Test // default enable=true
    public void test1() {
        Assert.assertEquals(true, true);
    }

    @Test(enabled = true)
    public void test2() {
        Assert.assertEquals(true, true);
    }

    @Test(enabled = false)
    public void test3() {
        Assert.assertEquals(true, true);
    }

}


Java
```

运行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-834820920\testng-customsuite.xml

PASSED: test1
PASSED: test2

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 12 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 33 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 4 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 42 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 4 ms
```