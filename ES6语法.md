

# ES6语法

[TOC]

在ES6中，提供了let关键字和const关键字，都具有块级作用域

## let关键字的使用

1. 仅仅在自己的块级作用域内起作用
2. 不存在变量提升，必须先进行变量的声明（否则会报错）
3. 在同一个块级作用域内，不允许声明同名的变量
4. 在函数内不能使用let关键字重新声明函数的参数

```js
function demo(ts){
	let ts = "哈哈";
	alert(ts)
}
demo(Hi); //报错
// 报错原因是使用了let关键字重新声明了函数的参数ts
```

5. 在for循环中使用let关键字可以解决闭包问题

```html
<div id="dv">
	<button>测试</button>
	<button>测试</button>
	<button>测试</button>
</div>
```

```js
var node = document.getElementByTagName("button");

for(let i=0;i<node.length;i++){
	node[i].onclick = function(){
		node[i].innerHTML = "成功了"
	}
}
```

6. 不影响作用域链

**总结：**使用let关键字声明的变量只在跨级作用域中起作用，比较适合for循环中，同时不会出现变量提升的现象；			且同一个代码块内，不可以重复声明相同的变量，也不可以重复声明函数内的参数。

## const关键字的使用

1. 只在块级作用域内起作用
2. 声明后的值不可以被修改
3. 不存在变量提升，必须先声明后才能使用
4. 不可以重复声明同一个变量，**且声明后必须赋值**
5. const常量可以是一个对象类型或数组，内部的数据是可以修改的

#### 总结：==var & let & const关键字之间的差异==

1. var 是在全局起作用的，而const和let关键字都是只在自己的块级作用域起作用的
2. const和let关键字都是不存在变量提升的，必须先进行声明；且不可以重复声明同一个变量
3. 使用const关键字在声明时必须同时赋值，而var和let关键字可以将声明和赋值分开进行
4. const关键字声明后的值不可以被修改，而var和let却可以
5. let关键字不能在函数内重新声明函数的参数

## 解构赋值

1. 数组的解构赋值

```js
var[a,b,c] = [10,20,30]; // 完全解构
var[a,,c] = [10,20,30]; // 不完全解构
```

2. 对象的解构赋值，**用到谁就解构谁**

```js
// 对象的属性没有次序，变量必须与属性同名，才能取到正确的值
var {foo,bar} = {bar:"aaa",foo:"bbb"};
```

3. 复杂结构的解构

```js
const person = {
				name: "超哥",
				age: 28,
				dog: {
					Dname: "Bruce",
					Dage: 10
				},
				sayHi: function() {
					console.log("Hi, I am chao")
				}
			}
const {name,age,dog:{Dname,Dage},sayHi} = person;
console.log(Dname,Dage); // Bruce 10
sayHi(); // Hi,I am chao
```

## 对象的简化写法

1. 对象的属性和方法都可以使用外部定义的属性或者函数

```js
var name = "强哥";
var age = 18;
var person = {
    name,
    age
}
console.log(person);
```

2. 对象中的方法，可以不使用function关键字来定义，直接书写最简单的方式

```js
var person = {
    name,
    age,
    sayHi(){
        console.log("hi,my name is qiangqiang")
    }
}
```



## 箭头函数

1. 基本语法

```js
// ES6允许使用箭头(=>)来定义函数
var f = (a,b)=>{
	return a+b;
}
// 箭头函数就相当于
function f(a,b){
    return a+b;
}

// 如果箭头函数不需要参数或者多个参数时，要将参数放入一对括号中;如果只有一个参数时，则可以不用括号
var f = ()=> 5;
var sum = (a,b)=>a+b;
var f = a=> a;
// 箭头函数的代码块多于一条语句时，要使用大括号将其包裹起来
// 如果箭头函数直接返回的是一个对象，必须在对象外面加上一个括号，防止它与外面的大括号发生冲突
var getItem = id=>({ id:id,name:"item" })

// 也可以和变量解构结合使用
const full =({first,last})=>{
    return first + "" +last;
}
```

2. 使用场景（定时器，数组中常用的方法，事件的回调函数。。。）

```js
setTimeout(()=>{
	console.log("test");
},1000)


arr.forEach((item,index)=>{
    console.log(item);
})


```

3. 不能作为构造函数去使用

