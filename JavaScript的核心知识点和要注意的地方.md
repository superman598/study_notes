# JavaScript的核心知识点和要注意的地方

[TOC]

## 原型链

最初JavaScript是没有class的概念，我们使用的类是以function模拟，继承的实现手段一般依靠原型链。这里给出终极原型链作为参考。



**终极原型链**

![image-20200903135513253](C:%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200903135513253.png)

## 闭包

闭包是JavaScript中一个重要知识点，闭包中最主要的就是解决作用域链的问题。

> JavaScript中，作用域链的作用就是控制变量的访问顺序（是一个数组结构），仅此而已
>
> 作用域：代码在定义的时候作用域就确定死了。



### 作用域链和执行上下文

**执行上下文（execution context）**全局环境 	函数环境

JavaScript在运行时需要一个环境，（程序在解析和运行时所依赖和使用的环境），这个环境就是我们所谓的执行上下文。

在JavaScript中，每个函数都有自己的执行上下文，每执行一个函数时，函数的执行上下文会被推入一个上下文栈中，函数执行结束后，这个上下文栈便会被弹出。

**执行上下文栈**

程序为了管理执行上下文（确保程序的执行顺序）所创建的一个栈数据结构，被称为执行上下文栈。



**执行流程：**

1. 程序执行了，但是代码没有执行
2. 程序执行，先创建全局执行上下文，压入栈
3. 收集变量，生成变量对象（包含预解析）
4. 确定此执行上下文，并确定执行过程中this的指向，this也是存在于执行上下文当中的，全局指window
5. 确定自己执行上下文的作用域链。
6. 函数在定义的时候就确定了作用域链是谁
7. 如果函数不存在引用的外层，是不会出现在作用域链上的。

**执行上下文面试题目**

```js
var x = 10;
function fn(){
    console.log(x);
}
function show(f){
    var x = 20;
    f();
}
show(fn); // 10
//****************************************************
var a;
function a(){}	//函数的优先级最高，var a提升就没有作用了
console.log(typeof a); // function
//****************************************************
if(!(b in window)){
    var b = 1;
}
console.log(b); // undefined
//****************************************************
var c = 1;
function c(c){
    console.log(c);
    var c = 3
}
c(2); // TypeError: c is not a function（var c 会被忽略，但是赋值不会被忽略）
//****************************************************
var fn = function(){
    console.log(fn);
};
fn(); // f() {console.log(fn)}	匿名函数


function fn(){
    console.log(fn)
};
fn(); // f fn(){ console.log(fn) } 有名函数
//****************************************************
var obj = {
    fn2: function(){
        console.log(fn2)
    }
};
obj.fn2(); // ReferenceError:fn2 is not defined(全局没有fn2这个函数，它是obj对象内的方法，只能通过obj.fn2去访问)
```

### 闭包的形成

闭包的形成就是一个函数执行上下文中有一个变量被其内部函数引用了，且这个内部函数被返回了，就形成了闭包。

**闭包的经典例子1:**

```js
function fn(){
    var num = 3;
    return function(){
        var n = 0;
        console.log(++n);
        console.log(++num);
    }
}
var fn1 = fn();
fn1(); // 1 4
fn1(); // 1 5

// 一般情况下，在函数fn执行完后，就应该连同它里面的变量一同被销毁。
// 在这个例子中，匿名函数作为fn的返回值被赋给了fn1，且匿名函数内部引用这fn里的变量num，
// 所以变量num无法被销毁。
// 而变量n是每次被调用时新创建的，所以每次fn1执行完后它就把属于自己的变量连同自己一起销毁了。
// 于是最后就剩下num自己了，这里就产生了内存消耗的问题。
```

**闭包的经典例子2（定时器与闭包）**

### 闭包的优缺点

优点：

1. 保护函数内的变量安全，实现封装，防止变量流入其他环境发生命名冲突
2. 在内存中维持一个变量，可以做缓存（但是使用多了会造成内存消耗）
3. 匿名自执行函数可以减少内存消耗

