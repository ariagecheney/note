## java format 
* %[argument_index$][flags][width][.precision]conversion
* [javadoc](https://docs.oracle.com/javase/1.5.0/docs/api/java/util/Formatter.html#syntax)
* [ref](https://dzone.com/articles/java-string-format-examples)

## jar 
* jar命令  
有生成、查看、更新、解开jar包的作用，包含META-INF/MANIFEST.MF文件。
它是jar包生成的时候，自动创建的，主要负责指定jar包的main文件位置和当前文件夹。
```sh
jar --help
Usage: jar {ctxui}[vfmn0PMe] [jar-file] [manifest-file] [entry-point] [-C dir] files ...
Options:
    -c  create new archive
    -t  list table of contents for archive
    -x  extract named (or all) files from archive
    -u  update existing archive
    -v  generate verbose output on standard output
    -f  specify archive file name
    -m  include manifest information from specified manifest file
    -n  perform Pack200 normalization after creating a new archive
    -e  specify application entry point for stand-alone application 
        bundled into an executable jar file
    -0  store only; use no ZIP compression
    -P  preserve leading '/' (absolute path) and ".." (parent directory) components from file names
    -M  do not create a manifest file for the entries
    -i  generate index information for the specified jar files
    -C  change to the specified directory and include the following file
If any file is a directory then it is processed recursively.
The manifest file name, the archive file name and the entry point name are
specified in the same order as the 'm', 'f' and 'e' flags.

Example 1: to archive two class files into an archive called classes.jar: 
       jar cvf classes.jar Foo.class Bar.class 
Example 2: use an existing manifest file 'mymanifest' and archive all the
           files in the foo/ directory into 'classes.jar': 
       jar cvfm classes.jar mymanifest -C foo/ .


```
## [java 反编译 之JAD ](https://varaneckas.com/jad/)
* 下载  
`wget https://varaneckas.com/jad/jad158e.linux.intel.zip`
`unzip jad158e.linux.intel.zip -d jad`
* 安装
无需安装，直接运行
`./jad `  
	1. show error:    
./jad: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory   
fix： `yum install glibc.i686`  
	2. show error:  
./jad: error while loading shared libraries: libstdc++-libc6.2-2.so.3: cannot open shared object file: No such file or directory  
fix:
`yum -y install compat-libstdc++-296`
* 使用  
`jad -o -r -sjava -dsrc 'tree/**/*.class' `

## [java 反编译之CFR](http://www.benf.org/other/cfr/)
* `wget http://www.benf.org/other/cfr/cfr_0_129.jar`
* `java -jar cfr_0_129.jar --help`
* class  
`java -jar ../pac/cfr_0_129.jar ChildController.class > ChildController.java
`
* jar  
`java -jar cfr_0_129.jar /path/to/aa.jar --outputdir /path/to/aa`

## [java 反编译之 在线](http://www.javadecompilers.com/)
## [Java封箱拆箱的一些问题](https://www.cnblogs.com/xiaozhang2014/p/5347407.html#undefined)

## [JAVA不可变类(immutable)机制与String的不可变性](https://www.cnblogs.com/jaylon/p/5721571.html)

## java equals and hashcode
### 重写equals必须注意：
*   1 自反性：对于任意的引用值x，x.equals(x)一定为true
*   2  对称性：对于任意的引用值x 和 y，当x.equals(y)返回true，y.equals(x)也一定返回true
*   3 传递性：对于任意的引用值x、y和ｚ，如果x.equals(y)返回true，并且y.equals(z)也返回true，那么x.equals(z)也一定返   回 true
* 4 一致性：对于任意的引用值x 和 y，如果用于equals比较的对象信息没有被修改，多次调用x.equals(y)要么一致地返回true，要么一致地返回false
*   5 非空性：对于任意的非空引用值x，x.equals(null)一定返回false
### [7.编写一个完美equals()的几点建议](https://blog.csdn.net/javazejian/article/details/51348320)

下面给出编写一个完美的equals方法的建议（出自Java核心技术 第一卷：基础知识）：

1) 显式参数命名为otherObject,稍后需要将它转换成另一个叫做other的变量（参数名命名，强制转换请参考建议5）

2) 检测this与otherObject是否引用同一个对象 ：if(this == otherObject) return true;（存储地址相同，肯定是同个对象，直接返回true）

