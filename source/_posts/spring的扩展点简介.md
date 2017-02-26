---
title: spring的扩展点简介
date: 2017-02-26 23:13:59
tags:
- spring
- java
categories:
- spring
---
#### 针对某一个bean扩展
##### Aware
Aware的一些子类：
![](https://ww4.sinaimg.cn/large/006tKfTcly1fbzvzjnjz8j30rq0lqq58.jpg)
常用的几个Aware：
![](https://ww3.sinaimg.cn/large/006tKfTcly1fbzw599f8uj31es09eaak.jpg)
> 通常，这些aware会在依赖装配后，初始化前进行回调（Invoked after population of normal bean properties but before an init callback）。

##### InitializingBean & DisposableBean
![](https://ww1.sinaimg.cn/large/006tKfTcly1fbzvxs6v0vj30fy0co3yk.jpg)
> 1. InitializingBean接口、@PostConstruct注解和init-method的xml配置，都能表示bean的初始化逻辑。
> 2. 同样，Disposable接口、@PreDestroy注解和destory-method的xml配置，都能表示bean的销毁的逻辑。
> 3. 从执行顺序上看，遵循 注解 -> 接口 -> xml配置 的回调顺序。

##### FactoryBean
> 用于自定义bean的初始化，对bean的创建、初始化有完全的控制权。

---
#### 针对所有bean扩展
##### BeanPostProcessor
> BeanPostProcessor在bean被装配后，init前、以及init后两个地方提供了扩展回调。

BeanPostProcessor的方法回调时机：
![](https://ww4.sinaimg.cn/large/006tKfTcly1fbzwaqtlj9j30nf096t9a.jpg)

BeanPostProcessor以及它的子接口：
![](https://ww2.sinaimg.cn/large/006tKfTcly1fbzmm1n5ynj31kw0gk76e.jpg)

###### InstantiationAwareBeanPostProcessor
> InstantiationAwareBeanPostProcessor在bean被实例化前、实例化后、属性装配前提供了扩展回调。

###### DestructionAwareBeanPostProcessor
> DestructionAwareBeanPostProcessor在非原型bean被销毁前提供了扩展回调。

---
#### 针对BeanFactory扩展
##### BeanFactoryPostProcessor
![](https://ww2.sinaimg.cn/large/006tKfTcly1fbzn9dhqi5j30ty0eoq3f.jpg)
> BeanFactoryPostProcessor在所有的bean创建前，对ConfigurableListableBeanFactory进行扩展，通常会对bean定义做一些修改。
> 最经典的子类是PropertyPlaceholderConfigurer，替换xml文件中的占位符，替换为properties文件中相应的key对应的value。
> 在容器启动时BeanFactoryPostProcessor的回调只会应用一次，而BeanPostProcessor会对每一个bean都应用，这里不要搞混了。

###### BeanDefinitionRegistryPostProcessor
> 在BeanFactoryPostProcessor回调前，提供了对BeanDefinitionRegistry的扩展回调，通常用于注册新的bean定义