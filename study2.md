[toc]



# 深入理解abstract class 和 interface

## 从语法定义层面看

在面向对象领域，抽象类主要用来进行类型隐藏。我们可以构造出一个固定的一组行为的抽象描述，但是这组行为却能够有任意个可能的具体实现方式。

使用 abstract class 的方式定义 Demo 抽象类的方式如下：

```java
abstract class Demo ｛
    abstract void method1();
    abstract void method2();
   ...
｝
```

使用 interface 的方式定义 Demo 抽象类的方式如下：

```java
interface Demo {
    void method1();
    void method2();
   ...
}
```

在abstract class 方式中，抽象类Demo可以有自己的数据成员，也可以有非abstract的成员方法，而interface方式的实现中，Demo只能够有静态的不能被修改的数据成员，也就是必须是static final的，不过interface中一般不定义数据成员，所有的成员方法都是abstract的，某种意义上说，interface是一种特殊的abstract class。

## 从编程层面看



首先，抽象类在java语言中表示的是一继承关系，一个类只能使用一次继承关系。但是一个类却可以实现多个interface。

其次，在抽象类定义中可以赋予方法的默认行为，但是在interface定义中方法不能有默认行为

## 从设计理念层面看

上面主要从语法定义和编程的角度论述了 abstract class 和 interface 的区别，这些层面的区别是比较低层次的、非本质的。本小节将从另一个层面：abstract class 和 interface 所反映出的设计理念，来分析一下二者的区别。作者认为，从这个层面进行分析才能理解二者概念的本质所在。

前面已经提到过，abstarct class 在 Java 语言中体现了一种继承关系，要想使得继承关系合理，父类和派生类之间必须存在”is a”关系，即父类和派生类在概念本质上应该是相同的（参考文献〔3〕中有关于”is a”关系的大篇幅深入的论述，有兴趣的读者可以参考）。对于 interface 来说则不然，并不要求 interface 的实现者和 interface 定义在概念本质上是一致的，仅仅是实现了 interface 定义的契约而已。为了使论述便于理解，下面将通过一个简单的实例进行说明。

考虑这样一个例子，假设在我们的问题领域中有一个关于Door的抽象概念，该 Door 具有执行两个动作 open 和 close，此时我们可以通过 abstract class 或者 interface 来定义一个表示该抽象概念的类型，定义方式分别如下所示：

使用 abstract class 方式定义 Door：

```java
abstract class Door {
        abstract void open();
        abstract void close()；
}
```

使用 interface 方式定义 Door：

```java
interface Door {
        void open();
    void close();
}
```

其他具体的 Door 类型可以 extends 使用 abstract class 方式定义的 Door 或者 implements 使用 interface 方式定义的 Door。看起来好像使用 abstract class 和 interface 没有大的区别。

如果现在要求 Door 还要具有报警的功能。我们该如何设计针对该例子的类结构呢（在本例中，主要是为了展示 abstract class 和 interface 反映在设计理念上的区别，其他方面无关的问题都做了简化或者忽略）？下面将罗列出可能的解决方案，并从设计理念层面对这些不同的方案进行分析。

**解决方案一：**

简单的在 Door 的定义中增加一个 alarm 方法，如下：

```java
abstract class Door {
        abstract void open();
        abstract void close()；
        abstract void alarm();
}
```

或者

```java
interface Door {
        void open();
    void close();
    void alarm();
}
```

那么具有报警功能的 AlarmDoor 的定义方式如下：

```java
class AlarmDoor extends Door {
        void open() {... }
            void close() {... }
        void alarm() {... }
}
```

或者

```java
class AlarmDoor implements Door ｛
    void open() {... }
            void close() {... }
        void alarm() {... }
｝
```

这种方法违反了面向对象设计中的一个核心原则 ISP（Interface Segregation Priciple），在 Door 的定义中把 Door 概念本身固有的行为方法和另外一个概念”报警器”的行为方法混在了一起。这样引起的一个问题是那些仅仅依赖于 Door 这个概念的模块会因为”报警器”这个概念的改变（比如：修改 alarm 方法的参数）而改变，反之依然。