3) 检测otherObject是否为null ，如果为null,返回false.if(otherObject == null) return false;

4) 比较this与otherObject是否属于同一个类 （视需求而选择）

	* 如果equals的语义在每个子类中有所改变，就使用getClass检测 ：if(getClass()!=otherObject.getClass()) return false; (参考前面分析的第6点)

	* 如果所有的子类都拥有统一的语义，就使用instanceof检测 ：if(!(otherObject instanceof ClassName)) return false;（即前面我们所分析的父类car与子类bigCar混合比，我们统一了批次相同即相等）

5) 将otherObject转换为相应的类类型变量：ClassName other = (ClassName) otherObject;

6) 现在开始对所有需要比较的域进行比较 。使用==比较基本类型域，使用equals比较对象域。如果所有的域都匹配，就返回true，否则就返回flase。

	* 如果在子类中重新定义equals，就要在其中包含调用super.equals(other)

当此方法被重写时，通常有必要重写 hashCode 方法，以维护 hashCode 方法的常规协定，该协定声明 相等对象必须具有相等的哈希码 。

###  在java中进行比较，我们需要根据比较的类型来选择合适的比较方式：

     1) 对象域，使用equals方法 。 
       2) 类型安全的枚举，使用equals或== 。 
      3) 可能为null的对象域 : 使用 == 和 equals 。 
     4) 数组域 : 使用 Arrays.equals 。 
     5) 除float和double外的原始数据类型 : 使用 == 。 
     6) float类型: 使用Float.foatToIntBits转换成int类型，然后使用==。  
      7) double类型: 使用Double.doubleToLongBit转换成long类型，然后使用==。

### jdk实现 hashcode 
```java
# Objects.hash(values)
public static int hash(Object... values) {
        return Arrays.hashCode(values);
    }
# Arrays.hashCode(values)
public static int hashCode(Object a[]) {
        if (a == null)
            return 0;

        int result = 1;

        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());

        return result;
    }
```

## [java string replace](https://blog.csdn.net/wangpeng047/article/details/8985236)

```txt
1. replace的参数是char和CharSequence，即可以支持字符的替换，也支持字符串的替换（CharSequence即字符串序列的意思,说白了也就是字符串）；

replaceAll的参数是regex，即基于规则表达式的替换，比如：可以通过replaceAll("\\d", "*")把一个字符串所有的数字字符都换成星号；

相同点：都是全部替换，即把源字符串中的某一字符或字符串全部换成指定的字符或字符串；

不同点：replaceAll支持正则表达式，因此会对参数进行解析（两个参数均是），如replaceAll("\\d", "*")，而replace则不会，replace("\\d","*")就是替换"\\d"的字符串，而不会解析为正则。

2. “\”在java中是一个转义字符，所以需要用两个代表一个。例如System.out.println( "\\" ) ;只打印出一个"\"。但是“\”也是正则表达式中的转义字符，需要用两个代表一个。所以：\\\\被java转换成\\，\\又被正则表达式转换成\，因此用replaceAll替换“\”为"\\"，就要用replaceAll("\\\\","\\\\\\\\")，而replace则replace("\\","\\\\")。

3. 如果只想替换第一次出现的，可以使用replaceFirst()，这个方法也是基于规则表达式的替换，但与replaceAll()不同的是，只替换第一次出现的字符串。

```

## tomcat servlet-api 版本
* http://tomcat.apache.org/whichversion.html
## ocr 识别
#### tesseract-ocr
* tesseract --list-langs
* tesseract imagename outputbase [-l lang] [-psm pagesegmode] [configfile...]  

