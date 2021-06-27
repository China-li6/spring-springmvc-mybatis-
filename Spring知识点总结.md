# Spring知识点总结

 

1. ## 简介一下Spring框架。

答：Spring框架是一个开源的容器性质的轻量级框架。主要有三大特点：容器、IOC（控制反转）、AOP（面向切面编程）。

 

2. ## Spring框架有哪些优点？谈谈你的看法。

答：Spring框架主要有三大优点：

(1) 容器。Spring框架是一个容器，能够管理项目中的所有对象。

(2) IOC（控制反转）。Spring将创建对象的方式反转了，从程序员自己创建反转给了程序。

(3) AOP（面向切面）。面向切面编程，简而言之，就是将纵向重复的代码横向抽取出来。Spring框架应用了面向切面的思想，主要体现在为容器中管理的对象生成动态代理对象。

 

3. ## 什么是IOC？

答：IOC：控制反转，指得是将对象的创建权反转给Spring。作用是实现了程序的解耦合。、

 

4. ## 什么是DI？

答：DI：依赖注入，需要有IOC环境，在Spring创建Bean对象时，动态的将依赖对象注入到Bean对象中去。依赖注入最大的好处就是解耦合。

 

5. ## 你对Spring框架中的BeanFactory接口和ApplicationContext接口有什么理解？二者有什么区别？

答：BeanFactory接口是Spring框架的顶层接口，是最原始的接口，通过new （BeanFactory的实现类）来启动Spring容器时，并不会创建Spring容器里面的对象，只有在每次通过getBean()获得对象时才会创建。

ApplicationContext接口是用来替代BeanFactory接口的，通过new （ApplicationContext接口的实现类）ClassPathXmlApplicationContext来启动Spring容器时，就会创建容器中配置的所有对象。

 

6. ## Spring中的工厂容器有哪两个，它们的区别是什么？

答：BeanFactory和ApplicationContext。

BeanFactory接口是Spring框架的顶层接口，是最原始的接口，ApplicationContext是对BeanFactory扩展，BeanFactory在第一次getBean时才会初始化Bean, ApplicationContext是会在加载配置文件时初始化Bean。

BeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));

IHelloService helloService = (IHelloService) beanFactory.getBean("helloService");

helloService.sayHello();

 

我们刚开始学习用spring创建Bean对象的时候，最先学的是通过配置<Bean>标签的形式，这一块有很多的知识点需要我们掌握。

 

7. ## 谈谈你对Spring容器中Bean标签的理解。

答：Bean标签用来描述Spring容器管理的对象。

假如有一个User对象，需要交给Spring容器来管理，这样就需要在Spring容器的主配置文件中通过Bean标签来描述该对象。Bean标签常见的属性有以下几个：

name属性：给被管理的对象起个名称，获得对象时要根据该名称来获得。

class属性：被管理对象的完整类名。

scope属性：scope属性常见的有两个属性值，singleton和prototype，这两个属性值用 来指定创建对象时是单例还是多例，默认为singleton(单例)，但在整合整合struts2 时,ActionBean必须配置为多例的.

 

8. ## Spring通过配置<bean>标签来生成Bean对象有哪三种方式？

答：空参构造方式、静态工厂方式和实例工厂方式。一般都只会用空参构造方式。如下：

<bean id="bean1" class="cn.itcast.spring.b_instance.Bean1"></bean>

 

9. Spring框架中属性注入有哪几种方式：

答：Spring中的输入注入方式包括set方法注入、构造函数注入、p名称空间注入、spel注入，除此之外，还包括复杂方式注入，如数组、List、Map、Properties等属性的注入。



10. 简述一下bean的生命周期？

答：bean的生命周期包括bean的定义、bean的初始化、bean的调用和bean的销毁。

在配置文件里面通过<bean></bean>来完成bean的定义，通过配置init-method属性来完成bean的初始化，通过得到bean的实例对象来完成bean的调用，通过配置destory-method属性来完成bean的销毁。

 

11. 简述一下bean的作用域？