**解决方案二：**

既然 open、close 和 alarm 属于两个不同的概念，根据 ISP 原则应该把它们分别定义在代表这两个概念的抽象类中。定义方式有：这两个概念都使用 abstract class 方式定义；两个概念都使用 interface 方式定义；一个概念使用 abstract class 方式定义，另一个概念使用 interface 方式定义。

显然，由于 Java 语言不支持多重继承，所以两个概念都使用 abstract class 方式定义是不可行的。后面两种方式都是可行的，但是对于它们的选择却反映出对于问题领域中的概念本质的理解、对于设计意图的反映是否正确、合理。我们一一来分析、说明。

如果两个概念都使用 interface 方式来定义，那么就反映出两个问题：1、我们可能没有理解清楚问题领域，AlarmDoor 在概念本质上到底是Door还是报警器？2、如果我们对于问题领域的理解没有问题，比如：我们通过对于问题领域的分析发现 AlarmDoor 在概念本质上和 Door 是一致的，那么我们在实现时就没有能够正确的揭示我们的设计意图，因为在这两个概念的定义上（均使用 interface 方式定义）反映不出上述含义。

如果我们对于问题领域的理解是：AlarmDoor 在概念本质上是 Door，同时它有具有报警的功能。我们该如何来设计、实现来明确的反映出我们的意思呢？前面已经说过，abstract class 在 Java 语言中表示一种继承关系，而继承关系在本质上是”is a”关系。所以对于 Door 这个概念，我们应该使用 abstarct class 方式来定义。另外，AlarmDoor 又具有报警功能，说明它又能够完成报警概念中定义的行为，所以报警概念可以通过 interface 方式定义。如下所示：

```java
abstract class Door {
        abstract void open();
        abstract void close()；
    }
interface Alarm {
    void alarm();
}
class AlarmDoor extends Door implements Alarm {
    void open() {... }
    void close() {... }
       void alarm() {... }
}
```

这种实现方式基本上能够明确的反映出我们对于问题领域的理解，正确的揭示我们的设计意图。其实 abstract class 表示的是”is a”关系，interface 表示的是”like a”关系，大家在选择时可以作为一个依据，当然这是建立在对问题领域的理解上的，比如：如果我们认为AlarmDoor在概念本质上是报警器，同时又具有 Door 的功能，那么上述的定义方式就要反过来了。

# Spring MVC

基于Servlet API构建的原始Web框架，并且围绕着前端控制器模式进行设计

## DispatcherServlet

SpringMVC 围绕着前端控制器模式进行设计，`DispatcherServlet`提供了用于请求处理的共享算法，并且和其他Servlet一样，需要根据Servlet规范通过使用Java配置或者在web.xml中进行声明和map。反过来，`DispatcherServlet`使用Spring配置发现请求Map，视图解析，异常处理等所需的委托组件

