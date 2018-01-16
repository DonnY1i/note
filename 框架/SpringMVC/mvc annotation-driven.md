## <mvc:annotation-driven />与<context:annotation-config />
1. <mvc:annotation-driven /> 是一种简写形式，完全可以手动配置替代这种简写形式，简写形式可以让初学都快速应用默认配置方案。  
<mvc:annotation-driven /> 会自动注册DefaultAnnotationHandlerMapping与AnnotationMethodHandlerAdapter 两个bean,是spring MVC为@Controllers分发请求所必须的。  
并提供了：数据绑定支持，@NumberFormat支持，@DateTimeFormat支持，@Valid支持，读写XML的支持（JAXB），读写JSON的支持（Jackson）。
2. 在基于主机方式配置Spring的配置文件中，你可能会见到<context:annotation-config/>这样一条配置，他的作用是式地向 Spring 容器注册 AutowiredAnnotationBeanPostProcessor、CommonAnnotationBeanPostProcessor、PersistenceAnnotationBeanPostProcessor 以及 RequiredAnnotationBeanPostProcessor 这 4 个BeanPostProcessor。  
注册这4个 BeanPostProcessor的作用，就是为了你的系统能够识别相应的注解。  
不过，我们使用注解一般都会配置扫描包路径选项<context:component-scan base-package=”XX.XX”/>  
该配置项其实也包含了自动注入上述processor的功能，因此当使用 <context:component-scan/> 后，就可以将<context:annotation-config/> 移除了。