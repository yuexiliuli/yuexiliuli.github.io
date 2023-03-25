---
title: Serializable接口
urlname: ynfztc
date: '2021-05-15 21:27:15 +0800'
tags: [Java]
categories: [Java]
---

# Serializable 接口

我们来看看 Serializable 到底是什么，跟进去看一下，我们发现 Serializable 接口里面竟然**什么都没有**，只是个**空接口**。

![20200719190629.png](https://272y4n7101.goho.co/i/2023/03/22/641adb14bdf3c.png)

一个接口里面什么内容都没有，我们可以将它理解成一个**标识接口**。

**比如**在课堂上有位学生遇到一个问题，于是举手向老师请教，这时老师帮他解答，那么这位学生的举手其实就是一个标识，自己解决不了问题请教老师帮忙解决。在 Java 中的这个 Serializable 接口其实是给 jvm 看的，通知 jvm，我不对这个类做序列化了，你(jvm)帮我序列化就好了。

> Serializable 接口就是 Java 提供用来进行高效率的异地共享实例对象的机制，实现这个接口即可。

## 什么是 JVM？

JVM 是 Java Virtual Machine（Java 虚拟机）的缩写，JVM 是一种用于计算设备的规范，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。

一般情况下，我们在定义实体类时会继承 Serializable 接口，类似这样：

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;

    private String userId;
    private String userName;

}
```

我们在实体类中引用了 Serializable 这个接口，那么这个接口到底有什么？细心的你会发现我们还定义了个 serialVersionUID 变量。这个变量到底有什么作用？

## **什么是 Serializable 接口**

> 一个对象序列化的接口，一个类只有实现了 Serializable 接口，它的对象才能被序列化。

Serializable 是 java.io 包中定义的、用于实现 Java 类的序列化操作而提供的一个语义级别的接口。

Serializable 序列化接口没有任何方法或者字段，只是用于标识可序列化的语义。

实现了 Serializable 接口的类可以被 ObjectOutputStream 转换为字节流，同时也可以通过 ObjectInputStream 再将其解析为对象。

例如，我们可以将序列化对象写入文件后，再次从文件中读取它并反序列化成对象，也就是说，可以使用表示对象及其数据的类型信息和字节在内存中重新创建对象。

而这一点对于面向对象的编程语言来说是非常重要的，因为无论什么编程语言，其底层涉及 IO 操作的部分还是由操作系统其帮其完成的，而底层 IO 操作都是以字节流的方式进行的，所以写操作都涉及将编程语言数据类型转换为字节流，而读操作则又涉及将字节流转化为编程语言类型的特定数据类型。

而 Java 作为一门面向对象的编程语言，对象作为其主要数据的类型载体，为了完成对象数据的读写操作，也就需要一种方式来让 JVM 知道在进行 IO 操作时如何将对象数据转换为字节流，以及如何将字节流数据转换为特定的对象，而 Serializable 接口就承担了这样一个角色。

## 什么是序列化？

序列化是指把对象转换为字节序列的过程，我们称之为对象的序列化，就是把内存中的这些对象变成一连串的字节(bytes)描述的过程。

而反序列化则相反，就是把持久化的字节文件数据恢复为对象的过程。

## 为什么要序列化对象化

对象的寿命通常随着生成该对象的程序的终止而终止，有时候需要把在内存中的各种对象的状态（也就是实例变量，不是方法）保存下来，并且可以在需要时再将对象恢复。虽然你可以用你自己的各种各样的方法来保存对象的状态，但是 Java 给你提供一种应该比你自己的好的保存对象状态的机制，那就是序列化。

> 把对象转换为字节序列的过程称为对象的序列化把字节序列恢复为对象的过程称为对象的反序列

## 什么情况下需要序列化？

大概有这样两类比较常见的场景：

1. 想把的内存中的对象状态信息持久化, 例如我们利用 mybatis 框架编写持久层 insert 对象数据到数据库中时 .
2. 想把对象的状态信息通过网络进行传输, 如我们使用 RPC 协议进行网络通信时;

## 如何序列化?

那为什么还要继承 Serializable。那是存储对象在存储介质中，以便在下次使用的时候，可以很快捷的重建一个副本。

或许你会问，我在开发过程中，实体并没有实现序列化，但我同样可以将数据保存到 mysql、Oracle 数据库中，为什么非要序列化才能存储呢？

只要一个类实现 Serializable 接口，那么这个类就可以序列化了。

先定义一个序列化对象 User：

```java
public class User implements Serializable {
    private static final long serialVersionUID = 1L;

    private String userId;
    private String userName;

    public User(String userId, String userName) {
        this.userId = userId;
        this.userName = userName;
    }
}
```

然后我们编写测试类，来对该对象进行读写操作，我们先测试将该对象写入一个文件：

```java
public class SerializableTest {

