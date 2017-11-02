# TestNG预期异常测试

在本教程中，我们将演示如何使用TestNG expectedExceptions来测试代码中的预期异常抛出。

创建一个名称为 **ExpectedExceptionTest** 的 Maven 工程，其结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0205/987140554_73159.png)

## 1. 运行时异常

此示例显示如何测试运行时异常。 如果`divisionWithException()`方法抛出一个运行时异常 — `ArithmeticException`，它会获得通过。

创建一个测试文件：*TestRuntime.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.Test;

public class TestRuntime {

    @Test(expectedExceptions = ArithmeticException.class)
    public void divisionWithException() {
        int i = 1 / 0;
        System.out.println("After division the value of i is :"+ i);
    }

}


Java
```

运行上面代码，得到以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-717368002\testng-customsuite.xml

PASSED: divisionWithException

===============================================
    Default test
    Tests run: 1, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 1, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 0 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 47 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 0 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 15 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 31 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 0 ms


Shell
```

## 2. 检查异常

查看一个简单的业务对象，保存和更新方法，如果有错误，则抛出自定义检查的异常。

创建一个测试文件：*OrderBo.java* ，其代码如下所示 -

```
package com.yiibai;

public class OrderBo {

    public void save(Order order) throws OrderSaveException {

        if (order == null) {
            throw new OrderSaveException("Order is empty!");
        }

        // persist it

    }

    public void update(Order order) throws OrderUpdateException, OrderNotFoundException {

        if (order == null) {
            throw new OrderUpdateException("Order is empty!");
        }

        // if order is not available in database
        throw new OrderNotFoundException("Order is not exists");

    }

}


Java
```

测试预期异常的示例。创建一个主测试文件：*TestCheckedException.java* ，其代码如下所示 -

```
package com.yiibai;

import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;



public class TestCheckedException {

    OrderBo orderBo;
    Order data;

    @BeforeTest
    void setup() {
        orderBo = new OrderBo();

        data = new Order();
        data.setId(1000);
        data.setCreatedBy("maxsu");
    }

    @Test(expectedExceptions = OrderSaveException.class)
    public void throwIfOrderIsNull() throws OrderSaveException {
        orderBo.save(null);
    }

    /*
     * Example : Multiple expected exceptions Test is success if either of the
     * exception is thrown
     */
    @Test(expectedExceptions = { OrderUpdateException.class, OrderNotFoundException.class })
    public void throwIfOrderIsNotExists() throws OrderUpdateException, OrderNotFoundException {
        orderBo.update(data);
    }

}


Java
```

运行上述单元测试代码，最终将通过测试，得到如下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--154903203\testng-customsuite.xml

PASSED: throwIfOrderIsNotExists
PASSED: throwIfOrderIsNull

===============================================
    Default test
    Tests run: 2, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 2, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@1b40d5f0: 16 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@6ea6d14e: 109 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@4563e9ab: 16 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@2aaf7cc2: 31 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@45c8e616: 0 ms


Shell
```

示例中的其它几个类(`OrderNotFoundException`，`OrderSaveException`等)，请下载代码参考。