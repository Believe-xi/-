# 你不知道的js

### 块级作用域

#### let

let关键字可以将变量绑定在其所在的任意作用域（{}），let隐式的为变量声明了作用域。且其不会想var一下进行变量提升，所以不能在let声明变量之前使用变量，因为声明变量之前还没有进行声明。

#### const

const和let一样可以为变量绑定作用域、但是其绑定的值是常量，是不能被改变的。

### 变量提升

var和函数声明都会有变量提升，先进行var变量声明再进行函数声明

### 作用域闭包

闭包就是函数的作用域中调用其他的函数作用域。导致调用的作用域无法被垃圾回收，形成了私有变量等。



## this和原型对象

this在任何情况下都不指向词法作用域，也不指向自身。this:执行上下文

### this绑定类型

1. 默认绑定： 函数直接调用，它的this会指向window，在非严格模式下
2. 隐式绑定： obj.fn() 这种隐形的绑定
3. 显性绑定：apply、call、bind。
4. new绑定： . 创建一个变量 ..把函数的原型指向变量的prototype  ... 新对象绑定到函数调用的this执行函数  .... 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象

### 间接引用

```javascript
function foo(){ console.log(this)}
const obj = {foo}
const obj1 = {foo}
(obj.foo = obj.foo)() // log: window
赋值返回的是应用值、所以是函数的引用被默认调用
```

### 被忽略的this

```javascript
foo.apply(null,1) // window
foo.bind() // window
let toNull = Object.create(null) // 空对象
可以给函数绑定空对象
```

### 对象类型的属性访问和键访问

```javascript
Obj = {a : 2}
obj.a // 2 属性访问
obj['a'] // 键访问
属性访问一定是连续字符串、键访问可以是任意值，然后会转换成字符串然后再进行访问
var myObject = { };
myObject[true] = "foo";
myObject[3] = "bar";
myObject[myObject] = "baz";
myObject["true"]; // "foo"
myObject["3"]; // "bar"
myObject["[object Object]"]; // "baz
```

### 深拷贝和浅拷贝

```javascript
深拷贝： 引用类型的地址不同
浅拷贝： 引用类型的地址相同
Object.assign(..) 就是使用 = 操作符来赋值
如果深拷贝中有循环引用就会陷入死循环
列如两个对象相互引用进行深拷贝就不上那么容易拷贝了，可以把对象进行序列化，但是这样就取决于浏览器内核怎么把序列化的转换成相应的引用值，可能在不同的浏览器中间是确定的。
JSON.parse(JSON.stringify) 

```

### Object.defineProperty

```javascript
writable: 可写 值是否可修改
configtable 可配置 可以使用defineProperty进行配置且不能使用关键字delete进行属性
enmutable 可枚举 for ... in
```

### 对象：常量、禁止拓展

1. 对象常量，可以使用writable：false以及configurable： false来是对象变成一个不可修改和删除的常量属性
2. 禁止拓展： 可以使用Object.preventExtensions()来让一个对象停止加入新的属性
3. 密封： Object.seal会使得对象完成不能添加新属性且无法删除，因为他会调用禁止拓展的函数并且使得每个属性的configurable=false
4. 冻结： 调用密封然后在每个属性上的writable=false使得对象属性不可新增删除修改

### Object赋值与屏蔽

```javascript
let obj = {
    a: 2
}
let objOne = Object.create(obj)]
obj.a // 2
obj1.a // 2
obj1.a++ // 3
obj.a// 2
如果对象原型上有相同的属性会分为三种情况：如果对象没有改属性，而原型上有的话赋值会出行的情况
1. 而原型上有该属性的话，且该属性没有可读等直接在原有对象上进行赋值
2. 如果原型上有该属性且只是可读的话进行修改就不会屏蔽
3. 如果原型上有该属性且有设置setter赋值也不会被屏蔽
```



### 类

