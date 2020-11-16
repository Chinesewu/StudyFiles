# Spring Framework

中文文档链接：

https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web.html#webmvc-client

官方文档链接：

https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc

## REST Clients

### RestTemplate

#### 概括

简化了HTTP请求以及处响应的过程，并且支持REST

#### 参考链接

1）https://www.cnblogs.com/caolei1108/p/6169950.html

2）http://www.54tianzhisheng.cn/2017/12/03/RestTemplate/

3）https://segmentfault.com/a/1190000011093597

4) https://www.cnblogs.com/yadongliang/p/13515319.html#_label1_0

5) https://www.imooc.com/article/44796



#### 对外开放接口

1) GET :

​	getForObject()  执行get请求

​	getForEntity

2)POST :

​	postForLocation

​	postForObject  执行post请求



3) DELETE

​	delete

4) HEAD

​	headForHeaders

5) OPTIONS

​	optionsForAllow

6) put

​	put

7) any

​	exchange

​	execute



### WebClient

#### 概述

WebClient是从Spring WebFlux 5.0版本开始提供的一个非阻塞的基于响应式编程的进行Http请求的客户端工具。它的响应式编程的基于Reactor的。WebClient中提供了标准Http请求方式对应的get、post、put、delete等方法，可以用来发起相应的请求。

#### 参考链接

1）https://www.docs4dev.com/docs/zh/spring-framework/5.1.3.RELEASE/reference/web-reactive.html#webflux-client

2）https://www.cnblogs.com/grasp/p/12179906.html



## MVC

### 什么是MVC

MVC(Model View Controller)是一种软件设计的**框架模式**，它采用**模型(Model)-视图(View)-控制器(controller)的方法把业务逻辑、数据与界面显示分离。把众多的业务逻辑聚集到一个部件里面**，当然这种比较官方的解释是不能让我们足够清晰的理解什么是MVC的。**用通俗的话来讲，MVC的理念就是把数据处理、数据展示(界面)和程序/用户的交互三者分离开的一种编程模式**。

 注意！MVC不是设计模式！

​    MVC框架模式是一种复合模式，MVC的三个核心部件分别是

​    1：**Model(模型)：所有的用户数据、状态以及程序逻辑**，独立于视图和控制器

​    2：**View(视图)：呈现模型，类似于Web程序中的界面**，视图会从模型中拿到需要展现的状态以及数据，对于相同的数据可以有多种不同的显示形式(视图)

​    3：**Controller(控制器)：负责获取用户的输入信息，进行解析并反馈给模型**，通常情况下一个视图具有一个控制器

### 为什么要使用MVC

程序通过将M(Model)和V(View)的代码分离，实现了前后端代码的分离，会带来几个好处

​    1：**可以使同一个程序使用不同的表现形式**，如果控制器反馈给模型的数据发生了变化，那么模型将及时通知有关的视图，视图会对应的刷新自己所展现的内容

​    2：因为模型是独立于视图的，所以**模型可复用**，模型可以独立的移植到别的地方继续使用

​    3：**前后端的代码分离**，使项目开发的分工更加明确，程序的测试更加简便，提高开发效率

​    下图是MVC的各个组件之间的关系以及功能图

