## 1. Bean的声明

@Component

@Repository

@Service

@Controller

## 2. Bean的注入

@AutoWired：通过 byType 的方式去注入的，使用该注解，要求接口只能有一个实现类。

@Resource：可以通过 byName 和 byType的方式注入， 默认先按 byName的方式进行匹配，如果匹配不到，再按 byType的方式进行匹配。

@Qualifier：按名称注入，但是注意是 **类名**。