### 流
* 流的本质是数据传输，根据数据传输特性将流抽象为各种类，方便更直观的进行数据操作。

* IO流的分类  
根据处理数据类型的不同分为：字符流和字节流  
根据数据流向不同分为：输入流和输出流  
基于磁盘操作的I/O接口：File  
基于网络操作的I/O接口：Socket（不在java.io包下）

* 影响IO性能的无非就是两大因素：数据的格式及存储的方式，字节流和字符流主要是数据格式方面的，后两个类是存储方式方面的：本地和网络。

* 字符流和字节流  
字符流的由来： 因为数据编码的不同，而有了对字符进行高效操作的流对象。本质其实就是基于字节流读取时，去查了指定的码表。   
字节流和字符流的区别：读写单位不同：字节流以字节（8bit）为单位，字符流以字符为单位，根据码表映射字符，一次可能读多个字节。  
处理对象不同：字节流能处理所有类型的数据（如图片、avi等），而字符流只能处理字符类型的数据。  
结论：只要是处理纯文本数据，就优先考虑使用字符流。 除此之外都使用字节流.是否需要按照指定的编码表，将数据存到文本,则选择InputStreamReader  和 OutputStreamWriter

* 输入流和输出流  
对输入流只能进行读操作，对输出流只能进行写操作，程序中需要根据待传输数据的不同特性而使用不同的流。


* 输入字节流InputStream
    1. InputStream 是所有的输入字节流的父类，它是一个抽象类。  
    2. ByteArrayInputStream、StringBufferInputStream、FileInputStream 是三种基本的介质流，它们分别从Byte 数组、StringBuffer、和本地文件中读取数据。PipedInputStream 是从与其它线程共用的管道中读取数据，与Piped 相关的知识后续单独介绍。
    3. ObjectInputStream 和所有FilterInputStream 的子类都是装饰流（装饰器模式的主角）。


* 输出字节流OutputStream
    1. OutputStream 是所有的输出字节流的父类，它是一个抽象类。
    2. ByteArrayOutputStream、FileOutputStream 是两种基本的介质流，它们分别向Byte 数组、和本地文件中写入数据。PipedOutputStream 是向与其它线程共用的管道中写入数据，
    3. ObjectOutputStream 和所有FilterOutputStream 的子类都是装饰流。


* LineNumberInputStream 主要完成从流中读取数据时，会得到相应的行号，至于什么时候分行、在哪里分行是由该类主动确定的，并不是在原始中有这样一个行号。在输出部分没有对应的部分，我们完全可以自己建立一个LineNumberOutputStream，在最初写入时会有一个基准的行号，以后每次遇到换行时会在下一行添加一个行号，看起来也是可以的。好像更不入流了。
* PushbackInputStream 的功能是查看最后一个字节，不满意就放入缓冲区。主要用在编译器的语法、词法分析部分。输出部分的BufferedOutputStream 几乎实现相近的功能。
* StringBufferInputStream 已经被Deprecated，本身就不应该出现在InputStream 部分，主要因为String 应该属于字符流的范围。已经被废弃了，当然输出部分也没有必要需要它了！还允许它存在只是为了保持版本的向下兼容而已。
* SequenceInputStream 可以认为是一个工具类，将两个或者多个输入流当成一个输入流依次读取。完全可以从IO 包中去除，还完全不影响IO 包的结构，却让其更“纯洁”――纯洁的Decorator 模式。
* PrintStream 也可以认为是一个辅助工具。主要可以向其他输出流，或者FileInputStream 写入数据，本身内部实现还是带缓冲的。本质上是对其它流的综合运用的一个工具而已。一样可以踢出IO 包！System.out 和System.out 就是PrintStream 的实例！


