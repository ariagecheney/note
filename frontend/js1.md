### 事件是可以被javascript检测到的行为
* 在事件值里直接写js代码  
onchange   文本内容改变事件  
onselect   文本内容选中事件  
onblur   光标移开事件  
onUnload   关闭网页事件  

* dom.property  dom API  
dom.style.property  css API  

* dom.add/remove EventListener();  推荐使用这种方式  ，且各事件不会相互覆盖

* 事件流  页面中接收事件的顺序  事件冒泡和捕获  常用冒泡

### 事件处理  (三种)
1. html  耦合
2. dom0 级
dom事件处理属性  onclick属性  
缺点 覆盖  
移除是赋值null  

3. dom2 级
 addEventListener（‘事件名注意去掉on’，‘事件处理函数’，‘true：事件捕获，false：冒泡’）  
 attachEvent/detachEvent("onclick",function)  ie8以下  


### 事件对象  (触发dom事件时产生一个对象)

* 属性方法  
  e.type  e.target  e.stopPropagation  e.preventDefault();


### dom
NodeList数组  每一个node都有很多属性  事件的this传入触发该事件的元素dom
ByName     dom数组
ByTagName  dom数组

getAttribute/set   dataset.attr    属性值大多为String  存取数据

dom.nodeType  数字  nodeName  大写String

表单dom 都有type value 属性

parent.insertBefore（dom, refChild）;

document.body.offsetHeight||document.documentElement.scrollHeight;   是否包括滚动条


### 模块化规范

* AMD --RequireJS     异步加载，依赖前置，提前执行     
* CMD --SeaJS      同步加载，依赖就近，延迟执行  

### Prototype模式的验证方法
为了配合prototype属性，Javascript定义了一些辅助方法，帮助我们使用它。  
* isPrototypeOf()  
这个方法用来判断，某个proptotype对象和某个实例之间的关系。  
　　alert(Cat.prototype.isPrototypeOf(cat1)); //true  
　　alert(Cat.prototype.isPrototypeOf(cat2)); //true  
* hasOwnProperty()  
每个实例对象都有一个hasOwnProperty()方法，用来判断某一个属性到底是本地属性，还是继承自prototype对象的属性。  
　　alert(cat1.hasOwnProperty("name")); // true  
　　alert(cat1.hasOwnProperty("type")); // false  
* in运算符
in运算符可以用来判断，某个实例是否含有某个属性，不管是不是本地属性。  
　　alert("name" in cat1); // true  
　　alert("type" in cat1); // true  
in运算符还可以用来遍历某个对象的所有属性。  
　　for(var prop in cat1) { alert("cat1["+prop+"]="+cat1[prop]); }

### js作用域  
* 函数级作用域（区别于块级作用域）  
内部的变量，内部都能访问，外部不能访问内部的变量，内部能访问外部的变量，若有相同的变量，采用按执行顺序，就近原则   
**js调用/访问前置,按顺序执行，但是先收集所有定义**  
* 闭包 (返回函数)  
使能访问到内部的变量（返回的函数返回了内部的变量） 容易造成内存泄露　　

* this 使用  指当前对象（环境） 谁调用者  闭包时则一般指向window  
由于 javascript的动态性（解释执行，当然也有简单的预编译过程），this的指向在运行时才确定。这个特性在给我们带来迷惑的同时也带来了编程上的 自由和灵活，结合apply(call)方法，可以使JS变得异常强大。
### js snipe
```js
var i = 100;
	~function(){
		console.log(i);
	}();
// 作用相当于（） 把后面变成表达式
```


* indexOf  

```javascript
表示从该位置开始向后匹配；对于lastIndexOf，表示从该位置起向前匹配。

"hello world".indexOf("o", 6)
// 7

"hello world".lastIndexOf("o", 6)
// 4
```

charAt方法返回一个字符串的给定位置的字符，位置从0开始编号。

var s = new String("abc");

s.charAt(1) // "b"
s.charAt(s.length-1) // "c"
这个方法完全可以用数组下标替代。

"abc"[1] // "b"

localeCompare  字符串方法 比较两个字母大小  ASCII码