4. ==箭头函数的this是不能改变的==，与this有关的操作都不适合用箭头函数。（例如：对象里面的方法）

```js
// this对象的指向是可变的，但是在箭头函数中，它是固定的
// 箭头函数导致this总是指向函数定义生效时所在的对象
// this的作用域就是定义时所在的对象，而不是使用时所在的对象

function foo(){
    setTimeout(()=>{
        console.log("id:",this.id)
    },1000) 
// setTimeout的参数是一个箭头函数，因此这个箭头函数定义生效是在foo函数生成时，且this指向函数定义生效时所在的对象，也就是42；
// 如果是普通的函数，this就是指向的是全局对象window；
}
var id = 21;
foo.call({id:42});	// 输出结果为： id：42

```

```js
// 箭头函数可以让setTimeout里面的this，绑定定义时所在的作用域，而不是指向运行时所在的作用域

function Timer() {
  this.s1 = 0;
  this.s2 = 0;
  // 箭头函数 this绑定定义时所在的作用域（即Timer函数）
  setInterval(() => this.s1++, 1000);
  // 普通函数 this指向运行时所在的作用域（即全局对象），setInterval是window对象下面的方法，因此里面的this指向window
  setInterval(function () {
    this.s2++;
  }, 1000);
}

var timer = new Timer();

setTimeout(() => console.log('s1: ', timer.s1), 3100);
setTimeout(() => console.log('s2: ', timer.s2), 3100);
// s1: 3
// s2: 0
```

this指向的固定化，是因为箭头函数没有自己的this，导致内部的this就是外层代码的this；

**this 的应用**

```js
var box = document.getElementById("box");
box.addEventListener("click",function(){
    var _this = this; // 此时function中的this指向事件源，也就是被点击的box；将this保留下来；
    setTimeout(function(){
    // 这里的回调函数就可以将this指向改成事件源了
        _this.style.width = "300px";
        _this.style.height = "300px";
    },1000)
},false)

//********************************************************************
var box = document.getElementById("box");
box.addEventListener("click",function(){
    setTimeout(()=>{
 // 如果回调函数使用的是箭头函数，就不用保存this了，因为箭头函数会直接去找绑定定义时所在的作用域中的this指向
        this.style.width = "300px";
        this.style.height = "300px";
    },1000)
},false)
```

5. 箭头函数中的arguments是不能使用的，用rest参数代替（args）

## 函数参数的默认值设置

ES6允许给函数形参赋值初始值；形参可以有默认值，==一般默认值靠后==；与解构赋值结合使用，解构赋值的形式先后顺序不影响。

```js
// 函数或方法调用的时候，传递的参数容易丢失，或者不想传递那么多参数，且结果能正常执行时。
function f1({name="超哥",age}){
	console.log(name,age) //输出  强哥 18
}
var obj = {
	name:"强哥",
	age:18
}
f1(obj);
// 如果在obj本身有属性name时，再利用解构赋值给name设置值时，不会起作用。
// 但是如果obj本身没有name属性时，利用解构赋值给name设置属性值时，可以起作用
function f1({name="超哥",age}){
	console.log(name,age) //输出  超哥 18
}
var obj = {
	age:18
}
f1(obj);
```

## rest参数

ES6中引入rest参数（真数组），代替arguments；arguments是对象（伪数组）

```js
function f1(...args){
// 这里的args是真数组，且rest参数必须放在最后    
	console.log(args)
}
f1(100,200,300); // [100,200,300]



// ... 延展运算符(可以拆包，也可以打包)
var obj1 = {
    name:"小猪佩琪",
    age:10
}
var obj2 = {
    gender:"男",
    height:180
}
var obj = {
// 拆包过程
    ...obj1,
    ...obj2
}
console.log(obj);


// 打包过程
var arr1 = [...arr]; // 内部拆包，外部打包
```

**扩展运算符（spread）的应用**

```js
// 将两个数组合并
var arr = [...arr1,...arr2];
console.log(arr); 

// 将两个对象合并
var obj = {
    ...obj1,
    ...obj2
}
console.log(obj); 

// 数组的克隆
var newCars = [...cars];

// 伪数组转真数组
var btnObjs = document.getElementById("btn"); // 伪数组
var btnObjs2 = [...btnObjs]; // btnObjs2 是真数组
```

## Symbol数据类型

