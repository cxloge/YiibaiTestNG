# TestNG分组测试

分组测试是TestNG中的一个新的创新功能，它在JUnit框架中是不存在的。 它允许您将方法调度到适当的部分，并执行复杂的测试方法分组。 您不仅可以声明属于某个分组的方法，还可以指定包含其他组的组。 然后调用`TestNG`，并要求其包含一组特定的组(或正则表达式)，同时排除另一个分组。 组测试提供了如何分区测试的最大灵活性，如果您想要背靠背运行两组不同的测试，则不需要重新编译任何内容。

使用`<groups>`标记在`testng.xml`文件中指定分组。 它可以在`<test>`或`<suite>`标签下找到。 `<suite>`标签中指定分组适用于其下的所有`<test>`标签。

在本教程中，我们将演示如何在TestNG中进行分组测试。

## 1. 在方法上的分组

下面是一个测试分组示例 -

- `runSelenium()`和`runSelenium1()`属于分组：`selenium-test`。
- `testConnectOracle()`和`testConnectMsSQL()`属于分组：`database` 。
- 如果分组`selenium-test`和`database`通过，则`runFinal()`将被执行。

创建一个 Maven 项目：**GroupsTest**，其目录结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/554160500_18525.png)

创建一个测试类：*TestGroup.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.AfterGroups;
import org.testng.annotations.BeforeGroups;
import org.testng.annotations.Test;

public class TestGroup {

    @BeforeGroups("database")
    public void setupDB() {
        System.out.println("setupDB()");
    }

    @AfterGroups("database")
    public void cleanDB() {
        System.out.println("cleanDB()");
    }

    @Test(groups = "selenium-test")
    public void runSelenium() {
        System.out.println("runSelenium()");
    }

    @Test(groups = "selenium-test")
    public void runSelenium1() {
        System.out.println("runSelenium()1");
    }

    @Test(groups = "database")
    public void testConnectOracle() {
        System.out.println("testConnectOracle()");
    }

    @Test(groups = "database")
    public void testConnectMsSQL() {
        System.out.println("testConnectMsSQL");
    }

    @Test(dependsOnGroups = { "database", "selenium-test" })
    public void runFinal() {
        System.out.println("runFinal");
    }

}


Java
```

运行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-326443385\testng-customsuite.xml

runSelenium()
runSelenium()1
setupDB()
testConnectMsSQL
testConnectOracle()
cleanDB()
runFinal
PASSED: runSelenium
PASSED: runSelenium1
PASSED: testConnectMsSQL
PASSED: testConnectOracle
PASSED: runFinal

===============================================
    Default test
    Tests run: 5, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 5, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 14 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 38 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 5 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 41 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 4 ms


Shell
```

## 2. 在类上的分组

“分组”可以在类上应用。 在下面的示例中，“`TestSelenium`”类的每个公共方法都属于分组：`selenium-test` 。

创建一个测试类：*TestSelenium.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

@Test(groups = "selenium-test")
public class TestSelenium {

    public void runSelenium() {
        System.out.println("runSelenium()");
    }

    public void runSelenium1() {
        System.out.println("runSelenium()1");
    }

}


Java
```

创建一个XML文件来运行`2`个测试类。

创建一个测试类：*testng.xml* ，其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >

<suite name="TestAll">

    <test name="final">
        <classes>
            <class name="com.yiibai.TestSelenium" />
            <class name="com.yiibai.TestGroup" />
        </classes>
    </test>

    <!-- Run test method on group "selenium" only -->
    <test name="selenium">

        <groups>
            <run>
                <include name="selenium-test" />
            </run>
        </groups>

        <classes>
            <class name="com.yiibai.TestSelenium" />
            <class name="com.yiibai.TestGroup" />
        </classes>

    </test>

</suite>


XML
```

运行上面代码，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\GroupsTest\src\main\java\com\yiibai\testng.xml

runSelenium()
runSelenium()1
runSelenium()
runSelenium()1
setupDB()
testConnectMsSQL
testConnectOracle()
cleanDB()
runFinal
runSelenium()
runSelenium()1
runSelenium()
runSelenium()1

===============================================
TestAll
Total tests run: 11, Failures: 0, Skips: 0
===============================================


Shell
```

## 3. 其它分组

测试方法也可以同时属于多个分组，如下代码所示 -

```
@Test(groups = {"mysql","database"})
public void testConnectMsSQL() {
    System.out.println("testConnectMsSQL");
}


Java
```