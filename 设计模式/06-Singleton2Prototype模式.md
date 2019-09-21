# 1. 概念理解

单例模式的定义：保证一个类仅有一个实例，并提供一个访问它的全局访问点，想确保任何情况下都绝对只有一个实例。在内存里只有一个实例，减少了内存开销，可以避免对资源的多重占用，设置全局访问点，严格控制访问。

![](http://upload-images.jianshu.io/upload_images/15921086-9128c832c16c058b.gif) 单例模式. gif

上图中 Singleton 是单例类，Client 是调用单例类的客户端，Client 通过调用 Single 的 getSingleton() 获取实例对象。

# 2. 单例模式

单例模式有多种写法，每种写法都有利弊，这里只讲 5 种常用的写法。

## 2.1 常用的写法

### 2.1.1 饿汉模式

```java
public class Singleton {
    
    private static Singleton instance = new Singleton(); 
    
    private Singleton() {
        
    }
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

饿汉模式的利弊：

*   饿汉模式是基于类加载机制，避免了多线程的同步问题。
*   饿汉模式在类加载的时候就完成了实例化，所以类加载较慢，获取实例对象的速度非常快。
*   饿汉模式在类加载的时候就完成了实例化，没有达到懒汉加载的效果，所以如果从始至终都没有使用到该实例对象，就会造成内存的浪费。

### 2.1.2. 懒汉模式（线程不安全）

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

懒汉模式（线程不安全）的利弊：

*   懒汉模式（线程不安全）在多线程的情况下可能产生线程的同步问题。
*   懒汉模式（线程不安全）声明了一个静态对象，在第一次调用 getInstance() 的时候完成静态对象的实例化，所以类加载很快，第一次获取实例对象的时候较慢。
*   懒汉模式（线程不安全）达到了懒汉加载的效果，所以即便是从始至终没有使用到该实例对象，也不会造成内存的浪费。

### 2.1.3. 懒汉模式（线程安全）

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}


```

懒汉模式（线程安全）的利弊：

*   懒汉模式（线程安全）使用 synchronized 关键字声明 getInstance() 方法，避免了多线程的同步问题，所以是线程安全的。
*   懒汉模式（线程安全）每次调用 getInstance() 方法都需要进行同步，这会造成不必要的同步开销，而且大部分时候是用不到同步的，所以不推荐使用这种方式。

### 2.1.4. 双重检查模式（DCL）

```java
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

双重检查模式（DCL）的利弊：

*   双重检查模式（DCL）的静态对象使用 volatile 关键字声明或多或少会影响性能，但是考虑到程序的正确性，牺牲这点性能还是值得的。
*   双重检查模式（DCL）在 getInstance() 方法中会对静态对象进行两次判空，第一次是为了不必要的同步，第二次是在静态对象为 null 时完成实例化。
*   双重检查模式（DCL）的优点是资源利用率高，在第一次调用 getInstance() 的时候完成实例化，效率高；缺点是第一次调用 getInstance() 的时候要进行一次同步和两次判空，反应稍慢一些。
*   双重检查模式（DCL）虽然在一定程度上解决了内存的浪费，多余的同步和线程安全等问题，但是在某些情况下还是会出现失效的问题，推荐使用静态内部类单例模式。

### 2.1.5. 静态内部类单例模式

```java
public class Singleton {

    private Singleton() {

    }

    public static Singleton getInstance() {
        return SingletonHolder.sInstance;
    }

    private static class SingletonHolder {
        private static final Singleton sInstance = new Singleton();
    }
}
```

静态内部类单例模式的利弊：

*   静态内部类单例模式在类加载的时候虚拟机不会去加载 SingletonHolder，所以 sInstance 实例对象也不会被初始化，只有调用 getInstance() 方法时，虚拟机才会去加载 SingletonHolder 并初始化 sInstance 实例对象。
*   静态内部类单例模式不仅能确保线程安全，还能保证实例对象的唯一性，也不会造成内存的浪费，所以推荐使用这种方式。

## 2.2 私有构造器



## 2.3 线程安全





## 2.4 延迟加载





## 2.5 序列化和反序列化安全



## 2.6 反射破坏





# 3. 多例模式

定义：指原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象，不需要知道任何创建的细节，不调用构造函数。

类初始化消耗较多资源
new产生的一个对象需要非常繁琐的过程（数据准备、访问权
限等）
构造函数比较复杂
循环体中生产大量对象时

原型模式性能比直接new-个对象性能高
简化创建过程

必须配备克隆方法
对克隆复杂对象或对克隆出的对象进行复杂改造时，容易引入风险
深拷贝、浅拷贝要运用得当

## 3.2 深克隆/浅克隆



总结
==

从以上介绍中了解到单例的构造函数是私有的，只对外提供一个 getInstance() 方法用来获取实例对象。单例模式的利弊主要是针对内存，同步和线程安全这几个方面的，所以在日常开发中，可以根据实际项目需求选取单例模式的实现。