答：bean有5种作用域，分别是singleton（单例，默认）、prototype（默认）、request、session、globalSession。

singleton

当一个bean的作用域为singleton, 那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。

prototype

Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean() 方法）时都会创建一个新的bean实例。根据经验，对所有有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用 singleton作用域

request

在一次HTTP请求中，一个bean定义对应一个实例；即每次HTTP请求将会有各自的bean实例， 它们依据某个bean定义创建而成。该作用 域仅在基于web的Spring ApplicationContext情形下有效。

session

在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

global session

在一个全局的HTTP Session中，一个bean定义对应一个实例。典型情况下，仅在使用portlet context的时候有效。该作用域仅在基于 web的Spring ApplicationContext情形下有效。


以上讲了Spring容器的常见配置，比如向Spring容器中注册一个对象，需要在Spring的主配置文件中用Bean标签来描述该对象，在实际开发中，一个项目中会有特别多的对象，如果都用Bean标签来配置，未免太过麻烦了。基于此，Spring容器为我们提供了注解的概念，用注解来代替配置。

 

如何使用Spring中的注解？

 

在使用注解之前，要先在spring的主配置文件中通过Context:component-scan标签来开启使用注解的开关。



12. 用注解将对象注册到Spring容器当中，有几种注解方式？它们有什么区别吗？

答：4种。分别是：@Component()、@Service()、@Controller()、@Respository()。

Spring框架最早出现的只有@Component()注解，但如果所有的对象都使用同一个注解，很难区分对象究竟属于哪一层架构。基于此，Spring又推出了@Service()、@Controller()、@Respository()三种注解，用于区分对象属于哪一层架构。但4种注解方式从功能上来说没有任何区别。

 

13. Spring框架中，什么注解可以用来指定对象的作用范围？

答：@Scope(scopeName=”singleton”)。

 

14. 如何用注解的方式来完成属性注入？

答：按类型分可以分为值类型注入和引用类型注入。

值类型注入可以通过@Value()注解来完成，该注解既可以声明在属性上，也可以声明在方法上，建议声明在方法上，但是更多的人会声明在属性上，因为更方便。

引用类型注入可以通过三种注解方式来完成，分别为：@Autowired、@Autowired和@Qualifier()二者结合、@Resource()。建议使用@Resource()，但是一般我都会用@Autowired。

 

Spring中的AOP

 

1. 谈谈你对AOP的理解。

答：AOP，面向切面编程。简单来讲就是将纵向重复的代码，横向抽取出来。

很明显的一个体现就是在Filter过滤器中。在没有Filter之前，解决servlet的乱码问题是很复杂的，每次在接收请求之前，都要写句代码来解决乱码问题,即：request.setCharacterEncoding(“UTF-8”)，只要写一个servlet，就是写这句代码来解决乱码问题。直到有一天，Filter出现了。我们把解决乱码的那句代码放到Filter中去，从此在servlet中，就再也不用重复写那句代码了。从架构上来说，Filter解决乱码的事架在了所有的servlet上，这样一来，切面就形成了。

面向切面编程的思想，还有一个直接的体现就是在拦截器中。（后面待补充）

 

2. Spring中的AOP思想靠什么来体现的呢？

答：Spring中的AOP思想体现在能够为容器中管理的对象生成动态代理对象。

 

3. aop名词学习

Joinpoint（连接点）：目标对象中，所有可以增强的方法

Pointcut（切入点）：目标对象，已经增强的方法。

Advice（通知/增强）：增强的代码

Target（目标对象）：被代理对象

Weaving（织入）：将通知应用到切入点的过程

Proxy（代理）：将通知织入目标对象之后，形成代理对象

 

Spring中的AOP

 

1. 谈谈你对AOP的理解。

答：AOP，面向切面编程，是对OOP(面向对象编程)的补充和完善。简单来讲就是将纵向重复的代码，横向抽取出来。

