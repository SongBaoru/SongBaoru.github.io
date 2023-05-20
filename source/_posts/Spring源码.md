---
title: Spring源码
date: 2023-05-05 21:02:38
tags:
  - Spring
---



# Spring特性

Spring基于J2EE技术实现了一套轻量的Java Web Service系统应用框架，有很多优秀的特性，包括，依赖注入（DI）、控制反转（IoC）、面向切面（AOP）、轻量、灵活

1. 控制反转
   1. 指的是对象依赖的对象，将会在容器的初始化完成后会主动传递给对象，而不需要对象自己创建或查询其依赖的对象，实现了系统对象之间依赖的解耦
   2. Spring通过依赖注入实现控制反转，依赖注入是一种设计模式，通过该模式，对象不再创建或管理它们所需要的其他对象或服务，而是由容器（例如Spring容器）负责创建和管理这些对象或服务，并注入到需要它们的对象中。
2. 面向切面
   1. 面向切面是一种编程范式，用于将系统的横切关注点（如安全性、事务、日志记录等）与业务逻辑分离
   2. 面向切面通过将横切关注点划分为独立的模块，并在运行时动态地将这些模块植入到程序中，从而实现了对业务逻辑的无侵入式增强
   3. Spring AOP通过使用动态代理技术来实现对目标对象的增强
3. 轻量
   1. spring-web-5.2.0.RELEASE.jar和spring-core-5.2.0.RELEASE.jar均仅有1.4M左右
   2. 只需要少量的操作系统资源
4. 灵活
   1. 是模块化的，可以按需引入模块（以jar包依赖的方式引入）

# Spring的核心JAR包

Spring是模块化实现的，每个模块对应不同的JAR包

Spring框架的所有JAR包：

| 名称                     | 简介                                                         |
| ------------------------ | ------------------------------------------------------------ |
| spring-aop               | 提供了Spring框架的面向切面编程（AOP）功能，用于在运行时动态地增强应用程序的功能。 |
| spring-aspects           | 提供了Spring框架的切面库，包括对AspectJ切面的支持和一些通用切面的实现。 |
| spring-beans             | 提供了Spring框架的BeanFactory和FactoryBean等工厂类，用于管理和配置应用程序中的对象。 |
| spring-context           | 提供了Spring框架的应用上下文（ApplicationContext），用于管理应用程序中的Bean对象，以及Spring框架的事件驱动编程模型。 |
| spring-context-indexer   | 提供了一个工具，用于在编译时为Spring应用程序生成索引文件，以提高应用程序启动的速度。 |
| spring-context-support   | 提供了一些扩展类，用于在Spring应用程序中支持特定的应用场景，例如JPA、Velocity等。 |
| spring-core              | Spring框架的核心模块，提供了Spring框架的基本功能，如依赖注入、控制反转、Bean工厂等。 |
| spring-expression        | 提供了Spring框架的表达式语言（SpEL），用于在应用程序中动态地访问和操作对象。 |
| spring-instrument        | 提供了Spring框架的Instrumentation API支持，用于在运行时通过Java Agent来提供增强功能。 |
| spring-instrument-tomcat | 提供了Spring框架在Tomcat服务器中使用Instrumentation API的支持。 |
| spring-jcl               | 提供了Spring框架的通用日志抽象库，可以在不同的日志实现之间进行切换。 |
| spring-jdbc              | 提供了Spring框架的JDBC支持，包括对JdbcTemplate和NamedParameterJdbcTemplate等的封装。 |
| spring-jms               | 提供了Spring框架的Java Message Service（JMS）支持，用于在应用程序中发送和接收消息。 |
| spring-messaging         | 提供了Spring框架的消息处理功能，包括对WebSocket、STOMP、AMQP等协议的支持。 |
| spring-orm               | 提供了Spring框架的对象关系映射（ORM）支持，包括对Hibernate、MyBatis等ORM框架的集成。 |
| spring-oxm               | 提供了Spring框架的对象XML映射（OXM）支持，用于在Java对象和XML文档之间进行转换。 |
| spring-test              | 提供了Spring框架的测试支持，包括对JUnit、TestNG等测试框架的集成，以及对Spring应用程序的集成测试支持。 |
| spring-tx                | 提供了Spring框                                               |



