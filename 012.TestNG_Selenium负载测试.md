# TestNG+Selenium负载测试

在本教程中，我们将演示如何使用`@Test`属性`invocationCount`和`threadPoolSize`在网站上执行负载测试或压力测试。

使用的工具 ：

- TestNG 6.8.7
- Selenium 2.39.0
- Maven 3

我们使用`Selenium`库自动化浏览器来访问网站。创建一个用于测试的Maven项目：**TestngSelenium** 。

## 1. 项目依赖文件配置

获取TestNG和`Selenium`库。如下 `pom.xml` 所示 -

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.yiibai</groupId>
    <artifactId>TestngSelenium</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>TestngSelenium</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <testng.version>6.8.7</testng.version>
        <selenium.version>3.4.0</selenium.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>${selenium.version}</version>
        </dependency>
    </dependencies>
</project>


XML
```

## 2. [@Test](https://github.com/Test)(invocationCount =？)

这个`invocationCount`确定TestNG应该运行这个测试方法的次数。

**示例 2.1**

```
package com.yiibai;


import org.testng.annotations.Test;

public class TestRepeatThis {

    @Test(invocationCount = 10)
    public void repeatThis() {
        System.out.println("repeatThis " );
    }
}


Java
```

输出 - `repeatThis()`方法将运行`10`次，结果如下所示 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-1220889222\testng-customsuite.xml

repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis

===============================================
    Default test
    Tests run: 10, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 10, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@6442b0a6: 32 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@22f71333: 15 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@57829d67: 63 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@3d24753a: 109 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@6108b2d7: 0 ms


Shell
```

**示例2.2** - 使用Selenium打开Firefox浏览器并加载“yiibai.com”。 此测试是确保页面标题始终为“**易百教程™ - 专注于IT教程和实例**”。

创建一个类文件：**TestMultipleThreads.java**，其代码如下所示：

```
package com.yiibai;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.AssertJUnit;
import org.testng.annotations.Test;

public class TestMultipleThreads {

    @Test(invocationCount = 3)
    public void loadTestThisWebsite() {

        //System.setProperty("webdriver.chrome.driver", "chromedriver.exe");
        //WebDriver driver = new ChromeDriver();
//        System.setProperty("webdriver.firefox.bin", "D:\\Program Files (x86)\\Mozilla Firefox\\firefox.exe");
        WebDriver driver = new FirefoxDriver();  
        driver.get("http://www.yiibai.com");
        System.out.println("Page Title is " + driver.getTitle());
        AssertJUnit.assertEquals("Google", driver.getTitle());
        driver.quit();

    }
}


Java
```

输出结果如下 - 您会注意到Firefox浏览器将提示关闭`3`次。

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-1220889222\testng-customsuite.xml

repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
repeatThis 
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis
PASSED: repeatThis

===============================================
    Default test
    Tests run: 10, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 10, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@6442b0a6: 32 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@22f71333: 15 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@57829d67: 63 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@3d24753a: 109 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@6108b2d7: 0 ms


Shell
```

## 3. [@Test](https://github.com/Test)(invocationCount = ?, threadPoolSize = ?)

`threadPoolSize`属性告诉TestNG创建一个线程池以通过多个线程运行测试方法。 使用线程池，会大大降低测试方法的运行时间。

**示例3.1 **- 启动一个包含`3`个线程的线程池，并运行测试方法`3`次。

```
package com.yiibai;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.AssertJUnit;
import org.testng.annotations.Test;

public class TestMultipleThreads2 {

    @Test(invocationCount = 3, threadPoolSize = 3)
    public void testThreadPools() {

        System.out.printf("Thread Id : %s%n", Thread.currentThread().getId());

    }
}


Java
```

执行上面代码，输出以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-103220503\testng-customsuite.xml

[ThreadUtil] Starting executor timeOut:0ms workers:3 threadPoolSize:3
Thread Id : 13
Thread Id : 11
Thread Id : 12
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools

===============================================
    Default test
    Tests run: 3, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 3, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@6442b0a6: 19 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@22f71333: 5 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@57829d67: 50 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@3d24753a: 46 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@6108b2d7: 5 ms


Shell
```

**示例3.2 **- 启动包含`3`个线程的线程池，并运行测试方法`10`次。