Symbol是为了防止属性名的冲突，保证每一个属性名都是独一无二的。

```js
// Symbol类型定义出来的数据，是唯一的；即使在创建时这种数据类型的时候传入的数据是相同的，但是得到的结果进行比较，也是不同的数据；如果非要使用Symbol创建相同的数据，那么要使用Symbol.for()的方式(几乎不用)
let s1 = Symbol("foo");
let s2 = Symbol("foo");
s1 === s2 // false

Symbol.for("bar") === Symbol.for("bar"); // true
```

1. 创建方式

```js
// 第一种创建方式
const s1 = Symbol();
console.log(s1); // Symbol()
// 第二种创建方式
const s2 = Symbol("强哥");
console.log(s2); // Symbo(强哥)

```

2. 应用1（通过Symbol向对象中添加方法）

```js
const person = {
    eat(){
        console.log("今晚吃什么")
    };
    run(){
        console.log("赶紧去")
    }
};
// 这样做的话，person对象中的eat方法和run方法会被覆盖掉
person.eat = function(){
     console.log("没想好")
}
person.run = function(){
    console.log("跑不动")
}
person.eat(); // 没想好
person.run(); // 跑不动



// 利用Symbol来解决这个问题
const obj = {
    eat: Symbol("eat"),
    run: Symbol("run")
}
// 向person对象中添加eat方法和run方法
person[obj.eat] = function(){
    console.log("新的eat方法")
}
person[obj.run] = function(){
    console.log("新的run方法")
}
// 调用方法时，
person.eat(); // 今晚吃什么
person.run(); // 赶紧去
person[obj.eat](); // 新的eat方法
person[obj.run](); // 新的run方法
```

3. 应用2（通过Symbol创建对象属性）

```js
var obj = {
	age: 20,
	[Symbol("name")]:"小强"，
    [Symbol("look")](){
        console.log("快看")
    }
};
// 使用for-in或者for-of是无法遍历到用Symbol创建的对象属性
// 只能利用下面的方法遍历
const result = Reflect.ownKeys(obj);
console.log(result); // ["age",Symbol(name),Symbol(look)]

for(var i=0;i<result.length;i++){
    console.log(result[i]); // age Symbol(name) Symbol(look)
}

for(var i=0;i<result.length;i++){
    console.log(obj[result[i]]); 
    // 20 小强 f [Symbol("look")](){console.log("快看")}
}

for(var key in result){
    console.log(key); // 0 1 2
}
```

4. Symbol 内置的属性值



## ES6中新引入的遍历命令，for-of循环

Iterator接口主要提供for-of消费，数组可以使用for-of遍历，是因为内部实现了iterator接口

```js
// 获取当前数组中的Symbol.iterator的函数，并直接调用
// 返回的结果是一个对象，指针对象
var arr = [10,20,30,40];
var iterator = arr[Symbol.iterator()];
```

for-of不能遍历对象，因为for-of无法遍历自己定义的属性，这是因为它的内部没有实现迭代器

```js
var obj = {
	name:"强哥",
	age:47,
	cars:["奔驰","宝马","奥迪","捷达"],
	[Symbol.iterator]:function(){
        let that = this;
        let index = 0;
		return{
			next: function(){
                if(index>=that.cars.length){
                    return{
                        value:"undefined",
                        done:true
                    }
                }else{
                   return{
                       value: that.cars[index++],
                       done:false
                   }
                }
				
			}
		}
	}
}

// 这时再使用for-of遍历就可以实现了
for(var key of obj){
    console.log(key);
}
```

## Generator生成器函数

ES6提供的解决异步编程的方案之一，Generator函数是一个状态，内部封装了不同状态数据，用来生成遍历对象。

1. 声明的语法特殊
2. 调用时返回的结果是指针对象
3. 允许出现yield语句，相当于代码的分隔符，后面的代码不会执行
4.  每执行一次next方法，执行一段js代码，并返回yield语句的结果

```js
function* gen(){
    console.log("强哥好可爱");
    yield "AAA";
    console.log("超哥好帅哦");
}

const iterator = gen(); // 不会执行的，返回来的是一个指向内部状态的指针对象
console.log(iterator.next()); // “强哥好可爱” {value:"AAA", done:false}
console.log(iterator.next()); // “超哥好帅哦” {value:"undefiend", done:true}
```