# Spring注解

Spring的注解将Bean的定义和依赖关系从XML配置中解放出来，应用程序只要使用注解依赖注入即可

Bean具体的定义和依赖关系由Spring的自动装配完成

## 依赖注入相关注解

| 注解       | 翻译名称    | 简介                                                         |
| ---------- | ----------- | ------------------------------------------------------------ |
| @Autowired | 自动注入    | 根据类型进行自动注入，如果有多个符合条件的Bean，可以通过指定名称或限定符来进行注入。 |
| @Qualifier | 限定符      | 与@Autowired一起使用，指定Bean的名称或限定符，以便进行注入。 |
| @Resource  | 资源注入    | 根据名称进行自动注入，可以与指定类型或名称的方式进行限定。   |
| @Value     | 属性注入    | 用于注入常量或表达式计算的结果值。                           |
| @Inject    | JSR-330注解 | 与@Autowired类似，但具有更加灵活的限定符支持。               |

## Bean定义相关注解

| 注解           | 翻译名称   | 简介                                                         |
| -------------- | ---------- | ------------------------------------------------------------ |
| @Component     | 通用组件   | 用于将类定义为Spring组件，并且可以与其他注解一起使用，如@Controller、@Service、@Repository等。 |
| @Configuration | 配置类     | 用于定义Spring应用程序的配置类，并且可以通过@Bean方法定义Bean对象。 |
| @Bean          | Bean定义   | 用于在配置类中定义Bean对象，并将其添加到Spring容器中。       |
| @Profile       | 环境选择   | 用于基于不同的应用程序环境选择Bean定义，可以与@Conditional一起使用。 |
| @Scope         | Bean作用域 | 用于定义Bean对象的作用域，包括Singleton、Prototype、Request、Session等。 |

## AOP相关注解

| 注解            | 翻译名称 | 简介                                           |
| --------------- | -------- | ---------------------------------------------- |
| @Aspect         | 切面定义 | 用于将类定义为切面，可以在其中定义切点和通知。 |
| @Pointcut       | 切点定义 | 用于定义切点，指定连接点的匹配规则。           |
| @Before         | 前置通知 | 在方法执行之前执行通知。                       |
| @After          | 后置通知 | 在方法执行之后执行通知。                       |
| @AfterReturning | 返回通知 | 在方法执行之后返回结果后执行通知。             |
| @AfterThrowing  | 异常通知 | 在方法执行时抛出异常后执行通知。               |
| @Around         | 环绕通知 | 在方法执行之前和之后都可以执行通知。           |

## Web相关注解

| 注解               | 翻译名称       | 简介                                                         |
| ------------------ | -------------- | ------------------------------------------------------------ |
| @Controller        | 控制器         | 用于将类定义为Spring MVC的控制器，处理HTTP请求并返回响应结果。 |
| @RestController    | REST控制器     | 与@Controller类似，但默认情况下返回JSON或XML格式的响应结果。 |
| @RequestMapping    | 请求映射       | 用于将HTTP请求映射到处理方法上，并指定请求的URL、请求方法、请求参数等。 |
| @GetMapping        | GET请求映射    | 用于将HTTP GET请求映射到处理方法上，简化了@RequestMapping的用法。 |
| @PostMapping       | POST请求映射   | 用于将HTTP POST请求映射到处理方法上，简化了@RequestMapping的用法。 |
| @PutMapping        | PUT请求映射    | 用于将HTTP PUT请求映射到处理方法上，简化了@RequestMapping的用法。 |
| @DeleteMapping     | DELETE请求映射 | 用于将HTTP DELETE请求映射到处理方法上，简化了@RequestMapping的用法。 |
| @PatchMapping      | PATCH请求映射  | 用于将HTTP PATCH请求映射到处理方法上，简化了@RequestMapping的用法。 |
| @PathVariable      | 路径变量       | 用于将URI中的变量绑定到处理方法的参数上。                    |
| @RequestParam      | 请求参数       | 用于将HTTP请求中的参数绑定到处理方法的参数上。               |
| @RequestBody       | 请求体         | 用于将HTTP请求体中的数据绑定到处理方法的参数上。             |
| @RequestHeader     | 请求头         | 用于将HTTP请求头中的数据绑定到处理方法的参数上。             |
| @CookieValue       | Cookie值       | 用于将HTTP Cookie中的值绑定到处理方法的参数上。              |
| @ResponseBody      | 响应体         | 用于将处理方法的返回值作为HTTP响应体返回给客户端。           |
| @ResponseStatus    | 响应状态码     | 用于指定处理方法的返回状态码。                               |
| @SessionAttributes | 会话属性       | 用于在会话中存储处理方法的模型属性。                         |
| @ModelAttribute    | 模型属性       | 用于将请求参数绑定到模型属性上。                             |
| @InitBinder        | 初始化绑定器   | 用于初始化WebDataBinder，用于数据绑定和格式化等操作。        |
| @ExceptionHandler  | 异常处理       | 用于处理控制器中抛出的异常。                                 |
| @CrossOrigin       | 跨域资源共享   | 用于处理跨域请求，允许指定允许跨域请求的来源、方法和头信息等。 |