```
package com.yiibai;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.AssertJUnit;
import org.testng.annotations.Test;

public class TestMultipleThreads3 {

    @Test(invocationCount = 10, threadPoolSize = 3)
    public void testThreadPools() {

        System.out.printf("Thread Id : %s%n", Thread.currentThread().getId());

    }
}


Java
```

执行上面代码，输出以下结果 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--548867586\testng-customsuite.xml

[ThreadUtil] Starting executor timeOut:0ms workers:10 threadPoolSize:3
Thread Id : 13
Thread Id : 11
Thread Id : 12
Thread Id : 11
Thread Id : 13
Thread Id : 12
Thread Id : 13
Thread Id : 11
Thread Id : 13
Thread Id : 11
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools
PASSED: testThreadPools

===============================================
    Default test
    Tests run: 10, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 10, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.XMLReporter@6442b0a6: 18 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@22f71333: 6 ms
[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@57829d67: 39 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 0 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@3d24753a: 105 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@6108b2d7: 11 ms


Shell
```

## 4. 负载测试示例

通过结合TestNG多线程和Selenium强大的浏览器自动化。 您可以创建一个简单而强大的负载测试，创建一个类文件：**TestMultipleThreadsLoad.java**，其代码如下所示：

```
Java
```

输出 - 以上测试将启动一个线程池`5`，并发送`100`个URL请求到指定的网站。

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse--404588393\testng-customsuite.xml

[ThreadUtil] Starting executor timeOut:0ms workers:100 threadPoolSize:5

[START] Thread Id : 14 is started!
[START] Thread Id : 11 is started!
[START] Thread Id : 13 is started!
[START] Thread Id : 12 is started!
[START] Thread Id : 15 is started!1493821695786    geckodriver    INFO    Listening on 127.0.0.1:45272
1493821695787    geckodriver    INFO    Listening on 127.0.0.1:41719
1493821695792    geckodriver    INFO    Listening on 127.0.0.1:44078
1493821695793    geckodriver    INFO    Listening on 127.0.0.1:20291
1493821695804    geckodriver    INFO    Listening on 127.0.0.1:43472
1493821696563    mozprofile::profile    INFO    Using profile path C:\Users\ADMINI~1\AppData\Local\Temp\rust_mozprofile.1bGOSGl5q8Ao
1493821696589    mozprofile::profile    INFO    Using profile path C:\Users\ADMINI~1\AppData\Local\Temp\rust_mozprofile.5CndLuoF3lLq
1493821696589    mozprofile::profile    INFO    Using profile path C:\Users\ADMINI~1\AppData\Local\Temp\rust_mozprofile.8UhBfB3XciVW
1493821696591    mozprofile::profile    INFO    Using profile path C:\Users\ADMINI~1\AppData\Local\Temp\rust_mozprofile.kGZmelveSBLV
1493821696592    geckodriver::marionette    INFO    Starting browser C:\Program Files (x86)\Mozilla Firefox\firefox.exe
1493821696594    mozprofile::profile    INFO    Using profile path C:\Users\ADMINI~1\AppData\Local\Temp\rust_mozprofile.duSn42IvmcXK
1493821696595    geckodriver::marionette    INFO    Starting browser C:\Program Files (x86)\Mozilla Firefox\firefox.exe
1493821696596    geckodriver::marionette    INFO    Starting browser C:\Program Files (x86)\Mozilla Firefox\firefox.exe
1493821696598    geckodriver::marionette    INFO    Starting browser C:\Program Files (x86)\Mozilla Firefox\firefox.exe
1493821696624    geckodriver::marionette    INFO    Starting browser C:\Program Files (x86)\Mozilla Firefox\firefox.exe
1493821696626    geckodriver::marionette    INFO    Connecting to Marionette on localhost:56795
1493821696627    geckodriver::marionette    INFO    Connecting to Marionette on localhost:56799
1493821696628    geckodriver::marionette    INFO    Connecting to Marionette on localhost:56796
1493821696631    geckodriver::marionette    INFO    Connecting to Marionette on localhost:56798
1493821696632    geckodriver::marionette    INFO    Connecting to Marionette on localhost:56797


Shell
```

### 一些问题

**问**：对于使用Selenium和TestNG的负载测试，为什么一次仅提示一个浏览器？
**答**：您的测试方法完成得太快，尝试放置`Thread.sleep(5000)`方法来延迟执行，现在您应该注意到多个浏览器同时提示(仅供演示用途)。