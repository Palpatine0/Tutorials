

## Spring

### Spring注解

**常用注解比较** [参考](https://program.blog.csdn.net/article/details/104151716)

**1 Spring**

**声明bean的注解**

- @Component 组件，没有明确的角色
- @Controller 在控制层使用，控制器的声明（controller层）
- @Service 在业务逻辑层使用（service层）
- @Repository 在数据访问层使用（dao层）

**注入bean的注解**

都可以注解在set方法和属性上，推荐注解在属性上（一目了然，少写代码）

- @Autowired：由Spring提供
- @Resource：由JSR-250提供
- @Inject：由JSR-330提供

**@Autowired注解与@Resource注解的区别**

相同点：@Resource的作用相当于@Autowired，均可标注在字段或属性的setter方法上。

不同点

- 提供方：@Autowired由Spring提供；@Resource由J2EE提供

- 注入方式：@Autowired只能按byType注入；@Resource能按自byType注入，也能按byName注入（默认）

- 属性：@Autowired按byType装配，默认要求对象必须存在，如果允许null值则可以设置它required属性为false；

  @Resource两个重要属性：name和type。

  name属性：注入对象的名称。如果没有指定name属性，当注解标注在字段上，即默认取字段的名称作为bean名称寻找依赖对象，当注解标注在属性的setter方法上，即默认取属性名作为bean名称寻找依赖对象。@Resource如果没有指定name属性，并且按照默认的名称仍然找不到依赖对象时， @Resource注解会回退到按类型装配。但一旦指定了name属性，就只能按名称装配了。

  type属性：注入类型

**java配置类相关注解**

- @Bean 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上）
- @Configuration 声明当前类为配置类，相当于xml形式的Spring配置（类上）
- @ComponentScan 用于对Component进行扫描，相当于xml中的（类上）

**@Bean的属性支持**

- @Scope 设置Spring容器如何新建Bean实例（方法上，得有@Bean） 

---

**2 SpringMVC**

- @RequestMapping 映射Web请求，包括访问路径和参数（类或方法上）
- @RequestBody request的参数，在request体中（放在参数前）
- @ResponseBody 将返回值放在response内，返回json数据（返回值旁或方法上）

---

**3 Spring Boot **

**Spring Boot的核心注解是哪个？它主要由哪几个注解组成的？**

@SpringBootApplication，主要组合包含了 3 个注解：

- @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
- @EnableAutoConfiguration：打开自动配置的功能，也可以关闭
- @ComponentScan：Spring组件扫描

### 控制反转IOC

#### IOC容器⭐

**Spring的IOC、DI理解 / 说一下Spring的IOC DI？/ IoC 和 DI 的区别？**[参考](https://blog.csdn.net/qq_36520235/article/details/79383238)

- IOC是控制反转，**代码**将对象的控制权转移给Spring，由Spring根据配置文件去创建和管理各个实例之间的依赖关系；DI是依赖注入，**IOC容器**向代码动态注入对象需要的外部依赖。
- 控制反转和依赖注入是同一个概念的不同角度的描述，控制反转是从代码的角度来看，将对象的控制权交给容器；依赖注入是从容器的角度看，把需要的对象注入代码
- 控制反转是目标；依赖注入是实现控制反转的一种手段。

**IOC的优点是什么？**

- 低侵入，松耦合
- 降低代码量
- 支持懒加载和即时加载。
- 容易测试，单元测试不再需要单例和JNDI查找机制。

**Spring IOC 的实现机制**⭐

- 工厂模式：可以把IOC容器看作是一个工厂，利用反射机制生成对象。
- 反射：在配置文件中定义Spring要生产的对象，由Spring根据类名生成对象

**IOC容器的初始化**⭐ [参考](https://blog.csdn.net/u010723709/article/details/47046211) [参考](https://blog.csdn.net/weixin_34190136/article/details/88873766) 

- Resource资源定位：查找数据
- 载入BeanDefinition：读取解析Resource定位的资源，把用户定义好的Bean表示成IOC容器内部的数据结构BeanDefinition
- 注册BeanDefinition：向IOC容器注册BeanDefinition，并将其注入到一个HashMap中

- 使用时遍历HashMap：逐条取出BeanDefinition，利用Java的反射机制实例化对象，将实例化后的对象保存在另外一个Map中

**spring 中有多少种 IOC 容器**⭐

| BeanFactory            | ApplicationContext      |
| ---------------------- | ----------------------- |
| 生产 bean 的工厂类     | 扩展了 BeanFactory 接口 |
| 懒加载                 | 即时加载                |
| 不支持基于依赖的注解   | 支持基于依赖的注解      |
| 语法创建和管理资源对象 | 自己创建和管理资源对象  |
| 不支持国际化           | 支持国际化              |

**懒加载和即时加载的区别**

- 懒加载
  - 优点：占用资源很少，对资源要求较高的应用比较有优势；
  - 缺点：速度会相对来说慢一些，有可能会出现空指针异常的错误
- 即时加载
  - 优点：应用启动时就加载所有的Bean，系统运行速度快且能尽早的发现系统中的配置问题

  - 缺点：把费时的加载操作放到了应用启动过程，消耗服务器的内存

#### 依赖注入DI⭐

**什么是Spring的依赖注入？ / 依赖注入（Dependency Injection）和依赖查找（Dependency Lookup）是什么？**

IOC主要实现方式有两种

- 依赖注入：IOC容器向代码动态注入对象需要的外部依赖
- 依赖查找：代码主动调用 IOC 容器提供的接口去获取Bean 对象

**依赖注入有什么优势**

- 查找操作与代码完全无关
- 不依赖于容器的API，可以很容易地在任何容器以外使用应用对象

**有哪些不同类型的依赖注入实现方式？**⭐

> 最好用构造器参数实现强制依赖，setter方法实现可选依赖。
>
> 接口注入：由于在灵活性和易用性比较差，从Spring4开始已被废弃

| 构造函数注入               | setter 注入                                                  |
| -------------------------- | ------------------------------------------------------------ |
| 通过类的构造器             | 通过调用bean的setter方法<br />（无参构造器或无参静态工厂方法实例化） |
| 没有部分注入               | 有部分注入                                                   |
| 适用于设置很多属性         | 适用于设置少量属性                                           |
| 不会覆盖 setter 属性       | 会覆盖 setter 属性                                           |
| 任意修改都会创建一个新实例 | 任意修改不会创建一个新实例                                   |

#### Spring Bean⭐

**概述**

**在Spring中BeanFactory和FactoryBean有什么区别呢**

- BeanFactory：一个工厂。生产 bean 的工厂类，提供了实例化对象和取出对象的功能
- FactoryBean：一个Bean。能生产和修饰对象，其实现类似工厂模式和修饰器模式

**Spring Bean的生命周期 / spring加载bean的流程是什么？**⭐ [参考](https://www.jianshu.com/p/1dec08d290c1) [参考](https://www.jianshu.com/p/9ea61d204559)

- 实例化：建立Bean，由BeanFactory读取Bean的定义文件，生成各个实例
- 属性注入：Setter注入，为new出来的对象填充属性
- 初始化：执行aware接口中的方法，完成`AOP`代理
  - BeanNameAware的setBeanName()：如果实现该接口，则执行其setBeanName方法
  - BeanFactoryAware的setBeanFactory()：如果实现该接口，则执行其setBeanFactory方法;
  - BeanPostProcessor的processBeforeInitialization()：如果有关联的processor，则在Bean初始化之前都会执行这个实例的processBeforeInitialization()方法;
  - **InitializingBean**的afterPropertiesSet()：如果实现了该接口，则执行其afterPropertiesSet()方法;
  - Bean定义文件中定义**init-method**;
  - BeanPostProcessors的processAfter0Initialization()：如果有关联的processor，则在Bean初始化之前都会执行这个实例的processAfterInitialization()方法;
- 销毁
  - DisposableBean的destroy()：如果Bean类实现了该接口，则执行它的destroy()方法;
  - 执行Bean定义文件中定义的destroy-method方法

<img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy90cEVJTGxFbHNrTGczWGZBa3V4VlB2elU4U3NGRnByZjAyQWh0MUhpYlNzaWEza214eWNUVHhqT0xweGZZaDNTUjFjZWlhVTNydUNPSWxkUkdkaGVhZTE1US82NDA?x-oss-process=image/format,png" alt="img" style="zoom:50%;" />

---

**Bean装配**

**什么是bean的自动装配？**

Spring 容器能够自动装配**相互依赖的bean**，而不用重复写已注入的bean关联

**解释不同方式的自动装配，spring 自动装配 bean 有哪些方式？**

@Autowired有如下选项：

- no：默认，不进行自动装配，需手工设置ref属性来装配
- **byName**：通过bean的名称装配
- **byType**：通过参数的数据类型装配
- constructor：通过构造函数装配，其中构造函数的参数通过byType装配
- autodetect：自动探测。如果有构造方法，通过 constructor 装配，否则使用 byType 装配。

**使用@Autowired注解自动装配的过程是怎样的？**

在启动IOC容器时，容器自动装载了一个AutowiredAnnotationBeanPostProcessor后置处理器（AOP？），当容器扫描到@Autowied、@Resource或@Inject时，就会在IOC容器自动查找需要的bean，并装配给该对象的属性。

- 如果查询结果刚好为一个，就将该bean装配给@Autowired指定的数据；
- 如果查询的结果不止一个，那么@Autowired会根据名称来查找；
- 如果上述查找的结果为空，那么会抛出异常。也可以设置 required=false。

**自动装配有哪些局限性？**

- 需要重写：需用配置来定义依赖，总要重写自动装配。
- 限制类型：不能自动装配简单的属性，如基本数据类型，String字符串和类。
- 模糊特性：自动装配不如显式装配精确

**@Autoweird注解在抽象类上生效吗 **[参考](http://t.zoukankan.com/jy107600-p-7490390.html)

不能。spring注入的是实例对象，实例化子类对象时，抽象父类没有实例化，不能自动装配。

方法：在抽象类中获取上下文对象，其是实例化产生的

**你可以在Spring中注入一个null 和一个空字符串吗？**

可以。

---

**bean作用域与线程安全**

**解释Spring支持的几种bean的作用域 / 讲讲springbean的scope**

- singleton（默认）: 单例，所有线程共享一个单例实例
- prototype：原型，每次从IOC容器中调用Bean时都返回一个新实例。
- request：请求，每次Http请求创建一个实例。适用于WebApplicationContext环境下。
- session：会话，同一会话共享一个实例。适用于WebApplicationContext环境下。
- global-session：全局会话，所有会话共享一个实例。适用于WebApplicationContext环境下。

**Spring中的bean是线程安全的吗？ **[参考](https://www.cnblogs.com/myseries/p/11729800.html) [参考](https://www.cnblogs.com/frankcui/p/14341795.html)

容器本身并没有提供Bean的线程安全策略，不过具体还是要结合具体scope分析

- 单例：所有线程共享一个单例实例Bean。线程不安全。

  > 但无状态的单例Bean是线程安全的，比如Spring mvc 的 Controller、Service、Dao等的Bean大多是无状态的
  >
  > 无状态即线程不会对Bean的成员执行查询以外的操作

- 原型：每次从IOC容器中调用Bean时都返回一个新实例。线程安全

**Spring中的bean如何保证线程安全？**

- 将bean的作用域scope改为原型……
- 使用ThreadLocal封装变量……

**spring的controller是单例还是多例，怎么保证并发的安全？**

在@Controller/@Service等容器中，默认情况下scope值是单例的，线程不安全。

- 将bean的作用域scope改为原型
- 使用ThreadLocal封装变量
- 尽量不要定义静态变量，因为静态变量属于类，故在单例和原型作用域下都线程不安全

#### 循环依赖⭐

**什么是循环依赖**

A依赖B的同时B也依赖了A，属性互相依赖、互相注入

测试知循环依赖会导致A生成B B生成A A又生成B……最终OOM

**循环依赖问题在Spring中主要有三种情况：**

- 通过构造方法进行依赖注入时产生的循环依赖问题。
- 通过setter方法进行依赖注入且是在多例模式下产生的循环依赖问题。
- 通过setter方法进行依赖注入且是在单例模式下产生的循环依赖问题。

在Spring中，只有第3种问题被解决了，其他两种方式在遇到循环依赖问题时都会产生异常。这是因为：

- 第一种，在new对象时就发生依赖注入了，从而导致循环依赖
- 第二种，每一次getBean()时都会产生一个新的Bean，反复下去就会有无穷无尽的Bean产生了，最终就会导致OOM问题的出现。

**循环依赖如何解决 / 三级缓存机制**⭐  [学习](https://hollis.blog.csdn.net/article/details/107551993) [学习](https://www.cnblogs.com/dalianpai/p/14265090.html) 

- 一级缓存：保存实例化、初始化都完成的对象
- 二级缓存：保存实例化完成，但初始化未完成的对象
- 三级缓存：保存对象工厂，如果对象被AOP代理，那么通过这个工厂获取到的就是代理后对象；如果对象没有被AOP代理，那么这个工厂获取到的就是实例化对象。

对象创建过程 

- 创建对象A，使用实例化后的对象A创建A对象工厂，并将其放入三级缓存。A属性注入时发现依赖B，转而去实例化B
- 创建B，注入属性时发现依赖A，从一级到三级缓存查询A，并在三级缓存通过对象工厂拿到A。然后把A放入二级缓存，删除三级缓存中的A。此时，B已经实例化且初始化完成，把B放入一级缓存。
- 继续创建A，从一级缓存拿到B。A创建完成，删除二级缓存中的A，把A放入一级缓存

<img src="https://typora-oss.oss-cn-beijing.aliyuncs.com/image-20210111234918883.png" alt="image-20210111234918883" style="zoom: 80%;" />

**为什么要使用三级缓存呢？二级缓存能解决循环依赖吗？**

如果使用二级缓存解决循环依赖，则意味着失去对象工厂，**Bean实例化后就要完成AOP代理**，违背了Spring设计的原则。

Spring在设计之初就是通过`AnnotationAwareAspectJAutoProxyCreator`这个后置处理器来在Bean生命周期的最后一步来完成AOP代理，而不是在实例化后就立马进行AOP代理。

### 面向切面编程AOP  

#### 核心概念

所谓AOP，即Aspect orientied program，面向方面(切面)的编程。

- 横切关注点：对哪些方法进行拦截，拦截后怎么处理，这些关注点称之为横切关注点
- 切面（aspect）：类是对物体特征的抽象，切面就是对横切关注点的抽象,面向切面编程，就是指 对很多功能都有的重复的代码抽取，再在运行的时候往业务方法上动态植入“切面类代码”。
- 连接点（joinpoint）：被拦截到的点，因为Spring只支持方法类型的连接点，所以在Spring中连接点指的就是被拦截到的方法，实际上连接点还可以是字段或者构造器
- 切入点（pointcut）：切入点在AOP中的通知和切入点表达式关联，指定拦截哪些类的哪些方法, 给指定的类在运行的时候动态的植入切面类代码。
- 通知（advice）：所谓通知指的就是指拦截到连接点之后要执行的代码，通知分为前置、后置、异常、最终、环绕通知五类
- 目标对象：被一个或者多个切面所通知的对象。
- 织入（weave）：将切面应用到目标对象并导致代理对象创建的过程
- 引入（introduction）：在不修改代码的前提下，引入可以在运行期为类动态地添加一些方法或字段
- AOP代理（AOP Proxy）：在Spring AOP中有两种代理方式，JDK动态代理和CGLIB代理。

> AOP编程其实是很简单的事情，程序员只需要参与三个部分：
>
> - 定义普通业务组件
> - 定义切入点，一个切入点可能横切多个业务组件
> - 定义增强处理，增强处理就是在AOP框架为普通业务组件织入的处理动作
>
> 所以进行AOP编程的关键就是定义切入点和定义增强处理，一旦定义了合适的切入点和增强处理，AOP框架将自动生成AOP代理
>
> 即：代理对象的方法=增强处理+被代理对象的方法。

#### 底层原理⭐

**AOP解决了什么问题**

- AOP是OOP（Object Oriented Programming，面向对象编程）的补充和完善。
- OOP引入封装、继承、多态等概念来建立一种对象层次结构，不过OOP允许开发者定义纵向的关系，但并不适合定义横向的关系，例如日志功能，只是为了满足这个单一需求， 就不得不在多个模块（方法）里多次重复相同的日志代码。如果日志需求发生变化, 必须修改所有模块。日志代码往往横向地散布在所有对象层次中，而与它对应的对象的核心功能毫无关系，这种散布在各处的无关的代码被称为**横切**（cross cutting）。在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。
- AOP技术恰恰相反，它利用一种称为"横切"的技术，并将那些影响了多个类的公共行为封装到一个可重用模块，便于减少系统的重复代码，降低模块之间的耦合度，并有利于未来的可操作性和可维护性。

**AOP 有哪些实现方式？**

- 静态代理 - 指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；
  - 编译时编织（特殊编译器实现）
  - 类加载时编织（特殊的类加载器实现）
- 动态代理 - 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。
  - JDK 动态代理
  - CGLIB

**Spring对AOP的支持 / 怎么实现动态代理？**⭐ [参考](https://zhuanlan.zhihu.com/p/70098824)

- 默认使用JDK动态代理：实现了被代理对象的接口

  - 创建需要被代理的**接口和实现类**
  - 创建**代理工厂类**，编写返回代理的get方法，即返回Proxy.newProxyInstance() 。在其中实现 InvocationHandler 接口以及接口的 invoke() 方法
  - 通过代理工厂类的get方法获得代理。通过代理调用方法，由代理最终调用之前写的 invoke() 方法

  > **newProxyInstance**(ClassLoader loader, Class[] interfaces, **InvocationHandler** h) 创建代理对象
  >
  > - loader：目标对象使用的类加载器
  > - interfaces：目标对象实现的接口类接口
  > - h：事件处理，执行目标对象的方法时，会触发事件处理器方法。new InvocationHandler() 需要实现invoke方法
  >
  > **invoke**(Object proxy, Method method, Object[] args) throws Throwable {...}  执行所调用的目标对象方法
  >
  > - proxy:　　JDK动态生成的最终代理对象
  > - method：方法的Method对象
  > - args:　　  调用对象方法时传入的参数
  >
  > 返回值Object为代理实例的方法调用返回的值。这个抽象方法在代理类中动态实现"。

- CGLIB（子类代理）：继承被代理对象。一个强大的高性能的代码生成包，可以在运行时扩展java类并实现java接口。

**JDK动态代理和CGLIB动态代理的区别**⭐

- JDK是实现被代理对象的接口；cglib是继承被代理对象（无法代理被final修饰的类）
- JDK直接写字节码；cglib使用ASM框架写字节码
- JDK通过反射机制调用方法；cglib通过Fashclass机制直接调用方法，效率更高
- cglib代理更复杂，生成代理类比JDK效率低

### Spring事务

#### 事务分类

- 编程式事务管理：Spring推荐使用TransactionTemplate，实际开发中使用声明式事务较多。
- 声明式事务管理：使用AOP实现，本质就是在目标方法执行前后进行拦截。即在目标方法执行前加入或创建一个事务，在执行方法执行后，根据实际情况选择提交或回滚事务。
  - 基于注解方式
  - 基于xml配置文件方式

**事务隔离级别**

1. ISOLATION_DEFAULT： 默认隔离级别，使用数据库默认的事务隔离级别。
2. ISOLATION_READ_UNCOMMITTED：读未提交
3. ISOLATION_READ_COMMITTED：读已提交
4. ISOLATION_REPEATABLE_READ：可重复读
5. ISOLATION_SERIALIZABLE：可串行化

**事务传播机制**

**Spring 的不同事务传播行为有哪些，干什么用的？**

当一个事务方法被另一个事务方法调用时，必须指定事务应该如何传播。

1. **PROPAGATION_REQUIRED**：如果存在一个事务，则支持当前事务，如果没有事务则开启。
2. **PROPAGATION_REQUIRES_NEW**：总是开启一个新的事务，如果一个事务已经存在，则将这个存在的事务挂起。
3. PROPAGATION_SUPPORTS：如果存在一个事务，支持当前事务。如果没有事务，则非事务的执行
4. PROPAGATION_MANDATORY： 如果已经存在一个事务，支持当前事务。如果没有一个活动的事务，则抛出异常。
5. PROPAGATION_NOT_SUPPORTED：总是非事务地执行，并挂起任何存在的事务。
6. PROPAGATION_NEVER：总是非事务地执行，如果存在一个活动事务，则抛出异常
7. PROPAGATION_NESTED：如果一个活动的事务存在，则运行在一个嵌套的事务中. 如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行

#### 事务原理⭐

**Spring的事务是怎么实现的** 

- AOP执行JDBC的事务提交及回滚动作
- 本质是数据库对事务的支持，没有数据库的事务支持，spring是无法提供事务功能的。

**transaction注解什么时候失效**

- 检查方法是不是public的（AOP）

- 检查是不是同一个类中的方法调用(如a方法调用同一个类中的b方法)

- 检查异常是不是unchecked异常

  如果想让checked异常也回滚，在注解上面写明异常类型即可: @Transactional(rollbackFor=Exception.class)

- 检查异常是不是被catch住了

- 检查数据库引擎是否支持事务

- 检查spring是否开启了对注解的解析

  <tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>

- 检查spring是否扫描到你使用注解事务的这个类所在的包

   <context:component-scan base-package="com.xxx.xxx" ></context:component-scan>

## SpringMVC

**Spring MVC的主要组件？** [参考](https://blog.csdn.net/baidu_38760069/article/details/96479412)

- 前端控制器DispatcherServlet：捕获来自客户端的请求和调度各个组件
- 处理器映射器HandlerMapping：根据url查找处理器
- 处理器适配器HandlerAdapter：执行处理器
- 处理器Handler：处理前端请求，完成业务逻辑
- 视图解析器ViewResolver：进行视图解析

**SpringMVC流程**⭐

1. 用户发起请求到`前端控制器`
2. `前端控制器`请求`处理器映射器`：查找`处理器`
3. `处理器映射器`向`前端控制器`返回执行链
4. `前端控制器`调用`处理器适配器`
5. `处理器适配器`执行`处理器`
6. `处理器`给`处理器适配器`返回ModelAndView
7. `处理器适配器`向`前端控制器`返回ModelAndView
8. `前端控制器`请求`视图解析器`进行视图解析
9. `视图解析器`向`前端控制器`返回View
10. `前端控制器`对视图进行渲染
11. `前端控制器`给用户响应结果

<img src="https://img2018.cnblogs.com/blog/660329/201909/660329-20190922093835529-1159443997.png" alt="img" style="zoom: 45%;" />

**SpringMVC中过滤器（Filter）和拦截器（Interceptor）区别**

1. Filter基于函数回调（doFilter()方法）；Interceptor基于Java反射（AOP思想）
2. Filter依赖Servlet容器；Interceptor不依赖Servlet容器
3. Filter几乎对所有的请求起作用；Interceptor只对action请求起作用
5. Filter只能在容器初始化时调用一次；Interceptor可以多次调用（在action的生命周期里）
6. Filter只能对request和response操作；interceptor可以对request、response、handler、modelAndView、exception进行操作
6. Interceptor可以访问Action的上下文，值栈里的对象，而Filter不能

## SpringBoot

**SpringBoot的启动过程**⭐ [参考](https://blog.csdn.net/Certain_/article/details/108125535?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242) [参考](https://baijiahao.baidu.com/s?id=1666047746833809358&wfr=spider&for=pc)

1. **创建并启动计时监控类**：监控并记录Spring Boot应用的启动时间

2. **声明应用上下文**对象和异常信息报告集合

3. 设置系统属性 headless 为true：表示运行一个headless服务器，可以用它来作一些简单的图像处理。

4. 创建所有Spring运行监听器并发布应用启动事件：获取配置的监听器名称并实例化所有的类。

5. 初始化默认应用的参数类：声明并创建一个应用参数对象

6. 准备环境：创建配置并绑定环境（通过property sources 和 profiles 等配置文件）

7. 创建Banner打印类：Spring Boot 启动时会打印 Banner 图片

8. **创建应用上下文**：根据不同的应用类型来创建不同的ApplicationContext上下文对象。

9. 实例化异常报告器：调用 getSpringFactoriesInstances() 方法来获取配置异常类的名称，并实例化所有的异常处理类。

10. **准备应用上下文**：主要作用是把上面已经创建好的对象，传递给prepareContext来准备上下文，例如将环境变量environment对象绑定到上下文中、配置bean生成器以及资源加载器、记录启动日志等操作。

11. **刷新应用上下文**：解析配置文件，加载bean对象，并且启动内置的web容器等操作。

12. 事件处理：这个方法的源码是空的，可以做一些自定义的后置处理操作。

13. **停止计时监控类**：停止此过程第一步中的程序计时器，并统计任务的执行信息。

14. 输出日志记录：把相关的记录信息，如类名、时间等信息进行控制台输出。

15. **发布应用上下文启动完成事件**：触发所有SpringApplicationRunListener监听器的started事件方法。

16. **执行所有Runner运行器**：执行所有的ApplicationRunner和CommandLineRunner运行器。

17. **发布应用上下文就绪事件**：触发所有的SpringApplicationRunListener监听器的running事件。

18. **返回应用上下文对象**：到此为止 Spring Boot 的启动程序就结束了

## Mybatis

**JPA 和 MyBatis 的区别?** [参考](https://my.oschina.net/u/2296445/blog/2249515?nocache=1539940192387)

JPA (Java Persistence API) 是 Sun 官方提出的 Java 持久化规范，Spring Data JPA一般会和Hibernate一起使用

- jpa是对象与对象之间的映射；mybatis是对象和结果集的映射。
- jpa自动生成sql语句，不用关心用什么数据库；mybatis使用sql语句，与数据库相关
- jpa修改相对简单；mybatis比较麻烦
- jpa适用与需求变化不多的中小型项目中，比如后台管理；mybatis适用于表关联较多的项目，持续维护开发迭代较快的项目，需求变化较多的项目，如互联网项目

**你说你用过MVC设计模式，除了MVC还有哪些？（ORM）**

**MVC**：模型(Model)，视图(View)，控制器(Controller)，出现的意义就是解耦      SpringMVC

- Model：负责业务对象和数据库的交互(ORM)。数据模型层，通常对数据加工和一些其它处理 ( 数据相关操作 )
- View：负责与用户的交互展示。视图层，跟用户交互的页面上东西。
- Controller：起到粘合剂的作用，接收请求参数调用模型和视图完成请求。业务处理层，处理业务逻辑。

**ORM**：对象关系映射，用于实现业务对象与数据表中的字段映射        Mybatis

比较流行 Sqlalchemy / Django自带的 Django ORM / 轻量级Peewee 。

- 优势：代码更加面向对象，代码量更少，灵活性高，提升开发效率。
- 缺点：拼接对象比较耗时，有一定性能影响

**一级缓存和二级缓存的区别** [参考](https://blog.csdn.net/qq_41541619/article/details/79967172?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&dist_request_id=1331645.22841.16184802181805299&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control) [参考](https://blog.csdn.net/qq_41541619/article/details/79967172?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control&dist_request_id=1331645.22841.16184802181805299&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-3.control)



**#{}和${}的区别是什么？**⭐

- #{}是预编译处理；${}是字符串替换。

- 处理#{}时，会将 sql 中的#{}替换为?号，调用 PreparedStatement 的 set 方法来赋值；处理${}时，会把${}替换成变量的值。

  故使用#{}可以有效的防止 SQL 注入，提高系统安全性。