以下 Java 配置示例注册并初始化`DispatcherServlet`，它由 Servlet 容器自动检测(请参见[Servlet Config](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-container-config))：

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletCxt) {

        // Load Spring web application configuration
        AnnotationConfigWebApplicationContext ac = new AnnotationConfigWebApplicationContext();
        ac.register(AppConfig.class);
        ac.refresh();

        // Create and register the DispatcherServlet
        DispatcherServlet servlet = new DispatcherServlet(ac);
        ServletRegistration.Dynamic registration = servletCxt.addServlet("app", servlet);
        registration.setLoadOnStartup(1);
        registration.addMapping("/app/*");
    }
}
```

### 上下文层次

`DispatcherServlet`期望其自己的配置为`WebApplicationContext`(纯`ApplicationContext`的 extensions)。 `WebApplicationContext`具有到`ServletContext`和与其关联的`Servlet`的链接。它还绑定到`ServletContext`，以便应用程序可以在`RequestContextUtils`上使用静态方法来查找`WebApplicationContext`(如果需要访问它们)。

对于许多应用程序来说，只有一个`WebApplicationContext`很简单并且足够。也可能具有上下文层次结构，其中一个根`WebApplicationContext`跨多个`DispatcherServlet`(或其他`Servlet`)实例共享，每个实例都有其自己的子`WebApplicationContext`配置。有关上下文层次结构功能的更多信息，请参见[ApplicationContext 的其他功能](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/core.html#context-introduction)。

根`WebApplicationContext`通常包含需要在多个`Servlet`实例之间共享的基础结构 Bean，例如数据存储库和业务服务。这些 Bean 是有效继承的，可以在 Servlet 特定子`WebApplicationContext`中重写(即重新声明)，该子`WebApplicationContext`通常包含给定`Servlet`本地的 Bean。下图显示了这种关系：

![mvc 上下文层次结构](https://www.docs4dev.com/images/spring-framework/5.1.3.RELEASE/mvc-context-hierarchy.png)

以下示例配置`WebApplicationContext`层次结构：

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class<?>[] { RootConfig.class };
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] { App1Config.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/app1/*" };
    }
}
```

Tip：

如果不需要应用程序上下文层次结构，则应用程序可以通过`getServletConfigClasses()`的`getRootConfigClasses()`和`null`返回所有配置。

以下示例显示了`web.xml`等效项：

```xm
<web-app>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/root-context.xml</param-value>
    </context-param>

    <servlet>
        <servlet-name>app1</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/app1-context.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>app1</servlet-name>
        <url-pattern>/app1/*</url-pattern>
    </servlet-mapping>

</web-app>
```

Tip：

如果不需要应用程序上下文层次结构，则应用程序可以仅配置“根”上下文，并将`contextConfigLocation` Servlet 参数保留为空。

![image-20201119103421637](C:\Users\EDZ\AppData\Roaming\Typora\typora-user-images\image-20201119103421637.png)

### 特殊 bean 类

