# Hello World

按照惯例，我们来编写一个最简单的 Web 项目。当我们访问项目时，界面会打印出 “Hello World”字样。

## 编写项目构建信息

我们复制我们在上一节用到的样例程序“initializr-start”，在该项目上做一点小变更，就能生成一个新项目的构建信息。

我们打开`build.gradle` 文件，做了如下变更。

### 1. 修改项目的名称

修改项目的名称为“hello-world”：

```
jar {
	baseName = 'hello-world'
	version = '0.0.1-SNAPSHOT'
}
```

### 2. 修改项目的仓库地址

为了加快构建速度，我们自定义了一个国内的仓库镜像地址：

```
repositories {
	maven {
        url 'http://maven.aliyun.com/nexus/content/groups/public/'
    }
}
```

好了，我们尝试执行 `gradle build`来对 “hello-world” 程序进行构建。如果构建成功，则说明构建信息一切正常。


## 编写程序代码

好了，现在终于可以进入编写代码的时间了。我们进入 src 目录下，我们应该能够看到 `com.waylau.spring.boot`包以及 `InitializrStartApplication.java` 文件。为了规范，我们将该包名改为 `com.waylau.spring.boot.hello`，将`InitializrStartApplication.java`更名为`Application.java`

## 1. 观察 Application.java

打开 Application.java 文件，我们首先看到的是`@SpringBootApplication`注解。对于经常使用 Spring 用户而言，
`@SpringBootApplication`注解等同于使用 `@Configuration`、 `@EnableAutoConfiguration` 和 `@ComponentScan` 的默认属性。

## 2. 编写控制器 HelloController

创建`com.waylau.spring.boot.hello.controller`包，用于放置控制器类。

HelloController.java 的代码非常简单。当请求到`/hello` 路径时，将会响应`"Hello World!`字样的字符串。代码如下：

```java
@RestController
public class HelloController {

	@RequestMapping("/hello")
	public String hello() {
	    return "Hello World! Welcome to visit waylau.com!";
	}
}
```

## 编写测试用例

我们进入 test 目录下，将`com.waylau.spring.boot`包名改为`com.waylau.spring.boot.hello`，并将 `InitializrStartApplicationTests.java` 更名为`ApplicationTests.java`文件。为

### 1. 编写 HelloControllerTest.java 测试类

对源程序一一对应，我们创建`com.waylau.spring.boot.hello.controller`包，用于放置控制器的测试类。

测试类 HelloControllerTest.java 代码如下：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class HelloControllerTest {

	@Autowired
    private MockMvc mockMvc;
	
    @Test
    public void testHello() throws Exception {
    	mockMvc.perform(MockMvcRequestBuilders.get("/hello").accept(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(content().string(equalTo("Hello World! Welcome to visit waylau.com!")));
    }
}
```

### 2. 运行测试类

用 JUnit 运行该测试，绿色代码测试通过。

## 运行程序

### 1. 编译程序

执行`gradle build`来对 “hello-world” 程序进行构建。

### 2. 访问程序

在浏览器，我们访问<http://localhost:8080/hello> ，可以到页面打印出了“Hello World! Welcome to visit waylau.com!”字样。

