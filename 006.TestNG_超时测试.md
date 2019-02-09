# TestNG超时测试

在本教程中，我们将演示如何在**TestNG**中执行超时测试。 “超时”表示如果单元测试花费的时间超过指定的毫秒数，那么**TestNG**将会中止它并将其标记为失败。

“超时”也可用于性能测试，以确保方法在合理的时间内返回。

创建一个名称为：*TimeoutTest* 的 Maven 项目，其结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/121150524_43652.png)

创建一个测试类：*TestTimeout.java*，其代码如下 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestTimeout {

    @Test(timeOut = 5000) // time in mulliseconds
    public void testThisShouldPass() throws InterruptedException {
        Thread.sleep(4000);
    }

    @Test(timeOut = 1000)
    public void testThisShouldFail() {
        while (true){
            // do nothing
        }

    }

}


Java
```

执行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--1892041198\testng-customsuite.xml

PASSED: testThisShouldPass
FAILED: testThisShouldFail
org.testng.internal.thread.ThreadTimeoutException: Method org.testng.internal.TestNGMethod.testThisShouldFail() didn't finish within the time-out 1000
    at com.yiibai.TestTimeout.testThisShouldFail(TestTimeout.java:14)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(Unknown Source)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(Unknown Source)
    at java.lang.reflect.Method.invoke(Unknown Source)
    at org.testng.internal.MethodInvocationHelper.invokeMethod(MethodInvocationHelper.java:84)
    at org.testng.internal.InvokeMethodRunnable.runOne(InvokeMethodRunnable.java:46)
    at org.testng.internal.InvokeMethodRunnable.run(InvokeMethodRunnable.java:37)
    at java.util.concurrent.Executors$RunnableAdapter.call(Unknown Source)
    at java.util.concurrent.FutureTask.run(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
    at java.lang.Thread.run(Unknown Source)


===============================================
    Default test
    Tests run: 2, Failures: 1, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 1, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 12 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 38 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 8 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 15 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 57 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 4 ms

```