```javascript
javascript中的构造函数有类似于类的功能，可以创建一个对象实例，这个实例的[[prototype]]指向构造函数的prototype。这种行为称作委托或者差异继承，就是一个构造函数可以构造出多个对象，每个对象的[[prototype]]都指向同一个对象，从而实现了一些属性的共有。
构造函数：在new执行的过程中的函数成为构造函数，创建实例对象的核心是Object.create({})
new的执行过程：
1. 创建一个对象
2. 把创建对象的proto指向函数的prototype
3. 把函数中的this替换成创建的对象执行函数
4. 返回对象
```



硬绑定函数没有prototype属性



### created

```
Object.createt(target,{})用来创建一个空对象，空对象的prototype指向target,第二个参数可以给对象增加属性，以及改变属性描述符的是否可读等

// 实现的思路
function foo(t1) {
	const Foo = function(){}
	Foo.prototype = t1
	return new Foo()
}
```



### 委托行为

使用create生成原型，从而用另外一种方式实现继承实际上应该是委托

```
const foo = {}
const bar = Object.create(foo)
const doo = Object.create(bar)
```



### 对于上册的理解

上册总共有两章，第一章讲的是作用域相关的问题，第二章讲的是this指向和原型继承的问题

第一章有告诉我们

作用域是什么：作用域就是使用get操作的时候能够检索的区域就是作用域，而set操纵会在当前所在的作用域新增，也可以说是属性起作用的范围叫做作用域

RHS 查询与简单地查找某个变量的值别无二致，而 LHS 查询则是试图 找到变量的容器本身，从而可以对其赋值

作用域链是什么：作用域链，当作用域中含有块级作用域或者函数作用域的时候，就会形成作用域链，因为每一个块级或者函数都是一个作用域，函数中调用某个变量，在当前作用域没有找到的时候，会想包含函数作用域的父级作用域进行检索，以此类推就形成了一条作用域链。

作用域闭包：作用域闭包就是函数作用域中包含其他的函数作用域叫做闭包，包含父级作用域让闭包更加的明显。



### 总结

第二章

this：名称是执行上下文，所谓执行上下文代表的时执行这个操作时的所属的对象，而不是当前对象本身。

```
如果在函数中有this的执行问题一般分为两个问题: 是默认执行还是对象执行
默认执行就是 fn执行的时候没有前缀, 指向的就是window默认对象
对象执行就是 obj.obj1.obj2.fn() 执行这个函数对象的this指向是执行时的作用域obj2
obj.value 代表的是在obj属性中找value，并且执行
```

类：讲述了js中的类不是真正意义上的类，真正的可以描述万物类的实例也有类的所有属性这是一种继承的关系，而js中的类是一种委托机制，是通过一种原型机制来实现的，所有的继承都是通过原型链来实现的。js中的类是一种语法糖，原型继承的语法糖。

原型：通过new fn() 构造函数来创建的实例对象，这个实例对象的原型就是这个构造函数的fn.prototype, fn.prototype.constructor = fn,创造的实例会有一个属性指向构造函数的原型，实例可以使用构造函数原型中的方法以及属性。

原型链：实例中找不到的属性可以原型对象中查找，原型对象中也没有就会在原型对象的基础上找他的原型，直到遍历完整个原型链为止。

形成原型继承只需要对象中有prototype属性就能形成原型继承，这就要使用到ctreate属性了。



## 类型和语法



### 类型

js的类型分为基本类型和引用类型，

1. Number
2. String
3. Boolean
4. null
5. undefined
6. object
7. symbol (es6)
8. bigint (es6)

这个大体可以分为两种引用类型object和除object以外的基本类型，object还有多个子类型有function类型他是可回调对象，因为function函数里面内置了[[call]]属性，有这个属性就是可以调用函数，`function a(a,b,c,d){}` a.length 表示该函数的入参个数

undefined 和 undeclear在typeof中的表现是一样的，然后你如果使用undecleard变量会报一个 a1 not a defined，a1没有被定义。故而可以在安全检测机制中检测除变量是否被声明



###  数组