* 字符输入流Reader  
1. Reader 是所有的输入字符流的父类，它是一个抽象类。
2. CharArrayReader、StringReader 是两种基本的介质流，它们分别将Char 数组、String中读取数据。PipedReader 是从与其它线程共用的管道中读取数据。
3. BufferedReader 很明显就是一个装饰器，它和其子类负责装饰其它Reader 对象。
4. FilterReader 是所有自定义具体装饰流的父类，其子类PushbackReader 对Reader 对象进行装饰，会增加一个行号。
5. InputStreamReader 是一个连接字节流和字符流的桥梁，它将字节流转变为字符流。FileReader 可以说是一个达到此功能、常用的工具类，在其源代码中明显使用了将FileInputStream 转变为Reader 的方法。我们可以从这个类中得到一定的技巧。Reader 中各个类的用途和使用方法基本和InputStream 中的类使用一致

* 字符输出流Writer  
1. Writer 是所有的输出字符流的父类，它是一个抽象类。
2. CharArrayWriter、StringWriter 是两种基本的介质流，它们分别向Char 数组、String 中写入数据。PipedWriter 是向与其它线程共用的管道中写入数据，
3. BufferedWriter 是一个装饰器为Writer 提供缓冲功能。
4. PrintWriter 和PrintStream 极其类似，功能和使用也非常相似。
5. OutputStreamWriter 是OutputStream 到Writer 转换的桥梁，它的子类FileWriter 其实就是一个实现此功能的具体类（具体可以研究一SourceCode）。功能和使用和OutputStream 极其类似，后面会有它们的对应图。

* 字符流与字节流转换
* InputStreamReader:字节到字符的桥梁
* OutputStreamWriter:字符到字节的桥梁
* 这两个流对象是字符体系中的成员，它们有转换作用，本身又是字符流，所以在构造的时候需要传入字节流对象进来。

* RandomAccessFile类  
该对象并不是流体系中的一员，其封装了字节流，同时还封装了一个缓冲区（字符数组），通过内部的指针来操作字符数组中的数据。 该对象特点：  
    1. 该对象只能操作文件，所以构造函数接收两种类型的参数：a.字符串文件路径；b.File对象。
    2. 该对象既可以对文件进行读操作，也能进行写操作，在进行对象实例化时可指定操作模式(r,rw)
    3. 注意：该对象在实例化时，如果要操作的文件不存在，会自动创建；如果文件存在，写数据未指定位置，会从头开始写，即覆盖原有的内容。 可以用于多线程下载或多个线程同时写数据到文件。  

* 一个IO操作其实分成了两个步骤：发起IO请求和实际的IO操作。  
    1. 同步IO和异步IO的区别就在于第二个步骤是否阻塞，如果实际的IO读写阻塞请求进程，那么就是同步IO。
    2. 阻塞IO和非阻塞IO的区别在于第一步，发起IO请求是否会被阻塞，如果阻塞直到完成那么就是传统的阻塞IO，如果不阻塞，那么就是非阻塞IO。

* 同步和异步是针对应用程序和内核的交互而言的，同步指的是用户进程触发IO操作并等待或者轮询的去查看IO操作是否就绪，而异步是指用户进程触发IO操作以后便开始做自己的事情，而当IO操作已经完成的时候会得到IO完成的通知。
* 而阻塞和非阻塞是针对于进程在访问数据的时候，根据IO操作的就绪状态来采取的不同方式，说白了是一种读取或者写入操作函数的实现方式，阻塞方式下读取或者写入函数将一直等待，而非阻塞方式下，读取或者写入函数会立即返回一个状态值。
所以,IO操作可以分为3类：同步阻塞（即早期的IO操作）、同步非阻塞（NIO）、异步（AIO）。
1. 同步阻塞：  
    在此种方式下，用户进程在发起一个IO操作以后，必须等待IO操作的完成，只有当真正完成了IO操作以后，用户进程才能运行。JAVA传统的IO模型属于此种方式。

2. 同步非阻塞：  
    在此种方式下，用户进程发起一个IO操作以后边可返回做其它事情，但是用户进程需要时不时的询问IO操作是否就绪，这就要求用户进程不停的去询问，从而引入不必要的CPU资源浪费。其中目前JAVA的NIO就属于同步非阻塞IO。
