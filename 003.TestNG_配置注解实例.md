# TestNG配置注解实例

在TestNG中，我们可以使用以下注释来执行测试类的配置，如设置/清理数据库，准备虚拟数据，部署/关闭服务器等。

- `@BeforeSuite` - 对于套件测试，在此套件中的所有测试运行之前运行。
- `@AfterSuite` - 对于套件测试，在此套件中的所有测试运行之后运行。
- `@BeforeTest` - 对于套件测试，在运行属于`<test>`标签内的类的任何测试方法之前运行。
- `@AfterTest` - 对于套件测试，在运行属于`<test>`标签内的类的所有测试方法都已运行之后运行。
- `@BeforeGroups`：在调用属于该组的第一个测试方法之前运行。
- `@AfterGroups`：在调用属于组的最后一个测试方法之后运行。
- `@BeforeClass`- 在当前类的第一个测试方法之前运行。
- `@AfterClass` - 运行当前类中的所有测试方法之后都运行。
- `@BeforeMethod` - 在每个测试方法之前运行。
- `@AfterMethod` - 在每个测试方法之后运行。

> 注： 套件测试是什么东西？ - 套件测试是一起运行的多个测试类。

查看以下示例以查看执行顺序 - 首先调用哪个方法，接下来又是哪一个。

创建一个名称为：`ConfigurationAnnotations` 的 Maven 项目，其结构如下所示 -

## 1. 单测试类

运行单个测试用例，演示如何使用 `group`, `class` 和 `method` 之前/之后。

创建文件：*TestDBConnection.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestDBConnection {

    @Test
    public void runOtherTest1() {
        System.out.println("@Test - runOtherTest1");
    }

    @Test
    public void runOtherTest2() {
        System.out.println("@Test - runOtherTest2");
    }

}


Java
```

执行上面测试类，输出结果如下 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-634199298\testng-customsuite.xml

@BeforeClass
@BeforeGroups
@BeforeMethod
@Test - runTest1
@AfterMethod
@AfterGroups
@BeforeMethod
@Test - runTest2
@AfterMethod
@AfterClass
PASSED: runTest1
PASSED: runTest2

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 14 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 50 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 6 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 42 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 3 ms


Shell
```

## 2. 套件测试类

再创建`2`个测试类来演示如何使用之前/之后的套件和测试。

创建文件：*DBConfig.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.AfterSuite;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.BeforeTest;

public class DBConfig {

    @BeforeSuite()
    public void beforeSuite() {
        System.out.println("@BeforeSuite");
    }

    @AfterSuite()
    public void afterSuite() {
        System.out.println("@AfterSuite");
    }

    @BeforeTest()
    public void beforeTest() {
        System.out.println("@BeforeTest");
    }

    @AfterTest()
    public void afterTest() {
        System.out.println("@AfterTest");
    }

}


Java
```

创建文件：*TestDBConnection.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestDBConnection {

    @Test
    public void runOtherTest1() {
        System.out.println("@Test - runOtherTest1");
    }

    @Test
    public void runOtherTest2() {
        System.out.println("@Test - runOtherTest2");
    }

}


Java
```

创建一个XML文件以一起运行多个测试用例。 阅读XML的注释，很容易就明白了。

创建文件：*testng.xml* ，其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >

<!-- @BeforeSuite -->
<suite name="TestAll">

    <!-- @BeforeTest -->
    <test name="case1">
      <classes>
        <class name="com.yiibai.TestConfiguration" />
        <class name="com.yiibai.TestDBConnection" />
        <class name="com.yiibai.DBConfig" />
      </classes>
    </test>
    <!-- @AfterTest -->

    <!-- @BeforeTest -->
    <test name="case2">
      <classes>
        <class name="com.yiibai.TestDBConnection" />
        <class name="com.yiibai.DBConfig" />
      </classes>
    </test>
    <!-- @AfterTest -->

</suite>
<!-- @AfterSuite -->


XML
```

执行上面代码，在文件：*TestDBConnection.java* 右键，在弹出的菜单项中选择 “**Run As **” => **Run Configures…** ，如下图所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/334140528_42763.png)

在新弹出框中输入对应的 **testng.xml** 文件，如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/971140530_53317.png)

输出结果如下 -

```
[TestNG] Running:
  F:\worksp\testng\ConfigurationAnnotations\src\main\java\com\yiibai\testng.xml

@BeforeSuite
@BeforeTest
@BeforeClass
@BeforeGroups
@BeforeMethod
@Test - runTest1
@AfterMethod
@AfterGroups
@BeforeMethod
@Test - runTest2
@AfterMethod
@AfterClass
@Test - runOtherTest1
@Test - runOtherTest2
@AfterTest
@BeforeTest
@Test - runOtherTest1
@Test - runOtherTest2
@AfterTest
@AfterSuite

===============================================
TestAll
Total tests run: 6, Failures: 0, Skips: 0
===============================================

```