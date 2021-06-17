



# Java异常







程序执行出现异常，退出方法，抛出一个封装了错误信息的对象。方法会立刻退出，后面的代码都无法执行了。

![image-20210531102922596](../picture/Java基础/image-20210531102922596.png)





## 异常分类





Throwable 是 Java **所有错误或异常的超类，下一层分为 Error 和 Exception**



- Error

Error类是指 Java 运行时系统的内部错误和资源耗尽错误。**应用程序不会抛出该类对象**。如果出现了这样的错误，除了告知用户，剩下的就是**尽力使程序安全的终止**

----

- Exception（RunTimeException(UncheckedException)，CheckedException（实际不存在））

RuntimeException 如 ： NullPointerException 、 ClassCastException ； 一 个 是 检 查 异 常CheckedException，如 I/O 错误导致的 IOException、 SQLException。 RuntimeException 是那些可能在 Java 虚拟机**正常运行期间抛出的异常**的超类。 如果出现 RuntimeException，那么**一定是程序员的错误**  



检查异常 CheckedException： 一般是**外部错误(不是程序员的问题，需要手动捕捉)**，这种异常都发生在编译阶段， Java 编译器会强制程序去捕获此类异常，即会出现要求你把这段可能出现异常的程序进行 try-catch 或 throw，该类异常一般包括几个方面：

1. 试图在文件尾部读取数据
2. 试图打开一个错误格式的 URL
3. 试图根据给定的字符串查找 class 对象，而这个字符串表示的类并不存在  





## Throw 和 Throws



throws 用在函数上，后面跟的是这个方法中捕捉的**异常类**，

throw用在方法内，后面跟的是**异常对象**



**throws 用来声明异常**，让调用者只知道**该功能可能出现的问题**，可以给出预先的处理方式； throw 抛出具体的问题对象，**执行到 throw，功能就已经结束了**，***跳转到调用者***，**并将具体的问题对象抛给调用者**。也就是说 throw 语句独立存在时，下面不要定义其他语句，因为**执行不到**。  



throws 表示**出现异常的一种可能性**，并**不一定会发生这些异常**； throw 则是抛出了异常，***执行 throw 则一定抛出了某种异常对象***。  



两者都是**消极处理异常**的方式，只是抛出或者可能抛出异常，但是**不会由函数去处理异常**，真正的处理异常由函数的**上层调用处理**。  







# Java 反射







**动态语言**，程序在运行时可以改变其结构，引进新的函数，删除已有的函数等结构上的变化，js，ruby，py都是，java是半动态语言



----

**反射机制概念（运行状态中知道类所有的属性和方法）**

![image-20210531122101136](../picture/Java基础/image-20210531122101136.png)

在 Java 中的反射机制是指在运行状态中，对于任意一个类都能够知道这个类**所有的属性和方法**；并且对于任意一个对象，都能够**调用它的任意一个方法**；这种**动态获取信息以及动态调用对象**方法的功能成为 Java 语言的反射机制  





---

**反射的应用场景**

**编译时类型和运行时类型**

java中许多对象在运行时都是会出现两种类型：编译时类型和运行时类型。

**编译时类型由  <声明对象时  使员工的类型来决定>，运行时类型由  <实际赋值给对象的类型决定>**

如：  `Person p = new Student();`，其中编译时类型为 Person，运行时类型为 Student。  



**编译时类型无法获取具体方法  **

运行时可能接收到**外部传入**的对象，该对象编译时 **类型为 Object，但是程序又需要调用该对象的运行时类型的方法。**

解决这些问题，***程序需要在运行时发现  对象和类的真实信息，才能够调用运行时类型的方法。***

如果编译时根本无法预知该对象和类属于哪些类，程序只能依靠运行时信息来发现该对象和类的真实信息。这时就必须使用反射了！



---

反射API

**反射API用来生成JVM中的类、接口或对象中的信息**

1. Class类：反射的核心类，可以获取类的属性，方法等信息
2. Field类：Java.lang.reflect包中的类，表示类的**成员变量**，可以用来获取和设置类之中的属性值
3. Method类：Java.lang.reflect包中的类，表示类的 **方法**，可以用来获取类中的方法信息或者执行方法
4. Constructor类：Java.lang.reflect包中的类，表示类的 **构造方法**



----

