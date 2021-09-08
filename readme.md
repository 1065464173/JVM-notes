⚠️整理不易～ 希望我的学习笔记可以给大家在学习JVM的道路上提供少许帮助！
---
# 第一章 JVM概述

## 1.1 WHAT-JVM(Java Virtual Machine)是什么

- **是JRE的一部分**。在真实计算机上模拟计算机功能（虚拟机）
- **Java语言可移植性**：“一次编译，多次运行”**是建立在JVM的基础上。任何平台装有针对该平台的JVM，字节码文件（.class）就可以在该平台上运行。
- **跨平台的软件和跨语言的平台**,可多语言混合编程,整合开放接口标准的各种API,技术
- **JVM和Java语言没有必然联系**，只与特定二进制文件格式.Class所关联。

## 1.2 WHY-为什么要学习JVM

- **面试的需要，技术深度的体现**：JVM 技术是大厂面试的必备技能
- **中高级程序员、架构师必备技能**：项目管理和性能调优需要。
- **设计高级扩展应用和诊断运行时问题的基础**：应用上线时的问题处理、GC日志等。
- **深入了解Java**：底层原理、JIT、垃圾回收。相当于数学公式的推导过程。

## 1.3 WHERE-使用场景和现状

* **作为平台**：JVM很重要。Scala、Kotlin、JavaScript、Groovy等都是Java平台一部分
* **作为文化**：第三方开源软件和框架，JDK、JVM的开源实现如OpenJDK、Harmony等
* **作为社区**：桌面应用、嵌入式开发、企业应用、后台服务器、中间件……

---

## 1.3 主要学习内容概述

* **学习版本和资料**：Oracle发布的HotSpot虚拟机规范、《深入理解Java虚拟机》
* **自动内存管理**
  * Java内存区域与内存溢出异常
  * 垃圾收集器与内存分配策略
  * 虚拟机性能监控和故障处理工具
* **虚拟机执行子系统**
  * 类文件结构
  * 虚拟机类加载机制
  * 虚拟机字节码执行引擎
* **程序编译与代码优化**
* **高效并发**
  * Java内存模型与线程
  * 线程安全与锁优化

## 1.4 前提概念

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210815202418.png" alt="image-20210815202417969" style="zoom: 45%;" />

### 1.4.1 字节码（.Class)

* 任何能在jvm平台上执行的字节码格式都一样，统称为**jvm字节码**
* 包含JVM指令集（或称为字节码、Bytecodes）和符号表、其他辅助信息
* 可以在不同的JVM上运行，不同编译器可以编译出相同字节码文件

### 1.4.2 虚拟机（Virtual Machine）

一款软件用于执行一系列计算机指令。上面的软件限制于虚拟机提供资源。

#### 1）虚拟机分类

* 系统虚拟机：物理计算机的仿真。如VMware等
* 程序虚拟机：专门执行单个计算机程序。JVM

#### 2）JVM位置和整体结构

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210815214741.png" alt="image-20210815214741547" style="zoom: 30%;" />

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210816111958.png" alt="image-20210816111958409" style="zoom:40%;" />

#### 3）JVM架构模型

* **基于栈指令集架构**：适用资源受限系统,简单,零地址指令,不依赖硬件,跨平台性好
* **基于寄存器指令集架构**：依赖硬件、性能执行优秀，多地址指令—指令少

*Java指令都根据栈设计(跨平台)—不同平台CPU架构不同，所以不能基于寄存器*

#### 4）JVM生命周期

* **启动**：通过引导类加载器(bootstrap class loader)创建初始类(initial class)完成
* **执行**：执行的为jvm的进程，进程启动时运行，结束时停止
* **退出**：正常结束、异常终止、操作系统错误终止、主动终止(exit或halt方法)、

#### 5）不同的JVM和发展历程

* **Classic**:JDK1.0,首款JVM,只提供解释器不提供即时编译器
* **Exact**:JDK1.2,准确式内存管理,现代高性能VM的雏形
* ***<u>HotSpot</u>:*JDK1.3,主流VM,热点代码探测,即时编译,栈上替换,编译器和解释器协同工作,取得响应时间与最佳执行性能的平衡,学习基于此虚拟机**
* ***<u>JRockit</u>***:专注于服务端应用,全靠即时编译器执行,不考虑响应时间(启动速度)—**行业测试是世界上最快的JVM**
* ***<u>IBM的J9</u>***:简称IT4J,市场定位和HotSpot相似,开源J9(Eclipse OpenJ9)
* **Azul**: 这款虚拟机与特定软硬件配合，非常优秀，发布Zing JVM通用平台
* **BEA Liquid**高性能Java虚拟机的战斗机，不需要操作系统支持，应用场景有限
* **Apache Harmony**：jdk1.5\1.6 曾经的开源JVM，被OpenJDK打败
* **Microsoft JVM**:只能在window平台下运行， 被Sun告了没了
* **TaobaoJVM**:阿里开发,基于OpenJDK,性能好,兼容性不行,严重依赖intel的cpu
* **Dalivik**:不是JVM,不能执行java的Class文件,基于寄存器,dex 格式,提前编译
* **其他**:Java Card、Squawk、JavaInJava、Maxine、Jikes、RVM、IKVK.NET、Jam、Cacao、Sable、Kaffe、Jelatine、Nano、MRP、Moxie
* **Graal:未来的趋势,在HotSpot基础上增强的跨语言全栈虚拟机,可作为任何语言的运行平台使用,取代HotSpot的希望最大,Oracle重点发展对象**

### 1.4.3 Java运行过程

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210815220222.png" alt="image-20210815220222020" style="zoom:50%;" />

## 1.5 常用调优工具

* VisualVM
* JDK命令行：jinfo、js、javap、jmap
* Eclipse：Memory Analyzer Tool
* Jconsole
* Jprofiler
* Java Flight Recorder
* CGViewer
* GC Easy

## 1.6 自己编译JDK（基于MacOS）

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210901140359.png" alt="image-20210901140359334" style="zoom: 33%;" />

1. **构建编译环境**：MacOSx10.13版本以上、**安装XCode**(提供了CLang编译器)

   <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210901135833.png" alt="image-20210901135833709" style="zoom:50%;" />

