## 1 Bean的声明

@Component

@Repository

@Service

@Controller

## 2 Bean的注入

@AutoWired：通过 byType 的方式去注入的，使用该注解，要求接口只能有一个实现类。

@Resource：可以通过 byName 和 byType的方式注入， 默认先按byName的方式进行匹配，如果匹配不到，再按 byType的方式进行匹配。

@Qualifier：按名称注入，但是注意是 **类名**。



// 完成扫描
// PostProcessor类型接口常用类分为两种，
// 一种是BeanFactoryPostProcessor，关于对象工厂BeanFactory创建完毕的回调处理；另一种是BeanPostProcessor关于通过对象工厂BeanFactory创建对象前后的回调处理
// BeanFactoryPostProcessor分两类
// 一类直接实现BeanFactoryPostProcessor#postProcessBeanDefinitionRegistry；另一类是BeanDefinitionRegistryPostProcessor#postProcessBeanFactory
PostProcessorRegistrationDelegate.invokeBeanFactoryPostProcessors(beanFactory, getBeanFactoryPostProcessors());