反射使用功能步骤（获取 Class 对象，调用对象方法）

1. 获取想要操作的类的Class对象，他是反射的核心，通过**Class对象**我们可以**任意调用类的方法**

2. 调用 Class 类中的方法，就是反射的使用阶段
3. 使用反射 Api来操作这些信息









---

**获取 Class 对象的 3 种方法**

- 调用某个对象的 getClass()方法  

```
Person p = new Person();
Class clazz = p.getClass();
```

- 调用某个类的 class 属性来获取该**类对应的 Class 对象**  

```
Class clazz=Person.class;
```

- 使用 Class 类中的 **forName()静态方法**(最安全/性能最好)  ，同时也将这个Class对象加载到内存中。

```
Class clazz=Class.forName("类的全路径"); (最常用)
```



当我们获得了想要操作的类的 Class 对象后，可以通过 Class 类中的方法获取并查看该类中的方法和属性。  

```java
Class clazz=Class.forName("reflection.Person");

Method[] method=clazz.getDeclaredMethods();

Field[] field=clazz.getDeclaredFields();

Constructor[] constructor=clazz.getDeclaredConstructors();

Constructor[] constructor=clazz.getDeclaredConstructors();
```





---

**创建对象的两种方法**



1. Class对象的 newInstance() 

**使用Class对象的newInstance()  方法来创建该Class对象对应类的实例，但是这种方法要求该Class对象对应的类有默认的空构造器**



2. 调用 Constructor 对象的 newInstance()

先使用 Class 对象获取指定的 **Constructor 对象**，再调用 Constructor 对象的 newInstance() 方法来创建 Class 对象对应类的实例,通过这种方法可以**选定构造方法创建实例**。  

```java

//获取 Person 类的 Class 对象
Class clazz=Class.forName("reflection.Person");

//使用.newInstane 方法创建对象
Person p=(Person) clazz.newInstance();

//获取构造方法并创建对象
Constructor c=clazz.getDeclaredConstructor(String.class,String.class,int.class);

//创建对象并设置属性
Person p1=(Person) c.newInstance("李四","男",20);
```





## class.forName 和 classLoader的区别





class.forName：

- 将类的.class文件加载到 jvm 中

- 对类进行解释，**执行类中的 static 块**



classLoader：

- 只干一件事：将.class文件加载到jvm中，不会执行static中的内容，**只有在newInstance才会去执行static块。**



Class.forName(className)：

- 内部调用的方法是 `Class.forName(className,true,classloader);`
  第2个boolean参数表示**类是否需要初始化**，  Class.forName(className)**默认是需要初始化。**
  一旦初始化，就会**触发目标对象的static块代码执行**，**static参数也也会被再次初始化。**



classLoader.loadClass(className)：

- 内部调用的方法是`ClassLoader.loadClass(className,false);`
  第2个 boolean参数，表示目标对象是否进行链接，**false表示不进行链接**，**链接阶段的准备过程对非final的静态变量初始化零值**，不进行链接意味着不进行包括初始化等一些列步骤，那么静态块和静态对象就不会得到执行







## 动态代理、静态代理的区别和使用场景



区别：

静态代理通常只代理 **一个类**，动态代理是代理一个接口下的 **多个实现类**

静态代理事先知道要代理的是什么。动态代理不知道要代理什么东西，**只有在运行时才知道**



动态代理是实现 JDK 里的 `invovationHandler` 接口的 `invoke` 方法，***<u>代理的是接口</u>***，也就是**业务类必须要实现接口**，通过 `Proxy` 里的 `newProxyInstance` 得到代理对象

还有一种动态代理：CGLIB，***<u>代理的是类</u>***，不需要业务类继承接口，**通过派生的子类来实现代理**，通过在运行时，动态修改字节码达到修改类的目的。**得到的代理类是被代理类的子类**





AOP 编程就是基于动态代理实现的，比如著名的 Spring 框架、Hibernate 框架等等都是动态代理的使用例子。

![image-20210127151855139](../picture/Java基础/image-20210127151855139.png)



基本的动态代理：