2.  到[OpenJDK](https://jdk.java.net)官网下载所需版本的Zip完整的源码包——主要编译src

   <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210901162309.png" alt="image-20210901162309853" style="zoom:50%;" />

3. 当前openjdk目录下进行编译前的自动配置工作：`sh configure`

   * 发现报错 1

   ```
   configure: error: No xcodebuild tool and no system framework headers found, use --with-sysroot or --with-sdk-name to provide a path to a valid SDK
   ```

   解决执行命令

   ```
   sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
   ```

   * 发现报错2

   ```
   The tested number of bits in the target (64) differs from the number of bits expected to be found in the target (32)
   ```

   尝试使用官方提供的cross-compilation命令`sh configure --with-target-bits=64 ` ，报新错误

   ```
   configure: error: It is not possible to use --with-target-bits=64 on a 32 bit system. Use proper cross-compilation instead.
   //提示我们不能在32位的系统上构建64位的jdk，但是设置32位又会报我们测试属于64位
   ```

   **暂时推断m1芯片暂时不能自主编译jdk**

   

---

# 第二章 内存与垃圾回收

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210819163243.png" alt="image-20210819163243381" style="zoom:50%;" />

## 2.1 类加载子系统

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210816113523.png" alt="image-20210816113523468" style="zoom:50%;" />

**加载Loading—>验证Verification—>准备Preparation—>解析Resolution—>初始化**

### 2.1.1 Loading—类加载过程一

* 获取定义类的二进制字节码
* 将字节流所代表的静态存储结构转化为方法区的运行时数据结构
* 内存中生成类的Class对象

### 2.1.2 Linking—类加载过程二

* **验证Verify**：保证加载类的正确性，四种验证(文件、字节码、元数据、符号引用)
* **准备Prepare**：为类变量分配默认初始值(不包含常量)—不初始化实例变量
* **解析Resolve**：常量池的符号引用转换为直接引用

```java
//prepare:a=0 //initial:a=1
//数值类型不同初始值也不同 常量在编译的时候就已经赋值了
private static int a = 1;
```

### 2.1.3 Initializaiton—类加载过程三

执行类构造器方法<clinit>的过程，该方法是自动收集类中所有的类变量赋值动作和静态代码块中的语句合并而成。不同于类的构造器

## 2.2 类加载器分类

类加载器是包含关系，不是上下层和继承关系

* **引导类加载器Bootstrap ClassLoader**：使用C/C++实现，用于加载Java核心类库
* **自定义类加载器 User-Definded ClassLoader**：所有继承于ClassLoader的类加载器
  * Extension Class Loader：加载系统属性所指定的目录,或jre/lib/ext子目录下的JAR
  * Sysytem Class Loader：加载环境变量classpath和用户自定义类

```java
// 获取用户自定类加载，默认使用系统类进行加载
ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
System.out.println(classLoader);//sun.misc.Launcher$AppClassLoader@18b4a
//String类是使用引导类加载的
//Java核心类库都是使用引导类加载的，只加载包名为java、javax，sun开头的类
ClassLoader classLoader1 = String.class.getClassLoader();
System.out.println(classLoader1);//null 
```

### 2.2.1 自定义类加载器

为了隔离加载类(防冲突),修改类的加载方式,扩展加载源,**防止源码泄漏(防反编译)**

* **复杂需求**：继承ClassLoader并重写findClass方法
* **简单需求**：继承URLClassLoader—避免自己编写findClass和获取字节码

### 2.2.2 ClassLoader抽象类

所有类加载器都直接或间接地继承ClassLoader除了启动类加载器。

**四种获取ClassLoader的方式**

### 2.2.3 双亲委派机制（parents delegation）

JVM对Class文件采用按需加载，加载类的Class时采用**双亲委派模式**—请求交给父类。为了防止系统被自定义类攻击。**（责任链模式）**

* **工作原理**
  * 类加载器收到加载请求时**把请求委托给父类加载器执行**。
  * 向上委托依次递归，请求最终到达顶层启动类加载器。
  * 如果父类可以完成类加载任务则成功返回，**无法完成时子加载器才会自己加载**。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210816154056.png" alt="image-20210816154056562" style="zoom:40%;" />

​	<u>解释了为何自定义String类时系统加载的还是核心库的类而不是自定义的类</u>

* **优势**：避免类重复加载、保护程序安全，防止核心API被随意篡改
* **沙箱安全机制**：对Java核心源代码的保护

### 2.2.4 其他 

* **判断两个class对象是是否为同一个类**

  * 全类名一致

  * 加载类的加载器必须相同——ClassLoader的实例对象

    换句话说，class对象来源同一个Class文件，被同一个虚拟机加载。

* **类加载器的引用**

  类是用户类加载器加载时，**JVM会保存类加载器的引用作为类型信息部分在方法区**。解析类型间的引用时，JVM需要保证两个类型的类加载器是相同的

* **类的使用**

  * **主动**：七种情况如创建类实例、访问类静态属性、反射、初始化子类、启动类等
  * **被动**：七种开外的其他情况，**不会导致类的初始化**

---

## 2.2 运行时数据区概念及线程

![image-20210816181332616](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210816181332.png)

* **内存**：硬盘和CPU的中间仓库和桥梁，承载着实时运行

* **Runtime 运行时类**：每个JVM只有一个Runtime实例—运行时环境

* **线程间共享(每个进程)：堆、堆外内存(永久代或元空间、代码缓存)**

* **每个线程：独立包括程序计数器、栈、本地栈。**
  * HotSpot里每个Java线程对应映射一个操作系统本地线程，同时创建回收
  * HotSpot里主要线程：虚拟机、周期任务、GC、编译、信号调度(线程)
  
* **常见面试题**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820143256.png" alt="image-20210820143256798" style="zoom:30%;" />

## 2.3 程序计数器（PC Register）

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818114420.png" alt="image-20210818114420476" style="zoom:50%;" />

JVM PC寄存器模拟物理PC寄存器储存指向下一条指令的地址，交给执行引擎读取。

* **很小的一块内存空间，几乎可以忽略不计，运行速度最快的内存储存区域。**
* **唯一一个没有规定任何OutOfMenoryError(OOM)情况的区域**
* JVM规范中，每个线程都有PC计数器—**线程私有**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210816183412.png" alt="image-20210816183412815" style="zoom:40%;" />

* **常见面试问题**：

  * **使用PC寄存器储存字节码指令地址有什么用？为什么使用？**

    保存现场记录，明确执行引擎下一条执行的字节码指令

  * **PC寄存器为什么被设定为线程私有的？**

    为了能够准确地记录各个线程正在执行的当前字节码指令地址。

## 2.4 Java虚拟机栈

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818114340.png" alt="image-20210818114340112" style="zoom:50%;" />

Java指令都基于栈设计，优点：跨平台，指令集小，易实现。缺点：指令多。

* **栈是运行时的单位。**是种快速的分配存储方式，访问速度仅次于PC计数器
* 线程私有。栈内部保存多个栈帧对应相应的方法调用—**保存方法局部变量和部分结果，参与方法的调用和返回**
* java虚拟机栈是对java方法执行的线程内存模型的描述。
* 栈空间是固定或动态的,通过-Xss设置线程最大栈空间,**不同操作系统有默认栈大小**。


### 2.4.1 栈的存储单位和运行

* **存储单元**：栈帧——线程的每个方法对应各自的栈帧
* **栈的操作**：入栈出栈（压栈出栈），遵循先进先出后进先出的原则
* **栈的运行**
  * 一个时间点只有一个活动栈帧—当前帧，对应的方法是当前方法。
  * 执行引擎运行的所有字节码指令只针对当前栈帧操作
  * 不同线程的栈帧是不能互相引用的（都是私有的）
  * 栈帧弹出——正常return或抛出异常

### 2.4.2 栈帧的内部结构

![image-20210817165329656](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210817184619.png)

**栈顶缓存技术的构想**：为了解决零指令过多的读写问题影响执行速度，栈顶元素全部缓存在屋里CPU寄存器中，降低对内存的读写次数，提升执行引擎的执行效率。

#### 1）结构一—**局部变量表Local Variables（局部变量数组或本地本量表）**

用于存储方法参数和其局部变量(**编译器可知的基本数据类型,对象引用,返回地址类型**)本质为一个数字数组，容量大小在编译器确定下来，其方法嵌套次数由栈大小决定。

* **局部变量表中的变量是重要的垃圾回收根节点，直接或间接引用对象不会被回收**

* **Slot 变量槽—局部变量表的基本存储单元**

  * 一个槽32位字节大小(引用类型占32位，double、float64位)
  * 由构造方法或实例方法创建的帧会**多一个this引用在index为0的slot处**
  * **栈帧局部变量表的槽是可以重复利用的—节省资源**

  ```java
  public void test(){ //共4个变量 this, a, b, c. 但是槽位为三个
    int a = 0;
    {
      int b = 0;//原因是b的作用域短，但是数组大小已经开辟了
      b = a + 1;
    }
    int c = a + 1; //b结束后的槽位给c使用——slot重复利用
  }
  变量按照在类中声明的位置分：
  ① 成员变量：在使用前，都经历过默认初始化赋值
        类变量： linking的prepare阶段给类变量默认赋值 
          			initial阶段：给类变量显式赋值即静态代码块赋值
        实例变量：随着对象的创建，会在堆空间中分配实例变量空间，并进行默认赋值
  ② 局部变量：在使用前，必须要进行显式赋值的！否则，编译不通过
  ```

#### 2）结构二—**操作数栈Operand Stack（表达式栈）**

我们所说的JVM**解释引擎是基于栈的执行引擎，其中的栈就是指操作数栈。**主要用于保存计算过程中的中间结果，作为计算机过程变量临时的存储空间。初始为空。

* 方法执行过程中，根据字节码指令，在栈中写入或提取数据（push/pop）
* 调用方法有返回值时，其返回值被压栈
* 有明确的栈的最大深度 max_stack，一个单位深度为32位。

#### 3）结构三—动态链接Dynamic Linking（*指向运行时常量池的方法引用*）

每个栈帧内部都包含指向**运行时常量池中该栈帧所属的方法的引用—为了实现动态链接**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210817185518.png" alt="image-20210817185518056" style="zoom: 40%;" />

#### 4）结构四—方法返回地址Return Address（正常退出异常退出的定义）

存放该方法的PC寄存器的值，方法结束退出后返回该方法被调用的位置。

* **正常完成出口**：**调用者的pc计数器的值作为返回地址，即调用该方法的指令的下一条指令地址**。返回指令根据方法返回值的类型而定（ireturn、lreturn… ）

* **异常完成出口**：抛出的异常处理储存在异常处理表。返回地址要**通过异常表确定**。

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210817221748.png" alt="image-20210817221748063" style="zoom:50%;" />

#### 5）结构五—一些附加信息

不一定有。可能携带与Java虚拟机实现相关的一些附加信息，如对程序调试提供的支持

#### 6）方法的调用

在JVM中，将符号的引用转换为调用方法的直接引用与方法的绑定机制相关。

* **静态链接**：调用的目标方法在编译期可知
  * **早期绑定**（this、super、继承）
  * **静态分派**：编译阶段依赖静态类型来决定方法执行版本的分派过程**（重载）**
  * **非虚方法**：静态方法、私有方法、final方法、实例构造器、父类方法
  * **调用指令**
    * invokestatic 调用静态方法
    * invokespecial 调用\<init>方法、私有以及父类方法
    * 子类调用父类的final修饰的方法没有明确显示父类调用则会使用invokevirtual
* **动态链接**：调用方法在编译期无法确定
  * **晚期绑定**（多态动态绑定）
  * **动态分派**：运行期间根据实际类型确定方法执行版本的分派过程**（重写）**
  * **虚方法**：能被重写的方法就是虚方法，不管是父类还是子类中定义的
  * **调用指令**
    * invokedynamic—动态指令Java7中增加,8的Lambda表达式促成指令的直接生成
    * invokevirtual 调用所有虚方法 —**除了final修饰的方法**
    * invokeinterface 调用接口方法
  * **重写的本质**：
    * 在操作数栈顶**找第一个元素所指向的对象的实际类型**,记作C。
    * 在类型C中找到与常量中描述符、简单名称都相符的方法,进行访问权限校验 
    * 如果通过则返回这个方法的直接引用,查找过程结束。如果不通过,则返回 java.lang.IllegalAccessError异常（运行时类发生不兼容的改变）
    * 否则,按照继承关系从下往上依次对C的各个父类进行第2步的搜索和验证过程。
    * 如果始终没有找到合适的方法,则拋出java.lang.AbstractMethodError异常。

* **虚方法表**：解决频繁的动态分派—原来每次分派都要递归父类查找。提高性能。
  * 通过建立虚方法表可以使用索引表代替查找。
  * 每个类中都有一个虚方法表存放各个方法的实际入口。
  * 表在**类加载的链接阶段被创建并初始化，在类变量初始化准备完成初始化完毕。**

#### 7）关于栈的相关面试题目

* 举例栈溢出的情况？
  * StackOverflowError—通过-Xss设置栈大小OOM
* 调整栈大小，能保证不会溢出吗？ 
  * 不能保证，能让StackOverflowError的时间出现的晚一些
* 分配的栈内存越大越好吗？
  * 理论上大能避免一定时间的StackOverflowError，但挤压了其他内存空间，不好。
* 垃圾回收会涉及到虚拟机栈吗？
  * 不会，栈都存在Error但不存在GC
* 方法中定义的局部变量是否线程安全？
  * 具体问题具体分析。对象逃逸可能不安全

## 2.5 本地方法区（接口、库）

一个Native Method 就是一个Java调用非Java代码的接口。通过使用本地方法，得以用Java实现jre的与底层系统的交互。融合不同的编程语言为Java所使用—C/C++。

* **现状：**现在该方法使用的越来越少了，除非是和硬件有关的引用。

```java
//使用native关键字修饰，不能abstruct共用。
//和abstruct的区别是：native有方法体，在本地，abstruct没有方法体。
public native void native1(int x);
public native static long native2();
public native sychronized float native3(Object o);
native void native4(int[] ary) throws Exception;
```

## 2.6 本地方法栈

![image-20210818114458704](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818114458.png)

Java虚拟机栈用于管理Java方法的调用，而本地方法栈用于管理本地方法的调用(native)

* 本地方法栈是线程私有的，和本地方法接口、本地方法库打交道。
* 运行被实现成固定或者动态大小
  * 固定容量超出时抛出StackOverflowError
  * 动态扩展无法申请空间时抛出OutOfMemoryError
* **当线程调用某本地方法时，就不再受虚拟机控制，它和虚拟机拥有同样权限。**
  * 本地方法可以**通过本地方法接口访问虚拟机内部运行时数据区**
  * 可以直接使用本地处理器中的寄存器
  * 直接从本地内存的堆中分配任意数量的内存
* 不是所有的JVM都支持本地方法。
* **HotSpot将本地方法栈和虚拟机栈合二为一**:使用动态衔接指向本地方法给执行引擎。

## 2.7 堆

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818114144.png" alt="image-20210818114144902" style="zoom:50%;" />

一个JVM实例只存在一个堆内存。堆空间大小创建时确定，是管理的最大一块内存空间(可以调节)。**几乎所有对象实例都在堆分配（有些在栈）**[堆空间参数查看](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html)

* **堆物理上不连续，理论上连续**，所有线程共享Java堆，可以划分**线程私有TLAB**
* 堆是GC执行垃圾回收的重点区域，堆内数据使用完后直到堆空间不足时才开始回收

* **内存细分**
  * JDK7逻辑上分：新生区（年轻代、新生代）+养老区（老年区、老年代）+**永久区**
  * JDK8逻辑上分：新生区+养老区+**元空间**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818134510.png" alt="image-20210818134510907" style="zoom:50%;" />

### 2.7.2堆内存和OOM

* 堆内存大小设置——默认初始值物理电脑大小/64，最大内存大小物理内存/4

  * **-Xms** 堆空间（年轻代+老年代）起始值，等价与-XX:InitialHeapSize。

  * **-Xmx** 堆最大内存，等价于-XX:MaxHeapSoze。超出抛OOM。

    开发中建议初始值和最大值设成相同值，避免GC频繁调整堆内存大小

  * 查看设置参数

    *  **jps、jstat -gc 进程id** ——控制台指令（from和to区轮流使用，只使用一个）
    * **-XX:PrintGCDetails**——JVM指令

* OOM（OurOfMemoryError）

### 2.7.3 老年代和年轻代（新生代）

**为什么要把Java堆分代？不分代可以吗？**

可以，分代的唯一理由就是优化GC性能。没有分代的话每次GC就需要对所有区域扫描

#### 1）JVM中的Java对象分类和堆结构中占比

* 生命周期短的瞬时对象——年轻代（YoungGen）又分Eden、from、to三个区
* 生命周期长的对象——老年代（OldGen）

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818165945.png" alt="image-20210818165945032" style="zoom:50%;" />

* 配置年轻代和老年代的堆结构占比
  * **默认年轻代占1，老年占2。一般不修改**
  * 可以修改**-XX:NewRatio=4**。表示年轻代1，老年4。年轻代占1/5
  * **默认年轻代中Eden和另外两个Survivor所占比例为8:1:1，但实际6:1:1**
  * 修改**-XX:SurvivorRadio** = 8 修改Eden和Survivor的空间比例
  * **-XX:（-/+）UseAdaptiveSizePolicy**——设置自适应的内存分配策略，**实战不开**
  * **-Xmn**：设置新生代的空间大小（一般不设置）

#### 2）对象分配过程和原则

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818200722.png" alt="image-20210818200722161" style="zoom:50%;" />

* 设置参数**- XX:MaxTenuringThreshold=\<N>**修改年龄阈值，默认15。

* **关于垃圾回收**：基本都在年轻代。很少在老年代收集。几乎不在永久区/元空间收集。

* **对象分配的特殊情况**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818202434.png" alt="image-20210818202434045" style="zoom:40%;" />

* **内存分配策略**

  * 优先分配到Eden
  * 大对象和长期存活对象分配到老年区
  * 动态对象年龄判断—如果Survivor中相同年龄的对象大小总和大于Survivor空间的一半，则大于或等于该年龄的对象至今进去老年区，无须等到阈值。
  * 空间分配担保 **-XX:HandlePromotionFailture**

* **TLAB（Thread Local Allocation Buffer）**

  由于并发环境下从堆中划分内存空间是线程不安全的。所以在每个Eden区分配一个**TLAB私有缓冲区**，避免费线程安全问题，提升吞吐量—**快速分配策略**

  * 通过**-XX:UseTLAB**设置是否开启TLAB空间。默认开启。
  * 通过**-XX:TLABWasteTargetPercent**设置空间所占百分比。默认占Eden空间的1%
  * 对象在TLAB空间分配内存失败时，JVM就尝试**使用加锁机制**确保数据操作原子性

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210819142345.png" alt="image-20210819142345368" style="zoom:40%;" />

#### 3）Minor GC、Major GC、Full GC

调优是希望尽量减少GC处理的情况，主要是Major GC 和Full GC，大部分回收为新生代。针对HotSpot VM的实现，GC分为两大类：部分收集（Partial GC）、整堆收集

* **部分收集（Partial GC）：**不是完整收集整个Java堆的垃圾收集

  * **新生代收集（Minor GC/Young GC）**：只是新生代的垃圾收集
    * 年轻代空间不足时触发，使用非常频繁
    * 会引发STW，暂停其他用户线程，等待垃圾回收结束线程恢复运行
  * **老年代收集（Major GC/Old GC）**：只是老年代的垃圾收集
    * 目前只有CMS GC会有单独收集老年代的行为
    * 老年代空间不足时触发Major GC，经常会伴随至少一次的Minor GC（非绝对）
    * Major GC比Minor GC慢10倍以上-STW时间更长。Major GC后空间不足则OOM

  * **混合收集**（Mixed GC）：收集整个新生代和部分老年代的垃圾收集

* **整堆收集（Full GC）**：收集整个java堆和方法区的垃圾收集，**常常会混淆老年代收集和整堆收集**，调优开发时需要尽量避免Full GC。（中篇细讲）执行情况：

  * 调用System.gc()时，系统建议执行Full GC，但不必然执行
  * 老年代空间不足
  * 方法去空间不足
  * 通过MInor GC后进去老年代的平均大小大雨老年代的可用内存
  * Eden、from 复制到to时，对象大小大于可用内存。且转移到老年区的内存仍小

### 2.7.4 堆是分配对象存储的唯一选择吗—逃逸分析

**不是**—JIT发展和逃逸分析技术的成熟，可栈上分配、标量替换优化技术。但是现在由于永久区被元数据区取代，intern和static直接堆上分配——**对象实例都是分配在堆上的**

* 经过**逃逸分析**发现一个对象并没有逃逸出方法的话可能会被优化成栈上分配—无需GC

  * **未逃逸：**new的属性被定义后只能在方法内部使用，方法结束栈空间就被移除
  * **逃逸：外部方法可以获取该属性的引用**。引用为static仍然会逃逸
  * JDK7后默认开启逃逸分析，早期版本可以通过**-XX:+DoEscapeAnalysis开启逃逸分析**，通过**-XX:+PrintEscapeAnalysis查看逃逸分析筛选结果**
  * **开发中能使用局部变量的就不要使用在方法外定义**

* **逃逸分析的三个代码优化策略**

  * **栈上分配**：对象指针永远不会逃逸，等于开启标量替换+开启逃逸分析
  * **同步省略**：对象只能从一个线程被访问，那么对这个对象的操作可以不同步
  * **分离对象或标量替换**：对象部分或全部可以不储存在内存而是储存在CPU寄存器
    * **标量**：无法再分解。通过**-XX:EliminateAllocations开启标量替换**，默认开
    * **聚合量**：可以分解为其他聚合量和标量

  服务器端才会有逃逸分析，客户端需要使用参数 -server开启Server模式。

* TaoBaoVm创新的GCIH实现off-heap，将大龄Java对象从heap中移到heap外，且GC不能管理GCIH内部的Java对象，以此达到降低GC的回收频率，提高回收效率。

## 2.8 方法区

### 2.8.1 方法区概念

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210818114604.png" alt="image-20210818114604674" style="zoom:50%;" />



<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210819163535.png" alt="image-20210819163535111" style="zoom:50%;" />

方法区(Non-heap)在**逻辑上属于堆的一部分**——大部分特性和堆一致，但是具体实现上可能不会进行GC或压缩。常看作独立于Java堆的内存空间。

* 部署的工程过多或者加载大量jar包、大量动态生成反射类，会导致OOM
* jdk7以前习惯把方法区称为永久代，**8开始使用元空间取代永久代**。本质上方法区永久区不等价（针对HotSpot），以前的永久区容易导致Java程序OOM
* **元空间和永久代的本质区别：元空间不再虚拟机设置的内存中，使用本地内存**

### 2.8.2 设置方法区大小和OOM

* 方法区空间可固定可动态
* **设置指令**
  * jdk7及以前，通过**-XX:PermSize设置永久代空间默认20.75M**，通过**-XX:MaxPermSize 设置永久代最大可分配空间**。超出抛OOM。
  * jdk8后，换成**-XX:MetaspaceSize和-XX:MaxMetaspaceSize指定**，默认值依赖对应的操作系统，如果触及到元空间最大Size，触发Full GC 卸载没有的类—类的对应类加载器不再存活。所以建议-XX:MaxMetaspaceSize设置相对高的值
* **解决OOM或heap space异常**：通过内存映像分析工具分析堆—确认内存中的对象是否必要(内存泄漏or内存溢出)
  * **内存泄漏：** 泄漏对象到GC Roots 的引用链，定位泄漏代码位置
  * **内存溢出**：调整堆参数，检查代码是都存在某些生命周期过长的对象

### 2.8.3 方法区内部结构

1. **类信息**：类型信息、域信息、方法信息

   * **类型信息**

     类型和其直接父类的全类名、修饰符、直接接口的有序列表

   * **域信息**（Field）：保存类型的所有域的相关信息以及域的声明顺序

     相关信息包括域名称,域类型,域修饰符

   * **方法信息**：所有方法的信息以及声明顺序

     方法信息包括名称,参数,返回类型,修饰符,字节码,操作数栈,局部变量表大小,异常表

   * **non-final的类变量** 静态变量和类关联在一起，理论上类数据的一部分

2. **运行时常量池字符串常量**：常量、静态变量、即使编译器编译后的代码缓存

   * **运行时常量池在方法区，常量池表在Class文件—加载后存放到运行时常量池**
   * 运行时常量池具有动态性

   常量池表：虚拟机指令根据这张表找到执行的类名方法名参数类型和字面量类型

### 2.8.4 方法区演进细节

**Hotspot中方法区变化**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820131606.png" alt="image-20210820131606703" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820133617.png" alt="image-20210820133617719" style="zoom:50%;" />

* **永久代为什么要被元空间替换？**

  动机是为融合JRockit和HotSpot，原因：永久代设计空间大小很难确定、调优困难

* **为什么StringTable字符串常量池要调整？**

  因为永久代GC效率低—Full GC每次到老年代空间不足时触发，导致StringTable回收效率不高，再导致永久代内存不足，放在堆里可以及时回收内存

* **静态变量放在哪里？**

  静态引用对应的对象实体始终在堆空间，静态引用存放在方法区

### 2.8.5 方法区的垃圾收集

JVM规范规定方法区可以不实现垃圾回收（所以常被认为是永久区）

* 常量池的回收策略：只要常量池中的常量没有被任何地方引用，就可以回收。判断常量是否废弃比较简单，**判断类是否废弃很苛刻**：
  * 类示例是否都已被回收—堆中没有任何派生子类的实例
  * 加载该类的类加载器已经被回收
  * 类对应的java.lang.Class对象没有在任何地方引用也无法在任何地方通过反射引用

## 2.9 对象的实例化内存布局与访问定位

### 2.9.1 对象的实例化

![image-20210820150840772](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820150840.png)

### 2.9.2 对象的内存布局

在HotSpot虚拟机里，对象在堆内存中的存储布局可以划分为三个部分：对象头（Header）、实例数据（Instance Data）和对齐填充(Padding)。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820152710.png" alt="image-20210820152710911" style="zoom:50%;" />

### 2.9.3 对象的访问定位

* **访问的两种方式：句柄访问、直接指针（HotSpot采用）**
* 栈帧通过栈上 本地变量表的reference定位到其内部的对象实例。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820155310.png" alt="image-20210820155310430" style="zoom:50%;" />

## 2.10 直接内存Direct Menory

不是JVM运行时数据区的一部分也不是JVM规范定义的内存区域。直接内存是在Java堆外，直接向系统申请的内存空间。内源于NIO。

* 通常访问直接内存的速度会优于Java堆，读写性能高，所以IO频繁场所考虑直接内存

|      IO       | NIO(New IO/Non-Blocking IO） |
| :-----------: | :--------------------------: |
| byte[]/char[] |            Buffer            |
|    Stream     |           Channel            |

### 2.10.1 直接缓冲区和非直接缓冲区

* **非直接缓冲区**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820160854.png" alt="image-20210820160854760" style="zoom: 50%;" />

* **直接缓冲区**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820161115.png" alt="image-20210820161115225" style="zoom:50%;" />

  * 可能导致OOM： Direct buffer memory
  * 空间大小在堆外，不受-Xmx限制，通过MaxDirectMemorySize 设置空间最大值。不指定默认和Xmx一样
  * 缺点：分配回收成本高，不受JVM内存回收管理

简单理解：Java内存所占空间=堆空间+本地内存

---

## 2.11 执行引擎

将字节码翻译编译解释成机器可识别的机器指令

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820183050.png" alt="image-20210820183050415" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820183350.png" alt="image-20210820183350249" style="zoom:50%;" />

**为什么说Java是半编译半解释语言？**一开始是解释执行，后来发展出可以生成本地代码的编译器。通常将解释执行和编译执行二者结合进行。

### 2.11.1 机器码、指令、汇编语言、字节码

* **机器码**：采用二进制编码方式的指令**机器指令码（机器语言）**，与CPU紧密相关
* **指令**：将机器码特定的0/1序列简化成对应指令—英语简写可读性好
  * **指令集**：不同平台支持不同的指令，平台所支持的指令称指令集(ARM指令集)
* **汇编语言**：指令可读性还是差，又发明汇编。使用助记符、地址符号、标号。
* **高级语言**：更接近人类语言。——解释、编译

无论是指令、汇编还是高级语言，都需要翻译成机器指令码给计算机执行。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210820184546.png" alt="image-20210820184546805" style="zoom:50%;" />

* **字节码**：中间状态（中间码）的二进制代码，比机器码更抽象。需要直译器转译后变成机器码，为了实现软件运行和软件环境，与硬件无关。应用：Java bytecode。

### 2.11.2 解释器

初衷为满足Java跨平台性。是一个**运行时的翻译者**，实现解释器运行时逐行解释字节码为本地机器指令执行程序。一条字节码指令执行完后，根据PC计数器记录的继续执行

* **解释器的分类**
  * 字节码解释器：老、逐行、效率低
  * 模板解释器：每条字节码和一个函数模板相关联，函数产生执行机器码
* **现状**：基于解释器执行已经沦落为低效的代名词。——即时编译技术支持的诞生

### 2.11.3 即时编译器（JIT - Just In Time）

#### 1）编译器概述

**HotSpot采用解释器和即时编译器并存的架构**。当程序启动后，**解释器先执行马上发挥作用**，省去编译时间立即执行。编译器把代码编译称本地代码需要一定执行时间，**编译后效率高**。（比如步行和坐公交，步行慢但是能马上走，公交车快但是需要等）

* **编译器分类**

  * **后端运行编译器（JIT）**：把字节码转换成机器码的过程——如C1、C2
  * **前端编译器**：.java文件转.class文件的过程——如Sun的javac、ECJ
  * **静态提前编译器（AOT）**：直接把.java文件编译称本地机器码的过程——GCT

* **热点代码**：被多次调用的方法或循环体

* **热点探测功能：**判断方法是否为热点方法——HotSpot采用基于计数器的热点探测

  * **方法调用计数器**：统计方法次数。默认Client 1500次/Server 10000次触发JIT编译

    * 通过**- XX:CompileThreshold**更改设置阈值

    * **热度衰减**：一段时间内方法调用次数不到阈值则计数器衰减一般——**半衰期**

      通过**-XX:UseCounteDecay关闭热度衰减**—运行足够长时间大部分方法被JIT

      通过**-XX:CounterHalfLifeTime设置半衰期时间**

  * **回边计数器**：统计循环体次数，两个计数器之和超过阈值触发OSR编译

#### 2）HotSpot设置程序执行方式

根据具体场景，通过命令显示指定JVM基于**完全采用解释器执行**还是**完全采用JIT执行**

* **-Xint**：完全采用解释器执行
* **-Xcomp**：完全采用JIT执行
* **-Xmixed**：混合模式执行

#### 3）C1和C2编译器

HotSpot含两个JIT——Client Compiler(C1)、Server Compiler(C2)。C2比C1快，启动慢

* **Client Compiler（C1）**
  * **-client**：指定Client模式，使用C1编译器——简单可靠耗时短
  * 方法内联、去虚拟化（唯一实现类内联）、冗余消除（不执行的代码折叠）
* **Server Compiler（C2）** 默认模式
  * **-server**：指定Server模式，使用C2编译器——耗时长的优化和激进优化，效率高
  * 标量替换、栈上分配、同步消除（synchronized）

#### 4）Graal编译器

JDK10起，HotSpot加入一个全新的即时编译器Graal，超过C2，未来可期。目前处于实验状态，使用**-XX:+UnlockExperimentalVmOptions  -XX:+UseJVMCICompiler**开启

#### 5）AOT编译器

JDK9 引入AOT编译器（静态体检编译器 Ahead of Time Complier）。编译工具jaotc，借助了Graal编译器将输入的Java类文件转换为机器码，并存放生成的动态共享库中。

* AOT是一个与即使编译相对立的概念。**AOT运行前，JIT运行时编译。**减少预热时间
* .java—>.class—>（AOT）—>.so  
* **缺点**
  * 打破了一次编译到处运行。要为不同硬件、os编译对应的发行包
  * **降低Java链接过程的动态行**。加载代码在编译器就必须全部已知
  * 需要继续优化，最初只支持Linux x64 java base
* Substrate VM 提前编译（Graal VM 0.20 里的极小型运行环境）

## 2.12 String Table

### 2.12.1 String Table概述

String可不变字符串(final不可继承)，实现了Serializable和Comparable。常量池是Java系统级别提供的缓存，8中基本类型的常量池都是系统协调的。

* **JDK8底层基于char[]，JDK9及以后基于byte[]—为了优化String的堆存储**
* **字符串常量池中不会存储相同内容的字符串**—底层是固定大小的**Hashtable**
  * 使用**-XX:StringTableSize**设置table长度，jdk8时1009是可设置的最小值

* **String常量池的使用方法**：双引号声明、String.intern() 方法
* **String常量池发展**：Jdk6永久代。JDK7堆。JDK8元空间—字符串常量池在堆

```java
//常见的字符串拼接操作
String s1 = "a"+"b"+"c";//字面量拼接放在常量池，原理是编译器优化
String s2 = s1 +"def";//拼接时有变量，则在堆中new String.
s2.intern();//判断常量池中是否存在s2的值，是返回地址，否添加到常量池并返回地址
//===================================================================
String s1 = "a",s2 = "b", s3 = s1 + s2;
/*如下是s1+s2的执行细节
1. StringBuiler s = new StringBuilder();
2. s.append("a");  s.append("b");
3. s3 = s.toString();————>约等于new String("ab") */
//字符串拼接不一定是StringBuilder。如果拼接符号两边是字符串常量或常量引用，使用编译器优化。如 final String s = “abc”; 则拼接用是常量引用

```

* **通过StringBuilder的append远比字符串拼接的方式效率快的多**，开发中建议使用StringBuilder(int capacity) 规定空间长度，避免频繁扩容。

### 2.12.1 Intern方法（面试题）

是个native方法。intern方法调用的时候，看常量池是否存在字符串`equal`当前字符串，存在返回地址引用，不存在添加后返回引用。

**结论：**对于程序中大量存在的字符串（尤其是重复的）使用intern后节省空间

* jdk6-intern：复制一份字符串对象在常量池中，有新串池的地址
* Jdk7-intern：复制对象的引用地址放入串池中

```java
//调用String生成对象的数量
new String("ab");//创建两个对象:1.new关键字在堆创建的.2.字符串常量池中"ab"
String str = new String("a") + new String("b"); //6个对象
//1.堆中new String("a") 2.new String("b")
//3.常量池中“a” 4.常量池中“b”
//5.堆中StringBilder  6.堆中StringBuilder的toString生成对象（⚠️常量池中没有	
```

### 2.12.2 String Table 垃圾回收

参数**- XX:+PrintStringTableStatistics** 打印串池相关信息

* **G1的String去重操作**：堆中String 占25%，重复有13.5%，平均长度45—内存浪费

  * **开启去重-XX:UseStringDeduplication** 默认关闭，**PrintStringDeduplicaitonStatistics打印去重统计信息**，**StringDeduplicationAgeThreshold 达到阈值的String被认为是去重候选对象**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210821163003.png" alt="image-20210821163003293" style="zoom:40%;" />

---

## 2.13 垃圾回收GC（面试点）

### 2.13.1 垃圾回收概述

极大提升开发效率，不断演进每个版本都会更新

1. **垃圾**：运行程序中没有任何指针指向对象，该对象为垃圾。 

2. **为什么需要GC？**：保证程序正常进行，避免悬垂指针，野指针，泄漏问题

3. **什么是内存泄漏？**：垃圾无法回收

4. **早期垃圾回收**：手动内存申请，容易内存泄漏，内存溢出到程序崩溃

5. **System.gc**：底层是Runtime.getRuntime.gc()显示触发Full GC。无法保证GC调用。

6. **OOM**：**没有空闲内存且GC无法提供内存**，在抛出OOM之前，GC通常会被触发清理空间(软引用、虚引用)。特殊情况对象超大，直接超过堆空间，不会触发GC。

7. **STW（Stop The World）**：GC时整个程序暂停没有响应，GC都有不能完全避免

8. **finalization机制**：允许开发人员提供**对象被销毁前的自定义处理逻辑**，GC前调用

   * **不要主动调用对象的finalize()，让给GC调用**，原因：
     * 可能复活—**可能在方法内和引用链上的对象建立关系。逃脱死亡的最后机会**
     * 执行时间由GC线程决定没有保障，极端情况下不发生GC就没有执行机会
     * 一个糟糕的finalize()就严重影响GC的性能
   * **finalize使对象处于三种可能状态**：可触及(可达)、可复活、不可触及(可回收)
   * **System.runFinalization()** 强制调用只用引用对象的finalize()方法

9. **并发：**同个处理器上执行。**同一时间段交替执行**。抢占资源。

10. **并行：**多个处理器上执行。**同一时刻同时执行**。不抢占资源。

11. **垃圾回收里的并发与并行**

    * 并行：同个时间点多个GC线程并行工作，STW。
    * 串行：GC一个个依次执行单线程执行，存在STW
    * 并发：**同时间段用户线程与GC线程同时进行**—不一定是并行。GC时不会有STW

12. **安全点Safe Point**：在特定位置STW开始GC。**太少GC等待时间太长。太多影响运行时性能**。执行时间长的指令可以作为安全点，如**方法调用、循环跳转、异常跳转**

    * **抢占式中断（目前不采用了）**：中断所有线程，如果有不在安全点的就恢复线程
    * **主动式中断**：设置中断标志，线程运行到true的中断标志时将自己挂起

13. **安全区域Safe Region**：代码片段中对象引用关系不会变化，该区域GC安全

14. **引用（高频冷门面试题）**：四种引用强度依次减弱。**默认引用类型为强引用**

    - **强引用StrongReference**：不回收，99%使用。内存泄漏的主要原因之一
    - **软引用SoftReference**：内存不足就回收。实现内存敏感的缓存如**高速缓存**，GC回收软可达对象时，就清理软引用，并可选地把引用存放在引用列表。
    - **弱引用WeakReference**：发现即回收
    - **虚引用PhantReference**：(幽灵引用/幻影引用 )对象回收跟踪通知。无法get()

    ```java
    Object obj = new Object();//强
    SoftReference<Object> sr = new SoftReference<>(new Object());//软
    WeakReference<Object> wr = new WeakReference<>(new Object());//弱
    PhantReference<Object> pr = new PhantReference<>(new Object(),new ReferenceQueue);//虚
    ```

    * **终结器引用FinalReference**：用来实现对象的finalize方法

### 2.13.2 垃圾回收相关算法

从如何判定对象消亡的角度出发，垃圾收集算法可以划分为**“引用计数式垃圾收集”（ReferenceCountingGC）和“追踪式垃圾收集”（Tracing GC）**两大类，这两类也常被称作**“直接垃圾收集”和“间接垃圾收集”**。主流的都是追踪式收集。

* **标记阶段**：对象存活判断-死亡(不被存活对象引用)。**引用计数算法、可达性分析算法**

* **垃圾清除阶段**：**标记-清除算法、标记-整理算法、复制算法**

  ![image-20210822130320251](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210822130320.png)

* **分代算法、分区算法、增量收集算法**

⚠️这些知识基本的算法思路，实际GC实现的过程都是复合算法，并行和并发兼备

#### 1）引用计数算法—标记阶段

对每个对象保存一个整型的引用计数器属性，用于记录对象被引用的情况。对于对象A，有任何对象引用A时，A的引用计数器+1，引用失效时-1，计数器为0时可回收。

* **优点**：简单、便于辨识，判定效率高，回收没有延迟性——随时可回收。

* **缺点：**增加空间时间开销。**无法处理循环引用（致命缺陷）**——导致没有使用此算法

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210821184523.png" alt="image-20210821184523728" style="zoom:50%;" />

* Java不使用引用计数算法，Python等语言使用，Python如何解除循环引用？

  * 手动解除：在合适时机解除引用关系
  * 使用弱引用weakref，Python提供的标准库，为了解决循环引用—是弱引用就回收

#### 2）可达性分析算法—标记阶段

也称为**根搜索算法、跟踪性垃圾收集**。相对于引用计数算法**有效地解决在引用计数算法中循环引用的问题。**

思路：以根对象（GC Roots）为起始点，从上至下搜**索被根对象集合所链接的对象是否可达**。被根对象直接或间接地连接的路径为**引用链**。若目标不可达则标记为垃圾对象。

* **垃圾收集标记方法的实现**
  * serial的对象头存储对象引用
  * g1 shenandoah单独开辟空间存储
  * zgc对象引用指针中存储

* **GC Roots包括的几类元素（面试高级问题）**——☑️必须能答上来

  **小技巧：一个指针保存了堆的对象，自己又不在堆内，则为一个Root**

  - ☑️虚拟机栈引用对象——如线程调用方法中的参数、局部变量等
  - ☑️本地方法栈内（JNI）引用对象
  - ☑️方法区静态属性引用对象——如Java类的引用类型静态变量
  - ☑️方法去常量引用对象——如字符串常量池里的引用
  - 所有被同步锁synchronized持有的对象
  - Java虚拟机内部的引用——基本类型对应的Class对象,常驻异常对象,系统类加载器
  - 反映Java虚拟机内部情况额JMXBean、JVMTi中注册的回调、本地代码缓存等
  - **对象的“临时性”加入比如分代收集、局部回收（Partial GC）等**。

* **使用MAT与JProfiler查看程序中的GC Roots——溯源**

* **缺点**：分析工作必须保障一致性。导致GC进行时必须停止用户线程——停顿

* **三色标记（Tri-color Marking)** 

  * 白色：表示对象尚未被垃圾收集器访问过。
  * 黑色：表示对象已经被垃圾收集器访问过，且此·对象的所有引用都已经扫描过。
  * 灰色：表示对象已经被垃圾收集器访问过，但至少存在一个引用还没有被扫描。

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902133704.png" alt="image-20210902133704897" style="zoom:50%;" />

#### 3）标记-清除算法（Mark-Sweep）—垃圾清除阶段

当堆中有效内存空间被耗尽的时候会停止项目，开始标记再而清除——**空闲列表分配**

* **标记：**标记可达对象——非垃圾对象，记录在对象的Header中
* **清除：**扫描整个堆清除未标记对象—垃圾地址保存在空闲列表，下次覆盖。
* **优点：**简单基础容易理解
* **缺点：**
  * 执行效率不稳定，GC时需要暂停用户线程。
  * 内存碎片化。清理的内存不连续，需要维护空闲列表。

#### 4）复制算法（Copying）—垃圾清除阶段

为了解决标记-清除算法面对大量可回收对象时执行效率低的问题

提出了一种称为**“半区复制”（Semispace Copying）**的垃圾收集算法，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。每次使用其中一块，GC时将正在使用的内存中存活对象复制到未被使用的内存中，清理内存中所有对象，交换两个内存角色——**指针碰撞**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210822123124.png" alt="image-20210822123124007" style="zoom:50%;" />

* **优点：**没有标记清除过程，实现简单运行高效，保证空间连续性—无碎片化问题
* **缺点：**两倍内存空间，复制需要维护region之间的对象引用关系，内存时间开销大。如果垃圾对象很少，存活对象很多。就会产生大量无用的复制操作
* **应用场景：**适合新生代的回收——大部分朝生夕死的

#### 5）标记压缩算法（Mark-Compact）—垃圾清除阶段

为了解决标记-复制算法在老年代不适用的情况。

也称做标记-整理算法，先标记后压缩，移动式。适合老年代了

* **压缩：**将存活对象压缩在内存的一端按顺序排放，清理剩下空间
* **优点：**解决标记清除、复制算法的缺点
* **缺点：**效率低与复制算法，移动对象时需要调整引用地址。需要STW停止用户程序。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210901175017.png" alt="image-20210901175017698" style="zoom:50%;" />

#### 6）分代算法

几乎或有的GC都采用分代算法执行垃圾回收，几乎所有GC都区分新生代老年代。——**不同生命周期的对象采取不同的收集方式，以提高回收效率**。

* **年轻代：**使用**复制算法**，速度快，大部分对象死，通过两个Survivor设计。
* **老年代：**使用**标记-清除、标记-整理算法的混合实现**
  * **Mark**阶段的开销与存活对象数量成正比
  * **Sweep**阶段的开销与所管理的区域大小成正比
  * **Compact**阶段的开销与存活对象的数据成正比

#### 7）增量收集算法

上述现有算法都会有STW（Stop The World）现象，影响用户体验或者系统稳定性。——增量收集算法的诞生。**每次GC只收集一小片内存空间，接着切换到应用程序线程。**反复直到GC完成。**基础是标记清除和复制算法。分阶段对线程间冲突的妥善处理**。

* **缺点：**垃圾回收总成本上升，造成系统吞吐量的下降

#### 8）分区算法

当前主流GC更关注低延迟而不是吞吐量。分区把整个堆空间划分为多个小空间**region**。

每个小区间独立使用独立回收。好处是可以控制一次回收多少个小区间。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210822142026.png" alt="image-20210822142026670" style="zoom:33%;" />

### 2.13.3 分代收集理论

当前商业虚拟机的垃圾收集器，大多数都遵循了“分代收集 ” （Generational Collection）的理论进行设计——也有其缺陷，最新出现（或在实验中）的几款垃圾收集器都展现出了面向全区域收集设计的思想，或可以支持全区域不分代收集的工作模式。

* **两个分代假说**

  * **弱分代假说（Weak Generational Hypothesis）**：绝大多数对象都是朝生夕灭的。
  * **强分代假说（StrongGenerational Hypothesis）**：熬过越多次垃圾收集过程的对象就越难以消亡。

* **分代收集理论的相关垃圾收集算法**

  “标记-复制算法”“标记-清除算法”“标记-整理算法”等

* **分代收集的困难**：对象不是孤立的，对象之间会存在跨代引用。所以遍历一代中存活对象，可能需要额外遍历另外的代对象确保可达性正确性。**需要添加额外经验法则**

  * **跨代引用假说（Intergenerational Reference Hypothesis）**：跨代引用相对于同代引用来说仅占极少数。

    推理得出的隐含推论：存在互相引用关系的两个对象，**是应该倾向于同时生存或者同时消亡的**。如果某个新生代对象存在跨代引用，由于老年代对象难以消亡，该引用会使得新生代对象在收集时同样得以存活，进而在年龄增长之后晋升到老年代中，这时跨代引用也随即被消除了。

    依据此假说。我们只需要在新生代上建立一个全局的数据结构（“记忆集”）

* **“记忆集”Remembered Set**：这个结构把老年代划分成若干小块，标识出老年代的哪一块内存会存在跨代引用。当发生MinorGC时，只有包含了跨代引用的小块内存里的对象才会被加入到GCRoots进行扫描。

  * 字长精度：每个记录精确到一个机器字长（处理机寻址位数，如32、64位）

  * 对象精度：每个记录精确到一个对象，该对象里有字段含有跨代指针。

  * **卡精度：每个记录精确到一块内存区域，该区域内有对象含有跨代指针。**

    其中，第三种“卡精度”所指的是用一种称为**“卡表”（CardTable）**的方式去实现记忆集，这也是目前最常用的一种记忆集实现形式。

    卡页的内存中通常包含不止一个对象，只要卡页内有一个（或更多）对象的字段存在着跨代指针，那就将对应卡表的数组元素的值标识为1，称为这个元素变脏（Dirty），没有则标识为0。

    在垃圾收集发生时，只要筛选出卡表中变脏的元素，就能轻易得出哪些卡页内存块中包含跨代指针，把它们加入GC Roots中一并扫描。

    * **写屏障**维护卡表状态
    * 卡表在高并发场景下还面临着**“伪共享”**
    * 在JDK7之后参数**-XX：+UseCondCardMark**决定是否开启卡表更新的条件判断

### 2.13.4 GC日志分析

* **内存分配与垃圾回收参数列表**
  * -XX:+PrintGC：打开GC日志，类似 -verbose:gc
  * -XX:+PrintGCDetails：输出GC的详细日志——**过时，现在-Xlog:gc***
  * -XX:+PrintGCTimeStamps/-XX:PrintGCDateStamps：输出GC时间戳
  * -XX:+PrintHeapAtGC：GC前后打印堆信息
  * -Xloggc:../logs/gc.log 日志文件输出路径
* **日志补充说明**
  * 日志老年代和新生代的名字由垃圾收集器决定
  * Allocation Failure：新生代空间不足引起的GC
  * 新生代GC![image-20210824174414157](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824174414.png)
  * 老年代GC![image-20210824174507460](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824174507.png)
* **常见日志分析工具：** *GCViewer、GCEasy*、GCHisto、GClogVirewer、Hpjmeter、garbagecat

---

## 2.14 垃圾回收器

* **查看默认收集器**
  * **-XX:+PrintCommandLineFlags**查看命令行相关参数（包活使用的默认GC）
  * **jinfo -flag  相关垃圾回收器参数  进程ID**

### 2.14.1 评估GC的性能指标

吞吐量、暂停时间、内存占用、收集频率、快速。吞吐量和暂停时间相互对立。当下尽量在吞吐量优先的情况下，降低停顿时间。

**-XX:MaxGCPauseMillis最大STW时间(慎用),-XX:GCTimeRatio吞吐量占比**

* **衡量垃圾收集器的三项最重要的指标**：款优秀收集器通常最多可同时达成其中两项。
  * ***吞吐量（大好）***：运行用户代码的时间占总运行的时间—补数为**垃圾收集开销**
    * 吞吐量 = 运行用户代码时间 / （运行用户代码时间 + 垃圾收集时间）
  * ***暂停时间***：执行垃圾收集时程序的工作线程被暂停的时间。
    * 交互式应用程序更关注低延迟（低暂停时间）
  * **内存占用**：Java堆所占内存大小。

### 2.14.2 垃圾收集器历史变化

虚拟机需要收集垃圾的机制`Garbage Collection` ,对应产品 `Garbage Collector`，场景不同使用最合适的收集器。——只有最合适的 

1. 1999，JDK 1.3.1 串行方式 `Serial GC` 。`ParNew GC` 是多线程版本
2. 2002.2.26，JDK1.4.2`Parallel GC`, `Concurrent Mark Sweep GC` （`CMS`）。`Parallel GC` 在 JDK6 之后成为 Hotspot 默认GC。
3. 2012，在 JDK1.7u4 中，`G1` 可用。
4. 2017，JDK9 中 `G1` 成为默认垃圾收集器，以替代 `CMS`。
5. 2018。3，JDK10 中 `G1` 的并行完整垃圾回收，实现并行性能改善最坏情况的延迟。
6. 2018.9，JDK11 引入 `Epsilon GC` ，又称为“No-Op（无操作）” 回收器；同时引入 `ZGC`： 可伸缩的低延迟回收器（Experimental）。
7. 2019.3，JDK12 增强 `G1`，自动返回未使用堆内存给操作系统。 同时，引入`Shenandoah GC`：低停顿时间的GC（Experimental）。
8. 2019.9，JDK13 增强 `ZGC`，自动返回未使用堆内存给操作系统。
9. 2020.3，JDK14 删除 `CMS`。扩展 `ZGC` 在 mac 和 windows 的应用。
10. 2020.8，JDK15`ZGC`脱离实验阶段—转正
11. 2021.3，JDK16 全并发`ZGC`默认GC仍是`G1`

### 2.14.3 七种经典的垃圾收集器

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210823211007.png" alt="image-20210823211007411" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824140654.png" alt="image-20210824140654512" style="zoom:50%;" />

![image-20210902163508192](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902163508.png)

#### 1）串行回收器Serial/Serial Old 

简单高效、额外内存消耗最小，单核cpu适使用。**-XX:+UseSerialGC**

* Serial：复制算法、串行回收、STW。
* Serial Old ：标记-压缩算法、串行回收、STW。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902134020.png" alt="image-20210902134020512" style="zoom:60%;" />

#### 2）并行回收器ParNew/Parallel/Parallel Old

多核下 新生代并行更高效。**-XX:+ParallelGCThreads设置线程个数**

* **ParNew**：Serial的多线程版，包括Serial可用所有参数。**-XX:+UseParNewGC**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902134408.png" alt="image-20210902134408747" style="zoom:60%;" />

​	JDK9以后取消了-XX:+UseParNewGC参数，意味着ParNew和CMS从此只能互相搭配使用，再也没有其他收集器能够和它们配合——可以理解为ParNew合并入CMS

* **Parallel Scavenge**：和ParNew差不多但底层框架不同。**自适应调节策略。JDK8中默认收集器**。**-XX:UseParallelGC/UseParallelOldGC**

  * **-XX:+UseAdaptiveSizePolicy**自动调节年轻代大小Eden Survivor比例等

  * **-XX:MaxGCPauseMillis**控制最大垃圾收集停顿时间
  * **-XX:GCTimeRatio**设置吞吐量大小

* **Parallel Old**：代替Serial Old，标记-压缩算法

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902135749.png" alt="image-20210902135749029" style="zoom:60%;" />

#### 3）并发回收器 CMS

**CMS(Concurrent-Mark-Sweep)：标记-清除算法**(保证用户线程执行不能选择标记压缩算法)底层框架无法和parallel配合工作。响应飞快。小内存时优势大

* 运作的四个步骤
  * 初始标记（CMSinitial mark）
  * 并发标记（CMS concurrent mark）
  * 重新标记（CMSremark）
  * 并发清除（CMS concurrent sweep）

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210823210347.png" alt="image-20210823210347523" style="zoom:50%;" />

* **缺点：**会产生内存碎片—提前触发Full GC，对CPU资源敏感，无法处理浮动垃圾—并发标记阶段的新垃圾。
* **优点：**并发低停顿
* **参数**：**-XX:+UseConMarkSweepGC **设置GC并开启ParNew。 **-XX:CMSInitiatingOccupanyFraction 设置堆内存使用阈值**内存缓增时增大阈值值降低CMS触发，增涨快时降低阈值避免触发老年代串行收集器。**-XX:+UseCMSCompactAtFullConllection **指定FullGC后的内存压缩整理。**-XX:CMSFullGCsBeforeCompact** 指定多少次FullGC后堆内存压缩整理。**-XX:ParallelCMSThreads **设置CMS线程数量

#### 4）并发回收器 G1

**G1(Garbage First)**：**分区算法**。适用服务端和大内存多处理器机器、需要低GC延迟。适合超过50%的堆被活动数据占用，对象分配频率大，GC停顿时间长的场景。

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902143429.png" alt="image-20210902143429834" style="zoom:50%;" />

* **优势：**兼具并行与并发。分区分代收集。空间碎片整合—Region间是复制，整体是压缩。可预测停顿时间模型—允许收集时间内优先回收价值最大区

* **参数：-XX:+UseG1GC **指定GC。**-XXG1HeapReginSize **设置每个Region大小。**-XXMaxGCPauseMillis **最大停顿时间。**-XX:ParallelGCThread** STW工作线程数。**-XXConGCThreads** 并发标记线程数。**-XX:InitiatingHeapOccupancyPercent** 触发GC周期的Java堆占用阈值 

* **分区Region：化整为零**

  * 将堆划分2048个相等Region—每块1～32MB。
  * **一个Region有可能属于Eden、Survivor、Old/Tenured**。如果对象超过1.5个Region，**放到H区(Humongouds)**，Region线程可以分配TLAB

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210823215914.png" alt="image-20210823215914877" style="zoom:50%;" />

* **Remenbered Set**：为了解决不同Region间的引用问题——造成MInor GC效率低。每个Region对应一个Remenbered Set 避免全局扫描。

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210823222510.png" alt="image-20210823222510207" style="zoom:50%;" />

* **G1回收过程**
  * 年轻代GC<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824121301.png" alt="image-20210824121301337" style="zoom:50%;" />
  * 并发标记<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824121554.png" alt="image-20210824121554491" style="zoom:40%;" />
  * 混合回收Mixed GC **回收整个Young Region和部分Old Region**<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824135443.png" alt="image-20210824135443524" style="zoom:50%;" />
  * (可选阶段)Full GC：堆内存太小没有空闲内存提供复制存活对象
  
* **下面的情况时，使用G1可能比CMS好：**

  * 超过50%的Java堆被活动数据占用
  * 对象分配频率或年代提升频率变化很大
  * GC停顿时间过长（长于0.5至1秒）。

### 2.14.3 Shenandoahl

Shenandoah只有OpenJDK包含，而OracleJDK里反而不存在的收集器——官方排挤。和G1有相似的堆内存布局，在初始标记、并发标记等许多阶段的处理思路上都高度一致。**G1就是由于合并了Shenandoah的代码才获得多线程FullGC的支持**。

* **对比G1收集器的不同之处**

  * Shenandoah实现了并发的整理算法

  * 默认不采用分代收集

  * 使用“**连接矩阵**”数据结构记录跨Region的引用关系——摒弃了消耗大的记忆集

    <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902152720.png" alt="image-20210902152720265" style="zoom:50%;" />

* **九个运作阶段**

  ![image-20210902155027570](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902155027.png)

  * **初始标记（Initial Marking）**：同G1
  * **‼️并发标记（Concurrent Marking）**：同G1
  * **最终标记（FinalMarking）**：同G1
  * **‼️并发清理（Concurrent Cleanup）**：清理连一个存活对象都没的Region
  * **‼️并发回收（Concurrent Evacuation）**：把回收集里面的存活对象先复制一份到其他未被使用的Region之中。**通过读屏障和Brooks Pointers转发指针解决复制对象和用户线程的同时并发进行**。
  * **初始引用更新（Initial Update Reference）**：把堆中所有指向旧对象的引用修正到复制后的新地址。短暂STW。
  * **并发引用更新（ConcurrentUpdate Reference）**：真正与用户线程并发进行引用更新操作，时间长短取决于内存中涉及的引用数量的多少。不再需要沿着对象图来搜索，按照内存物理地址的顺序，线性搜索把旧值改为新值即可。
  * **最终引用更新（Final Update Reference）**：解决了堆中的引用更新后，还要修正存在于GCRoots中的引用。最后一次STW。
  * **并发清理（ConcurrentCleanup)**