[与 Spring WebFlux 中的相同](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web-reactive.html#webflux-special-bean-types)

`DispatcherServlet`委托特殊 bean 处理请求并呈现适当的响应。所谓“特殊 bean”，是指实现 WebFlux 框架 Contract 的 SpringManagement 的`Object`实例。这些通常带有内置 Contract，但是您可以自定义它们的属性并扩展或替换它们。

下表列出了`DispatcherHandler`检测到的特殊 bean：

| **Bean type**                                                | **Explanation**                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| HandlerMapping处理器映射器                                   | 将请求与[interceptors](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-handlermapping-interceptor)列表一起映射到处理程序，以进行预处理和后期处理。映射基于某些条件，具体取决于`HandlerMapping`实现。`HandlerMapping`的两个主要实现是`RequestMappingHandlerMapping`(支持`@RequestMapping`带注解的方法)和`SimpleUrlHandlerMapping`(将 URI 路径模式的显式注册维护到处理程序)。 |
| HandlerAdapter处理器适配器                                   | 帮助DispatcherServlet调用映射到请求的处理程序，而不管处理程序实际是如何调用的。例如，调用带注释的控制器需要解析注释。HandlerAdapter的主要目的是保护DispatcherServlet不受这些细节的影响。 |
| [HandlerExceptionResolver](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-exceptionhandlers) | 解决异常的策略，可能将它们映射到处理程序、HTML错误视图或其他目标。 |
| [`ViewResolver`](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-viewresolver)视图解析器 | 解析从处理程序返回到实际`View`的基于逻辑`String`的视图名称，使用该视图名称呈现给响应的实际视图。。参见[View Resolution](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-viewresolver)和[View Technologies](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-view)。 |
| [`LocaleResolver`](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-localeresolver), [LocaleContextResolver](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-timezone) | 解析 Client 端正在使用的`Locale`以及可能的时区，以便能够提供国际化的视图。参见[Locale](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-localeresolver)。 |
| [ThemeResolve](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-themeresolver) | 解决 Web 应用程序可以使用的主题，例如提供个性化的布局。参见[Themes](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-themeresolver)。 |
| [MultipartResolver](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-multipart) | 用于借助一些 Multipart 解析库来分析 Multipart 请求(例如，浏览器表单文件上载)的抽象。参见[Multipart Resolver](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-multipart)。 |
| [FlashMapManager](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-flash-attributes) | 存储和检索“input”和“output”管理FlashMap，它们可用于将属性从一个请求传递到另一个请求，通常是通过重定向。参见[Flash Attributes](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-flash-attributes)。 |

###  Web MVC 配置

应用程序可以声明处理请求所必需的[特殊 bean 类](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-servlet-special-bean-types)中列出的基础结构 bean。 `DispatcherServlet`为每个特殊 bean 检查`WebApplicationContext`。如果没有匹配的 Bean 类型，它将使用[DispatcherServlet.properties](https://github.com/spring-projects/spring-framework/blob/master/spring-webmvc/src/main/resources/org/springframework/web/servlet/DispatcherServlet.properties)中列出的默认类型。

在大多数情况下，[MVC Config](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-config)是最佳起点。它使用 Java 或 XML 声明所需的 bean，并提供更高级别的配置回调 API 对其进行自定义。

Note：

Spring Boot 依靠 MVC Java 配置来配置 Spring MVC，并提供许多额外的方便选项。

### Servlet 配置

在 Servlet 3.0 环境中，您可以选择以编程方式配置 Servlet 容器，以替代方式或与`web.xml`文件结合使用。下面的示例注册一个`DispatcherServlet`：

```java
import org.springframework.web.WebApplicationInitializer;

public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext container) {
        XmlWebApplicationContext appContext = new XmlWebApplicationContext();
        appContext.setConfigLocation("/WEB-INF/spring/dispatcher-config.xml");

        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet(appContext));
        registration.setLoadOnStartup(1);
        registration.addMapping("/");
    }
}
```

`WebApplicationInitializer`是 Spring MVC 提供的接口，可确保检测到您的实现并将其自动用于初始化任何 Servlet 3 容器。名为`AbstractDispatcherServletInitializer`的`WebApplicationInitializer`的抽象 Base Class 实现通过覆盖方法来指定 servletMap 和`DispatcherServlet`配置的位置，从而使`DispatcherServlet`的注册更加容易。

对于使用基于 Java 的 Spring 配置的应用程序，建议这样做，如以下示例所示：

```
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[] { MyWebConfig.class };
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }
}
```

如果使用基于 XML 的 Spring 配置，则应直接从`AbstractDispatcherServletInitializer`扩展，如以下示例所示：

```
public class MyWebAppInitializer extends AbstractDispatcherServletInitializer {

    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }

    @Override
    protected WebApplicationContext createServletApplicationContext() {
        XmlWebApplicationContext cxt = new XmlWebApplicationContext();
        cxt.setConfigLocation("/WEB-INF/spring/dispatcher-config.xml");
        return cxt;
    }

    @Override
    protected String[] getServletMappings() {
        return new String[] { "/" };
    }
}
```

`AbstractDispatcherServletInitializer`还提供了一种方便的方法来添加`Filter`实例，并将它们自动 Map 到`DispatcherServlet`，如以下示例所示：

```
public class MyWebAppInitializer extends AbstractDispatcherServletInitializer {

    // ...

    @Override
    protected Filter[] getServletFilters() {
        return new Filter[] {
            new HiddenHttpMethodFilter(), new CharacterEncodingFilter() };
    }
}
```

每个过滤器都会根据其具体类型添加一个默认名称，并自动 Map 到`DispatcherServlet`。

`AbstractDispatcherServletInitializer`的受`isAsyncSupported`保护的方法提供了一个位置，以对`DispatcherServlet`及其 Map 的所有过滤器启用异步支持。默认情况下，此标志设置为`true`。

最后，如果您需要进一步自定义`DispatcherServlet`本身，则可以覆盖`createDispatcherServlet`方法。

### Processing

`DispatcherServlet`处理请求的方式如下：

- 搜索`WebApplicationContext`并将其绑定在请求中，作为控制器和流程中其他元素可以使用的属性。默认情况下，它是在`DispatcherServlet.WEB_APPLICATION_CONTEXT_ATTRIBUTE`键下绑定的。
- locale resolver语言环境解析器绑定到请求，以使流程中的元素解析在处理请求(呈现视图，准备数据等)时要使用的语言环境。如果不需要语言环境解析，则不需要语言环境解析器。
- theme resolver主题解析器绑定到请求，以使诸如视图之类的元素确定要使用的主题。如果不使用主题，则可以忽略它。
- 如果指定 Multipart file resolver 文件解析器，则将检查请求中是否有 Multipart。如果找到 Multipart，则将请求包装在`MultipartHttpServletRequest`中，以供流程中的其他元素进一步处理。有关 Multipart 处理的更多信息，请参见[Multipart Resolver](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-multipart)。
- 搜索适当的处理程序。如果找到处理程序，则执行与处理程序(预处理器，后处理器和控制器)关联的执行链，以准备模型或渲染。或者，对于带注解 的控制器，可以呈现响应(在`HandlerAdapter`内)，而不返回视图。
- 如果返回模型，则呈现视图。如果没有返回任何模型(可能是由于预处理器或后处理器拦截了该请求，可能出于安全原因)，则不会呈现任何视图，因为该请求可能已经被满足。

`WebApplicationContext`中声明的`HandlerExceptionResolver` bean 用于解决在请求处理期间引发的异常。这些异常解析器允许定制逻辑以解决异常。有关更多详细信息，请参见[Exceptions](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-exceptionhandlers)。

Spring `DispatcherServlet`还支持 Servlet API 指定的`last-modification-date`的返回。确定特定请求的最后修改日期的过程很简单：`DispatcherServlet`查找适当的处理程序 Map 并测试找到的处理程序是否实现`LastModified`接口。如果是这样，则`LastModified`接口的`long getLastModified(request)`方法的值返回给 Client 端。

您可以通过向`web.xml`文件中的 Servlet 声明中添加 Servlet 初始化参数(`init-param`元素)来自定义`DispatcherServlet`实例。下表列出了受支持的参数：

*表 1. DispatcherServlet 初始化参数*

| Parameter                        | Explanation                                                  |
| -------------------------------- | ------------------------------------------------------------ |
| `contextClass`                   | 实现`ConfigurableWebApplicationContext`的类，将由此 Servlet 实例化并在本地配置。默认情况下，使用`XmlWebApplicationContext`。 |
| `contextConfigLocation`          | 传递给上下文实例的字符串(由`contextClass`指定)，以指示可以在哪里找到上下文。该字符串可能包含多个字符串(使用逗号作为分隔符)以支持多个上下文。对于具有两次定义的 bean 的多个上下文位置，以最新位置为准。 |
| `namespace`                      | `WebApplicationContext`的命名空间。默认为`[servlet-name]-servlet`。 |
| `throwExceptionIfNoHandlerFound` | 在找不到请求处理程序时是否抛出`NoHandlerFoundException`。然后可以使用`HandlerExceptionResolver`捕获异常(例如，通过使用`@ExceptionHandler`控制器方法)，然后将其作为其他任何异常进行处理。 |


默认情况下，它设置为`false`，在这种情况下`DispatcherServlet`将响应状态设置为 404(NOT_FOUND)，而不会引发异常。
请注意，如果还配置了[默认 servlet 处理](https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#mvc-default-servlet-handler)，则始终将未解决的请求转发到默认 servlet，并且永远不会引发 404.