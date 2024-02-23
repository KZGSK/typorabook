# java

**字节码**：JVM 可以理解的代码就叫做字节码（即扩展名为 `.class` 的文件），它不面向任何特定的处理器，只面向虚拟机。Java 语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点。

**JRE、JDK**： JRE 是 Java 运行时环境，仅包含 Java 应用程序的运行时环境和必要的类库。而 JDK 则包含了 JRE，同时还包括了 javac、javadoc、jdb、jconsole、javap 等工具，可以用于 Java 应用程序的开发和调试

 **JIT**：JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。

**AOT**： JIT 不同的是，这种编译模式会在程序被执行前就将其编译成机器码，属于静态编译。避免了 JIT 预热等各方面的开销，可以提高 Java 程序的启动速度，避免预热时间长。并且，AOT 还能减少内存占用和增强 Java 程序的安全性（AOT 编译后的代码不容易被反编译和修改），特别适合云原生场景。

AOT 更适合当下的云原生场景，对微服务架构的支持也比较友好。除此之外，AOT 编译无法支持 Java 的一些动态特性，如反射、动态代理、动态加载、JNI（Java Native Interface）等。然而，很多框架和库（如 Spring、CGLIB）都用到了这些特性。

**Java 和 C++ 的区别?**

Java 不提供指针来直接访问内存，程序内存更加安全

Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。

Java 有自动内存管理垃圾回收机制(GC)，不需要程序员手动释放无用内存。

C ++同时支持方法重载和操作符重载，但是 Java 只支持方法重载（操作符重载增加了复杂性，这与 Java 最初的设计思想不符）。

**面向对象和面向过程的区别**

- 面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
- 面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。

**面向对象三大特征**

封装：把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。

继承：继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类。

多态：一个个对象具有多种的状态，具体表现为父类的引用指向子类的实例。

**深拷贝和浅拷贝**

**浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。

**深拷贝**：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。

**引用拷贝**：引用拷贝就是两个不同的引用指向同一个对象。

**基本类型和包装类型的区别**

基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。

成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。

**包装类型的缓存机制**

Java 基本数据类型的包装类型的大部分都用到了缓存机制来提升性能。`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据。`Character` 创建了数值在 **[0,127]** 范围的缓存数据。`Float`,`Double` 并没有实现缓存机制。如果超出对应范围仍然会去创建新的对象。注意==的判断。利用缓存的没有创建新对象。

**== 和 equals()**

基本数据类型`==` 比较的是值。对引用数据类型来说，`==` 比较的是对象的内存地址。**`equals()`** 不能用于判断基本数据类型的变量，只能用来判断两个对象是否相等。

**hashCode** 

`hashCode()` 的作用是获取哈希码（`int` 整数），也称为散列码。这个哈希码的作用是确定该对象在哈希表中的索引位置。重写 equals() 时必须重写 hashCode() 方法.

用到 HashSet 等集合时。存储的对象我们需要用 `hashcode 判断对象是否存在`，如果使用 Object 默认的hashcode方法，那我们同样属性的俩个用户一定是不相等

**静态方法为什么不能调用非静态成员?**

静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。在类的非静态成员不存在的时候静态方法就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。

**String、StringBuffer、StringBuilder 的区别？**

**线程安全性**：`String` 中的对象是不可变的，也就可以理解为常量，线程安全。`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

**性能**：每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

