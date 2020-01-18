# 1 Java8新特性

Java8（又称为jdk18）是Java语言开发的一个主要版本。Java8是 oracle公司于2014年3月发布，可以看成是自Java5以来最具革命性的版本。Java8为Java语言、编译器、类库、开发工具与JVM带来了大量新特性。

## 1.1 日期时间

### 1.1.1 LocalDate



### 1.1.2 LocalDateTime



### 1.1.3 DateTimeFormatter



### 1.1.4 ChronoUnit



## 1.1 Lambda表达式

Lambda是一个匿名函数，我们可以把Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

### 1.1.1 简介

    java8 于 2014 年发布，相比于 java7，java8 新增了非常多的特性，如 lambda 表达式、函数式接口、[方法引用](https://www.jianshu.com/p/62465b26818f)、[默认方法](https://www.jianshu.com/p/a58b6f5b0c54)、新工具（编译工具）、[Stream API](https://www.jianshu.com/p/2b40fd0765c3)、Date Time API、[Optional](https://www.jianshu.com/p/d81a5f7c9c4e) 等 。 当前很多公司的老产品依然使用的 java7，甚至开发人员开发新产品时依然没有选择升级， 写关于 java8 系列文章的目的在于梳理和分享 java8 新增的主要特性，开发时也可以用作参考。
    lambda 表达式是 java8 新增的主要特性之一，lambda 表达式又称闭包或匿名函数，主要优点在于简化代码、增强代码可读性、并行操作集合等。至于是否使用，有的同学觉得不适应，有的同学欲罢不能，见仁见智~
    
    技多不压身，本文将采用由浅入深的方式，讲解 java8 lambda 表达式的语法及使用，并附带代码进行演示。

### 1.1.2 Lambda语法

lambda的基本语法：lambda表达式的本质是作为接口的实例，接口必须是函数式接口！！！

左侧：形参列表，参数类型可以省略（类型推断）；只有一个形参的括号可以省略。

右侧：实例的方法体

```
(parameters) -> expression
or
(parameters) ->{ statements; }
```

lambda 表达式的特性：

1.  **可选类型声明**: 无需声明参数类型，编译器即可自动识别
2.  **可选的参数圆括号**: 仅有一个参数时圆括号可以省略
3.  **可选的大括号**：主体只包含一个语句时可省略大括号
4.  **可选的返回关键字**：主体只包含一个表达式返回值并省略大括号时，编译器会自动 return 返回值；有大括号时，需要显式指定表达式 return 了一个数值

特性示例：

```properties
// 格式一：无参，无返回值
() -> System.out.print("Java8 lambda.");

// 格式二：一个参数，没有返回值
(String str) -> System.out.print(str)

// 格式二：无参，有返回值
() -> 1 

// 格式三：有参，有返回值
x ->  5 * x 

// 格式四：无参，无返回值
(x, y) -> x - y

// 格式五：无参，无返回值
(int x, int y) -> x - y  
```

**个人建议，从程序的严谨性角度出发，尽量指明函数的参数类型，避免出错！！！**

### 1.1.3 Runnable接口

    用 lambdah 代替匿名类是 java8 中 lambda 的常用形式，本文以开发同学经常使用的 Runnable 接口匿名类为示例，演示如何用 lambda 表达式来代替匿名类：

1.  在 java8 之前：

```
   new Thread(new Runnable()
   {
    @Override
    public void run()
    {
         System.out.println("No use lambda.");
    }
   }).start();


```

2.  在 java8 之后：

```
   new Thread(() -> System.out.println("Use lambda")).start();


```

    可以看到，java8 中利用 lambda 表达式大大简化了代码编写。  
    此处简要提下，用 lambda 表达式代替匿名类的关键在于，匿名类实现的接口使用了 java.lang.FunctionalInterface 注解，且只有一个待实现的抽象接口方法，如 Runnable 接口：

```
  @FunctionalInterface
  public interface Runnable {
      public abstract void run();
  }


```

本文后面会详细讲解 FunctionalInterface 注解。

### 1.1.4 List迭代

    开发同学经常会使用到集合类，并对集合类对象进行迭代，以实现业务逻辑。  
    java8 中，集合类的顶层接口 java.lang.Iterable 定义了一个 forEach 方法：

```
    /* @param action The action to be performed for each element
     * @throws NullPointerException if the specified action is null
     * @since 1.8
     */
    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }


```

forEach 方法可以迭代集合的所有对象，其参数为 Consumer 对象，Consumer 类位于 java.util.function 包下，我们看下其定义：

```
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);

    default Consumer<T> andThen(Consumer<? super T> after) {
        Objects.requireNonNull(after);
        return (T t) -> { accept(t); after.accept(t); };
    }
}


```

到此已经很容易联想到，我们可以采用 lambda 表达式来实现 java8 集合的迭代逻辑，下面我们进行示例：

1.  在 java8 之前：

```
    List<Integer> features = Arrays.asList(1,2);
    for (Integer feature : features) {
         System.out.println(feature);
     }


```

2.  在 java8 之后：

```
    List<Integer> features = Arrays.asList(1,2);
    features.forEach(n -> System.out.println(n));


```

上述逻辑还可以用 java8 的方法引用来表示：

```
    List<Integer> features = Arrays.asList(1,2);
    features.forEach(System.out::println);


```

方法引用也是 java8 的新特性，由:: 操作符标示，详细可参考方法引用的文章，本文不赘述。

到这里，可以总结出，java8 中用 lambda 表达式代替匿名内部类，本质上是将接口定义为函数式接口，并将函数式接口隐式转换为 lambda 表达式。

## 1.2 Functional接口

### 1.2.1 简介

只包含一个抽象方法的接口，称为函数式接口。你可以通过 Lambda丧达式来创建该接口的对象（若 Lambda表达式抛出一个受检异常（即：非运行时异常），那么该异常需要在目标接口的抽象方法上进行声明）我们可以在一个接口上使用@ FunctionallInterface注解，这样做可以检查它是否是一个函数式接口。同时 javadoc也会包含一条声明，说明这个接口是一个函数式接口。在 java util. function包下定义了Java8的丰富的函数式接口。Jave从诞生日起就是一直倡导“一切皆对象”，在Java里面面向对象OOP）编程是一切。但是随着 python、 scala等语言的兴起和新技术的挑战，Java不得不做出调整以便支持更加广泛的技术要求，也即java不但可以支持OOP还
可以支持OOF（面向函数编程）在函数式编程语言当中，函数被当做一等公民对待。在将函数作为一等公民的编程语言中， Lambda表达式的类型是函数。但是在Java8中，有所不同。在Java8中， Lambda表达式是对象，而不是函数，它们必须依附于一类特别的对象类型——函数式接口。简单的说，在Java8中， Lambda表达式就是一个函数式接口的实例。这就是Lambda表达式和函数式接口的关系。也就是说，只要一个对象是函数式接口的实例，那么该对象就可以用 Lambda表达式来表示。所以以前用匿名实现类表示的现在都可以用 Lambda表达式来写。

### 1.2.2 函数式接口

```
在上一节中我们提到："用 lambda 表达式代替匿名类的关键在于，匿名类实现的接口使用了 java.lang.FunctionalInterface 注解，且只有一个待实现的抽象接口方法", 这里的接口便是函数式接口。  
函数式接口 (Functional Interface) 是 java8 新增的特性，它是一个**有且仅有一个抽象方法，但是可以有多个非抽象方法的接口**。函数式接口可以被隐式转换为 lambda 表达式。  
Runnable 接口是在 JDK1.8 之前已经存在的接口，在 JDK1.8 中加入了 @FunctionalInterface 注解，表示将其定义为一个函数式接口。在 JDK1.8 中定义的函数式接口还有：
```

- java.util.concurrent.Callable
- java.security.PrivilegedAction
- java.util.Comparator
- java.io.FileFilter
- java.nio.file.PathMatcher
- java.lang.reflect.InvocationHandler
- java.beans.PropertyChangeListener
- java.awt.event.ActionListener
- javax.swing.event.ChangeListener

JDK1.8 新增加的函数式接口有 **java.util.function** 包下的接口，典型的如上一节中提到的 Consumer 接口，感兴趣的读者可以阅读 JDK1.8 的源码，在此不逐个列出，在下一节本文还会列举 java.util.function 包中典型的函数式接口的使用。

### 1.2.3 内置四大接口

消费型：Consumer

供给型：Supplier

函数型：Function

断定型：Predicate

## 1.3 方法引用和构造器引用

当要传递给 Lambda体的操已经有实现的方法了，可以使用方法引用！
方法引用可以看做是 Lambda表达式深层次的表达。换句话说，方法引用就
是 Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向
个方法，可以认为是 Lambda表达式的一个语法糖。
要求：实现接口的抽象方法的参数列表和返回值类型，必须与方法引用的
方法的参数列表和返回值类型保持一致！
格式：使用操作符“∷将类（或对象）与方法名分隔开来。
如下三种主要使用情况：
对象：实例方法名

>类：静态方法名
>类：实例方法名

### 1.3.1 使用场景

当要传递的lambda体的操作，**已经有实现的方法**，可以使用方法引用。

方法引用的本质就是lambda表达式，而lambda表达式作为函数式接口的实例，所以方法引用，也是函数式接口的实例。

使用格式：类名（对象）::  方法名

三种情况

对象::非静态方法

类::静态方法

类::非静态方法

方法引用使用的要求：要求接口中的抽象方法的形参列表和返回值类型与方法引用的方法的形参列表和返回值类型相同！

## 1.4 Stream API

并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。相比较串行的流，并行的流可以很大程度上提高程序的执行效率。Java8中将并行进行了优化，我们可以很容易的对数据进行并行操作。Stream AP可以声明性地通过 paralle（）与 sequential（）在并行流与顺序流之间进行切换。

## 1.5 Optional





## 1.6 Nashorn





# 2 Java9新特性