### 2.14.4 ZGC

ZGC是基于Region内存布局的，暂时不分代，使用读屏障、染色指针、内存多重映射等技术实现**可并发的标记-压缩算法**，以低延迟为首要目标。**《ZGC设计与实现》**

* **四个工作阶段：**并发标记、并发预备重分配、并发重分配、并发重映射
* **-XX:UnlockExperimentalVMOptions,-XX:UseZGC**
* **标志性设计：**染色指针技术（Colored Pointer/Tag Poniter/Version Pointer）
* **ZGC内存分布**
  * 小型Region（Small Region）：容量固定为2MB，放置（0，256KB）对象。
  * 中型Region（MediumRegion）：容量固定为32MB，放置【256KB,4MB）对象。
  * 大型Region（Large Region）：容量可动态变化—2MB的整数倍，放置4MB或以上的大对象。每个大型Region中只会存放一个大对象——实际容量完全有可能小于中型。大型Region在ZGC的实现中是不会被重分配。

# 第三章 字节码与类的加载

## 3.1 Class 文件

一个Class文件都对应唯一一个类或接口的定义信息。是一组以8字节为单位的二进制流

* **字节码文件：**源代码通过前端编译器编译后生成的二进制类文件，内容是JVM指令
* **字节码指令：**特殊含义的操作码opcode以及0个或多个操作数operand构成
  * 查看方式一：使用二进制查看工具一个个查看（Notepad++、Binary Virewe……）
  * 查看方式二：使用javap <options> <classes>，jdk自带反编译工具
  * 查看方式三：使用JClasslib第三方工具