# Spring IoC原理

## IoC简介

Spring IoC通过Java反射功能实例化并建立Bean之间的依赖关系

Spring IoC在完成这些底层工作的基础上，还提供了Bean实例缓存管理、Bean生命周期管理、Bean实例代理、事件发布和资源装载等高级服务

## Bean的装配流程

Spring通过读取XML或注解获取Bean的配置信息，并在Bean容器中生成Bean配置注册表，然后根据配置注册表实例化Bean，将Bean实例载入Bean缓存池，业务程序就可以从Bean缓存池中获取Bean

## Bean的作用域

Bean有五种作用域：

- Singleton：单例作用域，表示在 Spring IoC 容器中只存在一个 Bean 对象实例，所有对该 Bean 的请求都将返回该唯一实例。
- Prototype：原型作用域，每次对该 Bean 的请求都将创建一个新的 Bean 实例。每次使用时都会创建新的对象。
- Request：请求作用域，每个 HTTP 请求都将创建一个新的 Bean 实例，该 Bean 仅在当前 HTTP 请求中有效。
- Session：会话作用域，每个 HTTP 会话都将创建一个新的 Bean 实例，该 Bean 仅在当前 HTTP 会话中有效。
- GlobalSession：全局会话作用域，仅适用于使用基于 Portlet 的 web 应用。它是在一个全局的 Portlet 会话中共享的 Bean 实例。

除了这五种标准作用域外，Spring 还支持自定义作用域。在 Spring 中，我们可以通过实现 Scope 接口并重写对应方法来实现自定义作用域。这样可以让我们更加灵活地管理 Bean 的生命周期，以满足应用程序的特定需求。

## Bean的生命周期

Spring Bean 的生命周期是 Spring IoC 容器管理的重要部分，它由一系列的回调函数来控制。在 Spring 容器创建 Bean 实例对象时，会经历以下阶段：

1. Bean 实例化：Spring IoC 容器通过反射机制实例化一个 Bean 对象，通常是使用默认的构造函数来创建 Bean 实例。

2. 属性注入：Spring IoC 容器通过 setter 方法或者直接访问 Bean 属性来注入 Bean 的属性。

3. BeanPostProcessor 前置处理器：在 Bean 实例化之后，Spring IoC 容器会自动检测是否有实现了 BeanPostProcessor 接口的类，并调用它们的 postProcessBeforeInitialization() 方法来对 Bean 进行前置处理。

4. 初始化：Spring IoC 容器调用 Bean 实现 InitializingBean 接口或者配置的 init-method 方法，执行 Bean 的初始化操作。

5. BeanPostProcessor 后置处理器：在 Bean 初始化之后，Spring IoC 容器会自动检测是否有实现了 BeanPostProcessor 接口的类，并调用它们的 postProcessAfterInitialization() 方法来对 Bean 进行后置处理。

6. 使用：Bean 实例化完成并初始化后，就可以在应用程序中使用它了。

