# Spring 思想

#### IoC (Invertion of Control 控制反转)

使用对象时候由主动new对象转换成由外部提供对象,此过程中对象的创建权由程序转移到外部，这种思想叫做控制反转
##### Spring技术对此提供的实现:
Spring提供了一个容器，称为IOC容器，用来充当IOC思想中的外部
IOC容器负责对象的创建、初始化等一系列工作，被创建或被管理的对象在IOC容器中统称为Bean

##### IoC的实现方法-DI (Denpendency Injection依赖注入):







**注** 依赖注入（Dependency Injection, DI）只有在 **Spring 容器管理的 Bean** 中才会生效，在 Spring 开发中，除了简单的实体类（POJO/Entity/DTO），对于 Service、Controller、Repository 等组件，**永远不要使用 `new` 关键字**，需要用到谁，就用 `@Autowired`