```java
public static void main(String[] args) {
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println(method);
                //对方法进行判断，来决定如何实现该方法
                if(method.getName().equals("morning")){
                    System.out.println("Good morning, " + args[0]);
                }
                return null;
            }
        };

        Hello hello = (Hello) Proxy.newProxyInstance(
                Hello.class.getClassLoader(),
                new Class[]{Hello.class},
                handler
        );

        hello.morning("Huang");
        /*
        自动生成了Method对象作为成员变量，调用这个method.invoke来调用动态代理生成的方法
        private static java.lang.reflect.Method com.test.$Proxy0.m1
        private static java.lang.reflect.Method com.test.$Proxy0.m2
        private static java.lang.reflect.Method com.test.$Proxy0.m3
        private static java.lang.reflect.Method com.test.$Proxy0.m0
         */
  System.out.println(Arrays.toString(hello.getClass().getDeclaredFields()));
        System.out.println( Hello.class.getClassLoader());
    }

    interface Hello {
        void morning(String name);
    }
```



在运行期动态创建一个`interface`实例的方法如下：

1. 定义一个**InvocationHandler**实例，它负责==实现接口的方法调用==；
2. 通过**Proxy.newProxyInstance()**创建**interface**实例，它需要3个参数：
   1. 使用的`ClassLoader`，通常就是**接口类的`ClassLoader`**；
   2. ***需要实现的接口数组***，至少需要*传入一个接口*  进去；
   3. 用来**<u>处理接口方法调用</u>**的InvocationHandler实例。
3. 将返回的`Object`**强制转型**为接口。

动态代理实际上是JDK在**运行期**动态创建**class字节码并加载的**过程，它并没有什么黑魔法，把上面的动态代理改写为静态实现类大概长这样：



==自动生成==了：

```java
public class HelloDynamicProxy implements Hello {
    //参数  handler
    InvocationHandler handler;
    public HelloDynamicProxy(InvocationHandler handler) {
        this.handler = handler; //这就是Proxy.newProxyInstance中传入的handler
    }
    
    //并不是内嵌了
    public void morning(String name) {
        //调用handler的invoke方法
        //invoke方法已经进行过重写  @override
        handler.invoke(
           this,
           Hello.class.getMethod("morning"),
           new Object[] { name });
    }
}
```

其实就是==JDK帮我们自动编写了一个上述类==（不需要源码，==**可以直接生成字节码**==）

> 并不存在可以直接实例化接口的黑魔法 :sweat_smile:



Java标准库提供了动态代理功能，允许在运行期动态创建一个接口的实例；

动态代理是**通过`Proxy`创建代理对象**，然后**将接口方法“代理”给`InvocationHandler`完成的（通过调用InvocationHandler来调用我们实际实现该接口的方法）。**











Spring AOP：通过动态代理，实现增强方法

```java
public static void main(String[] args) {

    //创建目标对象
    Target target = new Target();
    //获得增强对象
    Advice advice = new Advice();

    //返回值  就是动态生成的代理对象
    //代理对象和目标对象是兄弟关系，用接口来接收
    TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
            target.getClass().getClassLoader(),//目标对象类加载器
            target.getClass().getInterfaces(),//目标对象相同的接口字节码对象数组
            new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    advice.before();//前置增强
                    Object invoke = method.invoke(target, args);//执行目标方法
                    advice.after();//后置增强
                    return invoke;
                }
                //调用代理对象的任何方法，实质执行的都是invoke方法
            }
    );

    //调用代理对象的方法
    proxy.save();//save running
}
```









## **类什么时候被初始化？**



- 创建一个类的实例，也就是 new 一个对象
- 访问一个类的 **静态变量**，或者对其赋值
- 调用类的 **静态方法**
- 反射（`Class.forName("com.hcx.load")`）
- 初始化一个类的子类（会首先初始化子类的父类）
- JVM 启动时标明的启动类，即文件名和类名相同的那个类





## 类的初始化步骤





1. 如果这个类没被加载和链接，先加载和链接
2. 如果这个类存在直接父类，且父类没被初始化，那么直接初始化父类（接口除外）
3. 如果类中存在**初始化语句：如 `static 变量`和 `static块`** ，**依次** 执行这些初始化语句

在一个类加载器中，类只能初始化一次



































# Java 注解







Annotation 注解 是java提供的对源程序中元素关联信息和元数据  metadata的途径和方法。

Annotation是一个接口，程序可以通过**反射来获取**指定程序中元素的 Annotation 对象，然后通过该 Annotation 对象来**获取注解中的元数据信息**





---

四种标准元注解