调用Generator函数后，函数并不执行，返回的也不是函数执行后的结果，而是一个指向内部状态的指针对象（返回的是遍历器对象）。

下一步，必须调用遍历器对象的next方法，使得指针移向下一个状态。即：每次调用next方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个yield表达式（或return语句）为止。

==Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行。==

Generator函数的暂停执行的效果，意味着可以把异步操作写在yield语句里面，等到调用next方法时再往后执行。这实际上等同于不需要写回调函数了，因为异步操作的后续操作可以放在yield语句下面，反正要等到调用next方法时再执行。所以，==Generator函数的一个重要实际意义就是用来处理异步操作，改写回调函数。==

### 使用yield需要注意：

1. yield语句只能用于function* 的作用域，入股function* 的内部还定义了部还定义了其他的普通函数，则函数内部不允许使用yield语句。
2. yield语句如果参与运算，必须用括号括起来。

### return方法和next方法的区别：

1. return方法是终结遍历，之后的yield语句都失效；而next方法是返回本次yield语句的返回值

2. return没有参数时，返回{value:undefined,done:true};而next没有参数时，返回的是本次yield语句的返回值

3. return有参数时，覆盖本次yield语句的返回值，即{value：参数，done：true}；

   而next有参数时，覆盖上次yield语句的返回值，返回值可能与参数有关（参数参与计算的话），也可能与参数无关（参数不参与计算的话）

### 生成器函数的参数

```js
function* gen(args){
    console.log(args);
    let result1 = yield "AAA";
    console.log(result1);
    let result2 = yield "BBB";
    console.log(result2);
}
const iterator = gen("000");
console.log(iterator.next()); // 000 {value:"AAA", done:false}
console.log(iterator.next("111")); // 111 {value:"BBB", done:false}
console.log(iterator.next("222")); // 222 {value:"undefined", done:true}
```

**next（）方法参数**

表示上一个yield表达式的返回值，所以在第一次使用next方法时，传递参数是无效的。V8 引擎直接忽略第一次使用next方法时的参数，只有从第二次使用next方法开始，参数才是有效的。从语义上讲，第一个next方法用来启动遍历器对象，所以不用带有参数。

**生成器函数的实践1**

```js
// 解决回调地狱的问题
function one(){
	setTimeout(()=>{
		console.log("111");
		iterator.next();
	},1000)
}
function two(){
	setTimeout(()=>{
		console.log("222");
		iterator.next();
	},1000)
}
function three(){
	setTimeout(()=>{
		console.log("333");
		console.log(iterator.next());
	},1000)
}

function* gen(){
	yield one();
	yield one();
    yield three();
    yield "终于结束了";
}
var iterator = gen();
iterator.next();
```

**生成器函数的实践2**

```js
function getLogin(){
    setTimeout(()=>{
        const user = "用户信息";
        iterator.next(user);
    },1000)
}

function getProduct(){
    setTimeout(()=>{
        const product = "商品信息";
        iterator.next(product);
    },1000)
}

function getOrder(){
    setTimeout(()=>{
        const order = "订单信息";
        iterator.next(order);
    },1000)
}

function* gen(){
    const login = yield getLogin();
    console.log(login);
    
    const product = yield getProduct();
    console.log(product);
    
    const order = yield getOrder();
    console.log(order);
}

var iterator = gen();
iterator.next();
```

### for-in 与 for-of循环的区别

一般来说，for-in遍历对象（Object），for-of遍历数组会比较方便。

但是，for-of不能循环出自定义的属性。这是因为for-of循环的是可迭代的对象的的value，而for-in循环的是可迭代的对象的key值。

for-of不可以循环普通的对象，对于普通对象的属性遍历推荐使用for-in循环。

## Set集合的属性和方法

Set对象是值的集合，可以按照插入的顺序迭代它的元素，**且Set集合中的数据是唯一的**，重复元素在set中自动被过滤掉。

==Set()中放入的内容是可以通过for-of进行遍历的数据==：数组、字符串都可以，自定义的对象不行

```js
const num = new Set([1,2,3,4,5]);
const name = new Set("abcdef");

// 向Set集合中添加一个数据
num.add(100);
// 删除某个数据
num.delete(10);
// 判断集合中是否含有某个数据
num.has(100);
// 清空集合
num.clear();
```



**Set集合的应用**

