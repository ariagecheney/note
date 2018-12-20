# [转载自一书生VOID](https://lw900925.github.io/)

## 默认方法，静态方法
* java 可以单继承，多实现，但是在实现多个接口时（接口之间不存在继承关系），多个接口中都存在相同签名的 default 方法时，此时如果需要调用冲突的默认方法时：  
覆写存在歧义的方法，并可以使用 InterfaceName.super.methodName(); 的方式手动调用需要的接口默认方法。
* 当实现类或抽象类实现接口时，子类中与默认方法发生冲突的规则是：  
类的方法声明优先于接口默认方法，无论该方法是具体的还是抽象的,就是@override
* 接口默认方法不能覆写 Object 类的 equals、hashCode 和 toString 方法。
* 虽然 Java 8 的接口的默认方法就像抽象类，能提供方法的实现，但是他们俩仍然是 不可相互代替的：

    * 接口可以被类多实现（被其他接口多继承），抽象类只能被单继承。

    * 接口中没有 this 指针，没有构造函数，不能拥有实例字段（实例变量）或实例方法，无法保存 状态（state），抽象方法中可以。

    * 接口的方法修饰符只有 public 的，这明显也不能满足我们的日常设计
    * 抽象类不能在 java 8 的 lambda 表达式中使用。

* 接口中的静态方法必须是 public 的，public 修饰符可以省略，static 修饰符不能省略。
## [函数式编程初探](http://www.ruanyifeng.com/blog/2012/04/functional_programming.html) 
## [Lambda表达式](https://lw900925.github.io/java/java8-lambda-expression.html)
* 仅有一个抽象方法的接口称为函数式接口
* 以Lambda表达式的方式为函数式接口提供实现。所以可以认为：整个Lambda表达式作为接口的实现类
* 方法引用是Lambda表达式的简便写法.如果你的Lambda表达式只是调用一个方法，最好使用名称调用，而不是描述如何调用，这样可以提高代码的可读性。方法引用使用::分隔符，分隔符的前半部分表示引用类型，后面半部分表示引用的方法名称。例如：Integer::compareTo表示引用类型为Integer，引用名称为compareTo的方法。
## [Stream API](https://lw900925.github.io/java/java8-stream-api.html)
* 流创建后，中间操作的时候，可以更改数据源，但执行终点操作的时候,不应该更改数据源。
* 大部分流的操作的参数都是函数式接口,所以Lambda表达式的主体实现应该是无状态的，否则在多线程环境下执行的结果是不确定的。
* 副作用指的是行为参数(Lambda 表达式)在执行的时候有输入输入，比如网络输入输出等。这是因为Java不保证这些副作用对其它线程可见，也不保证相同流管道上的同样的元素的不同的操作运行在同一个线程中。
## [Optional类](https://lw900925.github.io/java/java8-optional.html)
## [新的时间和日期API](https://lw900925.github.io/java/java8-newtime-api.html)