* **Class文件格式**：无符号数+表
  * 无符号数：描述数字、索引引用。u1、u2、u4、u8表示对应字节个数。
  * 表：多个无符号数、其他表构成的复合数据符号（就像数组）。无固定长度。

### 3.1.1 字节码文件结构

```
ClassFile { 一个类文件由一个ClassFile结构组成
    u4             magic; 																魔数
    u2             minor_version;													副版本号
    u2             major_version;													主版本号
    u2             constant_pool_count;										常量池计数器
    cp_info        constant_pool[constant_pool_count-1];	常量池表
    u2             access_flags;													访问标识
    u2             this_class;														类索引
    u2             super_class;														父类索引
    u2             interfaces_count;											接口计数器
    u2             interfaces[interfaces_count];					接口索引集合
    u2             fields_count;													字段计数器
    field_info     fields[fields_count];									字段表
    u2             methods_count;													方法计数器
    method_info    methods[methods_count];								方法表
    u2             attributes_count;											属性计数器
    attribute_info attributes[attributes_count];					属性表
}
```

#### 1）magic 魔数

Class开头4个字节表示 ca fe ba be（0xCAFEBABE） 。用于识别Class文件格式，若Class文件不以该固定魔数开头JVM就会报错。

#### 2）version 版本号

minor_version+major_version。不同版本JVM编译Class文件版本不一样。高版本虚拟机可以运行低版本字节码文件，反之不行。

#### 3）constant_pool 常量池（最重要）

```
cp_info {	所有constant_pool表项的通用格式
    u1 tag; 								标签
    u1 info[];							常量信息
}
CONSTANT_Class_info {	用于表示类或接口
    u1 tag;									标签值CONSTANT_Class (7)
    u2 name_index;					名称索引 必须是CONSTANT_Utf8_info结构
}
CONSTANT_Fieldref_info {	字段结构
    u1 tag;									 CONSTANT_Fieldref(9)
    u2 class_index;
    u2 name_and_type_index;
}
CONSTANT_Methodref_info {	方法结构
    u1 tag;										CONSTANT_Methodref(10)
    u2 class_index;
    u2 name_and_type_index;
}
CONSTANT_InterfaceMethodref_info {	接口方法结构
    u1 tag;										CONSTANT_Methodref(11)
    u2 class_index;
    u2 name_and_type_index;
}
CONSTANT_Utf8_info {	用来表示常量字符串值
    u1 tag;										CONSTANT_Utf8 (1)
    u2 length;
    u1 bytes[length];
}
```

* constant_pool_count 计数器： 从1开始而不是0 。

* constant_pool[constant_pool_count-1] 常量池表项：容量=计数器-1。 这部分将在类加载后进入方法区的运行时常量池中存放。