字符串对象通过“+”的字符串拼接方式，实际上是通过 `StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 。会导致创建过多的 `StringBuilder` 对象。 JDK 9优化就没有这个问题了。

**字符串常量池** 

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

 **Java 值传递详解**

- **实参（实际参数，Arguments）**：用于传递给函数/方法的参数，必须有确定的值。
- **形参（形式参数，Parameters）**：用于定义函数/方法，接收实参，不需要有确定的值。

- **值传递**：方法接收的是实参值的拷贝，会创建副本。
- **引用传递**：方法接收的直接是实参所引用的对象在堆中的地址，不会创建副本，对形参的修改将影响到实参。

**在 Java 中只有值传递。**

对于引用类型的参数实际上也是值传递，只不过它拷贝创建的副本还是引用类型的地址，操作时改变的是地址的值所以才会影响到实参。

传入两个对象后在函数中交换地址，但主函数中其实并未改变。原因是交换的是副本地址。而不是真实的地址。没有改变地址的内容。

 **Java 序列化详解**

- **序列化**：将数据结构或对象转换成二进制字节流的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

属于OSI 七层协议模型中的表示层。 TCP/IP 四层模型中的应用层。

对于不想进行序列化的变量，使用 `transient` 关键字修饰(只能修饰变量，不能修饰类和方法)。`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。`static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化

JDK 自带的序列化，只需实现 `java.io.Serializable`接口即可。

序列化号 `serialVersionUID` 属于版本控制的作用。反序列化时，会检查 `serialVersionUID` 是否和当前类的 `serialVersionUID` 一致。serialVersionUID在类中由static修饰。对其做了特殊处理会被序列化到二进制字节流中；在反序列化时，也会解析它并做一致性判断。`serialVersionUID` 只是用来被 JVM 识别，实际并没有被序列化。

**Java 代理模式详解**

使用代理对象来代替对真实对象(real object)的访问，这样就可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能。代理模式有静态代理和动态代理两种实现方式。

静态代理中，我们对目标对象的每个方法的增强都是手动完成的。手动定义代理类，并在代理类中注入代理对象手动调用方法和自己想实现的额外功能。相比于静态代理来说，动态代理更加灵活。我们不需要针对每个目标类都单独创建一个代理类，并且也不需要我们必须实现接口，我们可以直接代理实现类( *CGLIB 动态代理机制*)。

从 JVM 层面来说， 静态代理在编译时就将接口、实现类、代理类这些都变成了一个个实际的 class 文件。动态代理是在运行时动态生成类字节码，并加载到 JVM 中的。

在 Java 动态代理机制中 `InvocationHandler` 接口和 `Proxy` 类是核心。当你使用代理对象调用方法的时候实际会调用到InvocationHandler实现类的invoke方法。可以在 `invoke()` 方法中自定义处理逻辑，比如在方法执行前后做什么事情。

JDK 动态代理有一个最致命的问题是其只能代理实现了接口的类。可以用 CGLIB 动态代理机制来避免。Spring 中的 AOP 模块中：如果目标对象实现了接口，则默认采用 JDK 动态代理，否则采用 CGLIB 动态代理。

~~~java
public class TestJDKProxy {
    public static void main(String[] args) {
        //1.创建原始对象
        //注意：由于后面匿名子类的方法中用到了userService，所以应该用final修饰
        //     而JDK1.8以后默认加了final，不需要手动加
        UserService userService = new UserServiceImpl();

        //2.JDK创建代理对象
        InvocationHandler handler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("----------- JDKProxy log -----------");\
                //目标方法运行：
                Object ret = method.invoke(userService, args);
                return ret;
            }
        };

        UserService userServiceProxy = (UserService) 					Proxy.newProxyInstance(userService.getClass().getClassLoader(), 							userService.getClass().getInterfaces(),handler);

        userServiceProxy.login("海绵宝宝", "1111");
        userServiceProxy.register(new User());

    }
}
public class TestCGlibProxy {
    public static void main(String[] args) {
        //1.创建原始对象
        UserServiceImpl userService = new UserServiceImpl();

        //2.通过CGlib创建代理对象
        //  2.1 创建Enhancer
        Enhancer enhancer = new Enhancer();
        //  2.2 设置借用类加载器
        enhancer.setClassLoader(TestCGlibProxy.class.getClassLoader());
        //  2.3 设置父类（目标类）
        enhancer.setSuperclass(userService.getClass());
        //  2.4 设置回调，额外功能写在里面
        enhancer.setCallback(new MethodInterceptor() {
            //相当于 InvocationHandler --> invoke()
            @Override
            public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                //额外功能：
                System.out.println("========= CGlibProxy log ========");
                //目标方法执行：
                Object ret = method.invoke(userService, args);
                return ret;
            }
        });
        //  2.5 通过Enhancer对象创建代理
        UserServiceImpl service = (UserServiceImpl) enhancer.create();

        //测试：
        service.register();
        service.login();

    }
}
~~~

**BigDecimal 详解**

浮点数的运算会有精度丢失的风险。计算机在表示一个数字时，宽度是有限的，无限循环的小数存储在计算机时，只能被截断，所以就会导致小数精度发生损失的情况。如0.2存储二进制时会发生01循环。`BigDecimal` 可以实现对浮点数的运算，不会造成精度丢失。**浮点数之间的等值判断，基本数据类型不能用 == 来比较，包装数据类型不能用 equals 来判断。**

为了防止精度丢失，推荐使用它的`BigDecimal(String val)`构造方法或者 `BigDecimal.valueOf(double val)` 静态方法来创建对象。`add` 方法用于将两个 `BigDecimal` 对象相加，`subtract` 方法用于将两个 `BigDecimal` 对象相减。`multiply` 方法用于将两个 `BigDecimal` 对象相乘，`divide` 方法用于将两个 `BigDecimal` 对象相除。大小比较使用compareTo，`a.compareTo(b)` : 返回 -1 表示 `a` 小于 `b`，0 表示 `a` 等于 `b` ， 1 表示 `a` 大于 `b`。

**泛型**

我们在向集合中添加元素的过程中add()方法里填入的是Object类，而Object类是所有类的父类，这就产生了一个问题——**添加的类型无法统一**，由此可能产生在遍历集合取出元素时类型不统一而报错。泛型提供了编译时类型安全检测机制，允许在编译时检测到非法的类型。使用泛型参数，可以增强代码的可读性以及稳定性。本质是**参数化类型**。

泛型是提供给javac编译器使用的，用于限定集合的输入类型，让编译器在源代码级别上，避免向集合中插入非法数据。但编译器编译完带有泛型的java程序后，生成的class文件中不再带有泛型信息，以此使程序运行效率不受影响，这个过程称为擦除。

泛型和Object相比优势：Object 有两个痛点：1、得强制转换；2、没有编译器级别的安全校验；

泛型为什么不支持基本类型：因为泛型在没有定义边界的时，进行类型擦除后，原始类型会变成Object，而基本类型不是对象

##**Java 集合**

主要是由两大接口派生而来：一个是 `Collection`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 和 `Queue`。

###**ArrayList**

`ArrayList` 的底层是数组队列，相当于动态数组。与 Java 中的数组相比，它的容量能动态增长。在添加大量元素前，应用程序可以使用`ensureCapacity`操作来增加 `ArrayList` 实例的容量。这可以减少递增式再分配的数量。

`public class ArrayList <E> extends AbstractList <E> implementsList <E> ,RandomAccess,Cloneable,java.io.Serializable{}`

**初始化**

底层是一个`Object[] elementData`的空数组。初始化时默认无参构造创建空数组。有参构造创建指定容量数组。或者传入其他集合转换为数组。

**扩容机制**

添加元素时会先保证数组容量充足。具体来说会计算数组所需要的容量。如果当前数组元素为空数组（初始情况），返回默认容量10和最小容量中的较大值作为所需容量。所需容量不足就会触发扩容。将新容量更新为旧容量的1.5倍。然后检查新容量是否大于最小需要容量，若还是小于最小需要容量，那么就把最小需要容量当作数组的新容量。再检查新容量是否超出了ArrayList所定义的最大容量，若超出了，则调用hugeCapacity()来比较minCapacity和 MAX_ARRAY_SIZE，如果minCapacity大于MAX_ARRAY_SIZE，则新容量则为Interger.MAX_VALUE，否则，新容量大小则为 MAX_ARRAY_SIZE。最后Arrays.copyOf将旧数组复制进新数组。

 `System.arraycopy()` 和 `Arrays.copyOf()`方法

`Arrays.copyOf()`内部实际调用了 `System.arraycopy()` 方法。`arraycopy()` 需要目标数组，将原数组拷贝到你自己定义的数组里或者原数组，而且可以选择拷贝的起点和长度以及放入新数组中的位置 `copyOf()` 是系统自动在内部新建一个数组，并返回该数组。

 `ensureCapacity`方法

用户手动扩容的方法。向 `ArrayList` 添加大量元素之前用 `ensureCapacity` 方法，以减少增量重新分配的次数

**删除操作**

需要调用 System.arraycopy() 将 index+1 后面的元素都复制到 index 位置上，在旧数 组上操作，该操作的时间复杂度为 O(N)，可以看到 ArrayList 删除元素的代价是非常高的

### LinkedList

`LinkedList` 是一个基于双向链表实现的集合类。

`public class LinkedList <E> extends AbstractSequentialList <E> implementsList <E> List<E>, Deque<E>, Cloneable, java.io.Serializable{}`

**初始化**

有空参初始化。带集合类型参数的初始化，将传入的集合添加到空双向链表中。

**插入元素**

当依据序号插入元素时，需要先移动到指定位置。移动平均 n/2 个元素，时间复杂度为 O(n)。

#### Arraylist 与 LinkedList 区别

- `ArrayList` 和 `LinkedList` 都是不同步的，也就是不保证线程安全；

- `ArrayList` 底层使用的是 **`Object` 数组**；`LinkedList` 底层使用的是 **双向链表** 数据结构

- `ArrayList` 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行`add(E e)`方法的时候， `ArrayList` 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话，时间复杂度就为 O(n)。`LinkedList` 采用链表存储，所以在头尾插入或者删除元素不受元素位置的影响时间复杂度为 O(1)，如果是要在指定位置 `i` 插入和删除元素的话， 时间复杂度为 O(n) ，因为需要先移动到指定位置再插入和删除。

- `LinkedList` 不支持高效的随机元素访问，而 `ArrayList`（实现了 `RandomAccess` 接口） 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于`get(int index)`方法)。
- `ArrayList` 的空间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。

### HashMap

HashMap 主要用来存放键值对，它基于哈希表的 Map 接口实现，是常用的 Java 集合之一，是线程不安全的。

JDK1.8 之前 HashMap 底层是 **数组和链表** 结合在一起使用也就是 **链表散列**。

HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

扰动函数？

(h = key.hashCode()) ^ (h >>> 16)；将 hashCode 无符号右移 16 位，高 16bit 和低 16bit 做异或。原因：当数组长度很小，假设是 16，那么 n-1 即为 1111 ，这样的值和 hashCode() 直接做按位与 操作，实际上只使用了哈希值的后 4 位。如果当哈希值的高位变化很大，低位变化很小，就很容易 造成哈希冲突了，所以这里把高低位都利用起来，让高16 位也参与运算，从而解决了这个问题

JDK1.8之后，当链表长度大于阈值（默认为 8）时，会首先调用 `treeifyBin()`方法。这个方法会根据 HashMap 数组来决定是否转换为红黑树。只有当数组长度大于或者等于 64 的情况下，才会执行转换红黑树操作，以减少搜索时间。否则，就是只是执行 `resize()` 方法对数组扩容。

**loadFactor 负载因子**

loadFactor 负载因子是控制数组存放数据的疏密程度，loadFactor 越趋近于 1，那么 数组中存放的数据(entry)也就越多，也就越密，也就是会让链表的长度增加，loadFactor 越小，也就是趋近于 0，数组中存放的数据(entry)也就越少，也就越稀疏。**loadFactor 太大导致查找元素效率低，太小导致数组的利用率低，存放的数据会很分散。loadFactor 的默认值为 0.75f 是官方给出的一个比较好的临界值**。

给定的默认容量为 16，负载因子为 0.75。Map 在使用过程中不断的往里面存放数据，当数量超过了 16 * 0.75 = 12 就需要将当前 16 的容量进行扩容，而扩容这个过程涉及到 rehash、复制数据等操作，所以非常消耗性能。

**初始化**

四个构造方法中，都初始化了负载因子 loadFactor。HashMap 中没有 capacity 这样的字段，即使指定了初始化容量 initialCapacity ，也只是通过 tableSizeFor 将其扩容到与 initialCapacity 最接近的 2 的幂次方大小，然后暂时赋值给 threshold ，后续通过 resize 方法将 threshold 赋值给 newCap 进行 table 的初始化。

在进行初始化操作时，只是初始化了创建数组的相关参数，并没有真正创建动态数组。真正动态数组的创建是在第一次进行数据写入时引发的。是一种懒加载操作，防止了初始化后而不用的内存浪费。

**put 方法**

通过hash发现要放入的元素的数组位置为null，则直接把该元素放在这里即可

如果定位到的数组位置有元素就和要插入的 key 比较，如果 key 相同就直接覆盖，如果 key 不相同，就判断 p 是否是一个树节点，如果是就调用`e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value)`将元素添加进入。如果不是就遍历链表插入(插入的是链表尾部)。

**数组扩容**

扩容分两步：一是将数组的长度变为原来的两倍。二是将已经hash分布到数组中的所有元素重新计算hash值，分配到新的数组中。而数组大小必须是2^n，就可以提升重hash的性能。

HashMap中所用的hash函数：hash=(key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16)。计算的数字最终要被映射到数组的某个位置tab[i = (n - 1) & hash]。(n-1)为数组中的最大位置，hash为哈希的结果，两者进行了逻辑与操作。与操作之后只能变小，所以与完后一定小于h-1。扩容2倍相当于二进制最高位增加1，与hash相与后处理最高位不一样，其余都与原始n-1计算相同。因此，这样就减少了计算和移动量。

**线程安全问题**

在多线程环境下，多个线程可能同时对HashMap进行修改（添加、删除、修改元素），从而导致不可预期的结果。当多个线程同时执行put操作时，可能会导致键值对的冲突和覆盖。在HashMap的内部实现中，使用了链表或红黑树（Java 8之后）来解决哈希冲突。但在多线程环境下，如果链表节点的next指针被并发修改，可能会导致链表变为环形(JDK7 下)，导致get操作无法正常结束。

### ConcurrentHashMap

线程安全的 HashMap

jdk1.7：`ConcurrnetHashMap` 由很多个分段锁 `Segment` 组合，而每一个 `Segment` 是一个类似于 `HashMap` 的结构，所以每一个 `HashMap` 的内部可以进行扩容。但是 `Segment` 的个数一旦**初始化就不能改变**，默认 `Segment` 的个数是 16 个，你也可以认为 `ConcurrentHashMap` 默认支持最多 16 个线程并发。

jdk1.8：JDK1.8 中的ConcurrentHashMap 选择了与 HashMap 相同的**数组+链表+红黑树**结构；在锁的实现上，抛弃了原有的 Segment 分段锁，采用`CAS + synchronized`实现更加细粒度的锁。将锁的级别控制在了更细粒度的哈希桶数组元素级别，也就是说只需要锁住这个链表头节点（红黑树的根节点），就不会影响其他的哈希桶数组元素的读写，大大提高了并发度。

**初始化**

有参初始化需要设置初始容量、负载因子、并发级别。可以依据并发级别计算划出高于并发级别之上最近的 2的次方值ssize。ssize来确定有几个Segment，是Segment总长度。以及每个Segment的容量。记录 `segmentShift` 偏移量，这个值为【容量 = 2 的 N 次方】中的 N

**put 方法**

###LinkedHashMap

继承自 `HashMap`，并在 `HashMap` 基础上维护一条双向链表，使得具备如下特性:支持遍历时会按照插入顺序有序进行迭代。支持按照元素访问顺序排序,适用于封装 LRU 缓存工具。因为内部使用双向链表维护各个节点，所以遍历时的效率和元素个数成正比，相较于和容量成正比的 HashMap 来说，迭代效率会高很多。

而 `LinkedHashMap` 是在 `HashMap` 的基础上为 bucket 上的每一个节点建立一条双向链表，这就使得转为红黑树的树节点也需要具备双向链表节点的特性，即每一个树节点都需要拥有两个引用存储前驱节点和后继节点的地址

`LinkedHashMap` 定义了排序模式 `accessOrder`(boolean 类型，默认为 false)，访问顺序则为 true，插入顺序则为 false。

**get 方法**

若accessOrder为true，则调用afterNodeAccess将当前元素移到链表末尾。

**remove 方法**

`LinkedHashMap` 并没有对 `remove` 方法进行重写，而是直接继承 `HashMap` 的 `remove` 方法，为了保证键值对移除后双向链表中的节点也会同步被移除，`LinkedHashMap` 重写了 `HashMap` 的空实现方法 `afterNodeRemoval`。`afterNodeRemoval` 方法的整体操作就是让当前节点 p 和前驱节点、后继节点断开联系，等待 gc 回收

**put 方法**

同样的 `LinkedHashMap` 并没有实现插入方法，而是直接继承 `HashMap` 的所有插入方法交由用户使用，但为了维护双向链表访问的有序性重写 `afterNodeAccess`以及`HashMap` 的 `afterNodeInsertion` 方法，当 `removeEldestEntry` 返回 true 时，会将链表首节点移除。除最老的缓存项(即链表的第一个元素)

重写 `removeEldestEntry` 方法当链表大小超过容量时返回 true，使得每次访问一个元素时，该元素会被移动到链表的末尾。一旦插入操作让 `removeEldestEntry` 返回 true 时，视为缓存已满，`LinkedHashMap` 就会将链表首元素移除，由此我们就能实现一个 LRU 缓存。

#### LinkedHashMap 和 HashMap 遍历性能

`LinkedHashMap` 维护了一个双向链表来记录数据插入的顺序，因此在迭代遍历生成的迭代器的时候，是按照双向链表的路径进行遍历的。这一点相比于 `HashMap` 那种遍历整个 bucket 的方式来说，高效需多。

###CopyOnWriteArrayList

线程安全 `List` 实现。`CopyOnWriteArrayList` 线程安全的核心在于其采用了 **写时复制（Copy-On-Write）** 的策略。当需要修改时，不会直接修改原数组，而是会先创建底层数组的副本，对副本数组进行修改，修改完之后再将修改后的数组赋值回去，这样就可以保证写操作不会影响读操作了。

**初始化**

有一个无参构造函数和两个有参构造函数。创建一个空的 CopyOnWriteArrayList、按照集合的迭代器返回的顺序创建一个包含指定集合元素的 CopyOnWriteArrayList、创建一个包含指定数组的副本的列表

**插入元素**

`add`方法内部用到了 `ReentrantLock` 加锁，保证了同步，避免了多线程写的时候会复制出多个副本出来。锁被修饰保证了锁的内存地址肯定不会被修改，并且，释放锁的逻辑放在 `finally` 中，可以保证锁能被释放。

每次写操作都需要通过 `Arrays.copyOf` 复制底层数组，时间复杂度是 O(n) 的，且会占用额外的内存空间。先创建一个新的数组来容纳新添加的元素，然后在新数组中进行写操作，最后将新数组赋值给底层数组的引用，替换掉旧的数组。

**读取元素**

读取操作时不需要进行同步控制和锁操作，可以保证数据的安全性。不过，`get`方法是弱一致性的，在某些情况下可能读到旧的元素值。

## 并发编程

进程是程序的一次执行过程，是系统运行程序的基本单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享进程的**堆**和**方法区**资源，但每个线程有自己的**程序计数器**、**虚拟机栈**和**本地方法栈**。

**程序计数器**用于记录当前线程执行的位置，从而当线程被切换回来的时候能够知道该线程上次运行位置。

**虚拟机栈：** 每个 Java 方法在执行之前会创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在 Java 虚拟机栈中入栈和出栈的过程。

**本地方法栈：** 和虚拟机栈所发挥的作用非常相似，区别是：**虚拟机栈为虚拟机执行 Java 方法 （也就是字节码）服务，而本地方法栈则为虚拟机使用到的 Native 方法服务。** 在 HotSpot 虚拟机中和 Java 虚拟机栈合二为一。

为了**保证线程中的局部变量不被别的线程访问到**，虚拟机栈和本地方法栈是线程私有的。

**堆和方法区**是所有线程共享的资源，其中堆是进程中最大的一块内存，主要用于存放新创建的对象 (几乎所有对象都在这里分配内存)，方法区主要用于存放已被加载的类信息、常量、静态变量、即时编译器编译后的代码等数据

对于单核 CPU 来说，如果任务是 CPU 密集型的，那么开很多线程会影响效率；如果任务是 IO 密集型的，那么开很多线程会提高效率。

###线程的生命周期和状态

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：

- NEW: 初始状态，线程被创建出来但没有被调用 `start()` 。
- RUNNABLE: 运行状态，线程被调用了 `start()`等待运行的状态。
- BLOCKED：阻塞状态，需要等待锁释放。
- WAITING：等待状态，表示该线程需要等待其他线程做出一些特定动作（通知或中断）。
- TIME_WAITING：超时等待状态，可以在指定的时间后自行返回而不是像 WAITING 那样一直等待。
- TERMINATED：终止状态，表示该线程已经运行完毕。

**sleep() 方法和 wait() 方法**

**`sleep()` 方法没有释放锁，而 `wait()` 方法释放了锁** 。wait()被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的 `notify()`或者 `notifyAll()` 方法。`sleep()` 是 `Thread` 类的静态本地方法，`wait()` 则是 `Object` 类的本地方法。因为wait让获得对象锁的线程实现等待，会自动释放当前线程占有的对象锁。要操作对应的对象（`Object`）而非当前的线程（`Thread`）。

**调用 `start()` 方法方可启动线程并使线程进入就绪状态，直接执行 `run()` 方法的话不会以多线程的方式执行。**

#### volatile

"volatile" 是一个关键字，用来修饰变量，主要用于多线程编程中，用来确保变量的可见性和防止指令重排序。相比于使用锁或者原子类等同步机制，volatile是一种轻量级的同步方式。它不需要上锁，不需要进行线程间的等待和唤醒操作，因此在某些场景下可以提升程序的性能。

![image-20231212085212991](../image/image-20231212085212991.png)

JMM中的数据原子操作：

- read（读取）：从主内存读取数据
- load（载入）：将主内存读取到的数据写入工作内存
- use（使用）：从工作内存读取数据来计算
- assign（赋值）：将计算好的值从新赋值到工作内存中
- store（存储）：将工作内存数据写入到主内存
- write（写入）：将store过去的变量值赋值给主内存中的变量
- lock（锁定）：将主内存变量加锁，标识为线程独占状态
- unlock（解锁）：将主内存变量解锁，解锁后其他线程可以锁定该变量

JMM(Java内存模型)操作变量的时候不是直接在主存进行操作的,而是每个线程拥有自己的工作内存,在使用前,将该变量的值copy一份到自己的工作内存,读取时直接读取自己的工作内存中的值.写入操作时,先将修改后的值写入到自己的工作内存,再讲工作内存中的值刷新回主存.

`volatile` 关键字可以保证变量的可见性（线程正在使用对象状态而另一个线程在同时修改该状态，需要确保当一个线程修改了对象状态后，其他线程能够看到发生的状态变化。），如果我们将变量声明为 **`volatile`** ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。`volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。

