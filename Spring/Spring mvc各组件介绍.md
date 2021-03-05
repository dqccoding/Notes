[TOC]

### 1.Spring MVC 和 servlet的区别？

https://blog.csdn.net/weixin_41399873/article/details/104932280

### 2.~~Spring MVC ServletContextListener是什么？~~

### 3.Spring MVC中的 listener filter servlet组件分别是什么？

![image-20210305111746926](D:\learn\Notes\Spring\Spring mvc各组件介绍.assets\image-20210305111746926.png)

![image-20210305111802547](D:\learn\Notes\Spring\Spring mvc各组件介绍.assets\image-20210305111802547.png)

### 4.如何理解spring中的 ApplicationContext接口？

参考: https://www.baeldung.com/spring-application-context#1-message-resolution

![image-20210303134243762](D:\learn\Notes\Spring\Spring mvc各组件介绍.assets\image-20210303134243762.png)

### 5.Spring MVC 启动流程详解

[SpringMVC启动过程详解（li）](https://www.cnblogs.com/RunForLove/p/5688731.html)

### 6.Servlet的本质是什么？它是如何工作的？

https://www.zhihu.com/question/21416727/answer/690289895

### 7.spring bean的生命周期？

![img](D:\learn\Notes\Spring\Spring mvc各组件介绍.assets\v2-55c5b61912d232437d49139c8b381b7a_720w.jpg)

第1步：调用bean的构造方法创建bean；

第2步：通过反射调用setter方法进行属性的依赖注入；

第3步：如果实现BeanNameAware接口的话，会设置bean的name；

第4步：如果实现了BeanFactoryAware，会把bean factory设置给bean；

第5步：如果实现了ApplicationContextAware，会给bean设置ApplictionContext；

第6步：如果实现了BeanPostProcessor接口，则执行前置处理方法；

第7步：实现了InitializingBean接口的话，执行afterPropertiesSet方法；

第8步：执行自定义的init方法；

第9步：执行BeanPostProcessor接口的后置处理方法。

这时，就完成了bean的创建过程。

在使用完bean需要销毁时，会先执行DisposableBean接口的destroy方法，然后在执行自定义的destroy方法。



### 8.Spring 的拓展接口？

**1.BeanFactoryPostProcessor**

是beanFactory后置处理器，支持在bean factory标准初始化完成后，对bean factory进行一些额外处理。在讲context初始化流程时介绍过，这时所有的bean的描述信息已经加载完毕，但是还没有进行bean初始化。例如前面提到的PropertyPlaceholderConfigurer，就是在这个扩展点上对bean属性中的占位符进行替换。

**2.BeanDefinitionRegistryPostProcessor**

它扩展自BeanFactoryPostProcessor，在执行BeanFactoryPostProcessor的功能前，提供了可以添加bean definition的能力，允许在初始化一般bean前，注册额外的bean。例如可以在这里根据bean的scope创建一个新的代理bean。

**3.BeanPostProcessor**

提供了在bean初始化之前和之后插入自定义逻辑的能力。与BeanFactoryPostProcessor的区别是处理的对象不同，BeanFactoryPostProcessor是对beanfactory进行处理，BeanPostProcessor是对bean进行处理。

**注：上面这三个扩展点，可以通过实现Ordered和PriorityOrdered接口来指定执行顺序。实现PriorityOrdered接口的processor会先于实现Ordered接口的执行。**

**4.ApplicationContextAware**

可以获得ApplicationContext及其中的bean，当需要在代码中动态获取bean时，可以通过实现这个接口来实现。

**5.InitializingBean**

可以在bean初始化完成，所有属性设置完成后执行特定逻辑，例如对自动装配对属性进行验证等等。

**6.DisposableBean**

用于在bean被销毁前执行特定的逻辑，例如做一些回收工作等。

**7.ApplicationListener**

用来监听spring的标准应用事件或者自定义事件。