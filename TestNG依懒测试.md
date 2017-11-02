# TestNG依懒测试

有时，我们可能需要以特定顺序调用测试用例中的方法，或者可能希望在方法之间共享一些数据和状态。 TestNG支持这种依赖关系，因为它支持在测试方法之间显式依赖的声明。

TestNG允许指定依赖关系：

- 在`@Test`注释中使用属性`dependsOnMethods`，或者
- 在`@Test`注释中使用属性`dependsOnGroups`。

在TestNG中，我们使用`dependOnMethods`和`dependsOnGroups`来实现依赖测试。 如果依赖方法失败，则将跳过所有后续测试方法。

为了方便演示使用，首先创建一个 Maven 项目： **DependOnTest**，其项目结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/287170544_57656.png)

## 1. dependOnMethods示例

一个简单的例子，“`method2()`”依赖于“`method1()`”。

### 1.1. 如果`method1()`通过，那么将执行`method2()`。

创建一个Java文件： *App.java*，其代码结构如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class App {

    @Test
    public void method1() {
        System.out.println("This is method 1");
    }

    @Test(dependsOnMethods = { "method1" })
    public void method2() {
        System.out.println("This is method 2");
    }

}


Java
```

执行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-1021352376\testng-customsuite.xml

This is method 1
This is method 2
PASSED: method1
PASSED: method2

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 91 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 117 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 7 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 1 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 150 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 3 ms


Shell
```

### 1.2. 如果`method1()`失败，则将跳过`method2()`。

创建一个Java文件： *App2.java*，其代码结构如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class App2 {

    // This test will be failed.
    @Test
    public void method1() {
        System.out.println("This is method 1");
        throw new RuntimeException();
    }

    @Test(dependsOnMethods = { "method1" })
    public void method2() {
        System.out.println("This is method 2");
    }

}


Java
```

执行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--1314180717\testng-customsuite.xml

This is method 1
FAILED: method1
java.lang.RuntimeException
    at com.yiibai.App2.method1(App2.java:11)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
    at java.lang.reflect.Method.invoke(Unknown Source)
    at org.testng.internal.MethodInvocationHelper.invokeMethod(MethodInvocationHelper.java:84)
    at org.testng.internal.Invoker.invokeMethod(Invoker.java:714)
    at org.testng.internal.Invoker.invokeTestMethod(Invoker.java:901)
    at org.testng.internal.Invoker.invokeTestMethods(Invoker.java:1231)
    at org.testng.internal.TestMethodWorker.invokeTestMethods(TestMethodWorker.java:127)
    at org.testng.internal.TestMethodWorker.run(TestMethodWorker.java:111)
    at org.testng.TestRunner.privateRun(TestRunner.java:767)
    at org.testng.TestRunner.run(TestRunner.java:617)
    at org.testng.SuiteRunner.runTest(SuiteRunner.java:334)
    at org.testng.SuiteRunner.runSequentially(SuiteRunner.java:329)
    at org.testng.SuiteRunner.privateRun(SuiteRunner.java:291)
    at org.testng.SuiteRunner.run(SuiteRunner.java:240)
    at org.testng.SuiteRunnerWorker.runSuite(SuiteRunnerWorker.java:52)
    at org.testng.SuiteRunnerWorker.run(SuiteRunnerWorker.java:86)
    at org.testng.TestNG.runSuitesSequentially(TestNG.java:1224)
    at org.testng.TestNG.runSuitesLocally(TestNG.java:1149)
    at org.testng.TestNG.run(TestNG.java:1057)
    at org.testng.remote.AbstractRemoteTestNG.run(AbstractRemoteTestNG.java:132)
    at org.testng.remote.RemoteTestNG.initAndRun(RemoteTestNG.java:230)
    at org.testng.remote.RemoteTestNG.main(RemoteTestNG.java:76)

SKIPPED: method2

===============================================
    Default test
    Tests run: 2, Failures: 1, Skips: 1
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 1, Skips: 1
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 13 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 63 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 17 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 72 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 72 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 5 ms


Shell
```

## 2. dependsOnGroups示例

下面我们创建几个测试用例来证明`dependsOnMethods`和`dependsOnGroups`的混合使用。 代码比较简单，参阅下面注释就能明白了。

创建一个Java文件： *TestServer.java*，其代码结构如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

//all methods of this class are belong to "deploy" group.
@Test(groups = "deploy")
public class TestServer {

    @Test
    public void deployServer() {
        System.out.println("Deploying Server...");
    }

    // Run this if deployServer() is passed.
    @Test(dependsOnMethods = "deployServer")
    public void deployBackUpServer() {
        System.out.println("Deploying Backup Server...");
    }

}


Java
```

创建一个Java文件： *TestDatabase.java*，其代码结构如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestDatabase {

    //belong to "db" group,
    //Run if all methods from "deploy" group are passed.
    @Test(groups="db", dependsOnGroups="deploy")
    public void initDB() {
        System.out.println("This is initDB()");
    }

    //belong to "db" group,
    //Run if "initDB" method is passed.
    @Test(dependsOnMethods = { "initDB" }, groups="db")
    public void testConnection() {
        System.out.println("This is testConnection()");
    }

}


Java
```

创建一个Java文件： *TestApp.java*，其代码结构如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestApp {

    //Run if all methods from "deploy" and "db" groups are passed.
    @Test(dependsOnGroups={"deploy","db"})
    public void method1() {
        System.out.println("This is method 1");
        //throw new RuntimeException();
    }

    //Run if method1() is passed.
    @Test(dependsOnMethods = { "method1" })
    public void method2() {
        System.out.println("This is method 2");
    }

}


Java
```

创建一个XML文件： *testng.xml*，其代码结构如下所示 -

```
<?xml version="1.0" encoding="UTF-8"?>
<suite name="TestDependency">

  <test name="TestCase1">

    <classes>
      <class
        name="com.yiibai.TestApp">
      </class>
      <class
        name="com.yiibai.TestDatabase">
      </class>
      <class
        name="com.yiibai.TestServer">
      </class>
    </classes>

  </test>

</suite>


XML
```

执行上面代码，得到以下结果 -

```
[TestNG] Running:
  F:\worksp\testng\DependOnTest\src\main\java\com\yiibai\testng.xml

Deploying Server...
Deploying Backup Server...
This is initDB()
This is testConnection()
This is method 1
This is method 2

===============================================
TestDependency
Total tests run: 6, Failures: 0, Skips: 0
===============================================


Shell
```

执行过程和时间如下 -

![img](http://www.yiibai.com/uploads/images/201705/0205/268170543_85571.png)