```js
// 数组去重
var arr1 = ["强哥","小强","小强"];
var arr2 = [...new Set(arr1)];
console.log(arr2); //["强哥","小强"]
// 获取数组中的相同数据----交集操作
var arr3 = [10,20,30,40,50];
var arr4 = [20,30,50,60,70];

var arr5 = [...new Set(arr3)];
var arr6 = arr5.filter(item=>{
    let s = new Set(arr4);
    return s.has(item)
})
console.log(arr6);
// 简化形式就是
var arr5 = [...new Set(arr3)].filter(item=>new Set(arr4).has(item));
console.log(arr5);

// 获取数组中的不同数据----并集操作
var arr7 = [10,20,30,40];
var arr8 = [10,20,40,60];
var arr9 = [...new Set([...arr7,...arr8])];
console.log(arr9);

// 获取数组的差集----我有的你没有，你有的我没有
var arr10 = [10,20,30,40];
var arr11 = [10,20,40,60];
var arr12 = [...new Set(arr10)].filter(item=>!(new Set(arr11).has(item)));
console.log(arr12); // [30]
```



## Map集合的属性和方法

Map容器：无序的key，不重复的多个key-value的集合体，放入的内容是可以通过for-of进行遍历的数据

由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值覆盖掉

```js
var m = new Map([["Bruce",18],["Michael",25]]);
// 添加数据,以键值对的方式
m.set("name","强哥");
// 获取数据
m.get(key);
// 删除某个数据
m.delete(key);
// 检查是否存在某个值
m.has(key);
// 清空数据
m.clear();
// 检查map中的内容大小
m.size;
```



## Class实例化对象

ES5中创建对象的方式：new Object() / 构造函数 / {}

ES6中可以直接通过class定义类（代替了ES5中的构造函数）

```js
class Person{
   // 构造器函数
    constructor(name,age){
        this.name = name;
        this.age = age;
    }
   // 实例方法1（不允许使用ES5中的方法,必须使用ES6中的写法,即省略function的写法）
    eat(){
        console.log("今晚吃什么？")
    }
   // 实例方法2
    run(){
        console.log("怎么去？")
    }
}
var per = new Person("强哥"，18);
console.log(per.name,per.age);
per.eat();
per.run();
```

**class实例**

```js
class SellBrother{
	constructor(name,age,gender,phone){
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.phone = phone;
    }
    cook(){
        console.log("红烧肉");
    }
    gave(){
        console.log("送饭来了，请接收");
    }
}
var ge = new SellBrother("小黄",35,"男",151);
ge.cook();
ge.gave();
```



## ES6实现静态成员

属性------静态属性和实例属性

方法------静态方法和实例方法

静态成员是构造函数对象的（函数对象上的，直接通过==函数名字点属性名==出来的，

调用时，静态成员是通过类来调用的，而不是通过实例对象调用的。

**ES5中添加静态成员的方法**

```js
function Person(age){
  // 这个age属于当前的Person的实例对象的属性  
	this.age = age;
    this.eat = function(){
        console.log("下雨了");
    }
}
// 静态属性------通过Person来调用的
Person.age = 100;
// 静态方法-------也是通过Person来调用的
Person.run = function(){
    console.log("快跑啊");
}
var per = new Person(20);
console.log(per.age); // 20
// 静态属性调用
console.log(Person.age); // 100
per.eat(); //下雨了
// 静态方法调用
Person.eat(); // 快跑啊
```

**ES6中添加静态成员的方法**

```js
class Person{
    constructor(age){
        this.age = age;
    }
   // 添加静态属性或方法
    static name = "人类";
	static eat(){
        console.log("人都要吃饭");
    }
}
// 调用静态成员的方法
console.log(Person.name);
Person.eat();
```



## ES6中实现继承的方法

**ES5中实现继承的方式（==借用构造函数==实现继承和==改变原型指向==实现继承）**

对象中—proto—,

函数中prototype

—proto—的指向必然是所在的实例对象使用的构造函数中的prototype，

函数也是对象，函数都是Function的实例