数组可以包含所有的类型，又因为数组也是一个对象所以也能使用arr[key]的方式来获取值，数组还可以不连续取下标赋值，a[1] = 1,a[0]表现为空和undefined有所区别，区别在于arr.length是取最后一个最大下标，因此中间有的没有值也会形成一个真实长度和length不一致的表现，又因为对象中的值没有被赋值默认是undefined，数组也是有如此的表现，所以arr的长度和最大数字下标有关

 

### Number

number有几个需要注意的点

NaN是一个非自反的值  `NaN == Nan // false` 

undefined可以作为变量赋值，含义是值为undefined或者没有定义，所有变量在赋值之 前默认值都是 undefined

null 表示 值为空

Infinity 正无穷  Infinity / number  Infinity除以任何带符号数都等于正负Infinity

-0

void vari // undefinde void返回的就是undefined



### 原生函数

 

```
typeof Function.protptype // funtion
```



###  JSON.stringify(value, splacer, space)

```
value: 要被字符串化的值
splacer: 字符串化返回的值可以是函数、数字和字符串数组
space: 格式化缩进
JSON.stringify('a') // '"a"'
JSON.stringify(undefined | Symbol('a') | function a(){} ) // undefined
JSON.stringify([undefined , Symbol('a') , function a(){}, 1 ]) // [null,null,null,1] 

```

JSON将JSON对象序列化为字符串，可以把number、boolean化为字符串，对象数组等也字符串化，但是有几个类型是不能够字符串化的，undefined、Symbol、function a() {}、以及对象的循环引用，数组中含有这几个错误的都会返回null



(1) 字符串、数字、布尔值和 null 的 JSON.stringify(..) 规则与 ToString 基本相同。 

(2) 如果传递给 JSON.stringify(..) 的对象中定义了 toJSON() 方法，那么该方法会在字符 串化前调用，以便将对象转换为安全的 JSON 值。



### Boolean 强制类型转换

```
假值列表
+-0 NaN
false
undefined
null
''
Boolean(document.all) // false
```

~number number的补码

~number // -(number + 1) 

~(-1) // 0



### parseInt

把字符串转换为整型数字，可以进行四舍五入，并且可以进行进制转换，1-9 a-i，如有超出就去字符串的前面的值作为转换的进制 



### 隐式类型转换

#### 字符串和数字

```
var a = 42
var b = ''
var c = 0

a + b // '42'
a - b // '42'
[1,2] + [2,3] // 1,22,3
[1,2].valueOf().toStirng() + [2,3].valueOf().toStirng()
先进行valueOf操作如果返回的不是基本数据类型再进行toString操作

var a = {
 valueOf: function() { return 42; },
 toString: function() { return 4; }
};
a + ""; // "42"
String( a ); // "4"
```

如果加减符号两边有字符串类型或者valueOf() 或者toString后 有字符串的加法就是字符串的拼接



#### 布尔值到数字的转换

真值转换为数字为1，假值转换为数字是0

```
true + true // 2
true => 1
false => 2

function (a ,b ,c) {
    return !!((a && !b && !c) ||
     (!a && b && !c) || (!a && !b && c));
}
可读性不是很好，可以使用
function () {
	var sum = 0;
    for (var i=0; i < arguments.length; i++) {
    // 跳过假值，和处理0一样，但是避免了NaN
        if (arguments[i]) {
        	sum += arguments[i];
        } || sum += Number(!!arguments[i])
    }
 	return sum == 1
}
```



#### 其他类型转换为布尔值

```
if()
while () || do while()
? : 三元表达式
&& ||

a && b 相当于 a ? b : a
a || b 相当于 a ? a : b
```



#### symbol强制类型转换到字符串

symbol只能显示强制类型转换，不能隐形强制类型转换。

```
Symbol('a') + ''  // typeError
String(Symbol('a')) // 'Symbol('a')'
```



###  == 、 ===宽松相等和严格相等

== 是允许类型转换的相等， ===则不允许

#### 如果基本类型相同就比较值是否相等、对象类型就是比较引用是否相等

#### 类型不相同的相等

1. 数字和字符串比较是否相等

   type(x)是数字，type(y)是字符串 , x == toNumber(y)

   type(x)是字符串，type(y)是数字 , toNumber(x)  == y