很明显的一个体现就是在Filter过滤器中。在没有Filter之前，解决servlet的乱码问题是很复杂的，每次在接收请求之前，都要写句代码来解决乱码问题,即：request.setCharacterEncoding(“UTF-8”)，只要写一个servlet，就是写这句代码来解决乱码问题。直到有一天，Filter出现了。我们把解决乱码的那句代码放到Filter中去，从此在servlet中，就再也不用重复写那句代码了。从架构上来说，Filter解决乱码的事架在了所有的servlet上，这样一来，切面就形成了。

面向切面编程的思想，还有一个直接的体现就是在拦截器中。（后面待补充）

 

2. Spring中的AOP思想靠什么来体现的呢？

答：Spring中的AOP思想体现在能够为容器中管理的对象生成动态代理对象。

以前我们要使用动态代理，我们需要自己调用下面的方法：

Proxy.newProxyInstance(xx,xx,xx);

生成代理对象。

 

Spring能帮助我们生成代理对象。

 

为什么叫springAOP呢？

Spring可以为所有service层的类生成动态代理对象，告诉spring，为每一个service层的类里面的方法添加管理事务的代码，spring一听，是！这样我们叫可以只写一遍管理事务的代码，就为所有的service就都加上了管理事务的代码。这样一来，aop思想就体现出来了。

SpringAOP的本质就是帮我们生成动态代理对象。

 

Spring实现AOP的原理？

答：JDK动态代理和cglib代理。

JDK动态代理有缺陷，就是被代理对象必须实现接口才能产生代理对象，如果没有接口，就不能使用动态代理技术。我们用spring容器来实现动态代理，假如要管理的对象没有实现接口，那么就不能产生代理对象了。为了让所有的对象都能产生动态代理对象，Spring又融入了第三方代理技术cglib代理。Cglib可以对任何类生成代理对象，它的原理是对目标对象进行继承代理，如果目标对象被final修饰，那么该类无法被cglib代理。

 

那么Spring到底使用的是JDK代理，还是cglib代理呢？

答：混合使用。如果被代理对象实现了接口，就优先使用JDK代理，如果没有实现接口，就用用cglib代理。

 

3. aop名词学习

Aspect（切面）：通知和切入点的结合。在SpringAOP中，切面通过带有@Aspect注解的类实现。

Joinpoint（连接点）：目标对象中，所有可以增强的方法

Pointcut（切入点）：目标对象中，已经增强的方法。

Advice（通知/增强）：增强的代码

Target（目标对象）：被代理对象

Weaving（织入）：将通知应用到切入点的过程

Proxy（代理）：将通知织入目标对象之后，形成代理对象

 

4. Spring切面可以应用5种类型的通知，哪5种？

答：前置通知（Before）、后置通知（After，在方法完成之后调用通知，无论方法执行是否成功）、后置通知（After-returning，在方法成功执行之后调用通知）、异常通知（After-throwing，在方法抛出异常后调用通知）、环绕通知(Around，在目标方法之前之后都调用)。

 

5. Spring中应用aop，需要哪些步骤？

答：（1）导包

（2）准备目标对象

（3）准备通知

（4）将通知织入目标对象中

 

        Spring中配置aop实际上主要配的是第四步——将通知织入目标对象中。有两种方式，一种是xml配置方式，一种是注解配置方式。



Spring AOP的实现方式有哪些？

答：1、经典的基于代理的AOP：使用Java代码实现，编写Advice、PointCut，然后提供给Advisor使用。开启自动代理后，即可在applicationContext中获得增强后的bean。  

2、@AspectJ注解驱动的切面：基于注解的开发（推荐使用），在项目中需要开启AOP自动代理<aop:aspectj-autoproxy/>。

3、XML Schema方式：需要实现相应的增强接口，如BeforeAdvice、AfterAdvice等。然后利用一下配置如：

 

ofo面试题：aop机制，实现，具体怎样使用，具体到标签？