有volatile修饰的共享变量在进行写操作时的汇编代码是具有lock前缀的指令，lock前缀的指令在多核处理器下会引发两件事：①将当前处理器缓存行的数据写回到系统内存。②处理器将缓存回写到内存的操作会使在其他CPU里缓存了该内存地址的数据无效。
为了提高处理速度，处理器不直接和内存进行通信，而是先将系统内存的数据读到内部缓存后再进行操作，但操作完不知道何时会写回内存。如果对声明了volatile的变量进行写操作，JVM就会向处理器发送一条lock前缀的指令，将这个变量在缓存行的数据写回到系统内存。。为了保证各个处理器的缓存是一致的就会实现缓存一致性协议，每个处理器通过嗅探在总线上传播的数据来检查自己缓存的值是不是过期了，当处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器的缓存行设置为无效状态，当处理器对这个数据进行修改操作的时候，会重新从系统内存中把数据读到处理器缓存里。

volatile实现可见性：用volatile关键字修饰后的变量在操作时，最终解析的汇编指令会在指令前加上`lock前缀指令`来保证工作内存中读取到的数据是主内存中最新的数据。具体的实现原理是在硬件层面上通过**MESI缓存一致性协议**实现的。即多个cpu从主内存读取数据到高速缓存中，如果其中一个cpu修改了数据，会通过总线立即回写到主内存中，其他cpu会通过`总线嗅探机制`感知到缓存中数据的变化并将工作内存中的数据失效，再去读取主内存中的数据。