![img](https://img-blog.csdn.net/20180810080446240?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDQ5NTE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

​    其实控制器的功能类似于一个中转站，会决定调用那个模型去处理用户请求以及调用哪个视图去呈现给用户

### JavaWeb中MVC模式的应用

​    在JavaWeb程序中，MVC框架模式是经常用到的，举一个Web程序的结构可以更好的理解MVC的理念

​    V：**View视图，Web程序中指用户可以看到的并可以与之进行数据交互的界面**，比如一个Html网页界面，或者某些客户端的界面，在前面讲过，MVC可以为程序处理很多不同的视图，用户在视图中进行输出数据以及一系列操作，注意：视图中不会发生数据的处理操作。

​    M：**Model模型：进行所有数据的处理工作，模型返回的数据是中立的，和数据格式无关**，一个模型可以为多个视图来提供数据，所以模型的代码重复性比较低

​    C：**Controller控制器：负责接受用户的输入，并且调用模型和视图去完成用户的需求**，控制器不会输出也不会做出任何处理，只会接受请求并调用模型构件去处理用户的请求，然后在确定用哪个视图去显示返回的数据

### Web程序中MVC模式的优点

​    **耦合性低**：视图(页面)和业务层(数据处理)分离，一个应用的业务流程或者业务规则的改变只需要改动MVC中的模型即可，不会影响到控制器与视图

​    **部署快，成本低**：MVC使开发和维护用户接口的技术含量降低。使用MVC模式使开发时间得到相当大的缩减，它使程序员（Java开发人员）集中精力于业务逻辑，界面程序员（HTML和JSP开发人员）集中精力于表现形式上

​    **可维护性高**：分离视图层和业务逻辑层也使得WEB应用更易于维护和修改

### Web程序中MVC模式的缺点

​    **调试困难**：因为模型和视图要严格的分离，这样也给调试应用程序带来了一定的困难，每个构件在使用之前都需要经过彻底的测试

​    **不适合小型，中等规模的应用程序**：在一个中小型的应用程序中，强制性的使用MVC进行开发，往往会花费大量时间，并且不能体现MVC的优势，同时会使开发变得繁琐

​    **增加系统结构和实现的复杂性**：对于简单的界面，严格遵循MVC，使模型、视图与控制器分离，会增加结构的复杂性，并可能产生过多的更新操作，降低运行效率

​    **视图与控制器间的过于紧密的连接并且降低了视图对模型数据的访问**：视图与控制器是相互分离，但却是联系紧密的部件，视图没有控制器的存在，其应用是很有限的，反之亦然，这样就妨碍了他们的独立重用。依据模型操作接口的不同，视图可能需要多次调用才能获得足够的显示数据。对未变化数据的不必要的频繁访问，也将损害操作性能

## Spring MVC框架



### Spring MVC简介及特点

​    Spring MVC采用了松散耦合的可插拔组件结构，比其他的MVC框架更具有灵活性和扩展性，Spring MVC通过使用一套注解，使一个Java类成为前端控制器(Controller)，不需要实现任何接口，同时，Spring MVC支持RES形式的URL请求，除此之外，Spring MVC在在数据绑定、视图解析、本地化处理及静态资源处理上都有许多不俗的表现。

​    **Spring MVC围绕DispatcherServlet(前端控制器)为中心展开**，DispatcherServlet(前端控制器)是Spring MVC的中枢，和MVC的思想一样，它负责从视图获取用户请求并且分派给相应的处理器处理，并决定用哪个视图去把数据呈现给给用户

**Spring MVC特点**

​    1：让我们能非常简单的设计出干净的Web层和薄薄的Web层；

​    2：进行更简洁的Web层的开发；

​    3：天生与Spring框架集成（如IoC容器、AOP等）；

​    4：提供强大的约定大于配置的契约式编程支持；

​    5：能简单的进行Web层的单元测试；

​    6：支持灵活的URL到页面控制器的映射；

​    7：非常容易与其它视图技术集成，如Velocity、FreeMarker等，因为模型数据不放在特定的API里，而是放在一 个Model里（Map数据结构实现，因此很容易被其他框架使用）；

​    8：非常灵活的数据验证、格式化和数据绑定机制，能使用任何对象进行数据绑定，不必实现特定框架的API；

​    9：提供一套强大的JSP标签库，简化JSP开发；

​    10：支持灵活的本地化、主题等解析；

​    11：更加简单的异常处理；

​    12：对静态资源的支持； 支持Restful风格。

### Spring MVC请求响应

​    SpringMVC把视图渲染、请求处理、模型创建分离了，遵循了MVC框架模式的思想

![img](https://img-blog.csdn.net/20180810092716689?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDQ5NTE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

​    SpringMVC的请求相应要经过七个阶段，蓝色的方框是Spring框架已经实现好的，第二阶段到第六阶段对应着Spring MVC中的一些核心理念，分别是前端控制器、处理映射器、控制器(处理器)、视图解析器、视图。要注意的是：前端控制器和控制器不是一个东西，前端控制器负责任务分发，控制器是模型的一部分，负责业务和数据的处理

​    SpringMVC核心控制类的请求流程

![img](https://img-blog.csdn.net/20180810094245464?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDQ5NTE4/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**SpringMVC的请求相应步骤如下**

解释一：

1、用户向服务器发送请求，请求被Spring 前端控制Servelt DispatcherServlet捕获

2、DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI）。然后根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain对象的形式返回

3、DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。（**附注**：如果成功获得HandlerAdapter后，此时将开始执行拦截器的preHandler(...)方法）

4、提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作

  HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息

​    1.数据转换：对请求消息进行数据转换。如String转换成Integer、Double等

​    2.数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等

​    3.数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中

5、Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象

6、根据返回的ModelAndView，选择一个适合的ViewResolver（必须是已经注册到Spring容器中的ViewResolver)返回给DispatcherServlet 

7、ViewResolver 结合Model和View，来渲染视图

8、将渲染结果返回给客户端

解释二：

第一步：客户端通过url发送请求
第二步：核心控制器DispatcherServlet接收到请求
第三步：通过系统的映射器找到对应的handler(处理器),并返回给核心控制器
第四步：通过核心控制器在找到handler(处理器)对应的适配器，
第五步：由找到的适配器，调用对应的handler(处理器)，并将结果返回给适配器
第六步:适配器将ModelandView对象返回给核心控制器
第七步：由核心控制器将返回的ModelandView给视图解析器
第八步：有核心控制将解析的结果进行响应，最终将结果响应到客户端

解释三：

1、 用户发送请求至前端控制器DispatcherServlet。

2、 DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3、 处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

4、 DispatcherServlet调用HandlerAdapter处理器适配器。

5、 HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

6、 Controller执行完成返回ModelAndView。

7、HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。

8、 DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

9、 ViewReslover解析后返回具体View。

10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet响应用户。

### 参考链接

1）https://blog.csdn.net/amanicspater/article/details/72540868?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param

2）https://blog.csdn.net/qq_38449518/article/details/81545578

## spring security

### spring security 简介

​    spring security 的核心功能主要包括：

- 认证 （你是谁）
- 授权 （你能干什么）
- 攻击防护 （防止伪造身份）

### 参考链接

1）https://www.cnblogs.com/lenve/p/11242055.html

2）https://blog.csdn.net/qq_22172133/article/details/86503223

3）https://didispace-wx.blog.csdn.net/article/details/78959233?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param

### 解释

Spring Security 支持两种不同的认证方式：

- 可以通过 form 表单来认证
- 可以通过 HttpBasic 来认证

**注解 @EnableWebSecurity**

   在 Spring boot 应用中使用 Spring Security，用到了 @EnableWebSecurity注解，官方说明为，该注解和 @Configuration 注解一起使用, 注解 WebSecurityConfigurer 类型的类，或者利用@EnableWebSecurity 注解继承 WebSecurityConfigurerAdapter的类，这样就构成了 Spring Security 的配置。

## IOC

### 介绍

 **引述**：IoC（控制反转：Inverse of Control）是Spring容器的内核，AOP、声明式事务等功能在此基础上开花结果。但是IoC这个重要的概念却比较晦涩隐讳，不容易让人望文生义，这不能不说是一大遗憾。不过IoC确实包括很多内涵，它涉及代码解耦、设计模式、代码优化等问题的考量，我们打算通过一个小例子来说明这个概念。

**那到底是什么东西的“控制”被“反转”了呢**？即是某一接口具体实现类的选择控制权从调用类中移除，转交给第三方决定。

对象A获得依赖对象B的过程,由主动行为变为了被动行为，控制权颠倒过来了，这就是“控制反转”这个名称的由来。

**IOC也叫依赖注入(DI)**

2004年，Martin Fowler探讨了同一个问题，**既然IOC是控制反转，那么到底是“哪些方面的控制被反转了呢？”，经过详细地分析和论证后，他得出了答案：“获得依赖对象的过程被反转了”。**控制被反转之后，获得依赖对象的过程由自身管理变为了由IOC容器主动注入。于是，他给“控制反转”取了一个更合适的名字叫做“依赖注入（Dependency Injection）”。他的这个答案，实际上给出了实现IOC的方法：注入。**所谓依赖注入，就是由IOC容器在运行期间，动态地将某种依赖关系注入到对象之中。**

　　所以，依赖注入(DI)和控制反转(IOC)是从不同的角度的描述的同一件事情，就是指通过引入IOC容器，利用依赖关系注入的方式，实现对象之间的解耦。

### **IoC的类型**

 从注入方法上看，主要可以划分为三种类型：**构造函数注入**、**属性注入**和**接口注入**。Spring支持**构造函数注入**和**属性注入**。

### 参考链接

1）https://www.iteye.com/blog/stamen-1489223

2）https://blog.csdn.net/ivan820819/article/details/79744797

## AOP

### 介绍

AOP（面向方面编程：Aspect Oriented Programing）和IoC一样是Spring容器的内核，声明式事务的功能在此基础上开花结果。

**连接点（Joinpoint）**

程序执行的某个特定位置：如类开始初始化前、类初始化后、类某个方法调用前、调用后、方法抛出异常后。一个类或一段程序代码拥有一些具有边界性质的特定点，这些代码中的特定点就称为“连接点”。Spring仅支持方法的连接点，即仅能在方法调用前、方法调用后、方法抛出异常时以及方法调用前后这些程序执行点织入增强。我们知道黑客攻击系统需要找到突破口，没有突破口就无法进行攻击，从某种程度上来说，AOP是一个黑客（因为它要向目前类中嵌入额外的代码逻辑），连接点就是AOP向目标类打入楔子的候选点。

连接点由两个信息确定：第一是用方法表示的程序执行点；第二是用相对点表示的方位。如在Test.foo()方法执行前的连接点，执行点为Test.foo()，方位为该方法执行前的位置。Spring使用切点对执行点进行定位，而方位则在增强类型中定义。

**切点（Pointcut）**
每个程序类都拥有多个连接点，如一个拥有两个方法的类，这两个方法都是连接点，即连接点是程序类中客观存在的事物。但在这为数众多的连接点中，如何定位到某个感兴趣的连接点上呢？AOP通过“切点”定位特定接连点。通过数据库查询的概念来理解切点和连接点的关系再适合不过了：连接点相当于数据库中的记录，而切点相当于查询条件。切点和连接点不是一对一的关系，一个切点可以匹配多个连接点。

在Spring中，切点通过org.springframework.aop.Pointcut接口进行描述，它使用类和方法作为连接点的查询条件，Spring AOP的规则解析引擎负责解析切点所设定的查询条件，找到对应的连接点。其实确切地说，用切点定位应该是执行点而非连接点，因为连接点是方法执行前、执行后等包括方位信息的具体程序执行点，而切点只定位到某个方法上，所以如果希望定位到具体连接点上，还需要提供方位信息。

**增强（Advice）**

增强是织入到目标类连接点上的一段程序代码。是不是觉得AOP越来越像黑客了：），这不是往业务类中装入木马吗？读者大可按照这一思路去理解增强，因为这样更形象易懂。在Spring中，增强除用于描述一段程序代码外，还拥有另一个和连接点相关的信息，这便是执行点的方位。结合执行点方位信息和切点信息，我们就可以找到特定的连接点了！正因为增强既包含了用于添加到目标连接点上的一段执行逻辑，又包含了用于定位连接点的方位信息，所以Spring所提供的增强接口都是带方位名的：BeforeAdvice、AfterRetuningAdvice、ThrowsAdvice等。BeforeAdvice表示方法调用前的位置，而AfterReturingAdvice表示访问返回后的位置。所以只有结合切点和增强两者一起上阵才能确定特定的连接点并实施增强逻辑。

**标对象（Target）**

增强逻辑的织入目标类。如果没有AOP，目标业务类需要自己实现所有逻辑，就如中ForumService所示。在AOP的帮助下，ForumService只实现那些非横切逻辑的程序逻辑，而性能监视和事务管理等这些横切逻辑则可以使用AOP动态织入到特定的连接点上。

**引介（Introduction）**

引介是一种特殊的增强，它为类添加一些属性和方法。这样，即使一个业务类原本没有实现某个接口，通过AOP的引介功能，我们可以动态地为该业务类添加接口的实现逻辑，让业务类成为这个接口的实现类。

**织入（Weaving）**

织入是将增强添加对目标类具体连接点上的过程，AOP像一台织布机，将目标类、增强或者引介通过AOP这台织布机天衣无缝地编织到一起。我们不能不说“织入”这个词太精辟了。根据不同的实现技术，AOP有三种织入的方式：
1）编译期织入，这要求使用特殊的Java编译器；
2）类装载期织入，这要求使用特殊的类装载器；
3）动态代理织入，在运行期为目标类添加增强生成子类的方式。
Spring采用动态代理织入，而AspectJ采用编译期织入和类装载期织入。

**代理（Proxy）**

一个类被AOP织入增强后，就产出了一个结果类，它是融合了原类和增强逻辑的代理类。根据不同的代理方式，代理类既可能是和原类具有相同接口的类，也可能就是原类的子类，所以我们可以采用调用原类相同的方式调用代理类。

**切面（Aspect）**

切面由切点和增强（引介）组成，它既包括了横切逻辑的定义，也包括了连接点的定义，Spring AOP就是负责实施切面的框架，它将切面所定义的横切逻辑织入到切面所指定的连接点中。

AOP的工作重心在于如何将增强应用于目标对象的连接点上，这里首先包括两个工作：第一，如何通过切点和增强定位到连接点上；第二，如何在增强中编写切面的代码。本章大部分的内容都围绕这两点展开。

**AOP的实现者**

AOP工具的设计目标是把横切的问题（如性能监视、事务管理）模块化。使用类似于OOP的方式进行切面的编程工作。位于AOP工具核心的是连接点模型，它提供了一种机制，可以识别出在哪里发生了横切。

 **AspectJ**

AspectJ是语言级的AOP实现，2001年由Xerox PARC的AOP小组发布，目前版本已经更新到1.6。AspectJ扩展了Java语言，定义了AOP语法，能够在编译期提供横切代码的织入，所以它有一个专门的编译器用来生成遵守Java字节编码规范的Class文件。主页位于http://www.eclipse.org/aspectj。

**AspectWerkz**
 
基于Java的简单、动态、轻量级的AOP框架，该框架2002年就已经发布，由BEA Systems提供支持。它支持运行期或类装载期织入横切代码，所以它拥有一个特殊的类装载器。现在，AspectJ和AspectWerkz项目已经合并，以便整合两者的力量和技术创建统一的AOP平台。他们合作的第一个发布版本是AspectJ 5：扩展AspectJ语言，以基于注解的方式支持类似AspectJ的代码风格。

**Spring AOP**

Spring AOP使用纯Java实现，它不需要专门的编译过程，不需要特殊的类装载器，它在运行期通过代理方式向目标类织入增强代码。Spring并不尝试提供最完整的AOP实现，相反，它侧重于提供一种和Spring IoC容器整合的AOP实现，用以解决企业级开发中的常见问题。在Spring中，我们可以无缝地将Spring AOP、IoC和AspectJ整合在一起。

### AOP 核心概念

- **Aspect**：即切面，切面一般定义为一个 Java 类， 每个切面侧重于特定的跨领域功能，比如，事务管理或者日志打印等。
- **Joinpoint**：即连接点，程序执行的某个点，比如方法执行。构造函数调用或者字段赋值等。在 **Spring AOP** 中，连接点只会有 **方法调用 (Method execution)**。
- **Advice**：即通知，在连接点要的代码。
- **Pointcut**：即切点，一个匹配连接点的正则表达式。当一个连接点匹配到切点时，一个关联到这个切点的特定的 **通知 (Advice)** 会被执行。
- **Weaving**：即编织，负责将切面和目标对象链接，以创建通知对象，在 **Spring AOP** 中没有这个东西。

#### Aspect 定义



```java
package aaric.springaopdemo;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class WeixinServiceAspect {

    @AfterReturning("execution(public void WeixinService.share(String))")
    public void log(JoinPoint joinPoint) {
        System.out.println(joinPoint.getSignature() + " executed");
    }
}
```

- 在 **Spring** 中使用 **Aspect** 需要使用 **@Component** 直接将其标记为一个 **Bean**
- 并且使用 **@Aspec** 注解将其标记为一个切面
- 然后在该类中定义上面我们说的切点，通知等

#### Pointcut 定义

这里我们说一下 **Pointcut** 的表达式如何写，我们首先将上面例子中的切面类修改如下



```java
package aaric.springaopdemo;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.AfterReturning;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class WeixinServiceAspect {

    @Pointcut("execution(public void WeixinService.share(String))")
    public void shareCut() {

    }

    @AfterReturning("shareCut()")
    public void log(JoinPoint joinPoint) {
        System.out.println(joinPoint.getSignature() + " executed");
    }
}
```

- 下面这个便是切点的定义
- 切点定义在方法上，并使用 **@Pointcut** 注解，注解中的值便是切点的表达式
- 切点的名称就是方法的名称，这里是 **shareCut()**，注意这里有括号



```java
    @Pointcut("execution(public void WeixinService.share(String))")
    public void shareCut() {

    }
```

- 若要将具体的通知 **Advice** 关联的某个切点上，在 **Advice** 的注解上写上切点的名称就可以了，如下



```java
    @AfterReturning("shareCut()")
    public void log(JoinPoint joinPoint) {
        System.out.println(joinPoint.getSignature() + " executed");
    }
```

#### Pointcut 指示器

切点的表达式以 **指示器** 开始， **指示器** 就是一种关键字，用来告诉 Spring AOP 如何匹配连接点，**Spring AOP** 提供了以下几种指示器

- **execution**
- **within**
- **this** 和 **target**
- **args**
- **@target**
- **@annotation**

下面我们依次说明这些指示器的作用

##### execution

该指示器用来匹配方法执行连接点，即匹配哪个方法执行，如



```java
@Pointcut("execution(public String aaric.springaopdemo.UserDao.findById(Long))")
```

上面这个切点会匹配在 **UserDao** 类中 **findById** 方法的调用，并且需要该方法是 **public** 的，返回值类型为 **String**，只有一个 **Long** 的参数。
 切点的表达式同时还支持**宽字符**匹配，如



```java
@Pointcut("execution(* aaric.springaopdemo.UserDao.*(..))")
```

上面的表达式中，第一个宽字符 * 匹配 **任何返回类型**，第二个宽字符 * 匹配 **任何方法名**，最后的参数 (..) 表达式匹配 **任意数量任意类型** 的参数，也就是说该切点会匹配类中所有方法的调用。

##### within

如果要匹配一个类中所有方法的调用，便可以使用 **within** 指示器



```java
@Pointcut("within(aaric.springaopdemo.UserDao)")
```

这样便可以匹配该类中所有方法的调用了。同时，我们还可以匹配某个包下面的所有类的所有方法调用，如下面的例子



```java
@Pointcut("within(aaric.springaopdemo..*)")
```

##### this 和 target

- 如果目标对象**实现**了任何接口，**Spring AOP** 会创建基于**CGLIB** 的动态代理，这时候需要使用 **target** 指示器
- 如果目标对象没有**实现**任何接口，**Spring AOP** 会创建基于**JDK**的动态代理，这时候需要使用 **this** 指示器



```java
@Pointcut("target(aaric.springaopdemo.A)") A 实现了某个接口
@Pointcut("target(aaric.springaopdemo.B)") B 没有实现任何一个接口
```

##### args

该指示器用来匹配具体的方法参数



```java
@Pointcut("execution(* *..find*(Long))")
```

这个切点会匹配任何以 **find** 开头并且只有一个 **Long** 类型的参数的方法。
 如果我们想匹配一个以 **Long** 类型开始的参数，后面的参数类型不做限制，我们可以使用如下的表达式



```java
@Pointcut("execution(* *..find*(Long,..))")
```

##### @target

该指示器不要和 **target** 指示器混淆，该指示器用于匹配连接点所在的类是否拥有指定类型的注解，如



```java
@Pointcut("@target(org.springframework.stereotype.Repository)")
```

##### @annotation

该指示器用于匹配连接点的方法是否有某个注解



```java
@Pointcut("@annotation(org.springframework.scheduling.annotation.Async)")
```

#### Advice 定义

**Advice** 通知，即在连接点出要执行的代码，分为以下几种类型

- **Around**
- **Before**
- **After**

#### 开启 Advice

如果要在 **Spring** 中使用 **Spring AOP** 需要开启 **Advice**，使用 **@EnableAspectJAutoProxy** 注解就可以了，代码如下



```java
@SpringBootApplication
@EnableAspectJAutoProxy
public class SpringAopDemoApplication {
}
```

#### Before Advice

**Before Advice** 用来执行在方法调用之前的操作，如果 **Before Advice** 在执行的过程中抛出异常的话，那么连接点的方法就不会被执行。



```java
@Aspect
@Component
public class PrintAspect {
 
    @Pointcut("@target(org.springframework.stereotype.Repository)")
    public void repositoryMethods() {};
 
    @Before("repositoryMethods()")
    public void logMethodCall(JoinPoint jp) {
        String name = jp.getSignature().getName();
        System.out.println("Before " + name);
    }
}
```

**logMethodCall** **Advice** 会在任何 **Repository** 方法执行之前调用。

#### After Advice

顾名思义，该 **Advice** 会在方法被调用之后被执行，但是有三种注解可以使用：

- @AfterReturing：该 **Advice** 会在方法正常返回以后执行
- @AfterThrowing： 该 **Advice** 会在方法抛出异常以后执行
- @After： 该 **Advice** 无论如何，在方法执行以后都会执行

#### Around Advice

这个是最有用的 **Advice**， 它可以控制方法在执行前后的行为。
 它可以选择是否继续执行连接点的方法，或者中断该方法的执行，而返回自己的返回值。

### AOP Annotation

最后，在这篇教程中，我们了解如何使用 AOP 以及 AOP 的 API，下面我们尝试自己定义一个 AOP 的 Annotation，**@CalculateExecuteTime**，任何使用该注解的方法，都会打印出该方法的执行时间。

#### 创建 Annotation



```java
package aaric.springaopdemo;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface CalculateExecuteTime {
}
```

#### 创建切面



```java
package aaric.springaopdemo;

import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class CalculateExecuteTimeAspect {

}
```

#### 创建切点和通知



```java
package aaric.springaopdemo;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class CalculateExecuteTimeAspect {
    @Around("@annotation(CalculateExecuteTime)")
    public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object proceed = joinPoint.proceed();
        long executionTime = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature() + " executed in " + executionTime + "ms");
        return proceed;
    }
}
```

- 这里的 **ProceedingJoinPoint** 代表连接的的方法

#### 在方法上加上自定义注解



```java
package aaric.springaopdemo;

import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;

@Service
public class WeixinService {

    @CalculateExecuteTime
    public void share(@NotNull String articleUrl) {
        try {
            TimeUnit.SECONDS.sleep(3);
        } catch (Exception ignored) {

        }
    }
}
```

- 这里我们模拟该方法会执行3秒的时间
- 运行程序以后得到如下结果

![img](https:////upload-images.jianshu.io/upload_images/14843627-257e7183d95c613e.png?imageMogr2/auto-orient/strip|imageView2/2/w/997/format/webp)

可以看到该 **Advice** 已经生效





### 参考链接

1）https://www.jianshu.com/p/a21256903fdd

2）https://www.iteye.com/blog/stamen-1512388

## SpEL

### 参考链接

1）https://www.jianshu.com/p/e0b50053b5d3

2）http://itmyhome.com/spring/expressions.html

3）https://baijiahao.baidu.com/s?id=1618293103449832238&wfr=spider&for=pc

4）https://www.cnblogs.com/figsprite/p/10774345.html

## Spring WebFLux

### 参考链接

1）https://blog.csdn.net/get_set/article/details/79466657

## Spring Boot

@SpringbootApplication
		---@Target(Element Type)//注解使用的范围
		---@Retention//注解的生命周期
		---@Documented//关于文档的信息
		---@inherited//是否可以被继承
		---@SpringbootConfiguration（非常重要）（先加载）
			---@configuration//spring中的配置类注解，加载程序中的配置文件，如果将来需要引入配置文件，
			则在springboot程序任意位置都能用
		---@EnableAutoConfiguration（非常重要）（后加载）自动化配置
			---@AutoConfigurationPackage //作用：添加springboot项目的包扫描路径，要求以后写项目必须在主启动类的同包及子包中编辑
			---@Import（AutoConfiguration.Registrar.class）//选择器，扫描springboot项目中的启动项文件，实现开箱即用
		---@ComponentScan