元注解的作用是负责**注解其他注解**。 Java5.0 定义了 4 个标准的 meta-annotation 类型，它们被用来提供**对其它 annotation 类型作说明**。  



- `@Target` 修饰的**对象范围** 

@Target说明了Annotation所修饰的对象范围： Annotation可被用于 **packages**、 **types**（类、接口、枚举、 Annotation 类型）、**类型成员**（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、 catch 参数） 。在 Annotation 类型的声明中使用了 target 可更加明晰其修饰的目标  

```java
@Target({ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD, ElementType.ANNOTATION_TYPE})
@Target({ElementType.TYPE})
```



- `@Retention` 定义 **被保留的时间长短**  

Retention 定义了**该 Annotation 被保留的时间长短**：表示需要在**什么级别**保存注解信息，用于描述注解的生命周期（即：被描述的注解**在什么范围内有效**），取值（RetentionPoicy）有：
 SOURCE:在**源文件**中有效（即源文件保留）
 CLASS:在 **class 文件**中有效（即 class 保留）
 RUNTIME:在**运行时**有效（即运行时保留）  

```java
@Retention(RetentionPolicy.RUNTIME)
@Retention(RetentionPolicy.SOURCE)
```



---

第一类是由编译器使用的注解   `RetentionPolicy.SOURCE`

> - `@Override`：让编译器检查该方法是否正确地实现了覆写；
> - `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。

这类注解不会被编译进入`.class`文件，它们***在编译后就被编译器扔掉了***。



第二类是由工具处理==.class==文件使用的注解  `RetentionPolicy.CLASS`

比如有些工具会在加载class的时候，**对class做动态修改**，实现一些特殊的功能。这类注解会被编译进入`.class`文件，但***加载结束后并  <不会存在于内存中 >***。这类注解只被一些**底层库使用**，一般我们不必自己处理。





第三类是在==程序**运行**期==**能够读取**的注解  `RetentionPolicy.RUNTIME` ，

它们在加载后一直**存在于JVM**中，这也是最常用的注解。例如，一个配置了`@PostConstruct`的方法会在**调用构造方法后自动被调用**（这是Java代码**读取该注解实现的功能**，JVM并不会识别该注解）。













- `@Documented` 描述  javadoc  

@ Documented 用于描述其它类型的 annotation 应该被作为被标注的程序成员的公共 API，因此可以被例如 javadoc 此类的工具文档化。  



- `@Inherited` 阐述了某个被标注的类型是被继承的  

@Inherited 元注解是一个标记注解， @Inherited 阐述了某个被标注的类型是**被继承的**。如果一个使用了@Inherited 修饰的 annotation 类型**被用于一个 class，则这个 annotation 将被用于该 class 的子类。**  







![image-20210531153752747](../picture/Java基础/image-20210531153752747.png)



---

**注解处理器**

如果没有用来读取注解的方法和工作，那么注解也就不会比注释更有用处了。

使用注解的过程中，很重要的一部分就是创建于使用注解处理器。 Java SE5 扩展了**反射机制的 API**，以帮助程序员快速的**构造自定义注解处理器**。 下面实现一个注解处理器。  



```java
//定义注解
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitProvider {
    /**供应商编号*/
    public int id() default -1;
    /*** 供应商名称*/
    public String name() default "";
    /** * 供应商地址*/
    public String address() default "";
}
```



```java
//注解使用
public class Apple {
    
	@FruitProvider(id = 1, name = "陕西红富士集团", address = "陕西省西安市延安路")
    private String appleProvider;
    
    public void setAppleProvider(String appleProvider) {
    	this.appleProvider = appleProvider;
    }
    
    public String getAppleProvider() {
    	return appleProvider;
    }
}

```

**注解处理器**

```java
//注解处理器

public class FruitInfoUtil {
    public static void getFruitInfo(Class<?> clazz) {
        String strFruitProvicer = "供应商信息： ";
        
        Field[] fields = clazz.getDeclaredFields();//通过反射获取处理注解
        for (Field field : fields) {
            //判断该field是否有FruitProvider这个注解？
            if (field.isAnnotationPresent(FruitProvider.class)) {
                //获得这个注解对象
                FruitProvider fruitProvider = (FruitProvider) field.getAnnotation(FruitProvider.class);
                //注解信息的处理地方
                strFruitProvicer = " 供应商编号： " + fruitProvider.id() + " 供应商名称： "
                + fruitProvider.name() + " 供应商地址： "+ fruitProvider.address();
                System.out.println(strFruitProvicer);
        	}
        }
    }
}