```js
function Person(name,age){
    this.name = name;
    this.age = age;
}
// 通过原型添加方法----实例对象来调用
Person.prototype.eat = function(){
    console.log("吃饭了");
}

function Student(name,age,gender,score){
 // 通过借用构造函数的方式来实现继承----属性的阶乘
    Person.call(this,name,age);
    this.gender = gender;
    this.score = score;
}
// 改变原型的指向，实现了实例方法的继承
Student.prototype = new Person();
// 通过原型添加方法----实例对象来调用
Student.prototype.sayHi = function(){
    console.log("好长一段话啊");
}


var stu = new Student("强哥"，18，"男",100);
stu.eat();
stu.sayHi();

```

**ES6中实现继承的方式**

```js
class Person{
	constructor(name,age){
		this.name = name;
		this.age = age;
	}
    sayHi(){
        console.log("你好");
    }
}
class Student extends Person{
    constructor(name,age,score){
      // 这里的super相当于ES5中的Person.call(this,name,age);
      // 继承了Person这个类中的属性，而且是实例属性
        super(name,age);
        this.score = score;
    }
}

const stu = new Student("强哥",20,50);
// 实例方法也同样被继承了
stu.sayHi();
```



## getter 和 setter方法的设置



```js
class Person{
 // 当前的这个name属性是使用get进行设置的，也就意味着这个name属性外部通过实例对象仅仅是调用获取该属性	  的值而已
 // 只有get，没有set时，外部只能读取   
	get name(){
        return this.myName;
    }
    set name(val){
        this.myName = val;
    }
}
var per = new Person();
per.name = "强哥";
console.log(per.name);
```



## 数值的扩展

**进制数（二进制、八进制、十进制、十六进制）**

## 对象扩展

```js
// 1. 判断两个值是否完全相等
// 几乎和===一样，但是有些区别：NaN和NaN的问题
Object.is(1,1);

// 2. 对象的合并(不定个数)
Object.assign(obj1,obj2,obj3);

// 3. 直接修改__proto__设置原型对象
```



## 深拷贝和浅拷贝

复制对象（数组），依据就是创建的新对象修改是否会影响原来的数组。

会产生影响的是浅拷贝，不会产生影响的是深拷贝

### 浅拷贝

```js
// concat 浅拷贝
var arr = [1,2,3,{a:4}];
var newArr = arr.concat();
newArr[3].a = 5;
console.log(arr); // a:5
console.log(newArr); // a:5

// slice 浅拷贝
const arr = [1,2,3,{a:4}];
const newArr = arr.slice(0);
newArr[3].a = 100;
console.log(arr); // a:5
console.log(newArr); // a:5

// 扩展运算符 浅拷贝
const arr = [1,2,3,{a:4}];
const newArr = [...arr];
newArr[3].a = 100;
console.log(arr);
console.log(newArr);
```

复制对象

```js
// assign 浅拷贝
const obj = {
    name: "guigu",
    xueke:["java","大数据","前端"]
}
const newObj = {}
Object.assign = (newObj,obj);
newObj.xueke[0] = "H5";
console.log(obj);
console.log(newObj);
```

### 深拷贝

#### JSON

JSON.stringify( ) 将js对象转换成字符串，**为了保存和传递**。

JSON.parse( ) 将JSON格式的字符串，转换为JS对象。

```js
const school = {
    name:"guigu",
}
// 将对象转换成字符串
let str = JSON.stringify(school);
// 将字符串转换成对象
let newSchool = JSON.parse(str);

// 此时再进行修改，就不会影响原来的对象了
newSchool.name = "新对象";

```

#### 递归实现深拷贝

```js
const school = {
    name:"guigu",
    xueke:["前端","JAVA","大数据","云计算"],
    founder:{
        name:"刚哥"，
        age: 42
    }，
    improve:function(){
        console.log("提高技能")
    }
}
// 思路
const newSchool = {};
newSchool.name = school.name;

newSchool.xueke = [];
newSchool.xueke[0] = school.xueke[0];
newSchool.xueke[1] = school.xueke[1];
newSchool.xueke[2] = school.xueke[2];
newSchool.xueke[3] = school.xueke[3];

newSchool.founder = {};
newSchool.founder.name = school.founder.name;


// 方法
function getDataType(data){
    return Object.prototype.toString.call(data).slice(8,-1)
}

function deepClone(data){
    let type = getDataType(data);
    console.log(type);
    
    let container;
    if(type === "Array"){
        container = [];
    }
    if(type === "Object"){
        container = {};
    }
    // 遍历
    for(let i in data){
        let t = getDataType(data[i]);
        if(t === "Array" || t === "Object"){
            container[i] = deepClone(data[i]);
        }else if(t === "Function"){
            container[i] = data[i].bind(container);
        }else{
            container[i] = data[i];
        }
    }
    return container;
}
```