split方法还可以接受第二个参数，限定返回数组的最大成员数。
"a|b|c".split("|", 2) // ["a", "b"]
"a|b|c".split("|", 3) // ["a", "b", "c"]
"a|b|c".split("|", 4) // ["a", "b", "c"]

如果分割规则为空字符串，则返回数组的成员是原字符串的每一个字符。

"a|b|c".split("")
// ["a", "|", "b", "|", "c"]

如果满足分割规则的两个部分紧邻着（即中间没有其他字符），则返回数组之中会有一个空字符串。

"a||c".split("|")
// ["a", "", "c"]

["a","b","c"].toString()
"a,b,c"

join方法以参数作为分隔符，将所有数组成员组成一个字符串返回。如果不提供参数，默认用逗号分隔。

var a = [1,2,3,4];

a.join() // "1,2,3,4"
a.join('') // '1234'
a.join("|") // "1|2|3|4"
通过函数的call方法，join方法（即Array.prototype.join）也可以用于字符串。

Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"

与搜索和替换相关的有4个方法，它们都允许使用正则表达式。

match：用于确定原字符串是否匹配某个子字符串，返回匹配的子字符串数组。
var matches = "cat, bat, sat, fat".match("at");

matches // ["at"]
matches.index // 1
matches.input // "cat, bat, sat, fat"
search：等同于match，但是返回值不一样。
"cat, bat, sat, fat".search("at")
// 1
replace：用于替换匹配的字符串。
replace方法用于替换匹配的子字符串，一般情况下只替换第一个匹配（除非使用带有g修饰符的正则表达式）。

"aaa".replace("a", "b")
// "baa"

split：将字符串按照给定规则分割，返回一个由分割出来的各部分组成的新数组。


Array.slice方法返回指定位置的数组成员组成的新数组，原数组不变。

Array.splice();
splice方法用于删除元素，并可以在被删除的位置添加入新的数组元素。它的返回值是被删除的元素。需要特别注意的是，该方法会改变原数组。

slice方法的一个重要应用，是将类似数组的对象转为真正的数组。

Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 })
// ['a', 'b']

Array.prototype.slice.call(document.querySelectorAll("div"));

Array.prototype.slice.call(arguments);
splice的第一个参数是删除的起始位置，第二个参数是被删除的元素个数。如果后面还有更多的参数，则表示这些就是要被插入数组的新元素。

var a = ["a","b","c","d","e","f"];

a.splice(4,2)
// ["e", "f"]

a
// ["a", "b", "c", "d"]
上面代码从原数组位置4开始，删除了两个数组成员。

var a = ["a","b","c","d","e","f"];

a.splice(4,2,1,2)
// ["e", "f"]

a
// ["a", "b", "c", "d", 1, 2]
上面代码除了删除成员，还插入了两个新成员。

如果只是单纯地插入元素，splice方法的第二个参数可以设为0。

var a = [1,1,1];

a.splice(1,0,2)
// []

a
// [1, 2, 1, 1]
如果只提供第一个参数，则实际上等同于将原数组在指定位置拆分成两个数组。

var a = [1,2,3,4];

a.splice(2)
// [3, 4]

a
// [1, 2]

window.open('http://www.sina.com','_blank');
这句代码的实际意思是把一个连接在一个指定的框架（frame）内打开，_self

,_top,_blank,这些是专有的框架名。

既然是表示浏览器的窗口，那就一定包含：

◆新建窗口（window.open()），

◆关闭窗口(window.close())，

◆改变窗口位置(resizeBy(),resizeTo())，

◆移动窗口（moveBy(),moveTo），

还有一些其固有的特性，比如：

◆打开新的连接，并可以指定打开方式

◆弹出系统对话框(alert(),confirm(),prompt())，

◆设置超时与暂停（setTimeout(),setInterval())，

◆状态栏，


然后再说一说让人迷惑的几个东东，parent,self,top,opener,，怎么样可区分清楚么？

其中self总是等于window，仅是名字不一样而已，不过正是由于这个特点，使用它可以使我们的代码更易于阅读，而top对象和parent对象，本人认为，只有在多框架(frames)下才会被用到，top对象指向最顶层的框架，也就是当一个页面使用了frame或iframe时，才会被用到。最后opener用于window.open()打开的子窗口。