* **Constant pool tags 常量池标签**：☑️可加载的常量池标签

  | Constant Kind                 | Tag  | `class` file format | Java SE |
  | ----------------------------- | ---- | ------------------- | ------- |
  | `CONSTANT_Utf8`               | 1    | 45.3                | 1.0.2   |
  | `CONSTANT_Integer`☑️           | 3    | 45.3                | 1.0.2   |
  | `CONSTANT_Float`☑️             | 4    | 45.3                | 1.0.2   |
  | `CONSTANT_Long☑️`              | 5    | 45.3                | 1.0.2   |
  | `CONSTANT_Double`☑️            | 6    | 45.3                | 1.0.2   |
  | `CONSTANT_Class`☑️             | 7    | 45.3                | 1.0.2   |
  | `CONSTANT_String`☑️            | 8    | 45.3                | 1.0.2   |
  | `CONSTANT_Fieldref`           | 9    | 45.3                | 1.0.2   |
  | `CONSTANT_Methodref`          | 10   | 45.3                | 1.0.2   |
  | `CONSTANT_InterfaceMethodref` | 11   | 45.3                | 1.0.2   |
  | `CONSTANT_NameAndType`        | 12   | 45.3                | 1.0.2   |
  | `CONSTANT_MethodHandle`☑️      | 15   | 51.0                | 7       |
  | `CONSTANT_MethodType`☑️        | 16   | 51.0                | 7       |
  | `CONSTANT_Dynamic☑️`           | 17   | 55.0                | 11      |
  | `CONSTANT_InvokeDynamic`      | 18   | 51.0                | 7       |
  | `CONSTANT_Module`             | 19   | 53.0                | 9       |
  | `CONSTANT_Package`            | 20   | 53.0                | 9       |

* 常量池主要存放两大类常量：字面量Literal和符号引用Symbolic References

  ![image-20210825145944875](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210825145944.png)

  * 简单名称：类似类中的add()方法、num字段的简单名称分别是add、num

  * 描述符：描述字段的数据类型、方法参数列表和返回值

    | `tag` Item | Type                 | `value` Item        | Constant Type      |
    | ---------- | -------------------- | ------------------- | ------------------ |
    | `B`        | `byte`               | `const_value_index` | `CONSTANT_Integer` |
    | `C`        | `char`               | `const_value_index` | `CONSTANT_Integer` |
    | `D`        | `double`             | `const_value_index` | `CONSTANT_Double`  |
    | `F`        | `float`              | `const_value_index` | `CONSTANT_Float`   |
    | `I`        | `int`                | `const_value_index` | `CONSTANT_Integer` |
    | `J`        | `long`               | `const_value_index` | `CONSTANT_Long`    |
    | `S`        | `short`              | `const_value_index` | `CONSTANT_Integer` |
    | `Z`        | `boolean`            | `const_value_index` | `CONSTANT_Integer` |
    | `s`        | `String`             | `const_value_index` | `CONSTANT_Utf8`    |
    | `e`        | Enum class           | `enum_const_value`  | *Not applicable*   |
    | `c`        | `Class`              | `class_info_index`  | *Not applicable*   |
    | `@`        | Annotation interface | `annotation_value`  | *Not applicable*   |
    | `[`        | Array type           | `array_value`       | *Not applicable*   |

  ```java
  Object[] arr = new Object[10];//[Ljava.lang.Object;@1540e19d
  String[] arr1 = new String[10];//[Ljava.lang.String;@677327b6
  long[][] arr2 = new long[10][];//[[J@14ae5a5
  ```

  ⚠️Class文件中不会保存各个方法和字段的最终内存分布信息

* **符号引用和直接引用**

  * 符号引用：与JVM实现的内存布局无关
  * 直接引用：指向目标指针、相对偏移量或间接定位到目标句柄。与内存相关

#### 4）access_flags 访问标识

常量池后跟着访问标识-两个字节

方法和类的访问标识如下，通常为ACC开头

