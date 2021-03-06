<!-- GFM-TOC -->
* [基础](#基础)
    * [ final](#-final)
    * [初始化顺序](#初始化顺序)
    * [访问权限](#访问权限)
* [容器](#容器)
    * [Set](#set)
    * [Queue](#queue)
    * [Map](#map)
* [反射](#反射)
* [异常](#异常)
* [参考资料](#参考资料)
<!-- GFM-TOC -->

# 基础

##  final

**final 数据**

声明数据为常量，可以是编译时常量，也可以是在运行时被初始化后不能被改变的常量。

对于基本类型，final 使数值不变；对于引用对象，final 使引用不变，也就不能引用其它对象，但是被引用的对象本身是可以修改的。

**final 方法**

声明方法不能被子类覆盖。

private 方法隐式地被指定为 final，如果在子类中定义的方法和基类中的一个 private 方法签名相同，此时子类的方法不是覆盖基类方法，而是重载了。

**final 类**

声明类不允许被继承。

## 初始化顺序

static 声明的静态数据在内存中只存在一份，只在类第一次实例化时初始化一次，优先于其它数据的初始化。

```java
public static String staticField = "静态变量";
```

static 语句块和 static 数据一样在类第一次实例化时运行一次，具体哪个先运行取决于它们在代码中的顺序。

```java
static {
    System.out.println("静态初始化块");
}
```

普通数据和普通语句块的初始化在静态数据和静态语句块初始化结束之后。

```java
public String field = "变量";
```

```java
{
    System.out.println("初始化块");
}
```

最后才是构造函数中的数据进行初始化

```java
public InitialOrderTest()
{
    System.out.println("构造器");
}
```

存在继承的情况下，初始化顺序为：

1. 父类（静态数据、静态语句块）
2. 子类（静态数据、静态语句块）
3. 父类（数据、语句块）
4. 父类（构造器）
5. 子类（数据、语句块）
6. 子类（构造器）

## 访问权限

Java 中有三个访问权限修饰符：private、protected 以及 public，如果不加访问修饰符，表示包级可见。

可以对类或类中的成员（字段以及方法）加上访问修饰符。成员可见表示其它类可以用成员所在类的对象访问到该成员；类可见表示其它类可以用这个类创建对象，可以把类当做包中的一个成员，然后包表示一个类，这样就好理解了。

protected 用于修饰成员，表示在继承体系中成员对于子类可见。但是这个访问修饰符对于类没有意义，因为包没有继承体系。

# 容器

![](https://github.com/CyC2018/InterviewNotes/blob/master/pics/114c49a6-72e3-4264-ae07-c564127094ac.png)

容器主要包括 Collection 和 Map 两种，Collection 又包含了 List、Set 以及 Queue。

## Set

- HashSet：使用 Hash 实现，支持快速查找，但是失去有序性；

- TreeSet：使用树实现，保持有序，但是查找效率不如 HashSet；

- LinkedListHashSet：具有 HashSet 的查找效率，且内部使用链表维护元素的插入顺序，因此具有有序性。

## Queue

只有两个实现：LinkedList 和 PriorityQueue，其中 LinkedList 支持双向队列。

## Map

- HashMap：使用 Hash 实现

- LinkedHashMap：保持有序，顺序为插入顺序或者最近最少使用（LRU）顺序

- TreeMap：基于红黑树实现

- ConcurrentHashMap：线程安全 Map，不涉及同步加锁

# 反射

每个类都有一个 **Class** 对象，包含了与类有关的信息。当编译一个新类时，会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。

类加载相当于 Class 对象的加载。类在第一次使用时才动态加载到 JVM 中，可以使用 Class.forName('com.mysql.jdbc.Driver.class') 这种方式来控制类的加载，该方法会返回一个 Class 对象。

反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。

Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库包含了 **Field**、**Method** 以及 **Constructor** 类。可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段，可以使用 invoke() 方法调用与 Method 对象关联的方法，可以用 Constructor 创建新的对象。

IDE 使用反射机制获取类的信息，在使用一个类的对象时，能够把类的字段、方法和构造函数等信息列出来供用户选择。

# 异常

Throwable 可以用来表示任何可以作为异常抛出的类，分为两种：**Error** 和 **Exception**，其中 Error 用来表示编译时系统错误。

Exception 分为两种：**受检异常** 和 **非受检异常**。受检异常需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；非受检异常是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序奔溃并且无法恢复。

![](https://github.com/CyC2018/InterviewNotes/blob/master/pics/48f8f98e-8dfd-450d-8b5b-df4688f0d377.jpg)

# 参考资料

- Eckel B, 埃克尔 , 昊鹏 , 等 . Java 编程思想 [M]. 机械工业出版社 , 2002.
- [Java 类初始化顺序 ](https://segmentfault.com/a/1190000004527951)
