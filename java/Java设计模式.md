### 参考
* [http://blog.csdn.NET/zhangerqing](http://blog.csdn.net/zhangerqing/article/details/8194653)

### UML类图关系
* [UML类图关系 by 明明的天天](http://www.cnblogs.com/olvo/archive/2012/05/03/2481014.html)

### **重要提示！** 此文档只是学习Java设计模式时，自己做的简要笔记，非常简陋，可以与代码一同参考，代码地址: [github designpattern](https://github.com/pmzgit/parent/tree/master/common-util/src/test/java/designpattern)

## 一、 创建型
#### 工厂模式

1. 静态工厂方法模式
* 角色：产品类 实现 产品功能接口; 工厂类->多个静态生产方法 返回 产品功能接口(实现：不同方法 返回 对应的产品类实例)
```java
// 产品功能接口
public interface Sender {  
    public void Send();  
}  

public class FactoryTest {  
  
    public static void main(String[] args) {      
        Sender sender = SendFactory.produceMail();  
        sender.Send();  
    }  
}  
```
* 一般扩展场景：需要能够增加新的产品时。可以增加新的产品类（实现产品功能接口），修改工厂类（增加对应的静态生产方法）.
* 缺点：不符合开闭原则 
2. 抽象工厂模式
* 角色：产品类 实现 产品功能接口; 工厂类（多个工厂实现类，不同实现类 返回 对应的产品组合实例） 实现 生产产品工厂接口;
```java
// 生产产品接口
public interface Provider {  
    public Sender produce();  
}  

public class Test {  
  
    public static void main(String[] args) {  
        Provider provider = new SendMailFactory();  
        Sender sender = provider.produce();  
        sender.Send();  
    }  
}  
```
* 一般扩展场景：需要能够增加新的产品时。可以增加新的产品类（实现产品功能接口），增加新的工厂类（方法返回 为 新的产品 组合接口）.
* 缺点： 

#### 单例模式：保证在一个JVM中，该对象只有一个实例存在
1. 使用场景
* 需要全局共享，并唯一的数据和功能，例如：核心引擎
* 减少复杂类，大类的实例创建，减轻系统开销
2. 考虑多线程环境下的同步问题
```java
public class Singleton {  
  
    /* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */  
    private static Singleton instance = null;  
  
    /* 私有构造方法，防止被实例化 */  
    private Singleton() {  
    }  
  
    private static synchronized void syncInit() {  
        if (instance == null) {  
            instance = new SingletonTest();  
        }  
    }  
  //原因： jvm 中 分配空间，赋值 和 初始化 分开进行
    public static SingletonTest getInstance() {  
        if (instance == null) {  
            syncInit();  
        }  
        return instance;  
    }  
  
    /* 如果该对象被用于序列化，可以保证对象在序列化前后保持一致 */  
    public Object readResolve() {  
        return instance;  
    }  
}  
```
3. 用静态内部类的方式初始化单例（原因：jvm中类加载过程中，能够保证线程互斥）
```java
public class Singleton {  
  
    /* 私有构造方法，防止被实例化 */  
    private Singleton() {  
    }  
  
    /* 此处使用一个内部类来维护单例 */  
    private static class SingletonFactory {  
        private static Singleton instance = new Singleton();  
    }  
  
    /* 获取实例 */  
    public static Singleton getInstance() {  
        return SingletonFactory.instance;  
    }  
  
    /* 如果该对象被用于序列化，可以保证对象在序列化前后保持一致 */  
    public Object readResolve() {  
        return getInstance();  
    }  
}  

```
#### 原型模式 ：将一个**对象**作为原型，对其进行复制、克隆，产生一个和原对象类似的新对象
1. 原型类Prototype 
* 实现Cloneable接口（某些实现需要实现Serializable，例如利用对象流的方式实现深拷贝）
* 重写Object类中的clone方法，并设置作用域为public
2. 使用场景
* 原型模式创建对象比直接new一个对象在性能上要好的多,因为Object类的clone方法是一个本地方法，它直接操作内存中的二进制流，特别是复制大对象时，性能的差别非常明显。
* 深拷贝与浅拷贝，单纯的super.clone方法只是浅拷贝，即8种基本数据类型和他们的封装类。（要实现深拷贝，必须将原型模式中的数组、容器对象、引用对象等另行拷贝，或者在某些场景下进行特殊拷贝逻辑）

#### 适配器模式（Adapter）
1. 使用场景
* 类的适配器模式：当希望将一个类转换成满足另一个新接口的类时，可以使用类的适配器模式，创建一个新类，继承原有的类，实现新的接口即可。
* 对象的适配器模式：当希望将一个对象转换成满足另一个新接口的对象时，可以创建一个Wrapper类，持有原类的一个实例，在Wrapper类的方法中，调用实例的方法就行。
* 接口的适配器模式：当不希望实现一个接口中所有的方法时，可以创建一个抽象类Adapter（这样就能保证，这个类只能被继承，不能实例化），实现所有方法，我们写别的类的时候，继承抽象类即可（e g：spring中的WebMvcConfigurerAdapter）。

#### 装饰器模式（Decorator）: 装饰模式就是给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例
1. 装饰器模式的应用场景：  
* 需要扩展一个类的功能。
* 动态的为一个对象增加功能，而且还能动态撤销。（继承不能做到这一点，继承的功能是静态的，不能动态增删。）
2. 缺点：
* 产生过多相似的对象，不易排错！
3. 实现
* 实现原理与 **对象的适配器模式** 相似

#### 代理模式（Proxy）：代理模式就是多一个代理类出来，即给某一个对象提供一个代理, 并由代理对象控制对原对象的引用.
1. 参考：
* [动态代理 by 永顺](https://segmentfault.com/a/1190000007089902)
* [JDK的动态代理](http://blog.jobbole.com/104433/)
2. 静态代理：和装饰器模式类似,代理模式主要涉及三个角色:
* Subject: 抽象角色, 声明真实对象和代理对象的共同接口.
* Proxy: 代理角色, 它是真实角色的封装, 其内部持有真实角色的引用, 并且提供了和真实角色一样的接口, 因此程序中可以通过代理角色来操作真实的角色, 并且还可以附带其他额外的操作.
* RealSubject: 真实角色, 代理角色所代表的真实对象, 是我们最终要引用的对象.
* 原理：需要实现一个从不同存储介质(例如磁盘, 网络, 数据库)加载图片的功能, LoadImageProxy 是代理类, 它会将所有的接口调用都转发给实际的对象, 并从实际对象中获取结果. 因此我们在实例化 LoadImageProxy 时, 提供不同的实际对象时, 就可以实现从不同的介质中读取图片的功能了.
3. 动态代理





## 二、行为型
#### 策略模式（strategy）
* 策略模式定义了一系列算法，并将每个算法封装起来，使他们可以相互替换，且算法的变化不会影响到使用算法的客户。需要设计一个接口，为一系列实现类提供统一的方法，多个实现类实现该接口。
