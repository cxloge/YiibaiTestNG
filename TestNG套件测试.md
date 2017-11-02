# TestNG套件测试

测试套件是用于测试软件程序的行为或一组行为的测试用例的集合。 在TestNG中，我们无法在测试源代码中定义一个套件，但它可以由一个XML文件表示，因为套件是执行的功能。 它还允许灵活配置要运行的测试。 套件可以包含一个或多个测试，并由`<suite>`标记定义。

`<suite>`是`testng.xml`的根标记。 它描述了一个测试套件，它又由几个`<test>`部分组成。

下表列出了`<suite>`接受的所有定义的合法属性。

| 属性           | 描述                         |
| ------------ | -------------------------- |
| name         | 套件的名称，这是一个强制属性。            |
| verbose      | 运行的级别或详细程度。                |
| parallel     | TestNG是否运行不同的线程来运行这个套件。    |
| thread-count | 如果启用并行模式(忽略其他方式)，则要使用的线程数。 |
| annotations  | 在测试中使用的注释类型。               |
| time-out     | 在本测试中的所有测试方法上使用的默认超时。      |

在本教程中，我们将向您展示如何一起运行多个TestNG测试用例(类)，也称为套件测试。

创建一个名称为：**SuiteTest** 的项目，其结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/259160559_89899.png)

## 1. 测试类

我们来看看以下三个测试类。

创建一个文件：`TestConfig.java`，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.AfterSuite;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.BeforeTest;

//show the use of @BeforeSuite and @BeforeTest
public class TestConfig {

    @BeforeSuite
    public void testBeforeSuite() {
        System.out.println("testBeforeSuite()");
    }

    @AfterSuite
    public void testAfterSuite() {
        System.out.println("testAfterSuite()");
    }

    @BeforeTest
    public void testBeforeTest() {
        System.out.println("testBeforeTest()");
    }

    @AfterTest
    public void testAfterTest() {
        System.out.println("testAfterTest()");
    }

}


Java
```

创建一个文件：`TestDatabase.java`，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestDatabase {

    @Test(groups = "db")
    public void testConnectOracle() {
        System.out.println("testConnectOracle()");
    }

    @Test(groups = "db")
    public void testConnectMsSQL() {
        System.out.println("testConnectMsSQL");
    }

    @Test(groups = "db-nosql")
    public void testConnectMongoDB() {
        System.out.println("testConnectMongoDB");
    }

    @Test(groups = { "db", "brokenTests" })
    public void testConnectMySQL() {
        System.out.println("testConnectMySQL");
    }

}


Java
```

创建一个文件：`TestOrder.java`，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestOrder {

    @Test(groups={"orderBo", "save"})
    public void testMakeOrder() {
      System.out.println("testMakeOrder");
    }

    @Test(groups={"orderBo", "save"})
    public void testMakeEmptyOrder() {
      System.out.println("testMakeEmptyOrder");
    }

    @Test(groups="orderBo")
    public void testUpdateOrder() {
        System.out.println("testUpdateOrder");
    }

    @Test(groups="orderBo")
    public void testFindOrder() {
        System.out.println("testFindOrder");
    }

}


Java
```

## 2. testng.xml

要运行上面的测试类，创建一个XML文件 - `testng.xml`(可以是任何文件名)文件，并定义以下内容细节：

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="TestAll">

    <test name="order">
        <classes>
            <class name="com.yiibai.TestConfig" />
            <class name="com.yiibai.TestOrder" />
        </classes>
    </test>

    <test name="database">
        <classes>
            <class name="com.yiibai.TestConfig" />
            <class name="com.yiibai.TestDatabase" />
        </classes>
    </test>

</suite>


XML
```

执行上面代码，得到如下结果 -

```
[TestNG] Running:
  F:\worksp\testng\SuiteTest\src\main\java\com\yiibai\testng.xml

testBeforeSuite()
testBeforeTest()
testFindOrder
testMakeEmptyOrder
testMakeOrder
testUpdateOrder
testAfterTest()
testBeforeTest()
testConnectMongoDB
testConnectMsSQL
testConnectMySQL
testConnectOracle()
testAfterTest()
testAfterSuite()

===============================================
TestAll
Total tests run: 8, Failures: 0, Skips: 0
===============================================


Shell
```

## 其他例子

这里有一些常用的例子。

3.1. 指定包名称而不是类名称：

创建一个XML文件：`testng-1.xml`，其代码如下所示 -

```
<suite name="TestAll">

    <test name="order">
        <packages>
            <package name="com.yiibai.*" />
        </packages>
    </test>

</suite>


XML
```

3.2. 指定包含或排除的方法，修改上面XML文件：`testng-1.xml`，其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="TestAll">

  <test name="order">
    <classes>
        <class name="com.yiibai.TestConfig" />
        <class name="com.yiibai.TestOrder">
            <methods>
                <include name="testMakeOrder" />
                <include name="testUpdateOrder" />
                <!--
                    <exclude name="testMakeOrder" />
                 -->
            </methods>
        </class>
    </classes>
  </test>

</suite>


XML
```

使用以上xml文件执行测试，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\SuiteTest\src\main\java\com\yiibai\testng-1.xml

testBeforeSuite()
testBeforeTest()
testMakeOrder
testUpdateOrder
testAfterTest()
testAfterSuite()

===============================================
TestAll
Total tests run: 2, Failures: 0, Skips: 0
===============================================


Shell
```

3.3. 指定要包括或排除某个分组，创建一个XML文件：`testng-2.xml`，其代码如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="TestAll">

  <test name="database">
    <groups>
        <run>
            <exclude name="brokenTests" />
            <include name="db" />
        </run>
    </groups>

    <classes>
        <class name="com.yiibai.TestDatabase" />
    </classes>
  </test>

</suite>


XML
```

使用以上xml文件执行测试，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\SuiteTest\src\main\java\com\yiibai\testng-2.xml

testConnectMsSQL
testConnectOracle()

===============================================
TestAll
Total tests run: 2, Failures: 0, Skips: 0
===============================================

```