| Flag Name          | Value  | Interpretation                                               | Scope        |
| ------------------ | ------ | ------------------------------------------------------------ | ------------ |
| `ACC_PUBLIC`       | 0x0001 | Declared `public`; may be accessed from outside its package. | Class&Method |
| `ACC_PRIVATE`      | 0x0002 | Declared `private`; accessible only within the defining class and other classes belonging to the same nest ([§5.4.4](https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-5.html#jvms-5.4.4)). | Class&Method |
| `ACC_PROTECTED`    | 0x0004 | Declared `protected`; may be accessed within subclasses.     | Class&Method |
| `ACC_STATIC`       | 0x0008 | Declared `static`.                                           | Class&Method |
| `ACC_FINAL`        | 0x0010 | Declared `final`; must not be overridden ([§5.4.5](https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-5.html#jvms-5.4.5)). | Class&Method |
| `ACC_ABSTRACT`     | 0x0400 | Declared `abstract`; no implementation is provided.          | Class&Method |
| `ACC_SYNTHETIC`    | 0x1000 | Declared synthetic; not present in the source code.          | Class&Method |
| `ACC_SYNCHRONIZED` | 0x0020 | Declared `synchronized`; invocation is wrapped by a monitor use. | Method       |
| `ACC_BRIDGE`       | 0x0040 | A bridge method, generated by the compiler.                  | Method       |
| `ACC_VARARGS`      | 0x0080 | Declared with variable number of arguments.                  | Method       |
| `ACC_NATIVE`       | 0x0100 | Declared `native`; implemented in a language other than the Java programming language. | Method       |
| `ACC_STRICT`       | 0x0800 | Declared `strictfp`; floating-point mode is FP-strict.       | Method       |
| `ACC_INTERFACE`    | 0x0200 | Was an `interface` in source.                                | Class        |
| `ACC_ABSTRACT`     | 0x0400 | Marked or implicitly `abstract` in source.                   | Class        |
| `ACC_ANNOTATION`   | 0x2000 | Declared as an annotation interface.                         | Class        |
| `ACC_ENUM`         | 0x4000 | Declared as an `enum` class.                                 | Class        |

#### 5）类索引、父类索引、接口索引

长度都为u2—— 2字节无符号整数，指向常量池索引，用于确定类的继承关系

* **this_class 类索引**：提供类的权限定名
* **super_class 父类索引：**提供当前类父类的权限定名，只能继承一个，不能是final
* **interface 接口索引：**提过一个符号引用到所有已实现接口
  * interfaces_count：接口计数器——直接接口数量
  * interfaces[]接口索引集合

#### 6）字段表集合

描述接口或类中声明的变量，字段field包括类级变量以及实例级遍历——不包含局部变量。字段名、数据类型等都引用于常量池。

⚠️内部类会自动添加指向外部类实例的字段。字段无法重载——不能重复声明

* **field_count字段计数器：**字段表的成员个数 u2

* **field[]字段表：**每个表都必须是个field_info结构的数据项

  ```
  field_info {	描述接口或类中声明的变量，包括类级变量以及实例级遍历
      u2             access_flags;			访问标识
      u2             name_index;				字段名索引
      u2             descriptor_index;	描述符索引
      u2             attributes_count;	属性计数器
      attribute_info attributes[attributes_count]; 属性集合
  }
  ```

#### 7）方法表集合

methods：指向常量池索引集合，完整描述每个方法的签名。

method_info 对应一个类或一个接口的方法信息

```
method_info {	对应一个类或一个接口的方法信息
    u2             access_flags;				访问标识
    u2             name_index;					字段名索引
    u2             descriptor_index;		描述符索引
    u2             attributes_count;		属性计数器
    attribute_info attributes[attributes_count];	属性集合
}
```

**方法访问和属性标志**

#### 8）属性表集合（了解）

Class文件所携带的辅助信息（用于JVM验证和运行、调试）：源文件名称、带有RetentionPolicy.CLASS\RetentionPolicy.RUNTIME的注解。

* attribute_count 属性计数器
* attribute[] 属性表：每项是个attribute_info结构。[截止JDK16共29个属性](https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-4.html#jvms-4.7)

```
attribute_info {属性集合
    u2 attribute_name_index;	属性名称索引
    u4 attribute_length;			属性长度
    u1 info[attribute_length];属性内容
}
```

* Code属性——最常用的属性，[JDK16-code属性的内部属性集合](https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-4.html#jvms-4.7-320)

  ```
  Code_attribute {
      u2 attribute_name_index;	属性名称索引
      u4 attribute_length;			属性长度
      u2 max_stack;							栈最大深度
      u2 max_locals;						局部变量表所需空间
      u4 code_length;						字节码指令长度
      u1 code[code_length];			存储字节码指令
      u2 exception_table_length;异常表长度
      {   u2 start_pc;					
          u2 end_pc;
          u2 handler_pc;
          u2 catch_type;
      } exception_table[exception_table_length];	异常表信息
      u2 attributes_count;			属性集合计数器
      attribute_info attributes[attributes_count]; 属性集合
  }
  //attribute_info attributes中的LineNumberTable、LocalVariableTable
  LineNumberTable_attribute { 描述Java源码行号与字节码行号之间的对应关系
      u2 attribute_name_index;
      u4 attribute_length;
      u2 line_number_table_length;
      {   u2 start_pc; 					字节码行号
          u2 line_number;				Java源码行号
      } line_number_table[line_number_table_length];
  }
  LocalVariableTable_attribute { 用于确定方法在执行过程中局部变量的信息
      u2 attribute_name_index;
      u4 attribute_length;
      u2 local_variable_table_length;
      {   u2 start_pc;	变量在字节码中的声明周期起始位置
          u2 length;		变量在字节码中的声明周期偏移位置
          u2 name_index;
          u2 descriptor_index;
          u2 index;
      } local_variable_table[local_variable_table_length];
  }
  ```

### 3.1.2 javap指令—解析Class文件

oracle官方提供的反解析工具javap——根据class文件反解析当前类的code区、局部变量表、异常表、和代码行偏移量映射表、常量池等信息。

* **javac <options> <file>** 生成字节码文件 javac -p ——idea默认编译
* **javap <options> <classes> ** ：javap -help。重要的option： **-v -p** -l -c
* 通过反编译分析，一个方法的执行通常涉及：
  * java栈中：局部变量表、操作数栈
  * java堆。通过对象的地址引用操作
  * 常量池
  * 
  * 其他如帧数据区、方法区的剩余部分等

## 3.2 字节码指令集

### 3.2.1 字节码指令概述

操作码（一个字节）+操作数（两个字节）（可能没有操作数）[官方字节码指令集](https://docs.oracle.com/javase/specs/jvms/se16/html/jvms-6.html)

* 操作码的长度为一个字节0～255。——操作码总数不会超过256

```
Java虚拟机的结束器使用以下执行模型来理解
do{
	自动计算PC寄存器的值+1;
	根据PC寄存器的指示位置，从字节码流中取出操作码;
	if(字节码存在操作数){
		从字节码流中取出操作码;
	}else{
		执行操作吗所定义的操作;
	}
}while(字节码长度>0)
```

* **字节码指令分类**：
  * 相关性分类：
    * 显示相关：**a(对象)、i(int)、l、s、b、c、f、d**
    * 隐式相关：如arraylength操作数组类型对象
    * 无关指令：如goto跳转指令——与数据类型无关的
  * **JVM官方分类**：*加载与储存指令、算术指令、类型转换指令、对象的创建与访问指令、方法调用与返回指令、操作数栈管理指令、异常处理指令、同步控制指令*

### 3.2.2 加载与存储指令（重点）

将数据从栈帧的局部变量表和操作数栈之间来回传递

* **常用指令**：含load、push、ldc、const一般为压栈，含store为保存到局部变量表
  * **局部变量压栈：**xload、xload_<n>
  * **常量入栈指令**：bipush、sipush、ldc、ldc_w、
  * **出栈装入局部变量表**：xstore、xstore_<n> （n为0～3）
  * **扩充布局变量表的访问索引的指令**：wide

#### 1）局部变量压栈

xload、xload_<n>——x为i/l/f/d/a，n为局部变量表的索引0～3

xload 0 指令占3个字节，xload_0 占一个字节

#### 2）常量入栈指令

将常量入栈，根据数据类型和入栈内容不同，指令又可以分为三类

* **指令const系列：**
  * iconst_<i>：i 为-1～5，整型数压入栈  iconst 6
  * lconst_<l>：l为0～1，长整型数压入栈
  * fconst_<f>：f为0～2，浮点型数压入栈
  * dconst_<d>：d为0～1，double型压入栈
  * aconst_null：a为对象，将null压住栈
* **指令push系列：**
  * bipush 接收8位整数作为参数入栈
  * sipush 接收16位整数作为参数入栈
* **指令ldc系列：**万能指令，可以接受8位的int、float、String索引入堆栈。类似的还有ldc_w 接受两个8位参数，支持索引范围大于ldc。ldc2_w接受long、double类型

#### 3）出栈装入局部变量表

xstore、xstore_<n> ，x为i、l、f、d、a。n为0～3。

### 3.2.3 算数指令

用于操作数栈上的两个值进行某种特定运算，并将结果压入栈

* **分类**：整数数据的运算、浮点类型数据的运算

  ![image-20210826141549726](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826141655.png)

* **运算时溢出：**负数、ArithmeticException

* **运算模式：**四舍五入、取整模式

* **NaN值使用：**操作产生溢表示无穷大。使用NaN作为操作数时返回NaN

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826143259.png" alt="image-20210826143258970" style="zoom: 60%;" />

* **i++/++i的区别**：如果不涉及其他运算，那在底层的操作码都完全一样iinc 1 by 1。涉及运算的话就是先load和后load 的区别

  ```java
  int i = 10;	i = i++; //10  
  ```

* **比较指令：**弹栈比较，设栈顶数v2，另一个v1，若v1=v2 呀栈0。若v1>v2 压栈1。 若v1<v2 压栈-1

### 3.2.4 类型转换指令

将两种不同的数值类型进行转换——显示类型转换。或处理数据类型无法对应的问题

* **宽化类型转换（小转大）**：精度损失问题

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826152051.png" alt="image-20210826152051652" style="zoom: 60%;" />

  int、long转换到float或long转换到double可能会精度丢失——几个最低有效位上的值

* **窄化类型转换（大转小）**：精度损失问题

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826154025.png" alt="image-20210826154025422" style="zoom:50%;" />

  * 浮点数转整数：如果浮点数是NaN，转换结果是int或long的0。否则取整
  * double转float：四舍五入
    * 转换结果的绝对值太小无法用float表示。返回float类型的正负0
    * 转换结果绝对值太大不误用float表示。返回float类型的正负无穷大
    * double类型的NaN值会转为float类型的NaN值

### 3.2.5 对象的创建与访问指令

对对象操作。进一步细分为 **创建指令、字段访问指令、数组操作指令、类型检查指令。**

* **创建指令** 
  * **创建类实例指令：new**。接收一个指向常量池的操作数。将对象引用压栈。
  * **创建数组的指令**：newarray:基本。anewarray:引用。multianewarray:多维。

* **字段访问指令**
  * **访问类字段—static**：getstatic、putstatic
  * **访问类实例字段—非static**：getfield、putfield

* **数组操作指令**：x为b c s i l f d a 。其中b为byte和boolean
  * **数组元素加载到操作数栈**：xaload。需要两个索引a i ，再把a[i]压栈。
  * **操作数栈的值存储到数组元素**：xastore 。需要值、索引、数组引用
  * **取数组长度**：arraylength。弹出数组、获取长度压栈
* **类型检查指令：**instanceof、checkcast
  * **instanceof**：判断两个对象是否是同一个，结果压栈
  * **checkcast**：检查类型强制转换是否可以进行。不可以的话抛ClassCastException

### 3.2.6 方法的调用与返回指令

* **方法调用指令：**
  * **invokevirtual**：最常见的方法**动态分派**方式——final
  * **invokeinterface：**调用接口方法，**但接口中的静态方法是invokestatic的**
  * **invokespecial：**调用构造器、私有方法、父类方法。**这些方法都是静态类型绑定**
  * **invokestatic：**调用类方法——静态方法。静态绑定
  * **invokedynamic：**调用动态绑定的方法
* **方法返回指令：**x为 i l f d a 。i 包括 boolean、cyte、char、short、int
  * **有具体返回值：**xreturn。弹栈并将弹栈元素压入调用者的操作数栈，其他丢弃。**如果返回的是synchornized方法。还会执行个隐含的monitorexit指令退出临界区**
  * **无具体返回值void：**return

### 3.2.7 操作数栈管理指令

直接操作操作数栈的指令。弹栈、复制、

* **弹栈：**pop、pop2(弹两个)
* **复制：**
  * 不带_x指令复制栈顶元素压栈。dup、dup2。2为复制的slot槽个数
  * 带_x指令复制栈顶元素**插入指令和x系数相加slot个数的位置**。dup_x1(1+1=2) 、dup_x2(1+2=3) 、dup2_x1(2+1=3)、dup2_x2(2+2=4)、
* **交换位置**：swap，交换栈顶两个slot数值——java虚拟机没有提供64位数据的交换
* **占位调试**：nop——字节码0x00。什么都不做

### 3.2.8 控制转义指令

为了支持条件控制。细分为比较、条件跳转、多条件分支跳转、无条件跳转指令

* **比较跳转指令：又属于算术指令**比较数值的大小——boolean、引用不能比较

  dcmpg、dcmpl、fcmpg、fcmpl、lcmp。g是指遇到NaN会压入1，l 会压入-1

* **条件跳转指令：**测试弹栈元素是否满足条件，满足跳转指定offset

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826185631.png" alt="image-20210826185631507" style="zoom:50%;" />

* **比较跳转指令**：比较和跳转指令的结合体。针对两边相等的int和对象类型

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826194039.png" alt="image-20210826194039656" style="zoom:50%;" />

* **多条件分支：**针对switch-case

  * tableswitch：针对case值连续，直接定位——效率高
  * lookupswitch：针对case值不连续，每次执行都要搜索——效率低

* **无条件跳转：**goto。指定指令跳转的偏移量跳转。

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826200831.png" alt="image-20210826200831540" style="zoom:50%;" />

### 3.2.9 异常处理指令

* **异常处理过程**
  * 异常对象生成——>throw（自动/手动）——>指令：**athrow**
  * 抓抛模型try-catch-finally——>**使用异常表**

* **抛出异常指令：**athorw—显示抛出异常的throw操作。许多JVM运行时自动检测抛异常不需要指令如算术异常等
* **异常处理与异常表：**使用throw new 异常类名()这种形式抛出异常。会在方法内多张异常表。

### 3.2.10 同步控制指令

* **组成：**方法级同步、方法内部一段指令序列的同步——都是用monitor支持

  * **同步方法**：隐式的。字节码没有区别——无须字节码控制。由JVM判断访问标识是否是一个同步方法，是会自动在方法调用前进行加锁。
  * **同步代码块**
    * **monitorenter** 请求进入同步代码块。若监视器计数为0。准许进入。为1判断线程是否为自身this。是则进入，否则等待为0。直到允许进入。**将监视器0变为1**
    * **monitorexit**声明退出同步代码块。释放锁。**将监视器1变为0**

  ```java
  synchronized void add(){}//同步方法
  void add(){//同步代码块
    sychronized(obj){}}
  ```

## 3.3 类的加载过程详解

JVM中数据分类为基本数据类型和引用数据类型。基本数据类型由虚拟机预先定义。**引用数据类型需要进行类的加载。**类从.class加载到内存再到卸载，生命周期共7个阶段。

**加载—>链接（验证—>准备—>解析）—>初始化—>使用—>卸载**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826214933.png" alt="image-20210826214933395" style="zoom:50%;" />

### 3.3.1 Loading加载阶段—类加载过程一

将字节码文件加载到机器内存中——查找并加载类的二进制数据生成Class示例。在内存中构建类模板对象（Java类在JVM内存的快照）——**反射机制基于此基础。**

* **加载类时必须完成的3件事**
  * 通过全类名获取类的二进制数据流
  * 解析类的二进制数据流为方法区内的数据结构——Java类模型
  * 堆中创建java.lang.Class类实例以表示类型。作为方法区此类的数据访问入口。
* **二进制流获取方式**不是ClassFile结构就抛ClassFormatError
  * 通过文件系统读入class后缀文件**（最常见）**——java命令
  * 读入jar、zip等归档数据包，提取类文件
  * 事先存放数据库中的二进制数据
  * 使用类似于HTTP等协议通过网络进行加载
  * 在运行时生成一段Class的二进制信息等

* **类模型的位置**：jdk8后为元空间。之前为永久代

* **Class实例的位置：**在堆中创建Class对象来封装位于方法区内的类结构。每个类对应一个Class对象——在加载类过程中创建。（instanceKlas—>mirror:Class 的实例）**Class类的构造器是私有的，只有JVM能够创建**。**是访问元数据接口，和反射的入口**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826220738.png" alt="image-20210826220738145" style="zoom:50%;" />

* **数组类的加载（特殊）**：**数组类本身不由类加载器负责创建**。由JVM运行时需要直接创建。**数组元素类型（引用类型）需要依靠类加载器创建**

### 3.3.2 Linking链接阶段——类加载阶段二

#### 1）链接阶段之Verification（验证）

保证加载字节码是合法合理并符合规范的。

* 格式检查会和加载阶段一起执行。格式验证之外的验证操作后在方法区中进行
* 字节码验证是验证过程中最复杂的一个过程 

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210826221349.png" alt="image-20210826221349798" style="zoom:50%;" />



#### 2）链接阶段之Perparation（准备）

**为静态变量分配内存，并为其初始化赋默认值**

使用static+final修饰，且显示赋值中**不涉及到方法或者构造器调用**的基本数据类型或者String类型的显式赋值将在准备中的链接阶段进行显示赋值。

* 对于准备阶段就完成赋值的，其字段下面的有属性ConstantValue，否则是没有。

* **对于String来说。使用static final修饰。**
  * 字面量赋值。那么显式赋值通常在链接准备阶段
  * **new 对象赋值或方法调用赋值**。显式赋值在初始化阶段

#### 3）链接阶段之Resolution（解析）

将类、接口、字段的**符号引用转换为直接引用**。符号引用就是一些字面量的引用。直接引用是数据的具体内存中的指针或者偏移量

### 3.3.3 Initialization初始化阶段——类加载阶段三

为类的**静态变量**赋初始值（显式赋值），以及需要开辟类内存或方法调用的的static final属性。

* 初始化阶段重要工作时执行类的初始化方法**\<clinit>**方法。
  * 该方法由编译器生成JVM调用。无法定义一个同名方法。更无法人工执行。
  * \<clinit>是由类静态成员的赋值语句和static语句块合并产生的

* **类不一定会有\<clinit>方法的情况**——什么情况会在链接准备阶段赋值？

  * 类没有声明任何类变量（静态成员），也没有静态代码块
  * 类的静态成员没有显示赋值。
  * 类中只包含static final修饰的基本数据类型的字段

* **\<clinit>方法的线程安全性**：类初始化需要确保其多线程环境中各的安全性

  多个线程同时初始化一个类，那么只有一个线程执行类的\<clinit>方法

* **类的主动使用和被动使用（重要）**

  * **主动使用：Class类首次使用才会被装载—不会无条件装载。**
    1. 创建类实例。比如使用new关键字、反射、克隆、反序列化时
    2. 调用类的静态方法。即使用字节码invokestatic指令
    3. 使用类、接口、静态字段时（final特殊考虑），比如使用getstatic、putstatic
    4. 使用反射方法时。如Class.forName(…)
    5. 初始化子类时，父类没有初始化则需要先触发父类初始化。不会初始化接口。初始化接口时，不会初始化接口的父接口
    6. 接口定义**default**方法，直接或间接实现该接口的类的初始化。该接口需初始化
    7. JVM启动时所指定的主类（包含main方法的类）
    8. 初次执行MethodHandle实例时，初始化MethodHandle指向方法所在的类。
  * **被动使用：不会引起类的初始化**——没有初始化的类不意味着没有加载
    * 访问静态字段时，只有真正声明这个字段的类会被初始化（通过子类调用父类的静态变量）
    * 通过数组定义类引用。
    * 引用类的常量
    * 调用ClassLoader类的loadClass方法加载一个类

### 3.3.4 Using/Unloading使用和卸载阶段

**类的使用**：开发人员可以在程序中访问调用类的静态类成员信息，或创建其对象实例

**类的卸载：**

* **类、类加载器、对象的引用关系**：类加载器与类双向关联。通过Class的getClassLoader获取类加载器。类的实例引用代表这个类的Class对象。通过实例的getClass获取其Class引用。Java类都有个静态属性class指向代表这个类的Class引用

* **类的生命周期**：当类的Class对象不再被引用—不可触及。Class对象结束生命周期，对应在方法区内的数据也会被卸载。**类*何时*死亡，取决于它的Class对象何时死亡。**

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210827155100.png" alt="image-20210827155059950" style="zoom:50%;" />

  * 启动类加载器加载的类型在整个运行期间**不可能被卸载**（jvm、jls规范）
  * 系统类加载器和扩展类加载器加载的类在运行期间**被卸载的可能性极小**
  * 自定义的类加载器加载的类型**只有在简单的上下文环境中才可能被卸载**

---

## 3.4 类的加载器详解 

### 3.4.1 类加载器概述

* **类的加载分类**：显示加载、隐式加载

  ```java
  Object obj = new Object();//隐式加载
  Class clazz = Class。forName("java.lang.Object");//显示加载
  ClassLoader.getSystemClassLoader().loadClass("java.lang.Object")//显示
  ```

* **了解类加载必要性**

  * 避免开发中遇到ClassNotFoundException异常或NoClassDeffoundError
  * 需要支持类的动态加载或需要堆编译后的字节码进行加解密操作
  * 编写自定义类加载器定义类的加载规则

* **类的命名空间**：类的唯一性——**加载器和类本身确认为类在JVM中的唯一性**

  * 类加载器都有自己的命名空间：由加载器及其所有父加载器所加载的类组成
  * 同一命名空间不会出现类名相同的类。不同命名空间中可能出现 

* **类加载机制的三个基本特性**

  * **双亲委派机制**，不是所有类加载遵守。部分利用上下文加载器。例如JDBC、JNDI
  * **可见性**：子类加载器可以访问父加载器加载类型。反之不许。**但是邻居互不可见**
  * **单一性：**父加载器加载过的类型不会子加载器中重复加载。**邻居间可以加载多次**

### 3.4.2 类加载器分类复习

分为引导类加载器Bootstrap ClassLoader和自定义类加载器User-Defined ClassLoader

* JDK9以后扩展机制被移除。扩展类加载器更名Platform ClassLoader 平台类加载器，用于旧JDK兼容性

![image-20210828143358540](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210828143358.png)

* 获取ClassLoader的三种途径——引导类加载器无法获取 

  ```java
  clazz.getClassLoader();//获取当前类加载器
  Thread.currentThread().getContextClassLoader();//线程上下文(系统)加载器
  clazz.getPaltformClassLoader();//获取平台类加载器(就是扩展类加载器JDK9开始)
  ClassLoader.getSystemClassLoader();//获取系统类加载器
  ```

* 数组类的Class对象不是由类加载器创建的。而是java运行期JVM根据需要自动创建的。如果素组的元素是基本数据类型，数组类则没有类加载器。

#### 1）引导类加载器 Bootstrap ClassLoader

使用**-XX:+TraceClassLoading** 获取类加载顺序过程

* **使用C/C++语言实现**，嵌套在JVM内部。**用来加载Java核心库**JAVA_HOME/jre/lib/rt.jar 或sun.boot/class.path 路径下的内容。
* 没有父加载器。最顶层的加载器
* 处于安全考虑。Bootstrap启动类加载器只加载包名为java、javax、sun开头的类
* **加载扩展类和应用程序类加载器，并指定为他们的父类加载器**

#### 2）扩展类加载器 Extension ClassLoader

现在已经被重命名为平台类加载器了PlatForm ClassLoader

* Java编写。由sun.misc.Launcher$ExtClassLoader实现。继承ClassLoader类
* 父类加载器为引导类加载器
* 从java.ext.dirs系统属性所指定的目录中加载类库。或从jre/lib/ext子目录下加载库。可以自定义jar放置于此目录下，自动由扩展类加载器加载

#### 3）应用程序加载器 Application ClassLoader

* Java编写。由sun.misc.Launch$AppClassLoader 实现。继承ClassLoader类
* 父类加载器器为扩展类加载器。是自定义类加载器的默认父加载器
* 加载环境变量classpath或系统属性java.class.path 指定路径下的类库
* 应用程序加载器是系统**默认的加载器**。

#### 4）用户自定义类加载器

* 自定义加载源。**实现绝妙的插件机制**，如著名的OSGI组件，Eclipse插件机制等

* **实现应用隔离**，如Tomcat和Spring等中间件和组件框架都实现了自定义加载器。
* 通常需要继承ClassLoader

---

### 3.4.3 ClassLoader 源码解析

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210827190853.png" alt="image-20210827190852948" style="zoom:50%;" />

* **loadClass**

  - 调用findLoadedClass(String)来检查类是否已经加载。
  - 在父类加载器上调用loadClass方法。 若 parent 为null ，使用JVM内置类加载器。
  - 调用findClass(String)方法来查找类。

  鼓励ClassLoader 的子类覆盖findClass(String) ，而不是这个方法。
  除非被覆盖，此方法在整个类加载过程中同步getClassLoadingLock方法的结果。

* **findClass**：查找具有指定二进制名称的类。 这个方法应该被遵循加载类委托模型的类加载器实现覆盖，并且在检查请求类的父类加载器后将被loadClass方法调用。

* **defineClass：**生成Class实例。通常和loadClass一起使用。

```java
 protected Class<?> loadClass(String name, boolean resolve)throws ClassNotFoundException{//双亲委派机制的主要逻辑
     synchronized (getClassLoadingLock(name)) {//同步保证只加载一次
         // 首先调用findLoadedClass(name)检查是否已加载同名类
         Class<?> c = findLoadedClass(name);
         if (c == null) {
             long t0 = System.nanoTime();
             try {
               //获取当前类加载器的父类加载器，存在父类就委托父类
                 if (parent != null) {
                     c = parent.loadClass(name, false);
                 } else {//parent==null：父类加载器是引导类加载器
                     c = findBootstrapClassOrNull(name);
                 }
             } catch (ClassNotFoundException e) {
               //如果没有类加载器支持加载此类，抛ClassNotFoundException
             }
             if (c == null) {
               //当前类加载器的父类加载器未加载此类或当前类加载器未加载此类
               //调用ClassLoader的findClass(name)方法 
               long t1 = System.nanoTime();
               c = findClass(name);
         			//找到此类加载器。记录数据
 						...记录过程
             }
         }
         if (resolve) {//加载class的同时是否进行解析操作
             resolveClass(c);
         }
         return c;
     }
}
 protected final Class<?> defineClass(String name, byte[] b, int off, int len,ProtectionDomain protectionDomain)throws ClassFormatError{  
  //off、len是Class在二进制数据中的偏移量和长度
  //使用可选的ProtectionDomain将字节数组转换为类Class的实例，类使用前必须被解析
   
  //preDefineClass检查保护域、类的签名是否存在
  protectionDomain = preDefineClass(name, protectionDomain);
  String source = defineClassSourceLocation(protectionDomain);
  Class<?> c = defineClass1(name, b, off, len, protectionDomain,source);
  postDefineClass(c, protectionDomain);
  return c;
}
```

* **SecureClassLoader 和 URLClassLoader**
  * SecureClassLoader新增几个使用相关的代码源和权限定义类验证方法。一般不用
  * URLClassLoader为findClass、findResouce方法等提供具体实现。**在编写自定义类的时候，如果没有过于复杂的需求，可以直接继承URLClassLoader**
* **ExtClassLoader和AppClassLoader**：都继承URLClassLoader。且都是由sun.misc.Launcher创建。都满足双亲委派机制
* **Class.forName()与ClassLoader.loadClass()的区别**
  * Class.forName()静态方法。根据传入对象的全类名返回Class对象。加载Class对象到内存并执行类的初始化
  * ClassLoader.loadClass()实例方法。需要ClassLoader实例调用。**Class文件加载到内存。不执行类的初始化。**指定类的类加载器。

### 3.4.4 双亲委派机制

JDK1.2 开始实行，保证平台安全。原理：类加载器收到加载类的请求时不直接加载类。而是把任务委托给父类。依次递归。没有父类可以加载时自己再加载。在classLoad.loadClass方法体现。重写该方法后还是不能修改核心类——因为最终会调用java.lang.ClassLoader.defineClass方法，执行**preDefineClass**提供的核心类保护。

* **优势：**避免类的重复加载。保护程序安全，防止核心API被随意篡改。

* **劣势：**类加载的委托过程是单向的。**但是顶层的ClassLoader无法访问底层的ClassLoader加载的类**——应用类访问系统类没有问题。反过来就有问题了。

  如接口和工厂方法都在启动类加载器中。该工厂方法就无法创建由应用类加载器加载的应用实例问题

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210827201019.png" alt="image-20210827201019015" style="zoom:50%;" />



* **三个破坏双亲委派机制行为**：jdk1.2之前不满足

  1. 一：重写了loadClass方法直接覆盖了双亲委派机制。——建议重写findClass

  - 二：越基础的类越上层加载器加载导致基础类型无法回调用户代码，如JNDI。——引入上下文类加载器（默认为应用程序类加载器）。父类加载器通过上下文类加载器请求子类加载器完成类加载。——JDK6又提供了**java.util.ServiceLoader**

  - 三：**动态性的代码热部署**导致，需要将双亲委派的树状结构替换成网状结构。

    <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210828130832.png" alt="image-20210828130832323" style="zoom:60%;" />

  *代码热部署（热替换）*：程序不间断，修改后能立即表现在运行中的系统中，Java实现热部署是通过灵活运用ClassLoader

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210828131220.png" alt="image-20210828131220732" style="zoom:50%;" />

### 3.4.5 沙箱安全机制

保护程序安全，保护Java原生JDK代码。Java代码在限定在JVM特定的运行范围中，并且严格限制代码堆本地系统资源的访问——Java的核心安全模型

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210828132601.png" alt="image-20210828132601361" style="zoom:50%;" />

### 3.4.6 自定义加载器

* **自定义加载器的目的和一些场景**（⚠️类型转换时需要时同一个加载器加载的）
  * 隔离加载类。类的冲裁——>类冲突
  * 修改类加载的方式。实现按需加载
  * 扩展加载类。从数据库、网络等进行加载
  * 防止源码泄漏。对源码进行编译加密则类加载器也需要自定义，还原加密字节码
* **实现方式**：直接或间接继承ClassLoader
  * 方式一：重写loadClass方法
  * 方式二：重写findClass方法（推荐）

---

# 第四章 性能监控与调优

## 4.1 性能监控与调优概述

* **生产环境中的问题**
  * 开发环境发生内存溢出如何处理？
  * 生产环境应该给服务器分配多少内存合适？给应用分配多少线程合适？
  * 如何对垃圾回收器的性能进行调优？
  * 生产环境CPU负载飙高如何处理？
  * 不加log，如何确定请求是否执行了某一行代码？
  * 不加log，如何实时查看某个方法的入参与返回值？

* **为什么要调优**：防止和解决OOM，减少Full GC的出现频率

* **不同阶段的考虑：**上线前、运行阶段、线上出现OOM

* **监控依据：**运行日志、异常堆栈、GC日志、线程快照、堆转储快照

* **调优方向：**合理代码、合理充分的硬件资源、合理的JVM调优

* **性能评价/测试指标**：

  * 响应(停顿)时间：提交请求和返回响应间所用时间、GC暂停工作线程的时间
  * 吞吐量：单位时间内完成的工作量。GC中用户代码运行的时间占总运行时间比
  * 并发数：同一时刻，对服务器有实际交互的请求数
  * 内存占用：堆区所占实际大小

  相互间关系（以高速公路为例）：吞吐量是每天高速收费站的车辆数，并发数是高速公路上正在行驶的车辆数目，响应速度为车速。车数越多车速越低，车速低堵死了收费站的车辆没就没了。

* **性能优化步骤**：三个步骤
  * **性能监控(发现问题)**:GC频繁、CPU过高、OOM、内存泄漏、死锁、响应时间长。
  * **性能分析(排查问题)**:通常在质量评估、系统测试、开发环境下进行
    * 打印GC日志。通过GCViewer或GCEasy分析日志
    * 使用命令行工具jstack（堆栈信息）、jmap、jinfo
    * 使用阿里Arthas、jconsole、jVisualVM实时查看JVM状态
  * **性能调优(解决问题)**:改善应用响应性或吞吐量而更改参数、源代码、属性配置
    * 适当增加内存。选择合适的垃圾回收器
    * 优化代码，控制内存使用
    * 增加机器、分散节点压力
    * 合理设置线程池线程数
    * 使用中间件提高程序效率：如缓存、消息队列
    * ……

## 4.2 JVM监控及诊断工具

造成性能问题的因素很多如线程控制、磁盘读写、数据库访问、网络I/O、垃圾收集等。**无监控不调优**！重点关注死锁Deadlock、等待资源Waiting on condition、等待获取监视器Waiting on monitor entry、 阻塞Blocked、执行中Runnable。

### 4.2.1 命令行监控及诊断

jdk的bin目录是java官方的辅助工具，用来获取JVM不同方面、层次的信息。**-help查看**

* 命令行监控及诊断存在下列局限：
  * 无法获取方法级别的分析数据，如方法间的调用关系、各方法的调用次数和调用时间等（这对定位应用性能瓶颈至关重要）
  * 要求用户登录到目标Java应用所在的宿主机上，使用起来不是很方便。
  * 分析数据通过终端输出，结果展示不够直观。

* **jps(Java Process Status)：查看正在运行的Java进程**。本地JVM进程和操作系统进程ID一致

  ```
  jps [-q] [-mlvV] [<hostid>] 
  -q 简洁显示线程ID -l 显示完整线程信息 -m 显示线程的传递参数 -v 列出JVM参数
  ```

* **jstat(JVM Statistics Monitoring Tool)：查看JVM统计信息**。显示本地或远程的JVM进程中的类装载、内存、GC、JIT。可以判断是否出现内存泄漏。

  * 第1步：在长时间运行的Java程序中，我们可以运行jstat命令连续获取多行性能数据，并取这几行数据中0U列（即已占用的老年代内存）的最小值。
  * 第2步：我们每隔一段较长的时间重复一次上述操作，来获得多组0U最小值。如果这些值呈上涨趋势，则说明该Java程序的老年代内存已使用量在不断上涨，这意味着无法回收的对象在不断增加，因此很有可能存在内存泄漏。

  ```
  jstat -<option> [-t][-h<lines>] <vmid> [<interval> [<count>]]
  option is one of: 
  	-class：显示Classloader的相关信息：
  	-gc：显示与GC相关的堆信息。包括Eden区、Survivor区、老年代、永久代等信息。
  	-gccapacity：与-gc基本相同，但主要关注Java堆各个区域使用的最大、最小空间。
  	-gcutil：与-gc基本相同，但输出主要关注已使用空间占总空间的百分比。
  	-gccause：与-gcutil功能一样，但是会额外输出最后一次GC产生的原因。
  	-gcnew：显示新生代GC状况
  	-gcnewcapacity：与-gcnew基本相同，输出主要关注使用到的最大、最小空间
  	-geold：显示老年代GC状况
  	-gcoldcapacity：显示内容与-gcold基本相同，输出主要关注使用到的最大、最小空间
  	-gcpermcapacity：显示永久代使用到的最大、最小空间。
    -compiler：显示JIT编译器编译过的方法、耗时等信息 
    -printcompilation：输出已经被JIT编译的方法
  vmid:进程号。包括interval毫秒间隔，count打印数量
  ```

  

  <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210828202932.png" alt="image-20210828202932066" style="zoom:50%;" />

* **jinfo(Configuration Info for Java)：查看虚拟机配置参数信息**，也可用于调整虚拟机的配置参数。

  * -XX:+PrintFlagsInitial 查看所有值的初始值
  * -XX:+PrintFlagsFinal 查看所有值的最终值
  * -XX:PrintCommandLineFlags 查看已经被用户或JVM设置过的详细的XX参数信息

```
 jinfo <option> <pid> 
option is one of: zulujdk 1.8只能执行后三个指令
		no option 输出全部的参数和系统属性
		-flags 输出全部的参数
		-sysprops 输出系统属性
    -flag <name> 输出对应名称的参数
    -flag [+|-]<name>  开启或者关闭对应名称的参数——标记manageable的才能设定
    -flag <name>=<value> 设定对应名称的参数，立即生效——标记manageable的才能设定
```

* **jmap(JVM Memory Map):：获取dump文件（堆存储快照文件）和Java进程 的内存相关信息**
  * jmap需要借助安全点机制—等待安全点
  * **导出内存映像文件** .hprof 文件 
    * 手动：jmap -dump:format=b,file=<filename.hprof>\<pid> 可以使用-dump:live
    * 自动：生成前触发FullGC
      * -XX:+HeapumpOnOutOfMemoryError 
      * -XX:HeapDumpPath=<filename.hprof>

```
jmap [option] <pid>
jmap [option] <executable <core>
jmap [option] [server_id@]<remote server Ip or hostname>
option is one of: zulujdk 1.8只能执行前两个指令
	-dump 生成Java堆转储快照：dump文件 
		特别的：-dump：live只保存堆中的存活对象
	-histo 输出堆中对象的统计信息，包括类、实例数量和合计容量
		特别的：-histo：live只统计堆中的存活对象
	-heap	输出整个堆空间的详细信息，包括GC的使用、堆配置信息，以及内存的使用信息等
	-permstat以ClassLoader为统计口径输出永久代的内存状态信息Linux/solaris平台有效
	-finalizerinfo显示在F-Queue中等待Finalizer线程执行finalize方法的对象平台同上
	-F 当虚拟机进程对-dump选项没有任何响应时，可强制执行生成dump文件平台同上
	-J <flag>传递参数给jmap启动的jvm
```

* **jhat(JVM Heap Analysis Tool)：分析jamp生成的heap dump文件**。妹纸HTTP/HTML服务器。生成分析结果——默认http://localhost:7000。jdk9以后没了

* **jstack（JVM Stack Trace）：用于生成虚拟机指定进程当前时刻的线程快照**—可用于定位线程出现长时间停顿的原因，如**线程间死锁、死循环、请求外部资源导致的长时间等待等问题**。线程出现停顿时可以用jstack显示各个线程调用的堆栈情况。

  ```
  jstack <option> <pid> pid线程ID
  option is one of: zulujdk 1.8只能执行前1个指令
  	-F 当正常输出的请求不被响应时，强制输出线程堆栈
  	-l 除堆栈外，显示关于锁的附加信息
  	-m 如果调用到本地方法的话，可以显示C/C++的堆栈
  ```

* **jcmd多功能工具，实现之前的除了jstat之外的所有命令功能**（推荐）

  ```
  jcmd <pid | main class> <command ...|PerfCounter.print|-f file>
  or: jcmd -l  列出所有jvm进程                                             
  or: jcmd -h  
  jcmd pid help 针对指定进程列出支持的所有命令
  jcmd pid <option> 指定进程的指定命令
  ```

* **jstatd：启用远程监控。**配合一些监控工具也支持对远程计算机的监控（如jps、jstat）使用。jstatd是一个RMI服务端程序，作用相当于代理服务器，建立本地与远程监控工具的通信—将本机的Java应用程序信息传递到远程计算机。

### 4.2.2 GUI工具监控及诊断

* **JDK自带的工具**
  * jconsole：JDK自带的可视化监控工具。查看Java应用程序的运行概况、监控堆信息、永久区（或元空间）使用情况、类加载情况等。
  * Visual VM：jvisualvm工具提供用于查看Java虚拟机上运行的基于Java技术的应用程序详细信息的可视化界面。jdk\bin\jvisualvm.exe（现在没有了）
  * JMC：Java Mission Control，内置Java Flight Recorder。能够以极低的性能开销收集Java虚拟机的性能数据。
* **第三方工具**
  * MAT（Memory Analyzer Tool）是基于Eclipse的内存分析工具，是一个快速、功能丰富的Java heap分析工具，它可以帮助我们查找内存泄漏和减少内存消耗
  * JProfiler：商业软件，需要付费。功能强大。
* **生成dump文件方法**
  * 一：jmap —— jmap -dump :format=b,file=\a.hprof 4568
  * 二：-XX:+HeapDumpOnOutOfMemoryError、-XX:+HeapDumpBeforeFullGC
  * 三：使用VisualVM可以导出堆dump文件
  * 四：使用MAT可以打开已有的堆快照，也可以从活动Java程序中导出堆快照。

#### 1）Jconsle

从Java5开始，在JDK中自带的java监控和管理控制台。用于对JVM中内存、线程和类等的监控，是一个基于JMX（java management extensions）的GUI性能监控工具。

* **启动**：命令行jconsle启动
* **连接方式**：Local本地、Romote通过URL远程，需要在环境变量中设置mx.remote.credentials来指定用户名密码进行授权。Advanced连接JMX代理

#### 2）Visual VM（推荐掌握）

集成了多个JDK命令行工具，使用VisualVM可用于显示虚拟机进程及进程的配置和环境信息（jps，jinfo），监视应用程序的CPU、GC、堆、方法区及线程的信息（jstat、jstack）等，甚至代替JConsole。它完全免费。也可以作为独立的软件安装。

* **连接方式**
  * 本地连接：监控本地Java进程的CPU、类、线程等
  * 远程连接
    * 确定远程服务器的ip地址
    * 添加JMX（通过JMX技术具体监控远端服务器哪个Java进程
    * 修改bin/catalina.sh文件，连接远程的tomcat
    * 在..onf中添加jmxremote.access和jmxremote.password文件
    * 将服务器地址改为公网ip地址
    * 设置阿里云安全策略和防火墙策略
    * 启动tomcat，查看tomcat启动日志和端口监听
    * JMX中输入端口号、用户名、密码登录
* **主要功能**
  * 生成/读取堆内存快照
  * 查看JVM参数和系统属性
  * 查看运行中的虚拟机进程
  * 生成/读取线程快照
  * 程序资源的实时监控
  * 其他功能：JMX代理连接、远程环境监控、CPU分析和内存分析

#### 3）eclipse MAT

MAT基于Eclipse开发的，可以单独使用，还可以作为插件的形式嵌入在Eclipse中使用。主要作用是dump文件分析。最吸引人的是**能生成内存泄漏报表**。

* 分析dump文件 

  * **histoaram**：展示了各个类的实例数目以及这些实例heap或Retainedheap的总和

  * thread overview：查看系统中的Java线程、查看局部变量的信息

  * 获得对象相互引用关系：with outgoing references 、 with incoming references

  * **浅堆与深堆**：

    * **shallow heap** 对象本身占用的内存，不包括其内部引用对象的大小。

    * **retained heap** 一个对象的深堆指只能通过该对象访问到的（直接或间接）所有对象的浅堆之和，**即对象被回收后，可以释放的真实空间。**保留集可以被认为是只能通过对象A被直接或间接访问到的所有对象的集合。

      <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210829164321.png" alt="image-20210829164321504" style="zoom:50%;" />

  * **支配树**：支配树体现了对象实例间的支配关系。在对象引用图中，所有指向对象B的路径都经过对象A，则认为对象A支配对象B。如果对象A是离对象B最近的一个支配对象，则认为对象A为对象B的直接支配者。支配树是基于对象间的引用图所建立的，它有以下基本性质：

    * 对象A的子树（所有被对象A支配的对象集合）表示对象A的保留集即深堆。 
    * 如果对象A支配对象B，那么对象A的直接支配者也支配对象B。
    * 支配树的边与对象引用图的边不直接对应。

    <img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210829170215.png" alt="image-20210829170215514" style="zoom:60%;" />

#### 4）JProfiler

适配于IDEA，远强大于MAT，收费。

* **特点**：简单且强大。对分析应用的影响小—提供模板。 CPU，Thread，Memory分析功能尤其强大支持对jdbc，noSql，jsp，servlet，socket等进行分析支持多种模式（离线，在线）的分析。支持监控本地、远程的JVM。跨平台，多种操作系统的版本
* **具体使用**
  * **安装配置**：需要在IDEA中配置Jprofiler 和Jprofiler中配置IDEA
  * **采集方式**：Sampling样本采集（推荐）和Instrumentation 重构模式
    * **Instrumentation**：JProfiler全功能模式。在class加载之前，JProfier把相关功能代码写入到需要分析的class的bytecode中，对正在运行的jvm有一定影响。
      * 优点：功能强大。在此设置中，调用堆栈信息是准确的。
      * 缺点：若分析的class较多，则对应用的性能影响较大，CPU开销可能很高。因此使用此模式一般配合Filter使用，只对特定的类或包进行分析。
    *  **Sampling**：每隔一定时间(5ms)将每个线程栈中方法栈中的信息统计出来。
      *  优点：对CPU的开销非常低，对应用影响小（即使你不配置任何Filter）
      * 缺点：一些数据/特性不能提供（例如：方法的调用次数、执行时间）
  * 遥感监测Telemetries：JVM概览情况
  * **内存视图Live Memory**：可分析内存泄漏
  * **堆遍历heap walker**：可分析内存泄漏
  * cpu视图cpuviews：相关方法的执行
  * 线程视图threads：监控线程运行状态
  * 监视器&锁Monitors&locks：所有线程持有锁的情况以及锁的信息。
* **内存溢出的分析案例**：内存使用情况越来越大—>可疑对象的占用空间越来越大—>记录可疑对象(Recorded Objects)——>将可疑对象在堆遍历中观察其快照—>查看其相关引用(income/outcome)—>追溯其GC Roots

#### 5）Arthas（推荐掌握）

阿里巴巴开源。Arthas适合服务器端使用命令行方式进行在线监控调优。在线排查问题，动态跟踪Java代码，实时监控JVM状态。

* [Arthas用户文档](https://arthas.aliyun.com/doc/)
* [相关指令](https://arthas.aliyun.com/doc/advanced-use.html)

#### 4)JMC(Java Mission Control)

[Oracle Java Mission Control](https://www.oracle.com/java/technologies/jdk-mission-control.html)Java官方提供的性能强劲的工具。是一个用于对Java应用程序进行管理、监视、概要分析和故障排除的工具套件。从JDK11开始，JMC中的JFR(Java Flight Recorder)已开源。

* 如果是远程服务器，使用前要开JMX。
* **Java Flight Recorder**：JFR的性能开销很小，能够直接访问虚拟机内的数据，并且不会影响虚拟机的优化。非常适用于生产环境下满负荷运行的Java程序。
  * 取样分析。必须先添加参数：-XX:+UnlockCommercialFeatures -XX:+FlightRecorder。
  * 事件类型
    * 瞬时事件（Instant Event）关心发生与否，例如异常、线程启动事件。
    * 持续事件（Duration Event）关心的是它们的持续时间，例如垃圾回收事件。 
    * 计时事件（Timed Event），是时长超出指定阈值的持续事件。
    * 取样事件（Sample Event），是周期性取样的事件。

### 4.2.3 其他工具

#### 1）Flame Graphs（火焰图）

[火焰图](http://www.brendangregg.com/flamegraphs.html)就是一种非常直观的展示cpu在程序整个生命周期过程中时间分配的工具。非常直观的显示出调用栈中的CPU消耗瓶颈。

* 每个框表示线程的栈帧。纵轴为函数调用栈。横轴为时间轴

#### 2）Tprofiler

**在海量业务代码里边准确定位需要性能提升的代码。可以使用阿里开源工具TProfiler** 来定位这些性能代码，解决GC过于频繁的性能瓶颈。TProfiler最重要的特性就是**能够统计出你指定时间段内 JVM的top method**极有可能造成JVM性能瓶颈的元凶。这是其他大多数JVM调优工具所不具备的—包括JRockit Mission Control。

大意是一个Java平台的安全的动态追踪工具。可以用来动态地追踪一个运行的Java程序。BTrace动态调整目标应用程序的类以注入跟踪代码（“字节码跟踪”）。

#### 3）Btrace、Youkit、JProbe、Spring Insight

## 4.3 内存泄漏和OQL查询语句

### 4.3.1 内存泄漏的理解和分类

可达性分析算法来判断对象是否是不再使用的对象，**本质都是判断一个对象是否还被引用。**由于代码的实现不同就会出现很多种内存泄漏问题（让JVM误以为此对象还在引用中，无法回收，造成内存泄漏）——**对象已经不被需要了但还在使用。**
**严格来说，只有对象不会再被程序用到了，但是GC又不能回收他们的情况，才叫内存泄漏。**但实际情况很多时候**一些不太好的实践（或疏忽）会导致对象的生命周期变得很长甚至导致OOM，也可以叫做宽泛意义上的“内存泄漏”**

<img src="https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210829173008.png" alt="image-20210829173008519" style="zoom:70%;" />

* **内存泄漏和内存溢出的关系**

  * 内存泄漏 memory leak ：占着茅坑不拉屎
  * 内存溢出 out of memory：坑位不够

* **泄漏的分类**

  * 经常发生：发生内存泄露的代码会被多次执行，每次执行，泄露一块内存；
  * 偶然发生：在某些特定情况下才会发生；
  * 一次性：发生内存泄露的方法只会执行一次；
  * 隐式泄漏：一直占着内存不释放，直到执行结束严格的说这个不算内存泄漏，因为最终释放掉了但是如果执行时间特别长，也可能会导致内存耗尽。

* **内存泄漏的8种情况**

  * **静态集合类：**如HashMap、LinkedList等等。

    如果容器为静态那么其生命周期与JVM程序一致，则容器中的对象在程序结束之前将不能被释放，从而造成内存泄漏——因为长生命周期对象持有短生命周期的引用导致不能被回收。

    ```java
    public class MemoryLeak {
    	static List List = new Arraylist(); 
      public void oomTests(){
    			object obj = new 0bject（）；//局部变量
          list.add(obj);//被静态长期对象持有而不能回收
      }
    }
    ```

  * **单例模式**：因为单例的静态特性（类似静态合集）。它的生命周期和JVM的生命周期一样长。如果持有外部对象的引用，外部对象也不会被回收。

  * **内部类持有外部类：**内部类持有外部类，如果一个外部类的实例对象的方法返回了一个内部类的实例对象。这个内部类对象被长期引用了，由于内部类持有外部类的实例对象，这个外部类对象将不会被垃圾回收，这也会造成内存泄漏。

  * **各种连接，如数据库连接、网络连接和IO连接等**

  * **变量不合理的作用域**：一个变量的定义的作用范围大于其使用范围，很有可能会造成内存泄漏。另一方面，如果没有及时地把对象设置为null，可能导致内存泄漏

    ```java
    public class UsingRandom {
    	private String msg; //变量不合理的作用域
    	public void receiveMsg(){ I
    		//private String msg;
        //由于msg从网络接收放到数据库不需要定义在外部。在内部定义即可
    		readFromNet（）；//从网络中接受数据保存到msg中
        saveDB（）；//把msg保存到数据库中
      }
    }
    ```

  * **改变哈希值**：当对象被存储进HashSet集合中，不能修改这个对象中的那些参与计算哈希值的字段了。否则修改后的哈希值与最初存储的不同。使用该对象的当前引用作为的参数去HashSet中检索对象将返回找不到对象的结果。也会导致无法从HashSet中单独删除当前对象，造成内存泄漏。 **这也是String被设置成了不可变类型的原因。把String当做HashMap的key值。**

  * **缓存泄漏**：一旦把对象引用放入到缓存中，就很容易遗忘。

    比如：代码中会加载一个表中的数据到缓存（内存）中，测试环境只有几百条数据，但是生产环境有几百万的数据。——**可以使用WeakHashMap代表缓存**

  * **监听器和回调**：如果客户端在实现的API中注册回调，却没有显示的取消，那么就会积聚。需要确保回调立即被当作垃圾回收的最佳方法是只保存它的弱引用，例如将他们保存成为WeakHashMap中的键。

### 4.3.2 内存泄漏案例

* **案例一：stack的出栈操作**

  ```java
  //存在内存泄漏
  //    public Object pop() { //出栈
  //        if (size == 0)
  //            throw new EmptyStackException();
  //        return elements[--size];//不用的内存对象还存在——过期引用
  //    }
  
      public Object pop() {
          if (size == 0)
              throw new EmptyStackException();
          Object result = elements[--size];
          elements[size] = null;//元素制空，避免泄漏
          return result;
      }
  ```

* **案例二：移动端的页面生成**

  每次访问页面时，生成一个页面对象——匿名线程。没有销毁线程的话重复访问会造成内存泄漏。——可采用WeakReference方法解决。另外在使用Handler，如存在Delay操作，也可以采用WeakReference。如果使用的是Handler+HandlerThread时，记住在周期性对象销毁时调用looper.quit()方法

### 4.3.3 OQL查询语句

一种类似于SQL的查询语言OQL （Object Query Language）。OQL使用类SQL语法，可以在堆中进行对象的查找和筛选。

```oql
SELECT * FROM java.lang.String s
SELECT FROM java.lang.String s WHERE toString(s) LIKE ".*java.*"
SELECT * FROM Ox37a0b4d
SELECT objects v.elementData FROM java.util.ArrayList v
```

# 第五章 JVM参数和GC日志分析

## 5.1 JVM参数选项类型

* **类型一：标准参数选项（了解，使用少）——使用java -help查看参数**
  * 特点：比较稳定，后续版本基本不会变化，以-开头
  * 各种选项：运行java或者java -help可以看到所有的标准选项
  * 补充内容：-server与-client
* **类型二：-X参数选项——使用java-X命令可以看到所有的X选项**
  * 特点：非标准化参数功能还是比较稳定的。但官方说后续版本可能会变更以
  * JVM的川编译模式相关的选项
  * 特别地
    * -Xms<size> 设置初始Java堆大小，等价于-XX：InitialHeapSize
    * -Xmx<size>设置最大Java堆大小，等价于-XX：MaxHeapSize
    *  -Xss<size>设置Java线程堆栈大小，-XX：ThreadStackSize
* **类型三：-XX参数属性**
  * 特点：非标准、使用频率最多，不稳定。以-XX开头
  * 作用：用于开发和调试JVM
  * 分类：
    * Boolean类型格式
      * -xx:+<option>表示启用option属性
      * -XX:-\<option>表示禁用option属性
    * 非Boolean类型格式（key-value类型） numberk可以带上单位
      * 子类型1：数值型格式-XX：\<option>=\<number>
      * 子类型2：非数值型格式-XX：\<name>=\<string>
  * 特别的：-XX:+PrintFlagsFinal
    * 输出所有参数的名称和默认值
    * 默认不包括Diagnostic和Experimental的参数
    * 可以配合-XX：+UnlockDiagnosticVMOptions和-XX：UnlockExperimentalVMOptions使用
  * **参数修改**：被标记为manageable的参数才能在程序运行时修改。
    * 使用java -XX:+PrintFlagsFinal -version grep managleable 查看可修改参数

## 5.3 常用的JVM参数选项



* **打印设置的XX选项及值**

  * -XX:+PrintCommandLineFlags 程序运行前打印手动或自动设置的XX选项
  * -XX:+PrintFlagsInitial 表示打印出所有XX选项的默认值
  * -XX:+PrintFlagsFinal 表示打印出XX选项在运行程序时生效的值
  * -XX:+PrintVMOptions打印JVM的参数

* **堆、栈、方法区等内存大小设置**

  * **栈**：-Xss128k 设置每个线程的栈大小为128k 等价于-XX:ThreadStackSize
  * **堆**：
    * -Xms3550m 等价于-XX：InitialHeapSize，设置JVM初始堆内存为3550M
    * -Xmx3550m 等价于-XX：MaxHeapSize，设置JVM最大堆内存为3550M
    * -Xmn2g 设置年轻代大小为2G，官方推荐配置为整个堆大小的3/8
    * -XX:NewSize=1024m 设置年轻代初始值为1024M
    * -XX:MaxNewSize=1024m 设置年轻代最大值为1024M
    * -XX:SurvivorRatio=8 设置年轻代中Eden区与一个Survivor区的比值，默认为8
    * -XX:+UseAdaptiveSizePolicy 自动选择各区大小比例 启用后原先默认的失效
    * -XX:NewRatio=4 设置老年代与年轻代（包括1个Eden和2个Suvivor区）的比值
    * -XX:PretenureSizeThreadshold=1024 设置让大于此阈值的对象直接分配在老年代，单位为字节——只对Serial、ParNew收集器有效

  * **方法区**
    * 永久代
      * -XX:PermSize=256m设置永久代初始值为256M
      * -XX:MaxPermSize=256m设置永久代最大值为256M
    * 元空间
      * -XX:MetaspaceSize初始空间大小
      * -XX:MaxMetaspaceSize最大空间，默认没有限制
      * -XX:+UseCompressedOops压缩对象指针
      * -XX:+UseCompressedClassPointers压缩类指针
      * -XX:CompressedClassSpaceSize设置Klass Metaspace的大小，默认1G
    * 直接内存：
      *  -XX:MaxDirectMemorySize 指定DirectMemory容量，默认值和Java堆一样

* **OOM相关的选项**

  * -XX:+HeapDumpOnOutOfMemoryError 出现OOM时生成heap dump文件
  * -XX:+HeapDumpBeforeFullGC 表示在出现FullGC之前，生成Heap转储文件
  * -XX:HeapDumpPath=<path> 指定heap转存文件的存储路径
  * -XX:OnOutOfMemoryError 指定一个可行程序或脚本，发生OOM时执行这个脚本

* **垃圾收集器相关选项**

  * **查看默认垃圾收集器Serial回收器**
    * FXX：+PrintCommandLineFlags：查看命令行相关参数（包含使用的GC）
    * 使用命令行指令：jinfo - flag相关垃圾回收器参数进程ID
  * **Serial回收器**：-XX:+UseSerialGC、
  * **ParNew回收器**：-XX:+UseParNewGC、-XX:ParallelGCThreads=N
  * **Parallel回收器**：-XX:+UseParallelGC、-XX:+UseParallelOldGC(互相激活)
    * -XX:ParallelGCThreads 设置年轻代并行收集器的线程数。最好与CPU数量相等
    * -XX:MaxGCPapiseMillis 设置垃圾收集器最大停顿时间（STW）毫秒。(慎用！)
    * -XX:GCTimeRatio 垃圾收集时间占总时间的比例=1/(N+1)。衡量吞吐量大小。
    * -XX:+UseAdaptiveSizePolicy设置Parallel收集器具有自适应调节策略
  * **CMS回收器**：XX:+UseConcMarkSweepGC （jdk14删除了）
    * -XX:CMSlnitiatingOccupanyFraction 设置堆内存使用率的阈值—**可降低FullGC**
    * -XX:+UseCMSCompactAtFullcollection 设置FullGC后对内存空间进行压缩整理
    * -XX:CMSFullGCsBeforeCompaction 执行多少次FullGC对内存空间压缩整理。
    * -XX：ParallelCMSThreads设置CMS的线程数量。
    * 其他参数……
  * **G1回收器**：XX:+UseG1GC
    * -XX:G1HeapRegionSize 设置每个Region的大小。默认是堆内存的1/2000
    * -XX:MaxGCPauseMillis 设置期望达到的最大GC停顿时间指标。默认200ms
    * -XX:ParallelGCThread 设置STW时GC线程数的值。最多设置为8
    * -XX:ConcGCThreads 设置并发标记的线程数。n设置并行回收线程数1/4左右
    * -XX:InitiatingHeapOccupancyPercent 设置触发并发GC周期的Java堆占用率阈值。超过此值，就触发GC。默认值是45。
    *  -XX:G1NewSizePercent、 -XX:G1MaxNewSizePercent 新生代占用整个堆内存的最小百分比（默认5%）、最大百分比（默认60%）
    * -XX:G1ReservePercent=10 保留内存区域，防止to space溢出
    * G1关于MixedGC调优常用参数：
      * -XX：InitiatingHeapOccupancyPercent
      * -XX：G1MixedGCLiveThresholdPercent
      * -XX：G1HeapWastePercent
      * -XX：G1MixedGcCountTarget
      * -XX：G10ldCSetRegionThresholdPercent

* **GC日志相关选项**

  * GC**常用参数**
    * -verbose:gc 输出gc日志信息，默认输出到标准输出 等同于-XX:+PrintGC
    * -XX:+PrintGCDetails 在发生垃圾回收时和程序退出时打印内存回收详细的日志
    * -XX:+PrintGCTimeStamps/-XX:+PrintGCDateStamps 输出GC发生时的时间戳
    * -XX:+PrintHeapAtGC 每一次GC前和GC后，都打印堆信息
    * -Xloggc:<file>把GC日志写入到一个文件中去，而不是打印到标准输出中
  * **GC其他参数**
    * -XX:+TraceClassLoading 监控类的加载
    * -XX:+PrintGCApplicationStoppedTime 打印GC时线程的停顿时间
    * -XX:+PrintGCApplicationConcurrentTime 垃圾收集之前打印未中断的执行时间
    * -XX:+PrintReferenceGC 记录回收了多少种不同引用类型的引用
    * -XX:+PrintTenuringDistributionMinorGC后打印当前Survivor中对象年龄分布
    * -XX:+UseGCLogFileRotation 启用GC日志文件的自动转储
    * -XX:NumberOfGClogFiles=1 GC日志文件的循环数目
    * -XX:GCLogFileSize=1M 控制GC日志文件的大小
  
* **其他参数**

  * -XX:+DisableExplicitGC 禁止hotspot执行System.gc()，默认禁用

  * -XX:ReservedCodeCacheSize=<n>[g|m|k]/-XX:lnitialCodeCacheSize=<n>[g|m|k] 指定代码缓存的大小

    -XX:+UseCodeCacheFlushing 让jvm放弃一些被编译的代码—避免代码缓存被占满时JVM切换到interpreted-only的情况

  * -XX:+DoEscapeAnalysis 开启逃逸分析

  * -XX:+UseBiasedLocking开启偏向锁

  * -XX:+UseLargePages 开启使用大页面

  * -XX:+UseTLAB 使用TLAB，默认打开

  * -XX:+PrintTLAB 打印TLAB的使用情况

  * -XX:TLABSize 设置TLAB大小

## 5.4 通过Java代码获取JVM参数

Java提供了**java.lang.management**包用于监视和管理JVM和Java运行时中的其他组件它允许本地和远程监控和管理运行的Java虚拟机。其中ManagementFactory这个类较常用。另外Runtime类也可以获取一些内存、CPU核数等相关的数据。通过这些api可以监控我们的应用服务器的堆内存使用情况，设置一些阈值进行报警等处理。

## 5.5 GC日志分析

HotSpot所有功能的日志都收归到了“-Xlog”参数

```
-Xlog [:[selector] [:[output][:[decorators][:output-options]]]]
```

![image-20210902163133503](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210902163133.png)

* **GC分类**：HotSpotVM——部分收集（Partial GC）和整堆收集（FullGC）

  * **部分收集（Partial GC）**：不是完整收集整个Java堆的垃圾收集。其中又分为：
    * 新生代收集（Minor GC/Young GC）：只是新生代（Eden\S0，S1）的垃圾收
    * 集老年代收集（Major GC/OldGC）：只是老年代的垃圾收集。
      * 目前，只有CMSGC会有单独收集老年代的行为。
      * **⚠️很多时候MajorGC会和FullGC混淆，需要具体分辨**。
    * 混合收集（MixedGC）：收集整个新生代以及部分老年代的垃圾收集。目前，只有G1 GC会有这种行为
  * **整堆收集**（FullGC）：收集整个java堆和方法区的垃圾收集。
    * 老年代空间不足、方法区空间不足、显示调用system.gc、会触发FullGC

* **GC日志分类**

  * MinorGC（或young GC或YGC）日志：

    ![image-20210824174414157](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824174414.png)

  * Full GC 日志 ：![image-20210824174507460](https://cdn.jsdelivr.net/gh/1065464173/markdown-image-rep/20210824174507.png)

* **GC日志结构剖析**：垃圾收集器、GC前后情况、GC时间(user，sys和real)
* **GC日志文件分析**：
  * 生成日志文件-Xloggc:d:/GcLogTest.log
  * 日志分析工具 GCEasy、GCViewer、GChisto、HPjmeter

# 第六章 OOM常见的场景和解决方案

# 第七章 性能优化案例