#### 指令重排

重排序是指编译器和处理器为了优化程序性能而对指令序列进行排序的一种手段。

**禁止指令重排序**

内存屏障：为了实现volatile的内存语义，编译器在生成字节码时，会在指令序列中插入内存屏障来禁止特定类型的处理器重排序。内存屏障有以下三种类型：

- Load屏障（Load Barrier）：确保所有的读操作都发生在Load Barrier之前。
- Store屏障（Store Barrier）：确保所有的写操作都发生在Store Barrier之前。
- Full屏障（Full Barrier）：包括Load屏障和Store屏障的效果，确保了读写操作的顺序性。

#### CAS算法

CAS 的全称是 **Compare And Swap（比较与交换）** ，用于实现乐观锁，被广泛应用于各大框架中。CAS 的思想很简单，就是用一个预期值和要更新的变量值进行比较，两值相等才会进行更新。CAS 是一个原子操作，底层依赖于一条 CPU 的原子指令。CAS 是一种无锁的非阻塞算法的实现。`Java` 中的 `CAS` 操作通常使用 `sun.misc.Unsafe` 类来实现

CAS 涉及到三个操作数：**V**：要更新的变量值，**E**：预期值，**N**：拟写入的新值。当且仅当 V 的值等于 E 时，CAS 通过原子方式用新值 N 来更新 V 的值。如果不等，说明已经有其它线程更新了 V，则当前线程放弃更新。