答： AOP，面向切面编程，是对OOP(面向对象编程)的补充和完善，简单来讲就是将纵向重复的代码，横向抽取出来。最明显的体现就是过滤器和拦截器的使用。

        Spring实现AOP的本质是动态代理，Spring采用两种动态代理的方式，分别是JDK动态代理和cglib动态代理。JDK动态代理的被代理对象必须实现接口，而cglib动态代理的被代理对象原则上可以是任何类，cglib实现动态代理的原理是对被代理对象进行继承，重写被代理对象中所有的方法，所以被代理对象不能被final修饰。
    
        Spring操作AOP，具体可分为4步。1.导包；2，准备目标对象；3.准备通知；4，将通知织入目标对象。其中2（准备目标对象）和3（准备通知）由java代码实现，4（将通知织入目标对象）有两种实现方式，一种是xml配置，一种是注解配置。其中，xml配置用的标签有<aop:config>、<aop:pointcut>、<aop:aspect>、<aop:before>、<aop:after-returning>、<aop:around>、<aop:after-throwing>、<aop:after>。注解配置用到的注解有：@Aspect、@Pointcut、@Before、@AfterReturning、@Around、@AfterThrowing、@After。

ofo面试题：Spring切面可以应用5种类型的通知，哪5种？

答：前置通知（Before）、后置通知（After-returning，在方法成功执行之后调用通知）、环绕通知(Around，在目标方法之前之后都调用)、异常通知（After-throwing，在方法抛出异常后调用通知)、后置通知（After，在方法完成之后调用通知，无论方法执行是否成功）。


Spring中的事务管理

 

1.  简单介绍一下Spring中的事务管理。

答：事务就是对一系列的数据库操作（比如插入多条数据）进行统一的提交或回滚操作，如果插入成功，那么一起成功，如果中间有一条出现异常，那么回滚之前的所有操作。这样可以防止出现脏数据，防止数据库数据出现问题。

现实世界中最常见的事务例子可能就是转账了。

 

2. 事务的4个特性？

答：ACID。

原子性（Atomic）：事务是由一个或多个活动所组成的一个工作单元。原子确保事务中的所有操作全部发生或全部不发生。如果所有的活动都成功了，事务也就成功了。如果任意一个活动失败了，整个事务也失败并回滚。

一致性（Consistent）：一旦事务完成（不管成功还是失败），系统必须确保它所建模的业务处于一致的状态。现实的数据不应该被损坏。

隔离性（Isolated）:事务允许多个用户对相同的数据进行操作，每个用户的操作不会与其他用户纠缠在一起。因此，事务应该被彼此隔离，避免发生同步读写相同数据的事情（注意的是，隔离性往往涉及到锁定数据库中的行或表）。

持久性（Durable）:一旦事务完成，事务的结果应该持久化，这样就能从任何的系统崩溃中恢复过来。这一般会涉及将结果存储到数据库或其他形式的持久化存储中。

 

通俗的说事务，指一组操作，要么都成功执行，要么都不执行（原子性）

在所有的操作没有执行完毕之前，其它会话不能够看到中间改变的过程（隔离性）

事务发生前和发生后，数据的总额保持不变（一致性）

事务产生的影响不能被撤销（持久性）



3. 在Spring的核心配置文件applicationContext.xml中配置事务，主要配置三大方面：事务管理器、事务通知和定义事务性切面。

代码如下：

<beans xmlns="http://www.springframework.org/schema/beans"

xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"

xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"

xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"

xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd

http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd

http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd

http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

<!-- 事务管理器 -->

<bean id="transactionManager"

class="org.springframework.jdbc.datasource.DataSourceTransactionManager">

<!-- 数据源 -->

<property name="dataSource" ref="dataSource" />

</bean>

<!-- 事务通知 -->

<tx:advice id="txAdvice" transaction-manager="transactionManager">

<tx:attributes>

<!-- 传播行为 -->

<tx:method name="save*" propagation="REQUIRED" />

<tx:method name="insert*" propagation="REQUIRED" />

<tx:method name="add*" propagation="REQUIRED" />

<tx:method name="create*" propagation="REQUIRED" />

<tx:method name="delete*" propagation="REQUIRED" />

<tx:method name="update*" propagation="REQUIRED" />

