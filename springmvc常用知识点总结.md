# [spring mvc常用知识点总结](https://www.cnblogs.com/panxuejun/p/6480778.html)

1.spring mvc是靠spring 启动的。通过springjar包的org.springframework.web.servlet.DispatcherServlet这个servlet类具体启动的。
<servlet-name>springmvc</servlet-name>
<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

这个dispatcherservlet这个类，有个contextConfigLocation参数，指定springmvc的配置文件xml。
2.理清一下思路：每个tomcat下的程序war包都有一个web.xml。web.xml可以启动servlet。
web.xml只能配置servlet相关信息，如servletmapping等等，不能配置bean的注入，扫描包路径，注解驱动等，这

3.二、配置解析

　　1.Dispatcherservlet

　　DispatcherServlet是前置控制器，配置在web.xml文件中的。拦截匹配的请求，Servlet拦截匹配规则要自已定义，把拦截下来的请求，依据相应的规则分发到目标Controller来处理，是配置spring MVC的第一步。4. <!-- scan the package and the sub package --><context:component-scan base-package="test.SpringMVC"/><!-- don't handle the static resource --><mvc:default-servlet-handler /><!-- if you use annotation you must configure following setting --><mvc:annotation-driven /><!-- configure the InternalResourceViewResolver --><bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"id="internalResourceViewResolver"><!-- 前缀 --><property name="prefix" value="/WEB-INF/jsp/" /><!-- 后缀 --><property name="suffix" value=".jsp" /></bean></beans>

5.spring mvc配置文件里的配置标签，必背：<context:component-scan base-package="test.SpringMVC"/><mvc:annotation-driven /><bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"<!-- scan the package and the sub package --><context:component-scan base-package="test.SpringMVC"/><!-- don't handle the static resource --><mvc:default-servlet-handler /><!-- if you use annotation you must configure following setting --><mvc:annotation-driven /><!-- configure the InternalResourceViewResolver --><bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"id="internalResourceViewResolver"><!-- 前缀 --><property name="prefix" value="/WEB-INF/jsp/" /><!-- 后缀 --><property name="suffix" value=".jsp" /></bean></beans>

6.spring mvc的那些controller标签，是注入到spring的ioc容器中的。不是spring mvc自己的ioc，spring mvc没有自己的ioc容器，都是spring的

7.@RequestBody

　　该注解用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上 ,再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上

　　@ResponseBody

　　该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区

8.requestParam正确用法@RequestParam(value = "invitCode", required = false) string invitCode
requestParam修饰的东西全在小括号里面（）。

9.四、自动匹配参数
//match automatically
@RequestMapping("/person")
public String toPerson(String name,double age){
System.out.println(name+" "+age);
return "hello";
}
　五、自动装箱

　　1.编写一个Person实体类
package test.SpringMVC.model;

public class Person {
public String getName() {
return name;
}
public void setName(String name) {
this.name = name;
}
public int getAge() {
return age;
}
public void setAge(int age) {
this.age = age;
}
private String name;
private int age;

}

　　2.在Controller里编写方法

//boxing automatically
@RequestMapping("/person1")
public String toPerson(Person p){
System.out.println(p.getName()+" "+p.getAge());
return "hello";
}


10.在SpringMVC中，bean中定义了Date，double等类型，如果没有做任何处理的话，日期以及double都无法绑定。


解决的办法就是使用spring mvc提供的@InitBinder标签

11.比较简单的可以直接应用springMVC的注解@initbinder和spring自带的WebDataBinder类和操作

[java] view plain copy
在CODE上查看代码片派生到我的代码片

@InitBinder
public void initBinder(WebDataBinder binder) {
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
dateFormat.setLenient(false);
binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, true));
}

还要在springMVC配置文件中加上

[html] view plain copy
在CODE上查看代码片派生到我的代码片

<!-- 解析器注册 -->
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
<property name="messageConverters">
<list>
<ref bean="stringHttpMessageConverter"/>
</list>
</property>
</bean>
<!-- String类型解析器，允许直接返回String类型的消息 -->
<bean id="stringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter"/>

这样就可以直接将上传的日期时间字符串绑定为日期类型的数据了