缺点：

1. 被引用的私有变量不能被销毁，增大了内存消耗，造成内存泄露（解决：使用完手动为它赋值为null）
2. 由于闭包涉及跨域访问，所以会导致性能损失，可以通过把跨作用域变量存储在局部变量中，然后直接访问局部变量，来减轻对执行速度的影响。



## DOM事件

### JavaScript注册dom事件的三种手段：

1. 直接写在html标签上，onclick的做法

```html
<input type="button" onclick="alert("执行了html的绑定方法")" />
```

2. 在js中获取到相对应的dom元素后绑定，event.onclick = function(){ };

```js
let btn1 = document.getElementById("btn");
btn1.onclick = function(){ 
	console.log("执行了js绑定的事件");
}
```

3. 在js中使用addEventListener（）实现绑定；

```js
let btn2 = document.getElementById("btn");
btn2.addEventListener("click",function(){
    console.log("执行了addEventListener绑定");
}, false);

```

### 事件流：

![image-20200906223231053](C:%5CUsers%5CAdministrator%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20200906223231053.png)

从图中我们可以知道：

1. 一个完整的JS事件流是从window开始，最后又回到window的一个过程
2. ==事件流的三个阶段：==（1~5）捕获过程、（5~6）目标过程、（6~10）冒泡过程

然而，有些情况下JS的事件流不会根据这个过程去推进（先捕获，再目标，最后冒泡）

|  DOM Level  | 捕获事件 | 冒泡事件 |
| :---------: | :------: | :------: |
| DOM Level 0 |  不支持  |   支持   |
| DOM Level 2 |   支持   |   支持   |
| DOM Level 3 |   支持   |   支持   |

* 在DOM Level 0中只能一个元素绑定一个事件，并且绑定的是最后绑定的那个事件（即后绑定的会覆盖前面绑定的事件）。要为事件解绑，就要设置

```js
btn.onclick = null;
```

* DOM Level 2 上可以为一个元素同时绑定多个事件

```js
btn1.addEventListener("click",function(){
    alert("btn1")；
},false);

btn1.addEventListener("click",function(){
    alert("btn2")；
},false);

// 两个都可以显示出来，先弹出btn1，然后是btn2
```

==DOM 0级和DOM 2级的区别==

	*	DOM 2级事件可以在一个元素上绑定多个事件；
	*	DOM 0级的兼容性好，可以支持所有的浏览器，但是DOM 2级事件就不可以；
	*	DOM 2级在IE 中绑定的是attachEvent,解除绑定是detachEvent；在标准浏览器中的绑定事件是addEventListener，解除绑定是removeEventListener；



### 兼容性处理

```js

var EventUtil = {
    addHandler: function(element,type,handler){
        if(element.addEventListener){
            element.addEventListener(type,handler,false);
        }else if(element.attachEvent){
            element.attachEvent("on"+type,handler);
        }else{
            element["on"+type] = handler;
        }
    },
    
    removeHandler: function(element,type,handler){
        if(element.removeEventListener){
           element.removeEventListener(type,handler,false);
        }else if(element.attachEvent){
            element.attachEvent("on"+type,handler);
        }else{
            element["on"+type] = null;
        }
    }
}
```



### 事件处理函数中的常见属性

1. stopPropagation() & preventDefault()

```js
// preventDefault() 主要用来阻止标签的默认行为
var tag = document.getElementById("tag");
tag.addEventListener("click",function(event){
    event.preventDefault();
},false);


// stopPropagation() 用来阻止事件冒泡，这个一般在一些特定业务中不需要冒泡的情况下可以使用
// 第三个参数false代表冒泡获取（由里向外），true代表捕获（由外向里）
var btn = document.getElementById("btn");
var content = document.getElementById("content");
btn.addEventListener("click",function(event){
    alert("btn");
    event.stopPropagation();
},false);
content.addEventListener("click",function(){
    alert("content");
},false);

```