2. 布尔和其他值比较

   type(x)是bool， type(y)是其他， toNumber(x) == y

   type(x)是其他，type(y)是bool ，x = toNumber(y)

3. 对象和非对象之前比较

   type(x)是对象， type(y)是数字或字符串， toPromitive(x) == y

   type(x)是其他，type(y)是bool ，x = toPromitive(y)

4. undefined 和 null的比较  null == undefined

```bi
布尔和其他值比较
var a = '42'
var b = false
a == b // false 由于第二条会先把bool转换成number类型然后再进行比较、再进行第一条 42 == 0 // false

undefined 和 null的比较
var a = null;
var b;
a == b; // true
a == null; // true
b == null; // true
a == false; // false
b == false; // false
a == ""; // false
b == ""; // false
a == 0; // false
b == 0; // false

对象和非对象之前比较
var a = 42;
var b = [ 42 ];
a == b; // true

var a = "abc";
var b = Object( a ); // 和new String( a )一样
a === b; // false b是对象 a是基本类型，类型不同
a == b; // true

var a = null;
var b = Object( a ); // 和Object()一样
a == b; // false
var c = undefined; 
var d = Object( c ); // 和Object()一样
c == d; // false
var e = NaN; 
var f = Object( e ); // 和new Number( e )一样
e == f; // false

没有 null 和 undefined 的对象类型，所以是空对象

1. 返回其他数字
Number.prototype.valueOf = function() {
 return 3;
};
new Number( 2 ) == 3; // true

var i = 2;
Number.prototype.valueOf = function() {
 return i++;
};
var a = new Number( 42 );
if (a == 2 && a == 3) {
 console.log( "Yep, this happened." );
}

[] == ![] // true => [] == false => [] == 0 => '' == 0 => 0 == 0

2 == [2]; // true
"" == [null]; // true 
[null].toString() // '' 

{} + [] // 0
[] + {} // [object Object]

第一个是{}被当成空的代码块， 直接是+[] => +'' => 0
第二个是 [].toString() 转换成了'' {}.toString() 转换成了[Object object]
```



### 抽象相等 > < 

比较不等式的大小是使用<算法来的

``` 
a > b =>  计算b < a
a < b

a >= b => 计算 a < b 然后取反

var a = { b: 42 };
var b = { b: 43 };
a < b; // false
a == b; // false
a > b; // false
a <= b; // true
a >= b; // true


```

根据规范 a <= b 被处理为 b < a，然后将结果反转。因为 b < a 的结果是 false，所 以 a <= b 的结果是 true。



### 语法

#### 结果表达式

var a = 42成为声明语句

a = 42 赋值表达式 不带var的



js中每个语句都会返回一个结果值，可以在浏览器的控制台发现，每输入一个语句都会返回一个结果值。

```
a = 42 // 42
var a = 42 // undefined

一般来说都是返回最后一个执行语句的结果值

if (true) a = 10  // 返回代码块最后一个执行语句

b = a++ 
b // 42
a // 43
++的副作用是（a递增）产生在表达式返回结果之前， a++副作用是产生在表达式返回结果之后
```



### 解构赋值

```
const obj = {
	a: 10,
	b: 20,
}
cosnt { a, b } = obj // { a: a, b: b } = obj
a // 10
b // 20
```



### javascript中的if else 语法

js中只有if()  {} else {} 没用else if

```
if (a) {
	//
} else if (b) {
	//
} else {
	//
}
等同于 
if (a) {
	//
} else {
	if (b) {
		//
	} else {
		//
	}
}

if (a) {
	//
} else 
    if (b) {
    //
    } else {
    //
    }


else if就是else没有代码块只影响一行的时候使用的
```



### 短路

运算符的优先级

```
false && false || true // true
(false && false) || true // true
false && (false || true) // false
&&的优先级高于||
// 
true ? false : true ? true : false // false
(true ? false : true) ? true : false // true
true ? false : (true ? true : false) // false

? : 三元运算符的优先级是从右到左
```

