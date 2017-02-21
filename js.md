# 判断一个变量是否为对象

```javascript
function isObject(o){
  return o = Object(o);
}
isObject([]) // true
isObject(true) // false
```

# 枚举对象属性、计算对象属性个数

```javascript
// 枚举
var a = ["hello","world"];
Object.keys(a);
// ["0", "1"] 不包含 不可枚举的属性
Object.getOwnPropertyNames(a);
// ["0", "1", "length"]
// 计算属性个数
Object.keys(a).length // 2
Object.getOwnPropertyNames(a).length // 3
```

# [].splice(startIndex,countToRemove,newEleToInsert);

```javascript
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
```

# 判断某对象是不是数组

```javascript
var a = [];
typeof a; // object
Array.isArray(a); // true
```

# [].toString()方法返回数组的字符串形式。

```javascript
var a = [1, 2, 3];
a.toString() // "1,2,3"

var a = [1, 2, 3, [4, 5, 6]];
a.toString() // "1,2,3,4,5,6"
```

# 字符串和数组 转换

```javascript
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

如果数组成员是undefined或null或空位，会被转成空字符串。

[undefined, null].join('#')
// '#'

['a',, 'b'].join('-')
// 'a--b'
通过函数的call方法，join方法（即Array.prototype.join）也可以用于字符串。

Array.prototype.join.call('hello', '-')
// "h-e-l-l-o"
```

# [].shift() 和 [].unshift()

```javascript
shift()
shift方法用于删除数组的第一个元素，并返回该元素。注意，该方法会改变原数组。

var a = ['a', 'b', 'c'];

a.shift() // 'a'
a // ['b', 'c']
shift方法可以遍历并清空一个数组。

var list = [1, 2, 3, 4, 5, 6];
var item;

while (item = list.shift()) {
  console.log(item);
}

list // []
push和shift结合使用，就构成了“先进先出”的队列结构（queue）。

unshift()
unshift方法用于在数组的第一个位置添加元素，并返回添加新元素后的数组长度。注意，该方法会改变原数组。

var a = ['a', 'b', 'c'];

a.unshift('x'); // 4
a // ['x', 'a', 'b', 'c']
unshift方法可以在数组头部添加多个元素。

var arr = [ 'c', 'd' ];
arr.unshift('a', 'b') // 4
arr // [ 'a', 'b', 'c', 'd' ]
```