2. target & currentTarget（使用频率较少，理解即可）

target这个属性指向的是目标过程中的DOM对象，currentTarget这个指向的是当前的对象，具体内容和this一样，所以当this指向的是目标的时候，target和currentTarget相同。

### 事件委托

更新HTML节点或者添加HTML节点的时候，会出现新添加的节点或更新的节点无法绑定事件的情况，表现行为是无法触发事件，这时就需要事件委托来帮助我们解决这个问题。

==事件委托利用了事件冒泡==，只指定一个事件处理程序，就可以管理某一类型的所有事件。因此我们给最外层的父级元素绑定事件，那么里面的元素触发事件后，就会冒泡到最外层的父级元素上，然后事件就被触发了。

```html
<ul class="ul">
    <li class="li">1</li>
    <li class="li">2</li>
    <li class="li">3</li>
    <li class="li">4</li>
    <li class="li">5</li>
</ul>
```

```js
var ul = document.getElementsByClassName("ul")[0];
var li = document.getElementsByClassName("li");
ul.onclick = function(){
    alert(1);
};
// 这样虽然可以让子元素实现点击事件，但是点击ul时，同样会触发点击事件
// 现在我们只希望让子元素可以触发事件响应，这时就需要用到Event对象中提供的一个target属性
// 这个属性，可以返回事件的目标节点，也就是事件源。
// 也就是说，target就可以表示为当前的事件操作的dom，但不是真正操作的dom

ul.onclick = function(event){
    // 这里要处理一下浏览器的兼容问题
    var event = event || window.event;
    // 标准浏览器使用event.target,IE浏览器使用event.srcElement
    var target = event.target || event.srcElement;
    if(target.nodeName == "LI"){
        alert(target.innerHTML);
    }
};
```

以上情况是每个li都执行相同的点击事件，如果每个子元素的事件不同呢？

```html
<div id="box">
    <input type="button" id="add" value="添加" />
    <input type="button" id="remove" value="删除" />
    <input type="button" id="move" value="移动" />
    <input type="button" id="select" value="选择" />
</div>
```



```js
var box = document.getElementById("box");
box.onclick = function(){
     var event = event || window.event;
     var target = event.target || event.srcElement;
     if(target.nodeName == "INPUT"){
        switch(target.id){
             case:"add":
                alert("添加");
                break;
             case:"remove":
                alert("删除");
                break;
             case:"move":
                alert("移动");
                break;
              case:"select":
                alert("选择");
                break;
        }
     }
}
```

另一个问题就是，对于新增的节点，会有事件响应吗？同样用事件委托来解决这个问题：

```html
<input type="button" id="btn" value="添加" />
<ul id="ul">
    <li>1111</li>
    <li>2222</li>
    <li>3333</li>
</ul>
```



```js
var btn = document.getElementById("btn");
var ul = document.getElementById("ul");
var li = document.getElementsByTagName("li");
var num = 3;
ul.onmouseover = function(event){
     	var event = event || window.event;
     	var target = event.target || event.srcElement;
    	if(target.nodeName == "LI"){
            target.style.background = "red";         
        }     
}
ul.onmouseout = function(event){
    	var event = event || window.event;
     	var target = event.target || event.srcElement;
    	if(target.nodeName == "LI"){
            target.style.background = "white";
        }   
}
btn.onclick = function(){
    num++;
    var newLi = document.createElement("li");
    newLi.innerHTML = 111*num;
    ul.appendChild(newLi);
};
// 这样的话，新添加的子元素是带有事件效果的。当用事件委托的时候，根本不需要去遍历元素的子节点，
// 只需要给父级元素添加事件就可以了，这样可以大大的减少dom操作，这才是事件委托的精髓所在。
```



## this指向

**this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁，实际上this的最终指向的是那个调用它的对象**

### 普通函数中的this

**例子1：**

```js
function a(){
    var user = "追梦人";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
// 这里函数a实际是被Window对象所点出来的，Window.a();
```