public class FruitRun {
    public static void main(String[] args) {
        FruitInfoUtil.getFruitInfo(Apple.class);
        /**************输出结果***************/
        // 供应商编号： 1 供应商名称：陕西红富士集团 供应商地址：陕西省西安市延
    }
}


```







# Java内部类





Java类中可以定义field和method，还可以定义类，**内部类**

内部类分为：**静态内部类，成员内部类，局部内部类，匿名内部类**四种



## 静态内部类



```java
public class Out {
    
    private static int a;
    private int b;
    
    public static class Inner {
        public void print() {
        	System.out.println(a);
        }
    }
}
```





1. 静态内部类可以访问外部类所有的静态变量和方法，即使是 private 的也一样。
2. 静态内部类和一般类一致，可以定义静态变量、方法，构造方法等。
3. 其它类使用静态内部类需要使用“**外部类.静态内部类**”方式，如下所示： Out.Inner inner =new Out.Inner();inner.print();
4. Java集合类HashMap内部就有一个静态内部类Entry。 Entry是HashMap存放元素的抽象，HashMap 内部维护 Entry 数组用了存放元素，但是 **Entry 对使用者是透明的**。像这种**和外部类关系密切的，且不依赖外部类实例的，都可以使用静态内部类**。  





## 成员内部类





定义在类内部的非静态类，就是成员内部类。

成员内部类**不能定义静态方法和变量**（final 修饰的除外）。这是因为成员内部类是非静态的， 类初始化的时候**先初始化静态成员**，如果允许成员内部类定义静态变量，那么**成员内部类的静态变量初始化顺序是有歧义的**。  







## 局部内部类  定义在方法中的类



定义在方法中的类，就是局部类。如果一个类只在某个方法中使用，则可以考虑使用局部类。  

```java
public class Out {
    private static int a;
    private int b;
    
    public void test(final int c) {
        final int d = 1;
        class Inner {
            public void print() {
            	System.out.println(c);
        	}
    	}
    }
}
```





## 匿名内部类







**匿名内部类必须要继承一个   父类或者是吸纳一个接口，当然也仅能继承一个父类或者实现一个接口。**

**没有class关键字，因为匿名内部类直接使用new来生成一个对象的引用**

`new Comparator<>(){public int compare(...)}`



```java
public abstract class Bird {
    private String name;
    
    public String getName() {
    	return name;
    }
    
    public void setName(String name) {
    	this.name = name;
    }
    
    public abstract int fly();
}
```



```java
public class Test {
    public void test(Bird bird){
        System.out.println(bird.getName() + "能够飞 " + bird.fly() + "米");
    }
    
    public static void main(String[] args) {
        Test test = new Test();
        //需要实现抽象类或者接口的方法，相当于 class Temp implements xxx / extends xxx{  @override ... }
        test.test(new Bird() {
            
            public int fly() {
                return 10000;
            }
            
            public String getName() {
                return "大雁";
            }
        });
    }
}
```







# Java 泛型





泛型提供了编译时类型安全检测机制，允许程序眼子啊编译时检测到非法的类型。

本质是：参数化类型，**所操作的数据类型被指定为一个参数**。

比如要写一个排序方法，需要对 整型、字符串数组进行排序，使用泛型



---

泛型方法

泛型方法在调用的时候可以**接收不同类型的参数**。根据传递给泛型方法的参数类型，编译器处理每个方法调用

```java
// 泛型方法 printArray
public static <E> void printArray( E[] inputArray )
{
    for ( E element : inputArray ){
    	System.out.printf( "%s ", element );
    }
}
```

> 1. <? extends T>表示该通配符所代表的类型是 **T 类型的子类**
> 2. <? super T>表示该通配符所代表的类型是 **T 类型的父类**  





----

泛型类 <T>

泛型类的声明和非泛型类的声明类似，在类名后面添加了类型参数声明部分。

泛型类的类型参数声明部分也**包含一个或多个类型参数**，参数间用逗号隔开。一个泛型参数，也被称为**一个类型变量**，是用于**指定一个泛型类型名称的标识符**。因为他们接受一个或多个参数，这些类被称为参数化的类或参数化的类型。  

```java
public class Box<T> {
    
