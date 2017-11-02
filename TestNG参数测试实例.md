# TestNG参数测试实例

另一个TestNG参数测试示例，是使用`@DataProvider`注解。

## 1. CharUtil类

创建一个将字符转换成ASCII或者副词的类，如何使用TestNG来做单元测试？

打开 **Eclipse** 创建一个 *Maven* 工程： **ParameterTesting**，其目录结构如下所示 -

![img](http://www.yiibai.com/uploads/images/201705/0305/530140526_89335.png)

类文件：*CharUtils.java* 的代码如下 -

```
package com.yiibai;

/**
 * Character Utility class
 *
 * @author yiibai
 *
 */
public class CharUtils {
    /**
     * Convert the characters to ASCII value
     *
     * @param character character
     * @return ASCII value
     */
    public static int CharToASCII(final char character) {
        return (int) character;
    }

    /**
     * Convert the ASCII value to character
     *
     * @param ascii ascii value
     * @return character value
     */
    public static char ASCIIToChar(final int ascii) {
        return (char) ascii;
    }
}


Java
```

## 2. TestNG [@DataProvider](https://github.com/DataProvider)示例

要测试它，创建一个接受两个参数(字符和预期ASCII)的[@Test](https://github.com/Test)方法，并且测试数据从数据提供者传递。

创建一个类文件：*CharUtilsTest.java* 的代码如下 -

```
package com.yiibai;

import org.testng.Assert;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;
/**
 * Character Utils Testing
 * @author 
 *
 */
public class CharUtilsTest {

    @DataProvider
    public Object[][] ValidDataProvider() {
        return new Object[][]{
            { 'A', 65 },{ 'a', 97 },
            { 'B', 66 },{ 'b', 98 },
            { 'C', 67 },{ 'c', 99 },
            { 'D', 68 },{ 'd', 100 },
            { 'Z', 90 },{ 'z', 122 },
            { '1', 49 },{ '9', 57 }
        };
    }

    @Test(dataProvider = "ValidDataProvider")
    public void CharToASCIITest(final char character, final int ascii) {

           int result = CharUtils.CharToASCII(character);
           Assert.assertEquals(result, ascii);

    }

    @Test(dataProvider = "ValidDataProvider")
    public void ASCIIToCharTest(final char character, final int ascii) {

           char result = CharUtils.ASCIIToChar(ascii);
           Assert.assertEquals(result, character);

    }
}


Java
```

执行上面测试类，得到结果如下 -

```
[TestNG] Running:
  C:\Users\Administrator\AppData\Local\Temp\testng-eclipse-85008796\testng-customsuite.xml

PASSED: ASCIIToCharTest(A, 65)
PASSED: ASCIIToCharTest(a, 97)
PASSED: ASCIIToCharTest(B, 66)
PASSED: ASCIIToCharTest(b, 98)
PASSED: ASCIIToCharTest(C, 67)
PASSED: ASCIIToCharTest(c, 99)
PASSED: ASCIIToCharTest(D, 68)
PASSED: ASCIIToCharTest(d, 100)
PASSED: ASCIIToCharTest(Z, 90)
PASSED: ASCIIToCharTest(z, 122)
PASSED: ASCIIToCharTest(1, 49)
PASSED: ASCIIToCharTest(9, 57)
PASSED: CharToASCIITest(A, 65)
PASSED: CharToASCIITest(a, 97)
PASSED: CharToASCIITest(B, 66)
PASSED: CharToASCIITest(b, 98)
PASSED: CharToASCIITest(C, 67)
PASSED: CharToASCIITest(c, 99)
PASSED: CharToASCIITest(D, 68)
PASSED: CharToASCIITest(d, 100)
PASSED: CharToASCIITest(Z, 90)
PASSED: CharToASCIITest(z, 122)
PASSED: CharToASCIITest(1, 49)
PASSED: CharToASCIITest(9, 57)

===============================================
    Default test
    Tests run: 24, Failures: 0, Skips: 0
===============================================


===============================================
Default suite
Total tests run: 24, Failures: 0, Skips: 0
===============================================

[TestNG] Time taken by org.testng.reporters.SuiteHTMLReporter@721e0f4f: 214 ms
[TestNG] Time taken by org.testng.reporters.JUnitReportReporter@5e025e70: 57 ms
[TestNG] Time taken by [FailedReporter passed=0 failed=0 skipped=0]: 1 ms
[TestNG] Time taken by org.testng.reporters.EmailableReporter2@71c7db30: 13 ms
[TestNG] Time taken by org.testng.reporters.XMLReporter@23223dd8: 21 ms
[TestNG] Time taken by org.testng.reporters.jq.Main@6833ce2c: 80 ms

```