document是一个既属于BOM又属于DOM的对象，而location对象，则是一个既属于window，又属于document的属性。从BOM的角度来看，document对象中包含了页面中一些通用的属性和集合，不过document中的很多属性(alinkColor,bgColor,fgColor,linkColor,vlinkColor)是可以通过css控制的，所以我的建议是能使用css控制的尽量使用css,而剩下的属性(lastModifie

d,referrer,title,URL),基本上没有多大的用处，要说有用的，我认为只有referrer可能有点用，它可以告诉你用户是怎么访问到你的页面的。其实document的主要作用是用于DOM。

location对象表示载入窗口的URL，同时还可以用于解析URL，比如要获得GET请求后的参数可以使用

location.search

简而言之，如果浏览器加载并执行脚本，会引起页面的渲染被暂停，甚至还会阻塞其他资源(比如图片)的加载。为了更快的给用户呈现网页内容，


// The .bind method from Prototype.js
Function.prototype.bind = function(){
var fn = this, args = Array.prototype.slice.call(arguments), object = args.shift();
return function(){
return fn.apply(object,
args.concat(Array.prototype.slice.call(arguments)));
};
};
对于那些不支持bind方法的老式浏览器，可以自行定义bind方法。

if(!('bind' in Function.prototype)){
    Function.prototype.bind = function(){
        var fn = this;
        var context = arguments[0];
        var args = Array.prototype.slice.call(arguments, 1);
        return function(){
            return fn.apply(context, args);
        }
    }
}

== Object.keys();可枚举，自身    Object.getOwnPropertyNames() 自身所有
Object.create(原型)继承   实例对象.hasOwnProperty("name"); 判断是否为自己的属性 ===

判断一个对象是否具有某个属性(忽略不可枚举)（不管是自身的还是继承的），使用in运算符。 for in


获得对象的所有属性（不管是自身的还是继承的，以及是否可枚举），可以使用下面的函数。

function inheritedPropertyNames(obj) {
  var props = {};
  while(obj) {
    Object.getOwnPropertyNames(obj).forEach(function(p) {
      props[p] = true;
    });
    obj = Object.getPrototypeOf(obj);
  }
  return Object.getOwnPropertyNames(props);
}
用法如下：

inheritedPropertyNames(Date)
// ["caller", "constructor", "toString", "UTC", "call", "parse", "prototype", "__defineSetter__", "__lookupSetter__", "length", "arguments", "bind", "__lookupGetter__", "isPrototypeOf", "toLocaleString", "propertyIsEnumerable", "valueOf", "apply", "__defineGetter__", "name", "now", "hasOwnProperty"]

### js中`!!`运算符
> `!! 表达式`类似于三元运算符，**目的是把其他类型的变量转化成boolean型	**当表达式的值是非空字符串、非零数字、返回true；当值是空字符串、数字0，null,false,undefined,NaN返回false
```
!! ""
false
!! "s"
true
!! "0"
true
!! 0
false
!! null
false
!! true
true
!! false
false
```

### 浏览器canvas检测
```javascript
//判断浏览区是否支持canvas
    function isSupportCanvas(){
        var elem = document.createElement('canvas');
        return !!(elem.getContext && elem.getContext('2d'));
    }
```
### js中`||`和`&&` 运算符
> 遵循短路原则，常用于判断逻辑，默认值赋值等
>
`2>3 && 5`返回值：false
>
eg:
成长速度为5显示1个箭头；
成长速度为10显示2个箭头；
成长速度为12显示3个箭头；
成长速度为15显示4个箭头；
其他都显示都显示0各箭头。
```
var add_level = (add_step==5 && 1) || (add_step==10 && 2) || (add_step==12 && 3) || (add_step==15 && 4) || 0;
//or
var add_level={'5':1,'10':2,'12':3,'15':4}[add_step] || 0;
```
eg:
成长速度为>12显示4个箭头；
成长速度为>10显示3个箭头；
成长速度为>5显示2个箭头；
成长速度为>0显示1个箭头；
成长速度为<=0显示0个箭头。
```
var add_level = (add_step>12 && 4) || (add_step>10 && 3) || (add_step>5 && 2) || (add_step>0 && 1) || 0;
```