<tx:method name="find*" propagation="SUPPORTS" read-only="true" />

<tx:method name="select*" propagation="SUPPORTS" read-only="true" />

<tx:method name="get*" propagation="SUPPORTS" read-only="true" />

</tx:attributes>

</tx:advice>

<!-- 定义事务性切面 -->

<aop:config>

<aop:advisor advice-ref="txAdvice"

pointcut="execution(* cn.e3mall.service..*.*(..))" />

</aop:config>

</beans>

 

4. 事务管理器

        Spring管理事务有多种方式，但无论选择将事务编码到Bean中还是将选择其定义为切面，你都需要使用spring事务管理器与平台相关的事务进行交互。让我们看一下Spring的事务管理器能如何帮你避免直接与平台相关的实现打交道？Spring并不直接管理事务，而是提供了多种事务管理器，每个事务管理器都会充当某一特定平台的事务实现的门面，这使得门户在Spring中使用事务时，几乎不用关注实际的事务实现是什么。

  操作事务的方法一般就三种：开启事务、提交事务、回滚事务。针对这三个方法，JDBC、Hibernate、Mybatis等都有不同的实现方式。因为在不同平台、操作事务的代码各不相同。为此，Spring提供了一个接口PlatformTransactionManager，针对不同的操作平台提供不同的实现类。比方说，如果是JDBC和iBatis，实现类是DataSourceTransactionManager，如果是Hibernate，实现类是HibernateTransactionManager。不同的实现类就是不同的事务管理器，为了使用事务管理器，你需要将其声明在应用程序上下文中。

<!-- 事务管理器 -->

<bean id="transactionManager"

class="org.springframework.jdbc.datasource.DataSourceTransactionManager">

<!-- 数据源 -->

<property name="dataSource" ref="dataSource" />

</bean>

  如果在应用程序中你直接使用JDBC/iBatis来进行持久化，DataSourceTransactionManager会为你处理事务边界。为了使用DataSourceTransactionManager，你需要使用如下的XML将其装配到应用程序的上下文定义中：

 

  如果应用程序的持久化是通过Hibernate实现的，那么你需要使用HibernateTransactionManager，你需要使用如下的XML将其装配到应用程序的上下文定义中：

<!-- 事务管理器 -->

<bean id="transactionManager"

class="org.springframework.orm.hibernate3.HibernateTransactionManager">

<!-- 数据源 -->

<property name="dataSource" ref="dataSource" />

</bean>

  当配置好事务管理器之后，在spring中具体配置事务时，又有两种选择：一种是编码事务（了解），一种是声明式事务。声明式事务是通过事务属性来定义的，具体的说，是通过传播行为、隔离级别、只读提示、事务超时及回滚规则来进行定义的。

 

5. 事务的传播行为指什么？

答：在业务（service）方法之间平行调用时，如何来处理事务的问题。

 

6. Spring中定义事务传播行为的属性有几种，分别是什么？

答：7种。分别是REQUIRED、REQUIRES_NEW、SUPPORTS、NOT_SUPPORTED、MANDATORY、NESTED、NEVER。我们常用的是propagation=”REQUIRED”，默认的就是REQUIRED，指得是支持当前事务，如果不存在，就新建一个（默认），所以这个属性不用配置。其余6个属性几乎不用。

 

7. 事务中可能会出现的并发问题有哪些？

答：脏读、不可重复读、幻读。

解决并发问题的方法就是设置隔离级别。

 

    在理想情况下，事务之间是完全隔离的，从而可以防止这些问题发生，但是完全隔离会导致性能问题，因为它通常会涉及锁定数据库中的记录，侵占性的锁定会阻碍并发性，要求事务互相等待以完成各自的工作。
    
    考虑到完全的隔离会导致性能问题，而且并不是所有的应用程序都需要完全的隔离，所以有时应用程序需要在事务隔离上有一定的灵活性。因此就会有各种隔离级别。

 


8. 事务中有几种隔离级别呢？

答：4种。读未提交（uncommitted）、读已提交（committed）、可重复读（repeatable_read）、串行化（serializable）。