## Less

#### 嵌套

```less
// 嵌套
.container{
    min-height: 500px;
    // 嵌套
    header{
        border: 1px solid #ddd;
    }
    //直接子元素
    > section{
        height: 200px;
        position: relative;
        // 伪元素
        &::after{
            content:"";
            position:absolute;
            width: 100px;
            height: 5px;
    	}
        .left{
            width: 70%;
            height: 400px;
            background:#789;
            // 设置hover样式
            &:hover{
                background:yellow;
            }
            
            //媒体查询
            // >=1200px
            @media screen and (min-width:1200px){
                 background: yellowgreen;
            }
            // >=992px && <=1200px
            @media screen and (max-width:1200px) and (min-width:992){
                 background: skyblue;
            }
            // <=992px
            @media screen and (max-width:992px){
                 background: orange;
            }
        }
    }
}
```

#### 混合

```less
// 使用混合
.header{
    width:100px;
    height:100px;
}
.center{
    // 调用了header里面的宽高
    .header();
    
}

// 不带输出的混合
.center(){ // 谁用谁就调用它
    position:absolute;
    left:50%;
    top:50%;
    transform: translate(-50%,-50%);
}
// 带参数混合
.bg(@color){
    background:@color;
}
.h(@height){
    height:@height;
}
footer{
    .bg(#798);
    .h(100px);
}

// 条件混合
@flag = 1;
.border(@border)when(@flag = 1){
    border:2px solid @color;
}
#test{
    .h(100px);
    .bg(#aef);
    .border(#145);
}
```

#### 变量

```less
// 可以使用变量
// 变量声明
@color: #aef；
@height: 100px;
header{
   background: @color;
   height: @height;
}

// 变量插值
@selector: #section;
@{selector}{
    height: 400px;
    background: #899;
}

// 变量拼接
// section,footer{ }
@one:section;
@two:footer;
@s:~'@{one},@{two}';
@{s}{
    background: #999;
}

// 四则运算(单位以前面的一个为准)
header{
    height: 100px + 200px;
}
```

#### 文件导入

```less
@import "button";// 直接写文件名就可以，不用家后缀.css

button{
    .btn();
}


// 在button.css文件中，
.btn(){
    padding: 10px 12px;
    border: none;
    outline:none;
    border-radius:5px;
}
```

#### Less的内置函数

```less
// ceil向上取整, floor 向下取整
header{
    width: ceil(100px/3);
    height: floor(100/6px);
}
// percentage将结果转成百分比
@color:#980;
section{
    width: percentage(1/3);
// 颜色操作函数
// lighten 颜色变浅，darken 颜色变深
//    background: lighten(@color,10%);
//    background: darken(rgb(90,80,70),20%);
    
// fadein 改变颜色的透明度（逐渐加深），fadeout（逐渐变透明）
    background:fadein(rgba(100,200,120,0.4),10%)； // rgba(100200,120,0.5)
}

```

#### Map

```less
@color: #379;
#color(){
    base: @color;
    abc:darken(@color,10%);
}

```

#### 封装（Bootstrap）

#### 栅格系统容器的实现





















## JavaScript中的错误对象

JavaScript定义了7种错误类型：

1. Error错误
2. EvalError全局错误
3. RangeError范围错误
4. ReferenceError引用错误
5. SyntaxError语法错误
6. TypeError类型错误
7. URIError编码错误

**Error**

基础类，其他错误类型都继承自该类型。很少见，如果有也是浏览器抛出的。

**EvalError**全局错误

在使用eval()函数时发生异常时抛出的错误。

**TypeError**

类型错误，对象表示值的类型非预期类型时发生的错误。

当传入函数的操作数或参数的类型并非操作符或函数所预期的类型时，会抛出这个错误

**ReferenceError**

引用错误，对象表明一个不存在的变量被引用。当你尝试引用一个未被定义的变量时，将会抛出这个错误

**RangeError**

试图传递一个number参数给一个范围内不包含该number的函数时会引发这个错误

**SyntaxError**

对象尝试解析语法上不合法的代码时引发的错误，可能是丢失运算符或者转译字符等。



