**ABA 问题**：如果一个变量 V 初次读取的时候是 A 值，并且在准备赋值的时候检查到它仍然是 A 值，在这段时间它的值可能被改为其他值，然后又改回 A，那 CAS 操作就会误认为它从来没有被修改过。这个问题被称为 CAS 操作的 **"ABA"问题。**ABA 问题的解决思路是在变量前面追加上**版本号或者时间戳**。

CAS 经常会用到自旋操作来进行重试，也就是不成功就一直循环执行直到成功。如果长时间不成功，会给 CPU 带来非常大的执行开销。

CAS高效原因：防止获取锁时频繁的上下文切换。CAS适合变量并发访问稍小的情况，大规模并发访问会导致CAS长期处于重试。

#### synchronized

synchronized是Java中的关键字，主要解决的是多个线程之间访问资源的同步性，可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。可以修饰实例方法、静态方法、代码块。

**Monitor**

对象大致可以分为三个部分，分别是对象头，实例变量和填充字节，对象头分成两个部分：MarkWord和 KlassWord。每一个锁都对应一个monitor对象

每个 Java 对象都可以关联一个 Monitor 对象，Monitor 也是 class，其**实例存储在堆中**，如果使用 synchronized 给对象上锁（重量级）之后，该对象的对象头的 Mark Word 中就被设置指向 Monitor 对象的指针，这就是重量级锁。

**工作流程**：

![image-20240222092319550](../image/image-20240222092319550.png)

- 开始时 Monitor 中 Owner 为 null，当 Thread-2 执行 synchronized(obj) 就会将 Monitor 的所有者 Owner 置为 Thread-2，Monitor 中只能有一个 Owner，obj 对象的 Mark Word 指向 Monitor，把对象原有的 MarkWord 存入。
- 在 Thread-2 上锁的过程，Thread-3、Thread-4、Thread-5 也执行 synchronized(obj)，就会进入 EntryList BLOCKED（双向链表）
- Thread-2 执行完同步代码块的内容，根据 obj 对象头中 Monitor 地址寻找，设置 Owner 为空， 把线程栈的锁记录中的对象头的值设置回 MarkWord
- 唤醒 EntryList 中等待的线程来竞争锁，竞争是非公平的，如果这时有新的线程想要获取锁，可能 直接就抢占到了，阻塞队列的线程就会继续阻塞
- 以前获得过锁的线程，但条件不满足会进入 WAITING 状态的线程（wait-notify 机制）



