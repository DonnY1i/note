内容协商视图ContentNegotiatingViewResolver是Spring3.0中新增加的一种视图解析器。他不负责具体的视图解析器，而是作为一个中间角色根据请求所要求的MIME类型来从上下文种选择一个合适的视图解析器来解析视图。通过这种视图我们可以混合使用多种视图技术。最常见是混合使用jsp,xml,json的视图。  
例子：  
SpringMVC配置
```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"   
    xmlns:mvc="http://www.springframework.org/schema/mvc"   
    xsi:schemaLocation="
           http://www.springframework.org/schema/beans     
           http://www.springframework.org/schema/beans/spring-beans-4.2.xsd     
           http://www.springframework.org/schema/context     
           http://www.springframework.org/schema/context/spring-context-4.2.xsd    
           http://www.springframework.org/schema/mvc     
           http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd"> 
                    
    <mvc:annotation-driven content-negotiation-manager="contentNegotiationManager"/>  
     
    <context:component-scan base-package="com.blog.controller"/>
    <!-- 配置静态资源 -->
    <mvc:default-servlet-handler />
     
    <!--配置视图解析器 -->
    <bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
        <property name="contentNegotiationManager"  ref="contentNegotiationManager"/>
        <property name="viewResolvers">
            <list>
                <bean class="org.springframework.web.servlet.view.BeanNameViewResolver" />
                <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                    <property name="prefix" value="/WEB-INF//"></property>
                    <property name="suffix" value=".jsp"></property>
                    <property name="viewClass" value = "org.springframework.web.servlet.view.JstlView"></property>
                </bean>
            </list>
        </property>
 
        <property name="defaultViews">
            <list>
                <bean  class="org.springframework.web.servlet.view.xml.MappingJackson2XmlView" />
                <bean  class="org.springframework.web.servlet.view.json.MappingJackson2JsonView" />                
            </list>
        </property>
    </bean>
 
    <bean id="contentNegotiationManager"  class="org.springframework.web.accept.ContentNegotiationManagerFactoryBean">
        <property name="mediaTypes">
            <map>
                <entry key="json"  value="application/json"/>
                <entry key="xml"  value="application/xml"/>
                <entry key="html"  value="text/html"/>
            </map>
        </property>
        <property name="defaultContentType"  value="application/json"/> 
        <property name="ignoreAcceptHeader"  value="true"/> 
        <property name="favorPathExtension"  value="true"/> 
         
    </bean>
 
</beans>
```
ContentNegotiatingViewResolver的配置主要分为3个部分，viewResolvers，defaultViews，contentNegotiationManager三个部分。  
contentNegotiationManager用来配置视图对请求的处理，设置请求的媒体类型，响应方式。defaultContentType用来设置默认的视图，这里我设置的是json,ignoreAcceptHeader用来设置是否要使用报文头来确定请求的类型，favorPathExtension设置是否根据拓展名确定视图类型，这两个我都设置成true。
## @RestController
MVC4为了支持Rest提供了一个新的控制器，这个控制器继承了@Controller,但是它使用起来更加简单，配置文件种只需要配置一下jsp页面视图就可以了。
```XML
    <bean  class="org.springframework.web.servlet.view.InternalResourceViewResolver">
                    <property name="prefix" value="/WEB-INF//"></property>
                    <property name="suffix" value=".jsp"></property>
                    <property name="viewClass" value = "org.springframework.web.servlet.view.JstlView"></property>
    </bean>
```
```Java
package com.blog.controller;
 
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;
 
import com.blog.domain.User;
import com.blog.enums.RoleEnum;
 
@RestController
@RequestMapping("/rest")
public class RestFulController {
     
    @RequestMapping(method = RequestMethod.GET, value = "user")
    public RESTFul<?> user(ModelMap model) {
        User user = new User();
        user.setId(1);
        user.setName("cxzhao");
        user.setPassword("123123");
        user.setRole(RoleEnum.admin);
        RESTFul<User> rest  =new RESTFul<User>();
        rest.setData(user);
        return rest;
    }
     
    @RequestMapping(method = RequestMethod.GET, value = "index")
    public ModelAndView index() {
        ModelAndView mv = new ModelAndView();
        mv.setViewName("/client/index");
        return mv;
    }   
}
```