    /**
     * 将User对象作为文本写入磁盘
     */
    public static void writeObj() {
        User user = new User("1001", "Joe");
        try {
            ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream("/Users/guanliyuan/user.txt"));
            objectOutputStream.writeObject(user);
            objectOutputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String args[]) {
        writeObj();
    }
}
```

运行上述代码，我们就将 User 对象及其携带的数据写入了文本 user.txt 中，我们可以看下 user.txt 中存储的数据此时是个什么格式：

```java
name:tom    age:22
name:tom    age:22
```

我们看到对象数据以二进制文本的方式被持久化到了磁盘文件中。在进行反序列化测试之前，我们可以尝试下将 User 实现 Serializable 接口的代码部分去掉，看看此时写操作是否还能成功，结果如下：

```java
java.io.NotSerializableException: cn.wudimanong.serializable.User
    at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1184)
    at java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:348)
    at cn.wudimanong.serializable.SerializableTest.writeObj(SerializableTest.java:19)
    at cn.wudimanong.serializable.SerializableTest.main(SerializableTest.java:27)
```

结果不出所料，果然是不可以的，抛出了 NotSerializableException 异常，提示非可序列化异常，也就是说没有实现 Serializable 接口的对象是无法通过 IO 操作持久化的。

接下来，我们继续编写测试代码，尝试将之前持久化写入 user.txt 文件的对象数据再次转化为 Java 对象，代码如下：

```java
public class SerializableTest {
    /**
     * 将类从文本中提取并赋值给内存中的类
     */
    public static void readObj() {
        try {
            ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream("/Users/guanliyuan/user.txt"));
            try {
                Object object = objectInputStream.readObject();
                User user = (User) object;
                System.out.println(user);
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }


    public static void main(String args[]) {
        readObj();
    }
}
```

通过反序列化操作，可以再次将持久化的对象字节流数据通过 IO 转化为 Java 对象，结果如下：

```java
cn.wudimanong.serializable.User@6f496d9f
```

此时，如果我们再次尝试将 User 实现 Serializable 接口的代码部分去掉，发现也无法再文本转换为序列化对象，报错信息为：

```java
ava.io.InvalidClassException: cn.wudimanong.serializable.User; class invalid for deserialization
    at java.io.ObjectStreamClass$ExceptionInfo.newInvalidClassException(ObjectStreamClass.java:157)
    at java.io.ObjectStreamClass.checkDeserialize(ObjectStreamClass.java:862)
    at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2038)
    at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1568)
    at java.io.ObjectInputStream.readObject(ObjectInputStream.java:428)
    at cn.wudimanong.serializable.SerializableTest.readObj(SerializableTest.java:31)
    at cn.wudimanong.serializable.SerializableTest.main(SerializableTest.java:44)
```

提示非法类型转换异常，说明在 Java 中如何要实现对象的 IO 读写操作，都必须实现 Serializable 接口，否则代码就会报错!

## **为什么要定义 serialversionUID 常量**

简单看一下 Serializable 接口的说明：

![20200719190740.png](https://272y4n7101.goho.co/i/2023/03/22/641adb14ee795.png)

从说明中我们可以看到，如果我们没有自己声明一个 serialVersionUID 变量,接口会默认生成一个 serialVersionUID

对于 JVM 来说，要进行持久化的类必须要有一个标记，只有持有这个标记 JVM 才允许类创建的对象可以通过其 IO 系统转换为字节数据，从而实现持久化，而这个标记就是 Serializable 接口。而在反序列化的过程中则需要使用 serialVersionUID 来确定由那个类来加载这个对象，所以我们在实现 Serializable 接口的时候，一般还会要去尽量显示地定义 serialVersionUID，如：

```java
private static final long serialVersionUID = 1L;
```

在反序列化的过程中，如果接收方为对象加载了一个类，如果该对象的 serialVersionUID 与对应持久化时的类不同，那么反序列化的过程中将会导致 InvalidClassException 异常。例如，在之前反序列化的例子中，我们故意将 User 类的 serialVersionUID 改为 2L，如：

```java
private static final long serialVersionUID = 2L;
```

那么此时，在反序例化时就会导致异常，如下：

```java
java.io.InvalidClassException: cn.wudimanong.serializable.User; local class incompatible: stream classdesc serialVersionUID = 1, local class serialVersionUID = 2
    at java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:687)
    at java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1880)
    at java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1746)
    at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2037)
    at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1568)
    at java.io.ObjectInputStream.readObject(ObjectInputStream.java:428)
    at cn.wudimanong.serializable.SerializableTest.readObj(SerializableTest.java:31)
    at cn.wudimanong.serializable.SerializableTest.main(SerializableTest.java:44)