读未提交（uncommitted）：允许读取尚未提交的数据变更。可能会导致脏读、幻读或不可重复读。

读已提交（committed）:允许读取并发事务已经提交的数据。可以阻止脏读、但是幻读或不可重复读仍有可能发生。

可重复读（repeatable_read）：可以阻止脏读和不可重复读，但可能会导致幻读。

串行化（serializable）：完全服从ACID的隔离级别，确保阻止脏读、不可重复读以及幻读。这是最慢的事务隔离级别，因为它通常是通过完全锁定事务相关的数据库表来实现的。

 

9. 声明式事务的第三个属性是只读。read-only=”true” 或者 read-only=”false”，在定义查询方法时，通常设置为只读。

 

10. 事务超时和回滚规则 这两个属性几乎不用，用到的时候再查资料。

 

11. 基于在5大事务属性，就可以在XML中定义事务了。代码如下：

<!-- 事务通知 -->

<tx:advice id="txAdvice" transaction-manager="transactionManager">

<tx:attributes>

<!-- 传播行为 -->

<tx:method name="save*" propagation="REQUIRED" />

<tx:method name="insert*" propagation="REQUIRED" />

<tx:method name="add*" propagation="REQUIRED" />

<tx:method name="create*" propagation="REQUIRED" />

<tx:method name="delete*" propagation="REQUIRED" />

<tx:method name="update*" propagation="REQUIRED" />

<tx:method name="find*" propagation="SUPPORTS" read-only="true" />

<tx:method name="select*" propagation="SUPPORTS" read-only="true" />

<tx:method name="get*" propagation="SUPPORTS" read-only="true" />

</tx:attributes>

</tx:advice>

    <tx:advice>只是定义了AOP通知，用于把事务边界通知给方法，但是这只是事务通知，而不是完整的事务性切面。我们在<tx:advice>中没有声明哪些Bean应该被通知——我们需要一个切点来做这件事，为了完整定义事务切面，我们必须定义一个通知器(advisor)。这就涉及aop命名空间了。以下的XML定义了一个通知器，它使用txAdvice通知所有实现SpitterService接口的Bean，代码如下：

<!-- 定义事务性切面 -->

<aop:config>

<aop:advisor advice-ref="txAdvice"

pointcut="execution(* cn.e3mall.service..*.*(..))" />

</aop:config>

    <tx:advice>配置元素极大地简化了Spring声明式事务所需要的XML，如果我告诉你它甚至可以进一步简化，你会感受如何呢？如果我再告诉你，你只需要在Spring上下文中添加一行XML，即可声明事务，你的感受如何呢？这就是用注解的方式来配置事务。

 


Spring整合SSM

 

1.使用Spring整合SSM框架，需要配置哪些东西？简单说一下。

答：Spring整合Dao，要配置数据库启动，配置SqlSessionFactory，配置Mapper动态代理。

      Spring整合Service，要配置包扫描器（开启注解驱动），要配置事务管理（事务管理器，事务通知，事务切面）。
    
      Spring整合Web，要配置包扫描器（扫描Controller）,配置处理器映射器和处理器适配器（采用注解驱动的方法），配置视图解析器。

  在web.xml，要加载Spring容器，并且要配置一个springMVC的前端控制器。


2.Spring的配置有哪些？简单说一下。（ofo面试）

答：Spring整合Dao，要配置数据库启动，配置SqlSessionFactory，配置Mapper动态代理。

   Spring整合Service，要配置包扫描器（开启注解驱动），要配置事务管理（事务管理器，事务通知，事务切面）。

   Spring整合Web，要配置包扫描器（扫描Controller）,配置处理器映射器和处理器适配器（采用注解驱动的方法），配置视图解析器。

  在web.xml，要加载Spring容器，并且要配置一个springMVC的前端控制器。

3.Spring平时主要用到的东西有哪些？（ofo面试）

答：控制反转（IOC）、依赖注入（DI）、属性注入、事务管理、面向切面编程（AOP）等。
