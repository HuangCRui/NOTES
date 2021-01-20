![image-20210120213108246](../picture/Java%E9%9B%86%E5%90%88%E7%B1%BB/image-20210120213108246.png)





> 对于集合类：
>
> 1. 底层数据结构
> 2. 增删改查方式
> 3. 初始容量、扩容方式、扩容时机
> 4. 线程安全与否
> 5. 是否允许空，是否允许重复，是否有序



# ArrayList

> ```java
> public class ArrayList<E> extends AbstractList<E>
>         implements List<E>, RandomAccess, Cloneable, java.io.Serializable
> ```

## 概述



ArrayList是实现List接口的 **动态数组**，动态：大小可变。实现了所有可选列表操作，并允许包括null在内的所有元素。除了实现List接口外，还提供一些方法来操作内部用来存储列表的数组的大小



默认尝试容量为10，随着ArrayList中元素的增加，它的容量也会不断地自动增长

每次添加新的元素时，ArrayList都会检查是否需要记性扩容操作：数据向新数组的**重新拷贝**。如果知道具体业务数据量，在构造ArrayList时可以给ArrayList**指定一个初始容量**，减少扩容时数据的拷贝问题。

```java
public void ensureCapacity(int minCapacity) {
        if (minCapacity > elementData.length
            && !(elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA
                 && minCapacity <= DEFAULT_CAPACITY)) {
            modCount++;
            grow(minCapacity);
        }
    }
```

在**添加大量元素**前，应用程序也可以使用ensureCapacity操作来增加ArrayList实例的容量，这可以减少**递增式再分配**的数量。



**注意！ArrayList实现不是同步的。如果多个线程同时访问一个ArrayList实例，而其中至少一个线程从结构上修改了列表，那么它必须保持外部同步。所以为了保证同步，最好的办法是在创建时完成，以防止意外对列表进行不同步的访问：**

```java
List list = Collections.synchronizedList(new ArrayList<>());

public static <T> List<T> synchronizedList(List<T> list) {
    return (list instanceof RandomAccess ?
            new SynchronizedRandomAccessList<>(list) :
            new SynchronizedList<>(list));
}
```



## 继承关系

ArrayList继承AbstractList抽象父类，实现了List接口（规定了List的操作规范）、**RandomAccess（可随机访问）**、Cloneable（可拷贝）、Serializable（可序列化）。

![image-20210120215802748](../picture/Java%E9%9B%86%E5%90%88%E7%B1%BB/image-20210120215802748.png)





ArrayList底层是一个object数组，并且由transient修饰

```java
transient Object[] elementData; // non-private to simplify nested class access 
//ArrayList底层数组不会参与序列化，而是使用另外的序列化方式
```

> transient：
>
> 1. 一旦变量被transient修饰，变量将**不再是对象持久化的一部分**，该变量内容在序列化后无法获得访问。
> 2. transient关键字只能修饰变量，而不能修饰方法和类。注意，本地变量是不能被transient关键字修饰的。变量如果是用户自定义类变量，则该类需要实现Serializable接口。
> 3. 被transient关键字修饰的变量**不再能被序列化**，一个**静态变量**不管是否被transient修饰，均不能被序列化。

使用writeObject方法进行序列化。就是只复制数组中有值的位置，其他未赋值的位置不进行序列化，可以节省空间

```java
private void writeObject(java.io.ObjectOutputStream s)
        throws java.io.IOException {
        // Write out element count, and any hidden stuff
        int expectedModCount = modCount;
        s.defaultWriteObject();

        // Write out size as capacity for behavioral compatibility with clone()
        s.writeInt(size);

        // Write out all elements in the proper order.有值的位置
        for (int i=0; i<size; i++) {
            s.writeObject(elementData[i]);
        }

        if (modCount != expectedModCount) {
            throw new ConcurrentModificationException();
        }
    }
```



## 增删改查



添加元素时，首先判断**索引是否合法**，然后检测**是否需要扩容**，最后使用System.arraycopy方法来完成数组的复制



```java
public void add(int index, E element) {
    rangeCheckForAdd(index);//判断索引是否合法
    modCount++;
    final int s;
    Object[] elementData;
    if ((s = size) == (elementData = this.elementData).length)//elementData是否满了？
        elementData = grow();
    System.arraycopy(elementData, index,//新加入的Element下标为index，要空出来这个位置给新元素
                     elementData, index + 1,
                     s - index);
    elementData[index] = element;
    size = s + 1;
}

public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)。
```

arraycopy进行数组元素的复制。即从指定源数组中复制一个数组，复制从指定的位置开始，到目标数组的指定位置结束。

将源数组src从srcPos位置开始复制到dest数组中，复制长度为length，数据从dest的destPos位置开始粘贴。

**将要加入新元素位置后面的数往右移(若不传入index，则默认加到数组末尾)**



---

删除元素时，同样判断索引是否合法，删除的方式是 **把被删除元素右边的元素左移，也使用System.arraycopy进行拷贝**



```java
public E remove(int index) {//删除指定索引位置的元素
    Objects.checkIndex(index, size);
    final Object[] es = elementData;

    @SuppressWarnings("unchecked") E oldValue = (E) es[index];
    fastRemove(es, index);

    return oldValue;
}

private void fastRemove(Object[] es, int i) {
    modCount++;
    final int newSize;
    if ((newSize = size - 1) > i)
        System.arraycopy(es, i + 1, es, i, newSize - i);
    es[size = newSize] = null;
}
```



---



**清空数组，将所有元素置为null，这样就可以让GC自动回收掉没有被引用的元素了**

