# YiibaiTestNG

测试是检查应用程序的功能的过程是否按要求工作，在开发人员层面进行单元测试，在采取适当措施来测试每一个实体（类或方法）以确保最终产品符合要求。单元测试是非常必要的，这是软件公司向他们的客户提供高质量的软件产品必要前提。

JUnit让开发人员了解测试的实用性，尤其是在单元测试这一模块上比任何其他测试框架都要简单明了。凭借一个相当简单，务实，严谨的架构，JUnit已经能够“感染”了一大批开发人员。 有关JUnit的特点，可以看看[Junit教程](http://www.yiibai.com/junit)了解。

**JUnit缺点：**

- 最初的设计，使用于单元测试，现在只用于各种测试。
- 不能依赖测试
- 配置控制欠佳(安装/拆卸)
- 侵入性(强制扩展类，并以某种方式命名方法)
- 静态编程模型(不必要的重新编译)
- 不适合管理复杂项目应用，JUnit复杂项目中测试非常棘手。

## TestNG是什么?

TestNG按照官方的定义：

**TestNG**是一个测试框架，其灵感来自JUnit和NUnit，但引入了一些新的功能，使其功能更强大，使用更方便。

TestNG是一个开源自动化测试框架;TestNG表示**下一代**(**N**ext **G**eneration的首字母)。 TestNG类似于JUnit(特别是JUnit 4)，但它不是JUnit框架的扩展。它的灵感来源于JUnit。它的目的是优于JUnit，尤其是在用于测试集成多类时。 TestNG的创始人是**Cedric Beust**(塞德里克·博伊斯特)。

TestNG消除了大部分的旧框架的限制，使开发人员能够编写更加灵活和强大的测试。 因为它在很大程度上借鉴了Java注解(JDK5.0引入的)来定义测试，它也可以显示如何使用这个新功能在真实的Java语言生产环境中。

**TestNG的特点**

- 注解
- TestNG使用Java和面向对象的功能
- 支持综合类测试(例如，默认情况下，不用创建一个新的测试每个测试方法的类的实例)
- 独立的编译时测试代码和运行时配置/数据信息
- 灵活的运行时配置
- 主要介绍“测试组”。当编译测试，只要要求`TestNG`运行所有的“前端”的测试，或“快”，“慢”，“数据库”等
- 支持依赖测试方法，并行测试，负载测试，局部故障
- 灵活的插件API
- 支持多线程测试

TestNG(**N**ext **G**eneration)是一个测试框架，它受到JUnit和NUnit的启发，而引入了许多新的创新功能，如依赖测试，分组概念，使测试更强大，更容易做到。 它旨在涵盖所有类别的测试：单元，功能，端到端，集成等…

## TestNG教程目录

- [TestNG Hello World示例](http://www.yiibai.com/testng/hello-world-example.html) - 开始使用TestNG，创建一个简单的测试用例以及如何执行它。
- [TestNG配置注释](http://www.yiibai.com/testng/configuration-annotations.html) - 此示例演示TestNG中支持的配置注释列表。
- [TestNG预期异常测试](http://www.yiibai.com/testng/expected-exception-test.html) - 这个例子演示了如何做异常测试 - [@Test](https://github.com/Test)(expectedExceptions =？)。
- [TestNG忽略测试](http://www.yiibai.com/testng/ignore-test.html) - 此示例演示如何启用和禁用测试方法 - `@Test(enabled = true)`。
- [TestNG超时测试](http://www.yiibai.com/testng/timeout-test.html) - 确保测试方法必须在指定时间内完成 - `@Test(timeOut = 5000)`。
- [TestNG分组测试](http://www.yiibai.com/testng/testng-groups.html) - 此示例演示如何进行组测试 - `@Test(groups =？)`，`@Test(dependsOnGroups？？)`。
- [TestNG套件测试](http://www.yiibai.com/testng/suite-test.html) - 此示例演示如何使用`testng.xml`运行多个测试类。
- [TestNG依赖性测试](http://www.yiibai.com/testng/dependency_test.html) - 此示例演示如何使用`dependOnMethods`和`dependsOnGroups`来实现依赖性测试。
- [TestNG参数测试(XML和DataProvider)](http://www.yiibai.com/testng/parameterized-test.html) - 此示例演示如何使用XML或[@DataProvider](https://github.com/DataProvider)将参数传递到测试方法中。
- [TestNG参数测试(DataProvider)](http://www.yiibai.com/testng/parameter-testing.html) - 另一个[@DataProvider](https://github.com/DataProvider)示例。
- [TestNG + Selenium](http://www.yiibai.com/testng/testng-selenium-load-testing.html) - 负载测试 - 此示例演示如何使用Selenium在网站上执行负载测试。
- [TestNG + Spring集成示例](http://www.yiibai.com/testng/testng-spring-integration.html) - 此示例演示如何使用TestNG测试Spring组件。
- [JUnit 4 Vs TestNG比较](http://www.yiibai.com/testng/junit-vs-testng-comparison.html) - JUnit 4和TestNG之间详细功能的比较。