```

如果我们在序列化中没有显示地声明 serialVersionUID，则序列化运行时将会根据该类的各个方面计算该类默认的 serialVersionUID 值。

那么默认生成的序列化 ID 有什么作用呢，有些时候，通过改变序列化 ID 可以用来限制某些用户的使用。

但是，Java 官方强烈建议所有要序列化的类都显示地声明 serialVersionUID 字段，因为如果高度依赖于 JVM 默认生成 serialVersionUID，可能会导致其与编译器的实现细节耦合，这样可能会导致在反序列化的过程中发生意外的 InvalidClassException 异常。

因此，为了保证跨不同 Java 编译器实现的 serialVersionUID 值的一致，实现 Serializable 接口的必须显示地声明 serialVersionUID 字段。

此外 serialVersionUID 字段地声明要尽可能使用 private 关键字修饰，这是因为该字段的声明只适用于声明的类，该字段作为成员变量被子类继承是没有用处的!

有个特殊的地方需要注意的是，数组类是不能显示地声明 serialVersionUID 的，因为它们始终具有默认计算的值，不过数组类反序列化过程中也是放弃了匹配 serialVersionUID 值的要求。

## 静态变量序列化

串行化只能保存对象的非静态成员交量，不能保存任何的成员方法和静态的成员变量，而且串行化保存的只是变量的值，对于变量的任何修饰符都不能保存。

如果把 Person 类中的 name 定义为 static 类型的话，试图重构，就不能得到原来的值，只能得到 null。说明对静态成员变量值是不保存的。这其实比较容易理解，序列化保存的是对象的状态，静态变量属于类的状态，因此 序列化并不保存静态变量。

## transient 关键字

经常在实现了 Serializable 接口的类中能看见 transient 关键字。这个关键字并不常见。 transient 关键字的作用是：阻止实例中那些用此关键字声明的变量持久化；当对象被反序列化时（从源文件读取字节序列进行重构），这样的实例变量值不会被持久化和恢复。

当某些变量不想被序列化，同是又不适合使用 static 关键字声明，那么此时就需要用 transient 关键字来声明该变量。

例如用 transient 关键字 修饰 name 变量

```java
class Person implements Serializable{

    private static final long serialVersionUID = 1L;

    transient String name;
    int age;

    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }
    public String toString(){
        return "name:"+name+"\tage:"+age;
    }
}
```

在反序列化视图重构对象的时候，作用与 static 变量一样： 输出结果为：

```java
name:null   age:22
```

在被反序列化后，transient 变量的值被设为初始值，如 int 型的是 0，对象型的是 null。

注：对于某些类型的属性，其状态是瞬时的，这样的属性是无法保存其状态的。例如一个线程属性或需要访问 IO、本地资源、网络资源等的属性，对于这些字段，我们必须用 transient 关键字标明，否则编译器将报措。

### 序列化中的继承问题

- 当一个父类实现序列化，子类自动实现序列化，不需要显式实现 Serializable 接口。
- 一个子类实现了 Serializable 接口，它的父类都没有实现 Serializable 接口，要想将父类对象也序列化，就需要让父类也实现 Serializable 接口。

第二种情况中：如果父类不实现 Serializable 接口的话，就需要有默认的无参的构造函数。这是因为一个 Java 对象的构造必须先有父对象，才有子对象，反序列化也不例外。在反序列化时，为了构造父对象，只能调用父类的无参构造函数作为默认的父对象。因此当我们取父对象的变量值时，它的值是调用父类无参构造函数后的值。在这种情况下，在序列化时根据需要在父类无参构造函数中对变量进行初始化，否则的话，父类变量值都是默认声明的值，如 int 型的默认是 0，string 型的默认是 null。

例如：

```java
class People{
    int num;
    public People(){}           //默认的无参构造函数，没有进行初始化
    public People(int num){     //有参构造函数
        this.num = num;
    }
    public String toString(){
        return "num:"+num;
    }
}
class Person extends People implements Serializable{

    private static final long serialVersionUID = 1L;

    String name;
    int age;

    public Person(int num,String name,int age){
        super(num);             //调用父类中的构造函数
        this.name = name;
        this.age = age;
    }
    public String toString(){
        return super.toString()+"\tname:"+name+"\tage:"+age;
    }
}
```

在一端写出对象的时候

```java
    Person person = new Person(10,"tom", 22); //调用带参数的构造函数num=10,name = "tim",age =22
    System.out.println(person);
    oos.writeObject(person);                  //写出对象
```

在另一端读出对象的时候

```java
    Person person = (Person)ois.readObject(); //反序列化，调用父类中的无参构函数。
    System.out.println(person);
```

输出为

```java
    num:0   name:tom    age:22
```

发现由于父类中无参构造函数并没有对 num 初始化，所以 num 使用默认值为 0。

## 总结

序列化给我们提供了一种技术，用于保存对象的变量。以便于传输。虽然也可以使用别的一些方法实现同样的功能，但是 java 给我们提供的方法使用起来是非常方便的。以上仅仅是我的一些理解，由于本人水平有限，不足之处还请指正。下篇学习 Cloneable 接口的用途。

## 参考资料：

[https://developer.51cto.com/art/201905/596334.htm](https://developer.51cto.com/art/201905/596334.htm)

[https://blog.csdn.net/u011568312/article/details/57611440](https://blog.csdn.net/u011568312/article/details/57611440)