3. 异步：  
    此种方式下是指应用发起一个IO操作以后，不等待内核IO操作的完成，等内核完成IO操作以后会通知应用程序。

### java NIO原理及通信模型  
    1. 由一个专门的线程来处理所有的 IO 事件，并负责分发。
    2. 事件驱动机制：事件到的时候触发，而不是同步的去监视事件。
    3. 线程通讯：线程之间通过 wait,notify 等方式通讯。保证每次上下文切换都是有意义的。减少无谓的线程切换。
* 具体可参考：[逸情公子](http://weixiaolu.iteye.com/blog/1479656)

### 多线程
* [Java核心技术点之多线程 by absfree](http://www.jianshu.com/p/4b0721633009)

### Socket 编程
* 概述  
socket通信是大家耳熟能详的一种进程间通信方式(IPC)，它是一种全双工的通信方式，不同于pipe这种单工方式.这篇文章将深入浅出的讲解一下什么是socket。
我们常说的socket通信有以下二种,主要会说一下Unix domain socket

* Internet domain socket  
该socket可以用于不同主机间的通信，就像聊QQ一样只要知道了对方的QQ号就可以聊天了。socket只要知道了对方的ip地址和端口就可以通信了所以这种socket通信是基于网络协议栈的。

* Unix domain socket  
该socket用于一台主机的进程间通信，不需要基于网络协议，主要是基于文件系统的。与Internet domain socket类似，需要知道是基于哪一个文件（相同的文件路径）来通信的
unix domain socket有2种工作模式一种是SOCK_STREAM，类似于TCP，可靠的字节流。另一种是SOCK_DGRAM，类似于UDP，不可靠的字节流。

* 工作模型  
socket通信有一个服务端，一个客服端
服务端：创建socket—绑定文件（端口）—监听—接受客户端连接—接收/发送数据—…—关闭
客户端：创建socket—绑定文件（端口）—连接—发送/接收数据—…—关闭
#### 参考:  

* [Unix Socket by Hly_Coder](http://www.jianshu.com/p/d4bb6d4f8e4c)  



参考以下文章：  
[残夜](http://www.cnblogs.com/oubo/archive/2012/01/06/2394638.html)
## tomcat 参数配置
* ide 中  
//-Xms1024m -Xmx2048m -XX:MaxNewSize=512m -XX:MaxPermSize=512m  
//-Xms128m -Xmx128m -XX:MaxNewSize=32m -XX:MaxPermSize=32m
* tomcat [参考](http://outofmemory.cn/c/java-outOfMemoryError)  
`JAVA_OPTS="-server -XX:PermSize=512M -XX:MaxPermSize=512m"`
* tomcat   
//URIEncoding="UTF-8"


/*3.变量的运算：
   ①自动类型转换：容量小的数据类型自动转换为容量大的数据类型。
    short s = 12;
     int i = s + 2;
    注意：byte  short char之间做运算，结果为int型！
   ②强制类型转换：是①的逆过程。使用“（）”实现强转。
*/
short s = 10;
s = s + 5;//报编译的异常
s = (short)(s + 5);
s += 5;//s = s + 5，但是结果不会改变s的数据类型。

//2.空指针的异常：NullPointerException
		//第一种：
//		boolean[] b = new boolean[3];
//		b = null;
//		System.out.println(b[0]);

		//第二种：
//		String[] str = new String[4];
//		//str[3] = new String("AA");//str[3] = "AA";
//		System.out.println(str[3].toString());

		//第三种：
		int[][] j = new int[3][];
		j[2][0] = 12;
数组的反转：
// 数组元素的反转
// for(int i = 0;i < arr.length/2;i++){
// int temp = arr[i];
// arr[i] = arr[arr.length-1 - i];
// arr[arr.length - 1 - i] = temp;
// }
for (int x = 0, y = arr.length - 1; x < y; x++, y--) {
	int temp = arr[x];
	arr[x] = arr[y];
	arr[y] = temp;
}
// 使用冒泡排序使数组元素从小到大排列
//		for (int i = 0; i < arr.length - 1; i++) {
//			for (int j = 0; j < arr.length - 1 - i; j++) {
//				if (arr[j] > arr[j + 1]) {
//					int temp = arr[j];
//					arr[j] = arr[j + 1];
//					arr[j + 1] = temp;
//				}
//			}
//		}

//使用直接选择排序使数组元素从小到大排列
//		for(int i = 0; i < arr.length - 1; i++){
//			int t = i;//默认i处是最小的
//			for(int j = i;j < arr.length;j++){
//				//一旦在i后发现存在比其小的元素，就记录那个元素的下角标
//				if(arr[t] > arr[j]){
//					t = j;
//				}
//			}
//			if(t != i){
//				int temp = arr[t];
//				arr[t] = arr[i];
//				arr[i] = temp;
//			}
//		}
还可以调用：Arrays工具类：Arrays.sort(arr);

public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());

    }
@Override
	public String toString() {
		return "Season [seasonName=" + seasonName + ", seasonDesc="
				+ seasonDesc + "]";
	}
Set<Map.Entry<String,Integer>> set = map.entrySet();
	for(Map.Entry<String,Integer> o : set){
		System.out.println(o.getKey() + "--->" + o.getValue());
	}

# 枚举类: 类的对象是有限个的，确定的  
1. 如何自定义枚举类。  
   1.1 私有化类的构造器，保证不能在类的外部创建其对象  
   1.2 在类的内部创建枚举类的实例。声明为：public static final  
   1.3 若类有属性，那么属性声明为：private final 。此属性在构造器中赋值。  
2. 使用enum关键字定义枚举类  
2.1其中常用的方法：values()  valueOf(String name);  
2.2枚举类如何实现接口   
①让类实现此接口，类的对象共享同一套接口的抽象方法的实现。  
①让类的每一个对象都去实现接口的抽象方法，进而通过类的对象调用被重写的抽象方法时，执行的效果不同  
```java
public class TestSeason1 {
	public static void main(String[] args) {
		Season1 spring = Season1.SPRING;
		System.out.println(spring);
		spring.show();
		System.out.println(spring.getSeasonName());

		System.out.println();
		//1.values()
		Season1[] seasons = Season1.values();
		for(int i = 0;i < seasons.length;i++){
			System.out.println(seasons[i]);
		}
		//2.valueOf(String name):要求传入的形参name是枚举类对象的名字。
		//否则，报java.lang.IllegalArgumentException异常
		String str = "WINTER";
		Season1 sea = Season1.valueOf(str);
		System.out.println(sea);
		System.out.println();

		Thread.State[] states = Thread.State.values();
		for(int i = 0;i < states.length;i++){
			System.out.println(states[i]);
		}
		sea.show();

	}
}
interface Info{
	void show();
}
//枚举类
enum Season1 implements Info{
	SPRING("spring", "春暖花开"){
		public void show(){
			System.out.println("春天在哪里？");
		}
	},
	SUMMER("summer", "夏日炎炎"){
		public void show(){
			System.out.println("生如夏花");
		}
	},
	AUTUMN("autumn", "秋高气爽"){
		public void show(){
			System.out.println("秋天是用来分手的季节");
		}
	},
	WINTER("winter", "白雪皑皑"){
		public void show(){
			System.out.println("冬天里的一把火");
		}
	};

	private final String seasonName;
	private final String seasonDesc;

	private Season1(String seasonName,String seasonDesc){
		this.seasonName = seasonName;
		this.seasonDesc = seasonDesc;
	}
	public String getSeasonName() {
		return seasonName;
	}
	public String getSeasonDesc() {
		return seasonDesc;
	}

	@Override
	public String toString() {
		return "Season [seasonName=" + seasonName + ", seasonDesc="
				+ seasonDesc + "]";
	}
//	public void show(){
//		System.out.println("这是一个季节");
//	}
}
```

二、注解Annotation
1.JDK提供的常用的三个注解
@Override: 限定重写父类方法, 该注释只能用于方法
@Deprecated: 用于表示某个程序元素(类, 方法等)已过时
@SuppressWarnings: 抑制编译器警告

2.如何自定义注解
以SuppressWarnings为例进行创建即可

3.元注解：可以对已有的注解进行解释说明。
Retention: SOURCE   CLASS  RUNTIME
Target:
Documented:javadoc
Inherited



//spring
/*
@RequestMapping(params={"uname=小明"}) --编码问题
*/




/*
	Web监听器
	用于监听ServletContext，httpSession，ServletRequest等域对象的创建和销毁事件
	监听域对象属性发生修改的事件
	可以在事件发生前，后做一些操作
	优先级  监听器》过滤器》Servlet 其中监听器和过滤器都是按照注册顺序作为启动顺序
	按监听器对象分类    监听应用程序环境对象ServletContext的事件监听器，用户会话对象HttpSession,请求消息对象ServletRequest
	按监听事件分类    监听域对象自身的创建和销毁，域对象中属性的增加和删除，监听HttpSession域中某对象状态
	一对多  参考js事件模型
	 dom 域对象；
	 事件 创建和销毁
	 事件处理  各实现接口listener的方法
	 事件对象 方法参数
	 作用：定时器   全局属性对象
	 HttpSessionListener  统计在线人数  访问日志记录 sessionConfig
	 ServletRequestListener  读取参数  记录访问历史  先Requestinit  sessioncreate  Requestdestroy

	 以及域对象中各属性的增加，删除和替换注意要和域对象的生命周期结合使用

	 session钝化机制：把服务器中不经常使用的session对象暂时序列化到系统文件或数据库中，当被使用时反序列化到内存中，整个过程有服务器自动完成
	 钝化后的文件保存在：安装路径apache-tomcat-8.0.24\work\Catalina\localhost\项目名\SESSION.ser 当重新启动时，加载并删除
	 session钝化机制由sessionManager管理
	 standardManager和persistentManager默认情况下tomcat提供两个钝化驱动类FileStor  和JDBCStore

	 该监听器不需要在web.xml中注册，且是普通的javabean去继承HttpSessionBindingListener
	 httpsessionactivationListener 以及实现  serialation接口
*/
/*
-Xms2048m -Xmx4096m -XX:MaxNewSize=512m -XX:MaxPermSize=512m

URIEncoding="UTF-8"
 */

 ## 内部类
 ```java
 public interface Father {

 }

 public interface Mother {

 }

 public class Son implements Father, Mother {

 }

 public class Daughter implements Father{

     class Mother_ implements Mother{

     }
 }
 ```
> 其实对于这个实例我们确实是看不出来使用内部类存在何种优点，但是如果Father、Mother不是接口，而是抽象类或者具体类呢？这个时候我们就只能使用内部类才能实现多重继承了。
其实使用内部类最大的优点就在于它能够非常好的解决多重继承的问题，但是如果我们不需要解决多重继承问题，那么我们自然可以使用其他的编码方式，但是使用内部类还能够为我们带来如下特性（摘自《Think in java》）：

      1、内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立。

      2、在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。

      3、创建内部类对象的时刻并不依赖于外围类对象的创建。

      4、内部类并没有令人迷惑的“is-a”关系，他就是一个独立的实体。

      5、内部类提供了更好的封装，除了该外围类，其他类都不能访问。


# java 引用
* https://www.cnblogs.com/absfree/p/5555687.html

# commons-pool2
* https://throwsnew.com/2017/06/12/commons-pool/
* https://zhuanlan.zhihu.com/p/36216932

# 远程调试
* https://www.jianshu.com/p/f902ac5d29e4
