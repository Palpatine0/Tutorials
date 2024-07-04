###  Java语法特性

#### JDK8 新特性

**详细介绍 lambda 表达式** [参考](https://www.cnblogs.com/haixiang/p/11029639.html)

lambda 表达式允许把函数作为参数传递到方法，简化匿名内部类代码。

- 对接口的要求：可以有多个方法，但只能有一个需要被实现
- 语法形式：() -> {}，() 为参数列表，-> 为 lambda运算符 goes to，{} 为方法体。具体情况下有的符号可以精简
- 1.8相关特性
  - 函数式接口：接口中的**抽象方法**只有一个,使用 `@FunctionalInterface` 注解
  - 方法引用：可以引用已有类或对象的方法和构造方法，进一步简化 lambda 表达式

> 样例？

**详细介绍 Stream 类** [参考](https://blog.csdn.net/scau_lth/article/details/83280762?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-0&spm=1001.2101.3001.4242)

Stream 类可以对集合中的数据进行聚合操作，通过内部迭代实现对流的处理。

- 转换成流：stream方法
- 中间操作：记录流的操作（不处理数据）
  - 有状态操作：需要所有元素。`limit` 取前 n 个元素、`skip` 跳过前 n 个元素
  - 无状态操作：只关心当前元素。`filter` 按条件过滤、`map` 映射加工
- 终端操作：流的归约、查找等操作
  - 短路操作：处理完符合条件的元素就可以停止。anyMatch，allMatch
  - 非短路操作：需要把所有元素都处理完。max，min，count，collect

<img src="https://images2015.cnblogs.com/blog/848293/201611/848293-20161103143545315-2110948414.png" alt="img" style="zoom:67%;" />

操作可以是串行，也可以是并行的。Stream 类提供了`parallelStream()`来启动并行流式处理，本质上基于java7的**Fork-Join框架**（线程池、自动分片聚合）实现，默认的线程数为主机的内核数。[参考](https://blog.csdn.net/shandeai520/article/details/103597755)

**JDK8 其他新特性**

- 日期类API：增强了日期和时间 API，新的 java.time 包包含了处理日期、时间、时区、时刻和时钟等操作。
- 接口：可以定义 `default ` 修饰的默认方法，降低了接口升级的复杂性，还可以定义静态方法。
- 注解：引入重复注解机制，相同注解在同地方可以声明多次。注解作用范围也进行了扩展，可作用于局部变量、泛型、方法异常等。
- 类型推测：加强了类型推测机制，使代码更加简洁。
- Optional 类：处理空指针异常，提高代码可读性。
- JavaScript：提供了一个新的 JavaScript 引擎，允许在 JVM上运行特定 JavaScript 应用。

#### 数据类型

**分类**

![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbmJsb2dzLmNvbS9jbmJsb2dzX2NvbS9zaW1wbGVmcm9nL2Jsb2cxMzEucG5n?x-oss-process=image/format,png)

引用数据类型还包括一种特殊的null类型。

- 自动类型转换：从小到大
- 强制类型转换：从大到小
- 数值型和布尔型不能进行类型转换，引用数据类型的转换只能在有继承关系的两个类型之间进行。

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9yYXcuZ2l0aHVidXNlcmNvbnRlbnQuY29tL0pvdXJXb24vaW1hZ2UvbWFzdGVyL0phdmElRTUlOUYlQkElRTclQTElODAlRTglQUYlQUQlRTYlQjMlOTUvSmF2YSVFNSU5RiVCQSVFNiU5QyVBQyVFNiU5NSVCMCVFNiU4RCVBRSVFNyVCMSVCQiVFNSU5RSU4Qi5wbmc?x-oss-process=image/format,png)

---

**Java是什么编码？**

默认GBK，但一般统一为 utf-8

**unicode码** [参考](https://tianjinsong.blog.csdn.net/article/details/52936943?utm_medium=distribute.pc_relevant.none-task-blog-searchFromBaidu-2.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-searchFromBaidu-2.not_use_machine_learn_pai)

Unicode是一个编码方案，为每种语言的字符设定了唯一的**二进制编码**，来满足跨语言、跨平台进行文本转换、处理的要求。

- utf-8最大特点为占用一到四个字节，能根据不同的符号而变化长度，节约空间
  - 数字、英文字母：1个字节
  - 中文：3个字节
- utf-16占用二或四个字节
- utf-32占用四个字节

---

**float f=3.4;是否正确**

编译报错。3.4 是双精度数，将双精度型（double）赋值给浮点型（float）属于下转型（down-casting，也称为窄化）会造成精度损失，因此需要强制类型转换float f =(float)3.4; 或者写成 float f =3.4F。

**short s1 = 1; s1 = s1 + 1;有错吗？short s1 = 1; s1 += 1;有错吗**

- s1 = s1 + 1;编译报错：由于 1 是 int 类型，因此 s1+1 运算结果也是 int型，需要强制转换类型才能赋值给 short 型。
- s1 += 1;可以正确编译：因为 s1+= 1;相当于 s1 = (short(s1 + 1);其中有隐含的强制类型转换

#### 包装类

**java中有了基本类型为什么还要有包装类型** [参考](https://blog.csdn.net/min996358312/article/details/62894674)

Java是一个面向对象的编程语言，为了让基本类型也具有对象的特征，出现了包装类，添加了属性和方法

**自动装箱拆箱**

Java 在SE5之后提供了自动的装箱和拆箱机制。

- 装箱就是自动将基本数据类型转换为包装器类型，通过调用包装器的 valueOf() 方法实现
- 拆箱就是自动将包装器类型装换为基本数据类型，通过调用包装器的 xxxValue() 方法实现（xxx代表对应的基本数据类型）

---

**Integer类型数据存储在哪里？占多少个字节** [参考](https://blog.csdn.net/luosanpao2016/article/details/105798432)

-128---127存储在**方法区**且缓存整数对象，新建时返回缓存即可；否则存储在**堆**，新建时需new一个新对象。

和int一样4个字节

> Integer a = 1; Integer b = 1; System.out.println(a==b);//true，方法区缓存
>
> Integer a = 1000; Integer b = 1000; System.out.println(a==b);//false ，堆创建新对象

**BigInteger的实现原理**⭐ [参考](https://blog.csdn.net/zhaoyunfullmetal/article/details/19325415?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.control)

- int数组来保存数据`int[] mag;` ：不会以'0'元素开头。in范围是-2^31至2^31-1，约2*10^9。为防止超出范围，每个元素保存**9位**的十进制整数
- int变量表示该数的正负`int signum;`
- 调用**toString**方法返回原先数串

#### static

**static关键字的用途** [参考](https://blog.csdn.net/shuyizhi/article/details/79700054)

- 静态变量：修饰成员变量，所有对象共享此变量
- 静态方法：修饰成员方法，可直接以“类名.方法名”的方式调用，避免了new对象的繁琐和资源消耗，常用于工具类
- 静态代码块：将多个类成员放在一起初始化，使得程序更加规整
- 静态导包：将类方法直接导入到当前类中，可直接以“方法名”的方式调用，更加方便。

**staic关键字的原理** [参考](https://www.pianshen.com/article/8963893738/)

static修饰的信息属于类，放入JVM的**方法区**中为整个类所共享

#### String⭐

**String为什么是不可变的？**

String 底层是一个 **char 类型的数组**并使用 **final** 修饰，故String不可被继承

```java
/** The value is used for character storage. */
private final char value[];
```

**String为什么设置成不变的？**

- 安全：String被许多的Java类(库)用做参数，如网络地址URL，文件路径path。若String不是固定不变的，将会引起各种安全隐患
- 常量池优化：当创建一个String对象时，假如此字符串值已经在常量池中则不会创建新对象，而是引用已存在的对象，节省空间
- hash码唯一：可以放心地缓存哈希值，不必每次重复计算，优化性能

---

**String str="i"与 String str=new String(“i”)一样吗？** [参考](https://blog.csdn.net/qq_33417486/article/details/82787598?utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

不一样，内存的分配方式不一样。

- String str="i"：java 虚拟机会将其分配到字符串常量池中，而常量池不会重复创建已存在对象。
  -  在编译期，JVM会去常量池来查找是否存在“abc”，如果不存在，就在常量池中开辟一个空间来存储“abc”；如果存在，就不用新开辟空间。
  - 然后在栈内存中开辟一个名字为str1的空间，来存储“abc”在常量池中的地址值。
- String str=new String(“i”) ：分到堆内存中。堆内存会创建新的对象。
  - 在编译阶段JVM先去常量池中查找是否存在“abc”，如果过不存在，则在常量池中开辟一个空间存储“abc”。
  - 在运行时期，通过String类的构造器在堆内存中new了一个空间，然后将String池中的“abc”复制一份存放到该堆空间中，
  - 在栈中开辟名字为str2的空间，存放堆中new出来的这个String对象的地址值。

```java
    String str1 = "i";
	String str2 = "i";
	String str3 = new String("i");
	
	//==比较内存地址
	System.out.println(str1 == str2);//ture,str2直接使用常量池存在的常量，不会再次创建，地址相同
	System.out.println(str2 == str3);//false,再次创建，两个地址不同

    //equals:引用类型，默认比较两个对象内存地址，重写后比较属性值。 String类的equals比较的是值
    System.out.println(str1.equals(str2)); //true
```

**String s = "aaa" + new String("bbb");创建了几个字符串对象**

如果没有这两个字符串，则4个

- "aaa"一个对象 （**常量池**）
- new Sring()一个对象 （**堆**）
- "bbb"一个对象 （**常量池**）
- “aa” + new Sring()一个对象 （**堆**）

**String  “+” 怎样实现？** [参考](https://blog.csdn.net/TomAndersen/article/details/104142900)

- 正常变量：创建一个**StringBuilder**对象。先调用其append方法来实现+，再调用其toString方法返回String变量
- final变量及字符串字面量：“常量传播”优化。编译期将变量替换为字符串值，并直接拼接

**String的最大长度是多少** [参考](https://www.zhihu.com/question/347048181)

编译期如果是javac编译就是65534。如果绕过javac编译的限制，其最大长度可以达到u2类型变达的最大值65535。

---

**String StringBuffer StringBuilder的区别是什么？**⭐

- 可变性
  - String类中使用字符数组保存字符串，使用final修饰，不可变
  - StringBuffer与StringBuilder都继承自AbstractStringBuilder类，也是使用字符数组保存字符串，但没使用final修饰，可变
- 线程安全性
  - String中的对象是不可变的，线程安全
  - **StringBuffer加了同步锁synchronized**，线程安全
  - StringBuilder没有加锁，非线程安全
- 性能：String < StringBuffer < StirngBuilder 
  - String 修改时会生成新的String对象，然后改变引用。内存占用大于后两者，也产生了垃圾对象，gc也有开销
  - StringBuffer 修改时需要加锁
  - StirngBuilder  修改时不用加锁

- 使用上
  - 少量数据 = String
  - 大量数据多线程 = StringBuffer
  - 大量数据单线程 = StringBuilder

#### Object类方法⭐

**说说Object类的方法**⭐

| 方法         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| **equals**   | 默认使用`==`比较                                             |
| **hashCode** | 由对象的内存地址得到int类型散列值 [参考](https://blog.csdn.net/weixin_43145361/article/details/105904810) |
| toString     | 返回对象值的一个字符串                                       |
| **clone**    | 默认浅拷贝对象副本                                           |
| **finalize** | 见JVM                                                        |
| getClass     | 返回对象所属类的 Class 对象                                  |
| **wait**     | 阻塞持有该对象锁的线程                                       |
| **notify**   | 唤醒持有该对象锁的线程                                       |

**== 和 equals 的区别是什么**⭐

-  ==：基本类型：比较值是否相等；引用类型：比较内存地址是否相等
-  equals() ：默认使用 `==` 比较，比较内存地址是否相等。也可按照需求重写对象的 equals() 方法，如String的equals方法比较的是对象的值。重写equals时必须重写hashCode方法。

**重写equals方法为什么要重写hashcode方法？**⭐ [参考](https://blog.csdn.net/qq_43259092/article/details/108249843?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_title-1&spm=1001.2101.3001.4242)

HashMap及HashSet中比较key时，会先使用hashCode方法比较hash值是否相等，若相等再使用equals方法比较，因为hash值相等的两个对象不一定相同。（两个都相等才行）

如果不对hashcode方法重写，那么将调只按地址比较，可能与预期结果有出入。（如只重写equals方法……）

**为什么要先比较hashCode，再比较equals**

减少equals比较次数，提高性能。因为原本的hashCode是位运算，性能更强

---

**如何使用clone()方法**

含 clone() 方法的类需要**实现Cloneable接口，并重写clone方法**，否则会抛出 CloneNotSupport 异常。

因为 clone() 方法是 protected 修饰符，如果不实现clone方法的话，在其他类中就不能调用其clone方法。

**clone默认是浅拷贝** [参考](https://www.cnblogs.com/ysocean/p/8482979.html)

静态字段肯定都复制

- 浅拷贝：复制非静态字段时，如果字段是数值型，则复制该字段；如果字段是引用型，则复制引用而不是复制引用的对象，因此原始对象及其副本引用同一个对象。
- 深拷贝：复制非静态字段时，无论该字段是数值型还是引用型，都进行复制

**clone实现深拷贝的思路**

- 每个引用类型内部都重写clone() 方法。但代码量很大
- 序列化。每个需要序列化的类都实现 Serializable 接口，如果有某个属性不需要序列化，可以将其声明为 transient

---

**final finally finalize区别**⭐

- final可以修饰类、变量、方法。修饰类不能被继承、修饰方法不能被重写、修饰变量……
- finally一般作用在try-catch代码块中。通常我们将一定要执行的代码放在finally代码块中，表示不管是否出现异常，该代码块都会执行。一般用来存放一些关闭资源的代码。
- finalize属于Object类方法，垃圾的两次判断……

---

**为什么线程通信的方法 wait(), notify()和 notifyAll()被定义在 Object 类里？** [参考](https://blog.csdn.net/zfy163520/article/details/104947177)

Java中任意对象均可以看成锁，而wait()方法会释放锁，因此这些相关的通信方法要被定义在Object类里。

### 面向对象

#### 封装 继承 多态

**1 封装**

封装把一个对象的属性私有化，同时提供一些可以被外界访问的属性和方法（如get set方法）。

**2 继承**

继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承我们能够非常方便地复用以前的代码。

关于继承如下 3 点请记住：

1. 子类继承父类非 private 的属性和方法。
2. 子类可以重写父类的方法。
3. 子类可以对父类进行扩展，拥有自己属性和方法。

> 构造器不能被继承，故不能被重写
>
> 重写要遵照里氏替换原则：返回值小于等于父类，抛出的异常小于等于父类，访问修饰符大于等于父类

**3 多态**⭐

**什么是多态机制？**[参考](https://www.cnblogs.com/lv-suma/p/12842575.html)

- 多态体现为父类引用变量可以指向子类对象。
- 父类引用变量的具体类型和方法在编程时不确定，而是在运行时才确定。所以不修改代码就可以改变运行时的具体代码，提高了程序的拓展性。

**Java语言是如何实现多态的？**

- 继承：在多态中必须存在有继承关系的子类和父类。
- 重写：子类对父类中某些方法进行重新定义。在调用这些方法时就会调用子类的方法。
- 向上转型：在多态中需要将子类对象赋给父类引用方法。该引用能调用父类的方法和子类的方法。

**重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？**⭐

- 重写发生在**继承**中，方法名和参数列表都相同；重载发生在**同类**中，方法名相同但参数列表不同（与方法返回值和访问修饰符无关）
- 重写实现的是运行时多态；重载实现的是编译时多态

#### 类 接口

**抽象类和接口的对比**⭐

**1 相同点**

- 接口和抽象类都不能实例化
- 都位于继承的顶端，用于被其他实现或继承
- 都包含抽象方法，其子类都必须实现这些抽象方法

**2 不同点**

| 参数       | 抽象类                                                       | 接口                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 声明       | abstract关键字                                               | interface关键字                                              |
| 实现       | 子类使用**extends**关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现 | 子类使用**implements**关键字来实现接口。它需要提供接口中所有声明的方法的实现 |
| 多继承     | 单继承                                                       | 多实现                                                       |
| 构造器     | 可以有                                                       | 不能有                                                       |
| 访问修饰符 | 任意                                                         | 默认public，不允许定义为 private 或 protected                |
| 字段声明   | 任意                                                         | 默认 static 和 final                                         |

**什么时候使用抽象类，什么情况下使用接口** [参考](https://www.jianshu.com/p/28e3b4d61945)

- 含义：抽象类适合定义某个领域的固有属性is，接口适合用来定义某个领域的扩展功能has。
- 代码：当需要为一些类提供公共代码时，优先考虑抽象类；当注重代码的扩展性跟维护性时，优先考虑接口（无继承层级等）

**抽象类能使用 final 修饰吗？**

不能，定义抽象类就是让其他类继承的，如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾，所以 final 不能修饰抽象类

#### 泛型

**Java中的泛型是什么？使用泛型的好处是什么？** [参考](https://blog.csdn.net/sunxianghuang/article/details/51982979?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.baidujs&dist_request_id=1328754.8217.16171605217144435&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-3.baidujs)

- 参数化类型机制。使代码适用于各种类型，编写更通用的代码，例如集合框架。
- 编译时类型确认机制。提供了编译时的**类型安全**，确保使用正确类型的对象，避免了在运行时出现ClassCastException。

**Java的泛型是如何工作的 / 原理是什么 / 什么是类型擦除**

由编译器在编译源码时做如下步骤：

- 类型检查：类型不符合则编译时报错
- **类型擦除**：泛型信息只存在于编译阶段，进入 JVM 前泛型相关信息会被擦除，用顶级父类替换，故Java泛型也称**伪泛型**。例如List<String>在运行时仅用List类型来表示
- 类型转换：在泛型出现的地方插入强制转换指令

**为什么要进行类型擦除** [参考](https://blog.csdn.net/u014674862/article/details/105676880) [参考](https://frank909.blog.csdn.net/article/details/76736356?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-2.control)

- 兼容性。泛型是 Java 1.5 版本才引进的概念，在这之前是没有泛型的概念的。
- 真泛型代价大。真泛型一般是通过**编译时膨胀法**，即对 JVM 源代码等进行修改、增加一套新的API等，修改量巨大。

**类型擦除的局限性**

由于泛型信息只存在于编译阶段，故使用反射可以绕过泛型的限制

**什么是泛型中的限定通配符和非限定通配符？**

- 限定通配符：对类型进行了限制
  - <? extends T>：通过确保类型必须是T的子类来设定类型的上界
  - <? super T>：通过确保类型必须是T的父类来设定类型的下界
- 非限定通配符：<?>，<T>，可以用任意类型来替代

**你可以把List<String>传递给一个接受List<Object>参数的方法吗？**

不可以。因为List<Object>可以存储任何类型的对象包括String, Integer等等，而List<String>却只能用来存储String。

> List<?> 是一个未知类型的List，故可以把List<String>, List<Integer>赋值给List<?>

#### 反射

**什么是反射机制？**

- 动态获取类信息：对于任意一个类，都能够知道这个类的所有属性和方法；
- 动态调用对象方法：对于任意一个对象，都能够调用它的任意一个方法和属性；

**反射的原理**

反射是利用**Jvm类加载**中加载到jvm中的.class文件来进行操作的。.class文件中包含java类的所有信息，当你不知道某个类具体信息时，可以使用反射获取class，然后进行各种操作。

**反射机制优缺点** [参考](https://blog.csdn.net/jikeehuang/article/details/51451442)

- 优点：灵活。可以动态执行，在运行期间根据业务功能动态执行方法、访问属性
- 缺点：对性能有影响。最主要的是编译器没法对反射相关的代码做优化，还有安全检查，访问控制等。

**Java获取反射的三种方法**

- 通过对象实现反射机制 
- 通过路径实现反射机制 
- 通过类名实现反射机制

```java
public class Student {
    private int id;
    String name;
    protected boolean sex;
    public float score;
}

public class Get {
    //获取反射机制三种方式
    public static void main(String[] args) throws ClassNotFoundException {
        //方式一(通过对象)
        Student stu = new Student();
        Class classobj1 = stu.getClass();
        System.out.println(classobj1.getName());
        //方式二（所在通过路径-相对路径）
        Class classobj2 = Class.forName("fanshe.Student");
        System.out.println(classobj2.getName());
        //方式三（通过类名）
        Class classobj3 = Student.class;
        System.out.println(classobj3.getName());
    }
}
```

 **哪里用到反射机制？**

- JDBC中，利用反射动态加载了数据库驱动程序。
- Web服务器中利用反射调用了Sevlet的服务方法。
- Eclispe等开发工具利用反射动态刨析对象的类型与结构，动态提示对象的属性和方法。
- 很多框架都用到反射机制，注入属性，调用方法，如Spring。

**反射可以访问private方法和属性吗？如何访问？**[参考](https://blog.csdn.net/kjfcpua/article/details/8496911)

可以，通过 setAccessible(true) 

### 异常

#### 异常架构

**Java异常架构是怎样的？**

- Exception：异常。可以在应用程序中进行捕获并处理，使程序继续正常运行。
- Error：错误。通常为虚拟机相关错误，如系统崩溃，内存不足，堆栈溢出等。编译器不会对这类错误进行检测，程序也不会进行捕获，一旦这类错误发生，通常应用程序会被终止，仅靠应用程序本身无法恢复；

**检查性异常 非检查性异常**⭐

编译器检查

- 非检查性异常(unchecked Exceptions) ：派生于Error或者RuntimeException的异常。不受编译器的检查。因为它们大多数是在运行时生成，不需要在throws子句中声明的异常。
  - 可以通过程序手段控制：例如常见的NullPointerException空指针异常和IndexOutOfBoundsException数组下标越界的异常，这些都可以事先使用if (xx != null) 以及 if (xxx.size() > i)来控制，
  - 完全无法通过程序手段控制：例如OutOfMemoryError内存溢出异常和StackOverflowError栈溢出异常。因为无法通过代码层面if就能避免的
- 检查性异常(checked exceptions) ：其他异常。受到编译器的检查。需对该异常做catch处理或者向上一层抛出

<img src="https://i.loli.net/2021/02/21/1Dnx48EAgVe7SlB.png" alt="image-20210221173519842"  />

**运行时异常 非运行时异常**

运行时发生

- 运行时异常都是RuntimeException类及其子类异常，如NullPointerException、IndexOutOfBoundsException等。这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。
- 非运行时异常是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException以及用户自定义的异常。

#### 异常处理

**JVM 是如何处理异常的？**

- 抛出异常：方法创建异常对象并转交给 JVM 。异常对象包含异常名称，异常描述以及异常发生时应用程序的状态。
- JVM 会顺着调用栈去查找看是否有可以处理异常的代码：方法调用的有序列表叫做**调用栈**。
  - 如果有，则调用异常处理代码。当 JVM 发现可以处理异常的代码时，会把发生的异常传递给它。
  - 如果没有，JVM 就会将该异常转交给默认异常处理器（默认为 JVM 的一部分），打印出异常信息并终止应用程序。

**throw 和 throws 的区别是什么？**

- throw 用在方法内部，只能用于抛出一种异常。受查异常和非受查异常都可以被抛出
- throws 用在方法声明上，可以抛出多个异常。调用该方法的方法中必须包含可处理异常的代码，否则也要用 throws 声明相应异常

**try-catch-finally 中哪个部分可以省略？**

catch 可以省略

**finally中的代码块在什么情况下不会被执行** 

- 没有执行 try{} 语句
- 调用 system.exit() 退出虚拟机
- finally在守护线程中，由于非守护线程结束导致守护线程结束

**异常处理的最佳实践是怎样的？**[参考](https://www.cnblogs.com/wupeixuan/p/11746117.html)

- **只抛出和方法相关的异常**
- **finally 块中永远不要抛出任何异常**
- **将所有相关信息尽可能地传递给异常**
- 在你的方法里抛出定义具体的检查性异常
- 要么记录异常要么抛出异常，但不要一起执行
- **始终只捕获可处理的异常**
- **捕获具体的子类而不是捕获 Exception 类**
- 永远不要捕获 Throwable 类
- 始终正确包装自定义异常中的异常，以便堆栈跟踪不会丢失
- 不要使用 printStackTrace() 语句或类似的方法
- 对于不打算处理的异常，直接使用 finally
- 记住早 throw 晚 catch 原则
- 尽早验证用户输入以在请求处理的早期捕获异常
- 一个异常只能包含在一个日志中
- **不要忽略捕捉的异常**
- **在异常处理后清理资源**
- **切勿在程序中使用异常来进行流程控制**
- 终止掉被中断线程
- 对于重复的 try-catch，使用模板方法
- 使用 JavaDoc 中记录应用程序中的所有异常

### 集合

#### 架构

**List，Set，Map三者的区别？List、Set、Map 是否继承自 Collection 接口？List、Map、Set 三个接口存取元素时，各有什么特点？**

<img src="https://imgconvert.csdnimg.cn/aHR0cHM6Ly9pbWcyMDE4LmNuYmxvZ3MuY29tL290aGVyLzE0MDgxODMvMjAxOTExLzE0MDgxODMtMjAxOTExMTkxODQxNDk1NTktMTU3MTU5NTY2OC5qcGc?x-oss-process=image/format,png" alt="img" style="zoom: 50%;" />

Collection

- List：一个有序（元素存入集合的顺序和取出的顺序一致）容器，元素可以重复，可以插入多个null元素，元素都有索引。
  - Arraylist： 数组，线程不安全
  - LinkedList： 双向循环链表，线程不安全
  - Vector： Object数组，线程安全

- Set：一个无序（存入和取出顺序有可能不一致）容器，不可以存储重复元素，只允许存入一个null元素，必须保证元素唯一性。
  - HashSet（无序，唯一）：基于 HashMap 
  - LinkedHashSet：继承于 HashSet，基于 LinkedHashMap 
  - TreeSet（有序，唯一）： 红黑树

Map（Map没有继承Collection接口）

- HashMap：见下文……
- LinkedHashMap：继承自 HashMap，增加了一条双向链表
- HashTable： 数组+链表，HashMap 1.7前结构
- TreeMap： 红黑树

**哪些集合类是线程安全的？**

- vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。
- hashtable：就比hashmap多了个线程安全。
- enumeration：枚举，相当于迭代器。

---

**comparable 和 comparator 的区别？** [参考](https://blog.csdn.net/wangnanwlw/article/details/108610001) [参考](https://blog.csdn.net/qq_40693828/article/details/81409004?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control&dist_request_id=1328767.40002.16175042954042947&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.control)

- comparable 在java.lang包下；comparator 在java.util包下
- comparable 必须重写compareTo(T o)；comparator必须重写compare(T o1,T o2)
- comparable 需要修改原先的实体类；Comparator 不用修改原先的类，优先级高应用广

> comparable 是内在比较器，对象直接可以比较this.compareTo(this)；comparator 是外在比较器，如果想比较两个类又没有实现comparable或者想实现比较排序的，可以用Comparator.compare(o1,o2);
>
> comparator 是典型的策略模式，每个类都可以实现comparator，实现比较排序。

#### List

**ArrayList的主要成员变量** [参考](https://blog.csdn.net/weixin_36378917/article/details/81812210?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-6.control) 

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
{
    // 版本号
    private static final long serialVersionUID = 8683452581122892189L;
    // 初始容量！！
    private static final int DEFAULT_CAPACITY = 10;
    // 空对象数组
    private static final Object[] EMPTY_ELEMENTDATA = {};
    // 缺省空对象数组
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
    // 元素数组，加上 transient 修饰！！
    transient Object[] elementData;
    // 实际元素大小，默认为0！！
    private int size;
    // 最大容量！！
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
}
```

**为什么 ArrayList 的 elementData 加上 transient 修饰？**

- elementData是一个缓存数组，通常会预留一些容量，没有实际存储元素
- ArrayList将elementData声明为transient，并重写了writeObject方法，保证**只序列化实际存储的元素**，节省空间和时间

**为什么数组的最大值是Integer.MAX_VALUE - 8？**

数组作为一个对象，需要一定的内存存储对象头信息，最大占用内存不超过8字节。如果数组长度过大，可能出现的两种错误

- OutOfMemoryError: Java heap space 堆区内存不足（这个可以通过设置JVM参数 -Xmx 来指定）。
- OutOfMemoryError: Requested array size exceeds VM limit 超过了JVM虚拟机的最大限制

---

**ArrayList add(E e)流程** [参考](https://blog.csdn.net/WINT0123/article/details/108154573)

- 判断 增加元素后所需最小容量minCapacity 是否小于 当前集合容量elementData.length
  - 若小于则容量充足
  - 反之则进行扩容操作
- 执行新元素的追加操作`elementData[size++] = e`

**ArrayList add(int index, E element)流程**

- 首先检查index是否越界，越界则报错
- 判断 增加元素后所需最小容量minCapacity 是否小于 当前集合容量elementData.length
  - 若小于则容量充足
  - 反之则进行扩容操作
- 将原来index位置上及后面的元素向后移动
- 执行新元素的追加操作`elementData[index] = e`

**ArrayList的扩容策略**⭐

- 先将容量扩容到当前容量的1.5倍，判断是否够用，不够用则将容量再次扩容1.5倍
- 判断扩容后的容量是否超出了最大容量MAX_ARRAY_SIZE= Integer.MAX_VALUE - 8
  - 未超出则没问题
  - 超出则大小为 Integer.MAX_VALUE - 8
- 创建扩容后大小的数组，并复制原数组元素

**ArrayList 会无限扩容吗？/ ArrayList设置了上限吗？超过了怎么办？**

不会，ArrayList设置了上限，最大最大为Integer.MAX_VALUE

超过时size大小为Integer.MAX_VALUE，因为再大则发生越界为负数，导致数组下标越界而报错

---

 **ArrayList 和 LinkedList 的区别是什么 / 数组和链表的区别**⭐ 

- ArrayList 底层是数组；LinkedList 底层是双向链表
- ArrayList 必须事先定义长度；LinkedList能动态进行内存分配
- ArrayList 必须申请连续的空间，内存要求高；LinkedList可以利用零碎的空间，内存利用率高
- ArrayList 查找效率高，适用于频繁读取场景；LinkedList 的增删效率高，适用于频繁增删场景
- LinkedList 更占内存，因为节点还存储了两个引用，一个指向前一个元素，一个指向后一个元素

**ArrayList和LinkedList插入一条数据时谁的效率高？插入上万条数据时谁的效率高？** 

如果末尾则差不多，如果首部则LinkedList；插入很多条的话考虑到ArrayList会扩容，LinkedList高

#### Map⭐

**说一下 HashMap 的实现原理？**⭐ [学习](https://www.imooc.com/article/313327?block_id=tuijian_wz)

JDK1.7：数组 + 链表，使用拉链法解决哈希冲突

<img src="https://img-blog.csdnimg.cn/2019121422243983.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly90aGlua3dvbi5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70" alt="jdk1.7中HashMap数据结构" style="zoom: 50%;" />

JDK1.8之后：在1.7的基础上，**链表长度大于阈值（默认为8）且数组长度大于64**时，链表会转为红黑树来提高查询效率

<img src="https://img-blog.csdnimg.cn/20191214222452844.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly90aGlua3dvbi5ibG9nLmNzZG4ubmV0,size_16,color_FFFFFF,t_70" alt="jdk1.8中HashMap数据结构" style="zoom:67%;" />

**JDK1.7 VS JDK1.8 比较**

| 不同          | JDK 1.7                                           | JDK 1.8                                                      |
| ------------- | ------------------------------------------------- | ------------------------------------------------------------ |
| 数据结构      | 数组 + 链表<br />无冲突时存放数组；冲突时存放链表 | 数组 + 链表 + 红黑树<br />无冲突时，存放数组；冲突 & 链表长度 < 8：存放单链表；冲突 & 链表长度 > 8 & 数组长度 > 64：树化并存放红黑树 |
| 初始化        | 单独函数`inflateTable()`                          | 集成到了扩容函数`resize()`中，延迟初始化以节约空间           |
| 插入数据      | 头插法                                            | 尾插法                                                       |
| 扩容下标      | hash&(table.length-1) 一个个重新计算下标          | hash & oldCap  0则下标不变，1则新下标 = 原下标+旧数组长度    |
| hash(key)？？ | 9次扰动：4次位运算 + 5次异或运算                  | 2次扰动：1次位运算 + 1次异或运算                             |

---

**HashMap的初始化及参数**⭐

**HashMap的几个参数**

- 默认初始容量为16
- 默认负载因子为0.75
- 容量阈值 threshold = 数组长度 * loadFactor，当元素个数大于等于threshold 时，HashMap扩容

```java
//1 默认初始容量为16
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;

//2 默认负载因子为0.75
static final float DEFAULT_LOAD_FACTOR = 0.75f;

//Hash数组(在resize()中初始化)
//Node是HashMap的一个内部类，用来表示一个key-value
transient Node<K,V>[] table;

//元素个数
transient int size;

//3 容量阈值(元素个数大于等于该值时会自动扩容)
int threshold;
```

**为什么默认初始化桶数组大小为16，为什么加载因子的大小为0.75**

桶初始化桶数组大小：太大浪费空间，太小容易扩容

加载因子：太大容易哈希冲突，太小容易扩容且浪费空间

> 《阿里巴巴Java开发手册》正例：initialCapacity=(需要存储的个数/负载因子)+1。刚刚好？注意负载因子（即loafactor）默认为0.75，如果暂时无法确定初始值大小，请设置为16(即默认值）
>

**HashMap 的长度为什么是2的幂次方 / hashcode方法为什么要&数组长度的length-1，&操作比%操作的好处？**

- 位运算更高效：能通过 **hash&(table.length-1)** 来计算元素在数组中的下标。因为当数组长度为2的幂次方时，这条公式等价于hash%table.length，且位运算效率更高。
- 增加hash值的随机性：此时 length-1 转为二进制是11111……的形式，可以使所有位同元素的hash值做与运算；反之比如length为15，则length-1为14，对应的二进制为1110，在和 hash 做与运算时，最后一位永远都为0 ，浪费空间。

---

**哈希**⭐

**什么是哈希？**

Hash，一般翻译为散列（哈希）。把任意长度的输入通过散列算法变换成固定长度的输出，该输出就是散列值**（哈希值）**

- key变化很小，结果变化很大
- 同一散列函数输出不同，则输入肯定也不同；但输出相同，输入也不一定相同。
- 不可逆推

**什么是哈希冲突？**

当两个不同的输入值，根据同一散列函数计算出相同的散列值的现象

**产生哈希冲突的影响因素**

装填因子（装填因子=数据总数 / 哈希表长）、哈希函数、处理冲突的方法

**Hash解决冲突的四种方法**

- 开放地址法

  - 线性探测：在原来值的基础上往后加一个单位，直至不发生哈希冲突
  - 再平方探测：在原来值的基础上先加1的平方单位，若仍然存在则减1的平方单位。随之是2的平方等等。直至不发生哈希冲突
  - 伪随机探测：在原来值的基础上加上随机数（随机函数生成），直至不发生哈希冲突

  优点

  - 记录更容易进行序列化操作
  - 如果记录总数可以预知，可以创建完美的哈希函数，尽量避免hash冲突，提高效率；

  缺点

  - 扩容成本太高；
  - 使用探测序列，造成额外计算时间； 
  - 删除的时候需要设置删除标记，造成额外的空间和操作；

- 链地址法：对于相同的值，使用链表进行连接，使用数组存储每一个链表。

  优点

  - 对于记录总数频繁可变的情况处理的较好；
  - 结点是动态分配，不会造成内存的浪费；
  - 删除记录比较方便，可是直接通过指针操作；

  缺点

  - 存储的记录是随机分布在内存中的，跳转访问时会带来额外的开销；
  - 由于使用指针，记录不容易进行序列化操作；

- 建立公共溢出区：将哈希表分为基本表和溢出表两部分，和基本表发生冲突的元素一律填入溢出表。

- 再哈希法：对于冲突的哈希值再次进行哈希处理，直至没有哈希冲突。

**HashMap是怎么解决哈希冲突的？**

- 使用链地址法来链接拥有相同hash值的数据；引入红黑树进一步降低遍历的时间复杂度，使得遍历更快；（处理冲突的方法）、
- 使用2次扰动（hash函数）来降低哈希冲突的概率，使得数据分布更平均；（哈希函数）

**hash(key)函数扰动**

 (h = key.hashCode()) ^ (h >>> 16) int有 32 位，右移16位就能让低16位和高16位进行异或。让hashCode的高位也参与运算，进一步降低hash碰撞的概率，使得数据分布更平均

> 相比在1.7中的4次位运算，5次异或运算（9次扰动）？？？在1.8中，只进行了1次位运算和1次异或运算（2次扰动）

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

---

**HashMap的put方法的具体流程？**⭐

<img src="https://img-blog.csdnimg.cn/20200331123933241.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI3Njg2Nzc5,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述"  />

- 计算`hash(key)`：对key的hashCode进行**高位与运算 ** (h = key.hashCode()) ^ (h >>> 16) 
- table 数组为空时，通过扩容函数初始化 table
- 通过**hash&(table.length-1)**公式算出元素下标后，执行插入操作
  - 如果该位置上没有元素，则新建Node节点存入数组
  - 如果该位置上有元素
    - 如果第一个节点key相等，则更新原值
    - 如果第一个节点key不等
      - 如果是红黑树，则红黑树插入
      - 如果是链表且key不存在，则插入链表尾部；插入后如果长度大于8，链表转化为红黑树
      
        > 红黑树或链表操作结束后，再次判断key是否存在，存在则更新原值（都要判断，故方法源码写在外面）？？
- 判断数组大小是否大于等于扩容阈值，是则进行扩容操作

```java
源码方法
    public V put(K key, V value)
    
    static final int hash(Object key)
    
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict)
```

---

**HashMap的扩容操作是怎么实现的？**⭐   

> 扩容时机：链表长度>8但数组大小小于64
>
> 扩容是一个非常消耗性能的操作，所以如果已经预知hashmap中元素个数，那么预设元素的个数能够有效的提高hashmap的性能。

- 计算table数组的新大小newCap及新阈值newThr

  - 若table数组长度大于0，说明已被初始化过，数组长度和阈值变为原来2倍

  - 若table数组未被初始化过且threshold大于0，说明之前调用了HashMap(initialCapacity, loadFactor)构造方法，把数组大小设为threshold

  - 若table数组未被初始化且threshold为0，说明之前调用了HashMap()构造方法，把数组大小设为16，threshold设为16*0.75

- 重新计算下标并移动

  - 若数组只链接一个节点，则直接放入新数组

  - 若是红黑树，则需进行红黑树的拆分

  - 若是链表，则需构建低位链表（扩容后下标不变）及高位链表（扩容后下标 = 原本下标 + 扩容前的数组长度）

    JDK1.8不需要像1.7那样一个个计算元素的hash值，而是通过**hash & oldCap（table数组的原大小）**的值来判断，若为0则索引位置不变，不为0则新索引=原索引+旧数组长度


```java
源码方法
    final Node<K,V>[] resize() 
```

**为什么HashMap要从头插法改成尾插法？** [参考](https://blog.csdn.net/weixin_45097458/article/details/103179093?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-1-103179093.pc_agg_rank_aggregation&utm_term=hashmap%E4%B8%BA%E4%BB%80%E4%B9%88%E7%94%A8%E5%B0%BE%E6%8F%92&spm=1000.2123.3001.4430)

- 多线程下头插法在**扩容时**会发生死循环，核心原因是线程2修改了原来的链表结构，而线程1却以原来的逻辑执行 [参考](https://www.jianshu.com/p/c4c4ff869149)
- JDK7用头插法可能是作者认为新插入的元素使用到的频率会比较高，插入头部的话可以减少遍历次数。但rehash旧链表迁移新链表时，链表元素会倒置，故原先使用头插法的理由已经不成立了

---

**查找**

- 首先通过自定义的hash方法计算出key的hash值，再次运算求出在数组中的位置
- 判断该位置上是否有节点，若没有则返回null，代表查询不到指定的元素
- 若有则判断该节点是不是要查找的元素，若是则返回该节点
- 若不是则判断节点的类型，如果是红黑树的话，则调用红黑树的方法去查找元素
- 如果是链表类型，则遍历链表调用equals方法去查找元素

---

**删除**

- 定位桶位置

- 遍历链表找到相等的节点

- 删除节点。可能破坏红黑树的平衡性质，removeTreeNode方法会对红黑树进行变色、旋转等操作来保持红黑树的平衡结构


---

**遍历**⭐

**hashmap 在迭代遍历的时候进行remove操作会发生什么，怎么解决**

会触发fail-fast(快速失败)机制。fail-fast是java集合的一种错误检测机制，当多个线程对集合进行结构上的改变的操作时，有可能会产生 fail-fast 机制。在集合中有一个名为modCount的变量，它用来表示集合被修改（插入或删除元素）的次数。每次遍历下一个元素之前，都会检测modCount变量是否为expectedmodCount，是的话就继续遍历；否则抛出异常，终止遍历。 

可以使用**迭代器iterator**自带的remove方法，每次迭代均把modCount赋值给expectedModCount，保证两者一致

```java
		Map<String, Integer> map = new HashMap<>();
        map.put("1", 1);
        map.put("2", 2);
        map.put("3", 3);
        Iterator<String> iterator = map.keySet().iterator();
        while (iterator.hasNext()){
            if (iterator.next().equals("2"))
                iterator.remove();
        }
```

---

**线程安全的Hashmap有哪些**⭐

- hashtable：采用synchronized方法上加锁，使用阻塞同步，效率低。
- synchronizedMap：和 hashtable 的唯一不同是可以用`Collections.synchronizedMap()`方法把任何的Map变成同步版本。
- concurrentHashMap ……

**HashMap 与 HashTable 有什么区别**⭐

- **数据结构**：HashMap 在 JDK1.8 后当链表长度大于阈值（默认为8）且数组长度大于64时，链表转化为红黑树，以减少搜索时间；Hashtable 和 JDK1.7的 HashMap 类似，采用数组+链表的形式。
- **线程安全原理**：HashMap 非线程安全；HashTable 线程安全，内部方法经过 `synchronized` 修饰，效率低
- **null键支持**：HashMap 中允许有一个 null 键；HashTable 不允许有 null 键，否则抛空指针异常
- 初始容量大小和扩容量大小
  - 创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。
  - 创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小。也就是说 HashMap 总是使用2的幂作为哈希表的大小，后面会介绍到为什么是2的幂次方。

> 在 Hashtable 的类注释可以看到，Hashtable 是保留类不建议使用，推荐在单线程环境下使用 HashMap 替代，如果需要多线程使用则用 ConcurrentHashMap 替代。
>

**如何决定使用 HashMap 还是 TreeMap（红黑树）**

- 对于在Map中插入、删除和定位元素这类操作，HashMap是最好的选择。
- 假如你需要对一个有序的key集合进行遍历、排序，TreeMap是更好的选择。

---

**红黑树**⭐

**HashMap为什么会转化为红黑树的结构？**

当我们的HashMap中存在大量数据时，加入我们某个bucket下对应的链表有n个元素，那么遍历时间复杂度就为O(n)，为了针对这个问题，JDK1.8在HashMap中新增了红黑树的数据结构，进一步使得遍历复杂度降低至O(logn)；

**HashMap为什么不直接采用红黑树**

因为红黑树需要进行左旋，右旋操作， 而单链表不需要

- 如果元素小于8个，查询成本高，新增成本低
- 如果元素大于8个，查询成本低，新增成本高

**HashMap为什么采用8和6进行红黑树和链表转化**

桶中链表元素超过8时，会自动转化成红黑树；若桶中元素小于等于6时，树结构还原成链表形式。

- 原因：红黑树的平均查找长度是log(n)，长度为8，**查找长度为log(8)=3**，链表的平均查找长度为n/2，当长度为8时，平均查找长度为8/2=4，这才有转换成树的必要；链表长度如果是小于等于6，6/2=3，虽然速度也很快的，但是转化为树结构和生成树的时间并不会太短？
- 选择6和8两个值的原因是：中间有个差值7可以防止链表和树之间**频繁的转换**。假设一下，如果设计成链表个数超过8则链表转换成树结构，链表个数小于8则树结构转换成链表，如果一个HashMap不停的插入、删除元素，链表个数在8左右徘徊，就会频繁的发生树转链表、链表转树，效率会很低。

**为什么table数组容量大于64才树化**

因为当table数组容量比较小时，键值对节点 hash 的碰撞率可能会比较高，进而导致链表长度较长。这个时候应该优先扩容

**红黑树特性**⭐

<img src="https://segmentfault.com/img/bV61di" alt="红黑树" style="zoom:67%;" />

1. 结点非红即黑
2. 根结点始终是黑色
3. 叶子结点（NIL 结点，在Java中，叶子结点是为null的结点）都是黑色。
4. **红色结点的两个直接孩子结点都是黑色**（即从叶子到根的所有路径上不存在两个连续的红色结点）
5. 从任一结点到每个叶子的所有简单路径都包含**相同数目的黑色结点**

**红黑树是怎么插入节点的**⭐ [学习](http://www.360doc.com/content/19/0311/07/25472797_820646156.shtml)

两方面：颜色+旋转

- 先通过二叉查找树的查找方法找到插入位置

- 插入结点应该是**红色**。以场景3为例，红色在父结点(如果存在)为黑色结点时，红黑树的黑色平衡没被破坏，不需要做自平衡操作。但如果插入结点是黑色，那么插入位置所在的子树黑色结点总是多1，必须做自平衡。（特性5）

  插入时需要维持红黑树特性，特别是4 5两点

![image-20210220225130178](https://i.loli.net/2021/02/20/rhZjzy1J8FpUPsO.png)

**红黑树时间复杂度？**

- 查找与普通二叉查找树相同，因为红黑树也是一个特化的**二叉查找树**
- 插入和删除时恢复红黑树的属性需要少量(O(log n))的颜色变更和不超过三次树旋转(对于插入操作是两次)，**O(log n)** 次 。

**平衡二叉树(BST)和红黑树深度差不能超过多少？** [参考](https://blog.csdn.net/yuhk231/article/details/51218244)

- 平衡二叉树：平衡因子为二叉树上任一结点的左子树深度减去右子树深度的差值。 平衡因子绝对值都不大于1
- 红黑树：一侧子树尽可能多的红色节点，另一侧不用红色节点，则相差最大（特性4 5）最大深度差红色节点的个数

![img](https://img-blog.csdn.net/20160422110311666?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

**红黑树的优点 / 跟平衡二叉树相比的优点** [参考](https://www.jianshu.com/p/37436ed14cc6)

红黑树是性能折中的选择，插入旋转简单但高度差变大，查找性能下降但插入性能上升

#### Set

**说一下 HashSet 的实现原理**

HashSet 是**基于 HashMap**实现的

- HashSet 的值存放于HashMap的key上，HashMap的value统一为PRESENT（可理解为没用到）
- HashSet 的操作基本上都是直接调用底层 HashMap 的相关方法来完成

**HashSet如何检查重复？HashSet是如何保证数据不可重复的？**

hashcode+equles 方法

**HashSet与HashMap的区别**

| HashMap                                                | HashSet                                                      |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| 实现了Map接口                                          | 实现Set接口                                                  |
| 存储键值对                                             | 仅存储对象                                                   |
| 调用put（）向map中添加元素                             | 调用add（）方法向Set中添加元素                               |
| HashMap使用键（Key）计算Hashcode                       | HashSet使用成员对象来计算hashcode值，再用equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false |
| HashMap相对于HashSet较快，因为它是使用唯一的键获取对象 | HashSet较HashMap来说比较慢                                   |

**Set时间查询复杂度都是O(1)吗 **

TreeSet 不是

### 网络编程基础

#### Java IO基础⭐

**BIO NIO AIO 有什么区别**⭐ [参考](https://www.cnblogs.com/keeplearningclc/p/10976296.html) [参考](https://zhuanlan.zhihu.com/p/84348426)

**1 BIO** 

同步阻塞IO (Blockig IO) ，数据读写阻塞在一个线程。`Socket` 和 `ServerSocket` 

**原理**

服务端通常由一个独立的 **Acceptor 线程**监听客户端的连接。

- 监听请求，一般在 `while(true)`循环中服务端会调用 `accept()`方法等待客户端的连接。
- 一旦接收到连接请求，就可以建立套接字进行读写操作。
- 此时不能再接收其他客户端的连接请求，只能等待当前操作完成。
- 要想同时处理多个客户端请求，必须使用多线程，开销大（可使用线程池等）

<img src="https://pic2.zhimg.com/80/v2-0a422a1795f97ffffcc9e3ffcc324f8d_720w.jpg" alt="img" style="zoom: 80%;" />

**特点**

模式简单使用方便，但并发能力低。

**2 NIO** 

同步非阻塞IO (New IO) ，面向缓冲、基于通道。 `SocketChannel` 和 `ServerSocketChannel`，支持阻塞和非阻塞

**原理 / 核心组件**

- Buffer（缓冲区）：包含要写入或读出的数据。读取数据时读到缓冲区中，写入数据时写入到缓冲区中。

  三个属性：capacity总容量  position指针当前位置  limit读/写边界位置

  <img src="https://upload-images.jianshu.io/upload_images/16937129-e94e24f72e0c6c4a.png?imageMogr2/auto-orient/strip|imageView2/2/w/896/format/webp" alt="img" style="zoom: 50%;" />

- Channel（通道）：通过通道异步读写缓冲区。一个通道代表和某一实体的连接，这个实体可以是文件、网络套接字等。

- Selector（选择器）：将多个通道注册到选择器，设置好关心的事件，并调用**select()**方法等待事件发生。channel 事件就绪时告诉服务端进行数据处理，发生**零拷贝**

  <img src="https://upload-images.jianshu.io/upload_images/16937129-719fd2c71413a220.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom: 33%;" />

**特点**

客户端和服务器端通过通道通信，实现了IO多路复用。

**JDK原生NIO程序的问题**

- NIO的类库和API繁杂，使用麻烦。
- 可靠性能力补齐的工作量和难度都非常大。如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常码流的处理
- NIO的各种BUG。如臭名昭著的 epoll bug 会导致Selector空轮询，最终导致CPU 100%，此bug至今仍未完全解决

**3 AIO**

异步非阻塞IO  (Asynchronous IO) ，基于事件和回调机制。

**原理**

操作后直接返回不会阻塞，处理完成时会通知相应线程进行后续操作

> 其余见操作系统

#### 序列化基础

**什么叫对象序列化，什么是反序列化？**

- 序列化：将对象中的数据编码为字节序列的过程
- 反序列化：将对象的字节序列反向解码为对象的过程

**什么时候用到序列化？**

- 把对象保存到文件或者数据库时
- 用套接字在网络上传送对象时，因为网络底层传输的是比特流
- 通过RMI（远程方法调用）传输对象时

**被什么样的变量修饰不会被序列化**

序列化通常使用网络传输对象，容易遭受攻击，因此不需要进行序列化的敏感属性应加上 **transient** 关键字

transient 的作用是把变量生命周期仅限于内存而不会写到磁盘，变量会被设为对应数据类型的零值。