    private T t;
    
    public void add(T t) {
    	this.t = t;
    }
    
    public T get() {
    	return t;
	}
}
```





---

类型通配符 ？

使用 ？ 代替具体的类型参数。如 ： List<?>在逻辑上是 List<String>, List<Integer> 等所有List<具体类型实参>的父类。



---

**类型擦除**

**java中的泛型基本上都是在编译器这个层次来实现的。在生成的java字节码中是不包含泛型中的类型信息的。**

使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉。这个过程称为类型擦除

如在代码中定义的 List<Object>和 List<String>等类型，在**编译之后都会变成 List**。 JVM **看到的只是 List**，而由泛型附加的类型信息对 JVM 来说是不可见的。

类型擦除的基本过程也比较简单，首先是找到**用来替换类型参数的具体类**。（将所有的泛型参数用其最左边界（最顶级的父类型）类型替换）。这个具体类一般是 Object。如果指定了**类型参数的上界**的话，则使用这个上界。

**把代码中的类型参数都替换成具体的类**  









# Java序列化



---

- **持久化对象及其状态到内存或者磁盘**

在内训中创建可复用的java对象，但一般只有当JVM运行时这些对象才可能存在，这些对象的生命周期不会比JVM的生命周期更长

**在现实应用中，可能要求在JVM停止运行后能够保存（持久化）指定的对象，并在将来重新读取被保存的对象**





---

***序列化对象移字节数组保持，静态成员不保存***

使用 Java 对象序列化， 在保存对象时，会把其状态保存为一组字节，在未来， 再**将这些字节组装成对象**。必须注意地是， 对象序列化保存的是**对象的”状态”，即它的成员变量（静态变量是类变量，不是对象的成员变量）**。由此可知，对象序列化**不会关注类中的静态变量**  





---

***序列化用户远程对象传输***

**使用RMI（远程方法调用），或在网络中传递对象时，都会用到对象序列化**。java序列化API为处理对象序列化提供了一个标准机制。





---

***Serializable 实现序列化***

在 Java 中， 只要一个类**实现了 java.io.Serializable 接口**，那么它就可以被序列化  



---

通过 ObjectOutputStream 和 ObjectInputStream 对对象进行序列化及反序列化  

在类中增加 writeObject 和 readObject 方法可以实现自定义序列化策略。  



虚拟机是否允许反序列化，不仅取决于类路径和功能代码是否一致，一个非常重要的一点是**两个类的序列化 ID 是否一致**（就是 private static final long serialVersionUID）  





---

***Transient 关键字阻止该变量被序列化到文件中***



1. 在变量声明前加上 Transient 关键字，可以**阻止**该变量被序列化到文件中
2. 在被反序列化后，transient 变量的值被**设为初始值**，如int是0，对象是null
3. 服务器端给客户端发送序列化对象数据，对象中**有一些数据是敏感的**，比如密码字符串等，希望**对该密码字段在序列化时，进行加密**，而客户端如果拥有解密的密钥，只有**在客户端进行反序列化时**，才可以**对密码进行读取**，这样可以一定程度**保证序列化对象的数据安全**。  





# Java 复制





将一个对象的引用复制给另一个对象，三种方式：

1. 直接赋值
2. 浅拷贝
3. 深拷贝



---

1. 直接赋值复制。

直接赋值。A a1 = a2，这实际上是复制的引用，**a1 和 a2指向的是同一个对象**。a1变化时，a2也会跟着变化



2. 浅拷贝。复制引用但不复制引用的对象

创建一个新对象，然后将当前对象的非静态字段复制到该新对象。

**如果字段是值类型的（基本类型），那么对该字段执行复制；如果该字段是引用类型的话，则复制引用但不复制引用的对象，因此，原始对象机器副本引用同一个对象**





3. 深拷贝。复制对象和其引用对象

深拷贝不仅复制对象本身，而且复制对象包含的引用指向的所有对象。  



![image-20210601141539208](../picture/Java基础/image-20210601141539208.png)









---

**序列化**（**深拷贝一种实现**）

在java语言里深拷贝一个对象，常常可以先使对象实现Serializable接口，然后把对象吓到一个流里，再读出来，便可以重建对象

































































































































































































