7. 销毁：当 Spring IoC 容器关闭时，它会调用 Bean 实现 DisposableBean 接口或者配置的 destroy-method 方法，执行 Bean 的销毁操作。

此外，Spring Bean 的生命周期可以被定制化，我们可以自定义 BeanPostProcessor 实现类或者配置 init-method 和 destroy-method 方法，来在 Bean 的生命周期中加入自己的逻辑处理。

BeanPostProcessor 前置处理器是 Spring IoC 容器中的一个扩展点，用于在 Bean 的初始化前进行额外的处理，可以对 Bean 对象进行修改或增强。BeanPostProcessor 接口定义了两个方法：

1. postProcessBeforeInitialization(Object bean, String beanName)：在 Bean 初始化之前调用该方法，可以对 Bean 对象进行一些修改或增强操作。

2. postProcessAfterInitialization(Object bean, String beanName)：在 Bean 初始化之后调用该方法，可以对 Bean 对象进行一些修改或增强操作。

在 Spring IoC 容器中，当一个 Bean 实例化完成后，会检查是否有实现了 BeanPostProcessor 接口的类，如果有，则会依次调用它们的 postProcessBeforeInitialization() 方法，然后进行 Bean 的初始化操作，最后再依次调用实现了 BeanPostProcessor 接口的类的 postProcessAfterInitialization() 方法。通过实现 BeanPostProcessor 接口，我们可以在 Bean 实例化前后进行一些自定义的操作，例如：

1. 为 Bean 注入日志处理、事务处理等公共的功能。

2. 在 Bean 初始化前后进行性能监控、安全检查等操作。

3. 对 Bean 进行代理，实现 AOP 的功能。

需要注意的是，在实现 BeanPostProcessor 接口时，必须小心处理，以免破坏 Bean 的正常生命周期。同时，也应该尽量保持 BeanPostProcessor 的轻量级，避免对系统性能产生过大的影响。

总之，BeanPostProcessor 前置处理器是 Spring IoC 容器中的一个重要扩展点，通过实现该接口，可以在 Bean 实例化前后进行一些自定义的操作，从而增强 Bean 的功能和灵活性。

# Spring AOP原理

## AOP简介

Spring AOP通过面向切面技术，将与业务无关或被业务模块共用的代码封装起来，以提高代码的复用度，降低模块间的耦合度

## AOP 的核心概念

| 概念                              | 描述                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| 切面（Aspect）                    | 对一个或多个横切关注点的封装，它包含了切点、通知和切点表达式等元素。切面定义了何时、何地以及如何将横切关注点织入到目标对象中。 |
| 切点（Pointcut）                  | 目标对象中的一组方法或者类，它们将被织入到横切关注点中。切点通常由切点表达式和其他过滤条件组成。 |
| 通知（Advice）                    | 在织入横切关注点时要执行的逻辑代码，它包括了前置通知、后置通知、环绕通知、异常通知和最终通知等不同类型。 |
| 切点表达式（Pointcut Expression） | 一种指定切点的语法规则，它可以根据方法名、返回值类型、方法参数等多种条件进行切点匹配。 |
| 连接点（Join Point）              | 程序执行过程中的某个特定位置，例如方法调用、方法执行、异常抛出等。连接点是织入横切关注点的具体执行位置。 |
| 织入（Weaving）                   | 将横切关注点应用到目标对象的过程，它可以通过代理模式实现。在 Spring AOP 中，织入分为编译期织入、类装载期织入和运行期织入三种方式。 |

上述概念是 Spring AOP 技术中的核心要素，了解这些概念对于掌握和使用 Spring AOP 技术非常重要。切面、切点、通知和切点表达式是定义 AOP 配置的基础，连接点则表示切点匹配到的具体执行位置，织入则是实现 AOP 功能的核心机制。

## AOP横切关注点

Srping将应用分为核心关注点和横切关注点两部分

- 核心关注点（Core Concerns）是指应用程序的基本业务逻辑，例如数据访问、业务逻辑处理等。核心关注点是应用程序的主要功能，通常是由应用程序开发人员直接实现的。