**例子2：**

```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();
// 这里的this指向的是对象o，因为你调用的这个fn是通过o点出来的，
```

下面这个例子3就推翻了前两个例子的说法（其实并不是推翻，就是前两个例子说的不是很准确）。

**例子3：**

```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子，这里的this就是指向对象o的
    }
}
// 其实我们创建的变量都是给window添加属性，所有都可以用window点的方式访问到。
// 也就是说fn最后就是由window调用的，
// 那么为什么window调用的，最后的this却不是指向window的呢？（下面有答案）
window.o.fn(); 
```

* 情况1：如果函数中有this，但是它没有被上一级对象调用，那么this就指向window；
* 情况2：如果函数中有this，且它被上一级对象调用了，那么this就指向这个上一级对象；
* 情况3：如果函数中有this，**这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this的指向也只是它上一级的对象（不管这个对象中有没有this要的东西）**。（就如例子3中所演示的那样）



另外一种比较特殊的情况，如例子4所示。 

**例子4：**

```js
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined 
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
// this永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的。
// 定义变量j时，只是进行了赋值，但没有调用
// 最后j()时，在全局下进行调用，所有fn中的this指向了window
```

### 构造函数中的this

```js
function Fn(){
    this.user = "追梦人"；
}
// 为什么这里的对象a可以点出函数fn中的user呢？（答案在下方）
var a = new Fn();
console.log(a.user); // 追梦人
```

new关键字可以改变this的指向，将这个this指向对象a；因为用来new关键字就是创建一个对象实例（相当于复制了一份Fn到对象a里面），但是此时仅仅是创建，没有执行；最后调用Fn的对象是a，那么this的指向自然是对象。

### 当this碰到return时

```js
function fn(){ 
    this.user = '追梦子';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
// **************************************

function fn(){
    this.user = '追梦子';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined
//**************************************

function fn(){ 
    this.user = '追梦子';  
    return 1;
}
var a = new fn;  
console.log(a.user); //追梦子
//***************************************

function fn(){
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //追梦子
//***************************************

function fn(){
    this.user = '追梦子';  
    return null;
}
var a = new fn;  
console.log(a.user); //追梦子
```

**总结：**

​		如果返回值是一个对象，那么this指向的就是那个返回的对象；如果返回值不是一个对象，马儿this就还是指向函数的实例。（注意：虽然null也是一个对象，但是this还是指向实例，因为null比较特殊）

**知识点补充：**

 1. 在严格版中的默认this不再是window，而是undefined；

 2. new操作符会改变函数this的指向问题

    ​	原理：new关键字会创建一个空对象，然后自动调用一个函数方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象代替。 

### JavaScript中的call、apply和bind方法（用来指定this的环境）

```js
var a = {
    user:"追梦子",
    fn:function(e,ee){
        console.log(this.user); //追梦子
        console.log(e+ee); //3
    }
}
var b = a.fn;
b.call(a,1,2);
//***************************

var a = {
    user:"追梦子",
    fn:function(e,ee){
        console.log(this.user); //追梦子
        console.log(e+ee); //11
    }
}
var b = a.fn;
b.apply(a,[10,1]);

// apply方法和call方法，都可以改变this的指向，区别就是apply传入的参数是数组；
// 注意：如果这两个方法中传入的第一个参数是null，那么this指向的是window；


var a = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);
        console.log(e,d,f); // 10 1 2
    }
}
var b = a.fn;
// bind方法，仅仅是这样还不能调用它
// b.bind(a);
// 因此:
var c = b.bind(a,10);
// bind也可以有多个参数，并且参数可以执行的时候再次添加；且参数是按照形参的顺序进行的
c(1,2); 
```



<img src="file:///D:\Backup\Documents\Tencent Files\1105683711\Image\C2C\870C538D9D389F6CBA3FF114698A6E3B.jpg" alt="img" style="zoom:200%;" />