```java
public void clear() {
    modCount++;
    final Object[] es = elementData;
    for (int to = size, i = size = 0; i < to; i++)
        es[i] = null;
}
```



---



修改元素，只需要检查下标是否合法，就可以进行修改：

```java
public E set(int index, E element) {
    Objects.checkIndex(index, size);
    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```



---

检查下标：

```java
private void rangeCheckForAdd(int index) {
    if (index > size || index < 0)
        throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
}

//Object.checkIndex()
public static
    int checkIndex(int index, int length) {
        return Preconditions.checkIndex(index, length, null);
    }
//jdk.internal.util.Preconditions#checkIndex
public static <X extends RuntimeException>
    int checkIndex(int index, int length,
                   BiFunction<String, List<Integer>, X> oobef) {
        if (index < 0 || index >= length)
            throw outOfBoundsCheckIndex(oobef, index, length);
        return index;
    }
```



## modCount



ArrayList继承自AbstractList的field

```java
//java.util.AbstractList#modCount
protected transient int modCount = 0;
```



在一个迭代器最初始的时候会赋予它调用这个迭代器的对象的modCount，如果在迭代器**遍历的过程中，一旦发现这个对象的modCount和迭代器中存储的modCount不一样，就抛异常**



> ***Fail-Fast 机制*** 
>
> java.util.ArrayLis**t 不是线程安全**的，ArrayList，那么将抛出ConcurrentModificationException，这就是所谓fail-fast策略。
>
> 这一策略在源码中的实现是通过 modCount 域，modCount 顾名思义就是**修改次数**，**对ArrayList 内容的修改都将增加这个值**，那么在迭代器初始化过程中会将这个值赋给迭代器的 **expectedModCount**。
>
> 在**迭代过程中**，判断 modCount 跟 expectedModCount 是否相等，如果不相等就表示已经有其他线程修改了 ArrayList。
>
> 所以在这里和大家建议，当**遍历那些非线程安全的数据结构时，尽量使用迭代器**

```java
//java.util.ArrayList.Itr#expectedModCount
int expectedModCount = modCount;
```



## 初始容量和扩容方式



初始容量是**10**



```java
//jdk12
private void add(E e, Object[] elementData, int s) {
    if (s == elementData.length)
        elementData = grow();
    elementData[s] = e;
    size = s + 1;
}
private Object[] grow() {
    return grow(size + 1);
}
private Object[] grow(int minCapacity) {
    return elementData = Arrays.copyOf(elementData,newCapacity(minCapacity));
}
private int newCapacity(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    //当新容量大于最大数组容量时，执行大数扩容
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}

//大数扩容
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE)
        ? Integer.MAX_VALUE //就扩充为int最大值，原先为max_value - 8↓
        : MAX_ARRAY_SIZE;
}
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;


//初始化时的数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
private static final int DEFAULT_CAPACITY = 10;

//初始化时将eleData赋值为default数组
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}

//在grow中调用此方法
//如果数组还是初始数组，那么最小的扩容大小就是size+1和初始容量中较大的一个，初始容量为10。
private int newCapacity(int minCapacity) { //minCapacity是size+1
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1); //1.5倍扩容
    if (newCapacity - minCapacity <= 0) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            return Math.max(DEFAULT_CAPACITY, minCapacity); //如果扩容后小于10  就将capacoty容量设为10
        if (minCapacity < 0) // overflow
            throw new OutOfMemoryError();
        return minCapacity;
    }
    return (newCapacity - MAX_ARRAY_SIZE <= 0)
        ? newCapacity
        : hugeCapacity(minCapacity);
}

```

![image-20210121031313070](../picture/Java%E9%9B%86%E5%90%88%E7%B1%BB/image-20210121031313070.png)

**ensureCapacity在jdk12中已经弃用。。。。。。**

-> 真正执行扩容的方法`grow`



**为什么每次扩容处理会是1.5倍**，而不是2.5、3、4倍呢？

通过google查找，发现1.5倍的扩容是最好的倍数。因为一次性扩容太大(例如2.5倍)可能会浪费更多的内存(1.5倍最多浪费33%，而2.5被最多会浪费60%，3.5倍则会浪费71%……)。但是一次性扩容太小，需要**多次**对数组**重新分配**内存，对性能消耗比较严重。所以1.5倍刚刚好，既能满足性能需求，也不会造成**很大的内存消耗**。



ArrayList还提供了**将底层数组的容量调整为当前列表保存的实际元素的大小**的功能，它可以通过trimToSize()方法来实现。该方法可以最小化ArrayList实例的存储量

```java
//jdk12 已经是弃用了~~~
public void trimToSize() {
    modCount++;
    if (size < elementData.length) {
        elementData = (size == 0)
          ? EMPTY_ELEMENTDATA
          : Arrays.copyOf(elementData, size);
    }
}
```



---



```java
//这是jdk1.8
public boolean add(E var1) {
    this.ensureCapacityInternal(this.size + 1);
    this.elementData[this.size++] = var1;
    return true;
}

private void ensureCapacityInternal(int var1) {
    this.ensureExplicitCapacity(calculateCapacity(this.elementData, var1));
}

//精确扩容  
private void ensureExplicitCapacity(int var1) {
    ++this.modCount;
    if (var1 - this.elementData.length > 0) {
        this.grow(var1);
    }
}
private static int calculateCapacity(Object[] var0, int var1) {
    return var0 == DEFAULTCAPACITY_EMPTY_ELEMENTDATA ? Math.max(10, var1) : var1;
}
```







## 线程安全

ArrayList是 **线程不安全的**，在其迭代器iteator中，如果有多线程操作导致**modcount改变**，**会执行fastfail**。抛出异常。

```java
final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
```





# Vector









































