- 横切关注点（Cross-Cutting Concerns）是指应用程序中与核心业务逻辑无关的横切问题，例如日志记录、事务管理、安全控制等。

在 Spring AOP 中，横切关注点可以通过定义切面（Aspect）来实现。通常情况下，切面是一个 Java 类，其中包含了一些切点（Pointcut）、通知（Advice）和切点表达式（Pointcut Expression）等元素。

- 切点（Pointcut）：用于定义一个或多个目标对象中哪些方法需要被织入横切关注点。
- 通知（Advice）：定义了横切关注点在目标对象中何时被执行以及执行的逻辑。
- 切点表达式（Pointcut Expression）：用于指定切点的匹配规则。

Spring AOP 的实现是基于代理模式的，它通过创建代理对象来织入切面逻辑。Spring AOP 支持两种代理方式：JDK 动态代理和 CGLIB 代理。对于实现了接口的类，Spring AOP 将使用 JDK 动态代理来创建代理对象；对于没有实现接口的类，Spring AOP 将使用 CGLIB 代理来创建代理对象。

## AOP的5种通知类型

Spring AOP 提供了以下五种类型的通知：

- 前置通知（Before Advice）：在目标方法执行前执行。
- 后置通知（After Returning Advice）：在目标方法返回后执行。
- 环绕通知（Around Advice）：在目标方法执行前后都执行。
- 异常通知（After Throwing Advice）：在目标方法抛出异常时执行。
- 最终通知（After Advice）：无论目标方法是否正常执行完成，最终通知都会被执行。

## AOP的应用

| 应用场景 | 切面类型                  | 示例                                                       |
| -------- | ------------------------- | ---------------------------------------------------------- |
| 日志记录 | 前置通知（Before）        | 在用户登录时记录登录时间和 IP 地址                         |
| 性能监控 | 环绕通知（Around）        | 在对数据库进行查询时统计查询时间和资源占用情况             |
| 安全控制 | 前置通知（Before）        | 在访问受保护的资源时检查用户的身份和权限信息               |
| 事务管理 | 环绕通知（Around）        | 在对数据库进行更新操作时开启和提交事务                     |
| 异常处理 | 异常通知（AfterThrowing） | 在文件上传时捕获文件格式不正确等异常信息，并进行相应的处理 |

# Spring MVC原理

## MVC简介

Spring的MVC即模型-视图-控制器，该框架围绕DispatcherServlet设计而成，DispatcherServlet会把请求分发给各个处理器

SpringMVC 的工作流程主要包括以下几个步骤：

1. 客户端发送请求：客户端向服务器发送请求，请求可以是一个 URL 地址、一个表单提交或者一个 AJAX 请求。

2. DispatcherServlet 接收请求：DispatcherServlet 是 SpringMVC 框架的核心控制器，它负责接收客户端发送的请求，并将请求转发给对应的处理器。

3. HandlerMapping 查找处理器：HandlerMapping 负责根据请求 URL 查找对应的处理器，处理器可以是一个 Controller 或者一个 Restful Web Service。

4. HandlerAdapter 调用处理器：HandlerAdapter 负责调用处理器，将请求传递给处理器进行处理，并获取处理器的处理结果。

5. 处理器处理请求：处理器根据请求的类型和参数，进行相应的业务处理，并返回一个 ModelAndView 对象。

6. 视图解析器解析视图：视图解析器根据 ModelAndView 中的视图名，将其解析成对应的视图对象，视图可以是一个 JSP 页面、一个 Thymeleaf 模板或者一个 HTML 片段等。

7. 渲染视图：视图对象根据数据模型和视图模板，生成 HTML 内容，并将其返回给客户端。

8. 返回响应：DispatcherServlet 将视图渲染的结果返回给客户端，客户端可以是一个浏览器、一个移动应用或者一个 API 调用。

总之，SpringMVC 的工作流程涉及到多个组件之间的协作，其中 DispatcherServlet 负责接收请求和控制流程，HandlerMapping 负责查找处理器，HandlerAdapter 负责调用处理器，视图解析器负责解析视图，视图对象负责渲染视图，最终将响应返回给客户端。





