`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。`synchronized` 修饰的方法并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 `ACC_SYNCHRONIZED` 标识，该标识指明了该方法是一个同步方法。

JDK1.6中引入偏向锁和轻量级锁对synchronized进行优化。此时的synchronized一共存在四个状态：无锁状态、偏向锁状态、轻量级锁状态和重量级锁状态。随锁竞争激烈程度，锁的状态会出现一个升级的过程。即可以从偏向锁升级到轻量级锁，再升级到重量级锁。锁升级的过程是单向不可逆的，即一旦升级为重量级锁就不会再出现降级的情况。

**偏向锁**：**在大多数情况下锁不仅不存在多线程竞争关系，而且大多数情况都是被同一线程多次获得**。如果一个线程获得了锁，那么锁就进入偏向模式。当这个线程再次请求锁时，无需再做任何同步操作，即可获取锁的过程。

当锁对象第一次被线程获得的时候进入偏向状态，标记为 101，同时使用 CAS 操作将线程 ID 记录 到 Mark Word。如果 CAS 操作成功，这个线程以后进入这个锁相关的同步块，查看这个线程 ID 是自己的就表示没有竞争，就不需要再进行任何同步操作。当有另外一个线程去尝试获取这个锁对象时，偏向状态就宣告结束。

**轻量级锁**：**对于大部分的锁，在整个同步生命周期内都不存在竞争。**JVM会利用CAS尝试把对象原本的Mark Word 更新为锁记录Lock Record的指针，成功就说明加锁成功

![image-20240222094107211](../image/image-20240222094107211.png)

使用轻量级锁时会创建锁记录Lock Record对象，每个线程的栈帧都会包含一个锁记录的结构，存储锁定对象的 Mark Word。锁记录的Object reference 指向锁住的对象。并且使用CAS将Object的MarkWard更改为锁记录地址以及状态00标识轻量级锁。并将原始的MarkWard存入锁记录中。

CAS失败则有两者情况：一、其他线程持有了锁对象，这证明有竞争，执行锁膨胀。二、由于可重入锁的特性，可能是自己线程以及获取了锁，则就添加一条锁记录作为重入的计数。

当退出synchronized(解锁)时，如果有取值为 null 的锁记录，表示有重入，这时重置锁记录，表示重入计数减 1，如果锁记录的值不为 null，这时使用 CAS 将 Mark Word 的值恢复给Object对象头。成功，则解锁成功；失败，说明轻量级锁进行了锁膨胀或已经升级为重量级锁，进入重量级锁解锁流程。

**重量级锁**：存在锁竞争时，轻量级锁会进入锁碰撞变成重量级锁。

![image-20240222095048950](../image/image-20240222095048950.png)

当 Thread-1 进行轻量级加锁时，Thread-0 已经对该对象加了轻量级锁。Thread-1 加轻量级锁失败，进入锁膨胀流程：为 Object 对象申请 Monitor 锁，将 Monitor 的 Owner 置为 Thread-0，将 Object 的对象头指向重量级锁地址，然后自己进入 Monitor 的 EntryList BLOCKED。

**锁的优化**

**自旋锁**：基于**在大多数情况下，线程持有锁的时间都不会太长**。操作系统实现线程之间的切换时需要从用户态转换到核心态。自旋锁会假设在不久将来，当前的线程可以获得锁，因此虚拟机会让当前想要获取锁的线程做几个空循环(这也是称为自旋的原因)，不断的尝试获取锁。

**锁消除**：对于被检测出不可能存在竞争的共享数据的锁进行消除，这是 JVM 即时编译器的优化。锁消除主要是通过逃逸分析来支持的。

**锁粗化**：对相同对象多次加锁，导致线程发生多次重入，频繁的加锁操作就会导致性能损耗，可以使用锁粗化方式优化。如果虚拟机探测到一串的操作都对同一个对象加锁，将会把加锁的范围扩展（粗化）到整个操作序列的外部。

**死锁**

Java 死锁产生的四个必要条件： 

1. 互斥条件，即当资源被一个线程使用（占有）时，别的线程不能使用 
2. 不可剥夺条件，资源请求者不能强制从资源占有者手中夺取资源，资源只能由资源占有者主动释放 
3. 请求和保持条件，即当资源请求者在请求其他的资源的同时保持对原有资源的占有 
4. 循环等待条件，即存在一个等待循环队列：p1 要 p2 的资源，p2 要 p1 的资源，形成了一个等待环路

定位死锁：使用 jps 定位进程 id，再用 jstack id 定位死锁，找到死锁的线程去查看源码，解决优化

#### AQS

![image-20240222101135048](../image/image-20240222101135048.png)

AQS是抽象队列同步器，它是Java中用于创建同步器的一个抽象基类。AQS使用一个FIFO的等待队列来管理多个线程的竞争和等待，它通过维护一个状态变量来表示同步状态，并提供了一系列的方法来操作状态变量和等待队列，以实现线程的同步和互斥。

AQS 核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是基于 **CLH 锁**  实现的。

CLH 锁是对自旋锁的一种改进，是一个虚拟的双向队列，暂时获取不到锁的线程将被加入到该队列中。AQS 将每条请求共享资源的线程封装成一个 CLH 队列锁的一个结点（Node）来实现锁的分配。

首节点独占锁，后面的结点排序等候，状态state为0表示初始状态可以占有锁，state=1表示已有线程占有，大于1是重入锁，设置的state和入队都是通过cas保证原子操作。首节点结点状态为signal时释放锁将state置为0且唤醒后续结点，后续结点开始自旋判断state获取锁。

#### ReentrantLock

`ReentrantLock` 实现了 `Lock` 接口，是一个可重入且独占式的锁，和 `synchronized` 关键字类似。更灵活、更强大，增加了轮询、超时、中断、公平锁和非公平锁等高级功能。

`ReentrantLock` 里面有一个内部类 `Sync`，`Sync` 继承 AQS，添加锁和释放锁的大部分操作实际上都是在 `Sync` 中实现的。`Sync` 有公平锁 `FairSync` 和非公平锁 `NonfairSync` 两个子类。默认使用非公平锁。

**公平锁**是指多个线程按照申请锁的顺序来获取锁，线程直接进入同步队列中排队，队列中最先到的线程先获得锁。**非公平锁**是多个线程加锁时每个线程都会先去尝试获取锁，如果刚好获取到锁，那么线程无需等待，直接执行，如果获取不到锁才会被加入同步队列的队尾等待执行。

**原理**：底层通过AQS实现的。

**非公平锁**：lock方法调用CAS方法设置state的值，state=0证明无锁，当前线程可以占据。否则执行acquire(1)。nonfairTryAcquire方法首先调用getState方法获取state的值，如果state的值为0(之前占领锁的线程刚好释放了锁)，那么占据。否则那就调用查看占用锁的线程是不是自己，如果是的话那就直接将state + 1，然后返回true。如果state不为0且锁的所有者又不是自己，那就返回false，然后线程会进入到同步队列中。

**公平锁**：lock方法调用CAS方法设置state的值，state=0证明无锁。此时要先考虑同步队列是否有线程等待，再考虑自己线程。若！=0，则看是否是自己线程的锁，如果是的话那就直接将state + 1，然后返回true。不是就将本线程加入同步队列。

**synchronized 和 ReentrantLock 区别**

两者都是可重入锁，线程可以再次获取自己的内部锁。`synchronized` 是依赖于 JVM 实现的，`ReentrantLock` 是 JDK 层面实现的。相比`synchronized`，`ReentrantLock`增加了一些高级功能。**等待可中断**、**可实现公平锁** 、**可实现选择性通知（锁可以绑定多个条件）**。

- **等待可中断** : `ReentrantLock`提供了一种能够中断等待锁的线程的机制，通过 `lock.lockInterruptibly()` 来实现这个机制。也就是说正在等待的线程可以选择放弃等待，改为处理其他事情。

- **可实现公平锁** : `ReentrantLock`可以指定是公平锁还是非公平锁。而`synchronized`只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁。`ReentrantLock`默认情况是非公平的，可以通过 `ReentrantLock`类的`ReentrantLock(boolean fair)`构造方法来指定是否是公平的。

- **可实现选择性通知（锁可以绑定多个条件）**: `synchronized`关键字与`wait()`和`notify()`/`notifyAll()`方法相结合可以实现等待/通知机制。`ReentrantLock`类当然也可以实现，但是需要借助于`Condition`接口与`newCondition()`方法。


Synchronized适合于并发竞争低的情况，因为Synchronized的锁升级如果最终升级为重量级锁在使用的过程中是没有办法消除的，意味着每次都要和cpu去请求锁资源，而ReentrantLock主要是提供了阻塞的能力，通过在高并发下线程的挂起，来减少竞争，提高并发能力。

#### ThreadLocal

![image-20231212142444317](../image/image-20231212142444317.png)**原理**

![image-20240129092149529](../image/image-20240129092149529.png)

每个Thread线程内部都有一个ThreadLocalMap。当第一次调用ThreadLocal的get()或set()方法时，会自动创建并初始化该线程的ThreadLocalMap对象。这个ThreadLocalMap对象是线程的属性。但ThreadLocalMap这个类是ThreadLocal的内部类，没有实现 Map 接口。ThreadLocalMap里面存储线程本地对象ThreadLocal（key）和线程的变量副本（value）。

![image-20240129094246943](../image/image-20240129094246943.png)

其ThreadLocalMap中的Entry使用的是K-V方式来组织数据，Entry中key是ThreadLocal对象，且是一个弱引用（弱引用，生命周期只能存活到下次GC前）。

**ThreadLocalMap为什么要用ThreadLocal做key，而不是用Thread做key？**

一个线程中很有可能不只使用了一个ThreadLocal对象。通过Thread对象，无法知道要获取哪个ThreadLocal对象。 

**Entry的key为什么设计成弱引用？**

假如key对ThreadLocal对象的弱引用，改为强引用。ThreadLocal变量生命周期完了，设置成null了，但由于key对ThreadLocal还是强引用。ThreadLocal对象和ThreadLocalMap都将不会被GC回收，于是产生了内存泄露问题。如果key是弱引用，当ThreadLocal变量指向null之后，在GC做垃圾清理的时候，key会被自动回收，其值也被设置成null。

**设计成弱引用就彻底不存在内存泄漏了吗？**

假如ThreadLocalMap中存在很多key为null的Entry，但后面的程序，一直都没有调用过有效的ThreadLocal的get、set或remove方法。 那么，Entry的value值一直都没被清空。其结果就是：Entry和ThreadLocalMap将会长期存在下去，会导致内存泄露。因为只有当其他的ThreadLocal变量，调用了它的get、set或remove，三个方法中的任何一个方法，都会自动触发清理机制，将key为null的value值清空。这样在GC时才会清理这个Entry不会内存泄漏。所以为了防止这样的内存泄漏需要在使用完ThreadLocal对象之后调用一次remove方法。

**hash冲突**

当你在一个线程需要保存多个变量时，需要创建多个ThreadLocal对象防止Key相同造成hash冲突。和HashMap的最大的不同在于，ThreadLocalMap解决Hash冲突的方式就是简单的步长加1或减1及线性探测，寻找下一个相邻的位置。当set方法存储数据时，使用hash方法计算位置，若位置有值会一直探测下一个地址。如果找到最后一个，还是没有找到，则再从头开始找。

**清理**

在探测过程中会复用key为null的脏对象并进行垃圾清理防止内存泄漏。

探测式清理：从当前节点(hash计算的值)开始遍历数组，key==null的将entry置为null，key!=null的对当前元素的key重新hash分配位置，若重新分配的位置上有元素就往后顺延。

启发式清理：从当前节点开始，进行do-while循环检查清理过期key，遇到时会触发探测式清理。结束条件是连续`n`次未发现过期key就跳出循环，n是经过位运算计算得出的，可以简单理解为数组长度的2的多少次幂次。例如：数组长度是16，那么24=16，也就是连续4次没有过期Entry。

在set、get方法中遇到过期key时会触发探测式清理。rehash中会触发探测式清理。set方法最后会执行一次发式清理，在启发式清理中发现过期key会触发探测式清理。

**ThreadLocal是如何扩容的？**

存在一个扩容阈值，为数组长度的2/3.

在set方法最后会进行扩容。扩容开始先触发探测式清理。当清理完成之后现有数组超过3/4阈值则会进扩容。数组扩容为原来的2倍。重新计算key的散列值。重新设置阈值为2/3的新数组值。

**InheritableThreadLocal**

InheritableThreadLocal类可以在子线程中能够正常获取父线程中设置的值。在它的init方法中会将父线程中往ThreadLocal设置的值，拷贝一份到子线程中。

**线程池中如何共享数据？**

使用InheritableThreadLocal不能满足。因为第一次submit任务的时候，该线程池会自动创建一个线程。因为使用了InheritableThreadLocal，所以创建线程时，会调用它的init方法，将父线程中的inheritableThreadLocals数据复制到子线程中。但第二次submit后是复用之前线程，不再调用它的init方法。此时应当使用阿里开源的TransmittableThreadLocal

#### 线程池

线程池就是管理一系列线程的资源池。当有任务要处理时，直接从线程池中获取线程来处理，处理完之后线程并不会立即被销毁，而是等待下一个任务。池化技术的思想主要是为了减少每次获取资源的消耗，提高对资源的利用率。

线程池的参数：核心线程数、最大线程数、存活时间、时间单位、任务队列、线程工厂、拒绝策略

顶级接口Executor提供了一种方式，解耦任务的提交和执行，只定义了一个 execute(Runnable command) 方法用来提交任务，至于具体任务怎么执行则交给他的实现者去自定义实现。ExecutorService 接口继承 Executor，且扩展了生命周期管理的方法、返回 Futrue 的方法、批量提交任务的方法。AbstractExecutorService 抽象类继承 ExecutorService 接口，对 ExecutorService 相关方法提供了默认实现，用FutureTask 包装 Runnable 任务，交给 execute() 方法执行，然后可以从该 FutureTask 阻塞获取执行结果。

ThreadPoolExecutor 继承 AbstractExecutorService，采用池化思想管理一定数量的线程来调度执行提交的任务，且定义了一套线程池的生命周期状态，用一个 ctl 变量来同时保存当前池状态（高3位）和当前池线程数（低29位）。ThreadPoolExecutor 类里的方法大量有同时需要获取或更新池状态和池当前线程数的场景，放一个原子变量里，可以很好的保证数据的一致性以及代码的简洁性。

**通过`ThreadPoolExecutor`构造函数来创建**

~~~java
// 实例化一个线程池
ThreadPoolExecutor executor = new ThreadPoolExecutor(3, 10, 60,
        TimeUnit.SECONDS, new ArrayBlockingQueue<>(20));
// 使用线程池执行一个任务        
executor.execute(() -> {
    // Do something
});
// 关闭线程池,会阻止新任务提交，但不影响已提交的任务
executor.shutdown();
// 关闭线程池，阻止新任务提交，并且中断当前正在运行的线程
executor.showdownNow();

public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,//时间单位
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,//线程工厂，用来创建线程，一般默认即可
                              RejectedExecutionHandler handler//拒绝策略，
                               )
//corePoolSize 线程池的核心线程数量,有任务提交到线程池时，如果线程池中的线程数小于corePoolSize,那么则直接创建新的线程来执行任务。
//workQueue 是一个阻塞队列，用于存储来不及执行的任务的队列。当有任务提交到线程池的时候，如果线程池中的线程数大于等于corePoolSize，那么这个任务则会先被放到这个队列中，等待执行。
//maximumPoolSize 线程池的最大线程数,表示线程池支持的最大线程数量。当一个任务提交到线程池时，线程池中的线程数大于corePoolSize,并且workQueue已满，那么则会创建新的线程执行任务，但是线程数要小于等于maximumPoolSize。
//keepAliveTime 线程数大于核心线程数时，多余的空闲线程存活的最长时间
//handler 当线程池中的线程达到maximumPoolSize线程数后且workQueue已满的情况下，再向线程池提交任务则执行对应的拒绝策略
~~~

![image-20231212144805549](../image/image-20231212144805549.png)

线程池从诞生到死亡，中间会经历running、shutdown、stop、tidying、terminated五个生命周期状态。

线程池任务流程

![image-20231212150022684](../image/image-20231212150022684.png)

**阻塞队列**

- ArrayBlockQueue：由数组结构组成的有界阻塞队列 
- LinkedBlockingQueue：由链表结构组成的无界（默认大小 Integer.MAX_VALUE）的阻塞队列 
- PriorityBlockQueue：支持优先级排序的无界阻塞队列 
- DelayedWorkQueue：使用优先级队列实现的延迟无界阻塞队列 
- SynchronousQueue：不存储元素的阻塞队列，每一个生产线程会阻塞到有一个 put 的线程放入元 素为止 
- LinkedTransferQueue：由链表结构组成的无界阻塞队列 
- LinkedBlockingDeque：由链表结构组成的双向阻塞队列

线程池饱和策略分为一下几种：

1）AbortPolicy：线程池默认的拒绝策略，触发时会抛出 RejectedExecutionException 异常。如果是一些比较重要的业务，可以使用该拒绝策略，在系统不能进一步支持更大并发量的情况下通过抛出异常及时发现问题并进行处理。

2）CallerRunsPolicy：在线程池没关闭的情况下，由调用者线程去处理任务，反之直接丢弃。此拒绝策略追求任务都能被执行，不丢失，比较适合并发量不大并且不允许丢失任务的场景场景，性能较低。

3）DiscardPolicy：丢弃任务，不抛出异常，一般无感知。建议一些无关紧要的任务可以使用此策略。

4）DiscardOldestPolicy：丢弃队列中最老的任务，然后重新提交被拒绝的任务。需要根据业务场景进行选择是否要用。

3、4 这两种拒绝策略都在会在无感知的情况下丢弃任务，需要根据业务场景决定是否要使用。

**Worker 类**

Worker 是ThreadPoolExecutor的内部类，继承AQS，实现的Runnable,所以本身具有锁的特性，又是一个可执行任务。Worker 类内部有线程属性Thread，即为线程池的线程，Runnable类的任务firstTask。

执行线程池 `execute()` 方法提交任务时，如果需要创建一个新的工作线程，则会调用 `addWorker()` 方法，并传递Runnable类的任务绑定给线程。调用 worker对象 中 thread 的 `start()` 方法启动线程。线程的任务是worker对象本身(实现l1的Runnable接口)。

启动任务后，会执行到 Worker 的 `run()` 方法，最终的任务执行逻辑在 `runWorker()` 方法中：执行创建时添加的 `firstTask` 任务或不断调用 `getTask()` 方法从队列拉取任务，获取到任务后先执行 beforeExecute() 钩子函数，再执行任务，然后再执行 afterExecute() 钩子函数。若超时获取不到任务会调用 processWorkerExit() 方法执行 Worker 线程的清理工作。

**ThreadPoolExecutor中的锁**

**mainLock 锁**：ThreadPoolExecutor 内部维护了 ReentrantLock 类型锁 mainLock，在访问 workers 成员变量以及进行相关数据统计（ largestPoolSize、completedTaskCount）时需要获取该重入锁。目的是workers 变量用的 HashSet 是线程不安全的，是不能用于多线程环境的。以及一些数据变量也没有用volatile 修饰，需要保护。

相比于线程安全容器，此处更适合用 lock，主要原因之一就是串行化 interruptIdleWorkers() 方法，避免了不必要的中断风暴。interruptIdleWorkers() 方法在多线程访问下就会发生这种情况。一个线程调用interruptIdleWorkers() 方法对 Worker 进行中断，此时该 Worker 出于中断中状态，此时又来一个线程去中断正在中断中的 Worker 线程，这就是所谓的中断风暴。

 largestPoolSize、completedTaskCount不使用volatile ，为了保证这两个参数的准确性，在获取这两个值时，能保证获取到的一定是修改方法执行完成后的值。如果不加锁，可能在修改方法还没执行完成时，此时来获取该值，获取到的就是修改前的值，然后修改方法一提交，就会造成获取到的数据不准确了。

**Worker 线程锁**：该锁主要是用来维护运行中线程的中断状态。在runWorker() 方法中获取到任务开始执行前，需要先调用 w.lock() 方法，lock() 方法会调用 tryAcquire() 方法，tryAcquire() 实现了一把非重入锁，通过 CAS 实现加锁。interruptIdleWorkers() 方法会中断那些等待获取任务的线程，会调用 w.tryLock() 方法来加锁，如果一个线程已经在执行任务中，那么 tryLock() 就获取锁失败，保证不能中断运行中的线程了。

**execute() 和 submit() 不同**

execute() 无返回值，submit() 有返回值，会返回一个 FutureTask，然后可以调用 get() 方法阻塞获取返回值。Callable接口类似于Runnable，但是Runnable不会返回结果，并且无法抛出返回结果的异常，而Callable功能更强大一些，被线程执行后，可以返回值。

FutureTask 表示一个可以取消的异步运算类。它有启动和取消运算、查询运算是否完成和取回运算结果等方法。只有当运算完成的时候结果才能取回，如果运算尚未完成get方法将会阻塞。一个FutureTask对象可以对调用了Callable和Runnable的对象进行包装，由于FutureTask也是调用了Runnable接口所以它可以提交给Executor来执行。

调用 submit() 方法提交的任务（Runnable or Callable）会被包装成 FutureTask() 对象。FutureTask 类提供了多种任务状态。并且有Callable类的任务以及线程Thread runner。创建 FutureTask 对象时 state 置为 NEW，callable 赋值为我们传入的任务，并设置线程启动。 执行任务，执行成功后会将结果赋值给 outcome。

**优雅关闭线程池**

shuwdown：如果线程正在执行线程池里的任务，即便任务处于阻塞状态，线程也不会被中断，而是继续执行。如果线程池阻塞等待从队列里读取任务，则会被唤醒，但是会继续判断队列是否为空，如果不为空会继续从队列里读取任务，为空则线程退出。

shutdownnow：将线程池的状态设置为STOP，正在执行的任务则被停止，没被执行任务的则返回。

线程池如何进行监控？
①taskCount，线程池需要执行的任务数量。②completedTaskCount，线程池在运行过程中已经完成的任务数量，小于或等于taskCount。③largestPoolSize，线程池里曾经创建过的最大线程数量，通过这个数据可以知道线程池是否曾经满过，如果该数值等于线程池的最大大小表示线程池曾经满过。④getPoolSize，获取线程池的线程数量，如果线程池不销毁的化线程池里的线程不会自动销毁，所以这个数值只增不减。⑤getActiveCount，获取活动的线程数。
通过扩展线程池进行监控，可以继承线程池来自定义，重写线程池的beforeExecute、afterExecute和terminated方法，也可以在任务执行前、执行后和线程池关闭前来执行一些代码进行监控，例如监控任务的平均执行时间、最大执行时间和最小执行时间。

**CPU 密集型任务(N+1)：** 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1。比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。

**I/O 密集型任务(2N)：** 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。

动态修改线程池参数：对线程池的核心参数实现自定义可配置corePoolSize、maximumPoolSize、workQueue。

根据任务的优先级来执行的线程池

考虑使用 `PriorityBlockingQueue` （优先级阻塞队列）作为任务队列。`PriorityBlockingQueue` 是一个支持优先级的无界阻塞队列，可以看作是线程安全的 `PriorityQueue`，两者底层都是使用小顶堆形式的二叉堆，即值最小的元素优先出队。

