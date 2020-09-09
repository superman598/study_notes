## JS代码分别为：混合模式和严格模式
如果js代码中出现了'use strict' 此时为严格模式
严格模式有两种：全局声明，函数内声明

## 严格模式
1.全局声明：在script标签中最上面，整个script标签内部都是属于全局的严格模式
2.函数声明：在含糊中使用'use strict' 是函数内的严格模式

1.全局的严格模式
```js
'use strict'
```
2.函数内声明（IIFE)
```js
 function f1(){
     'use strict'
     console.log('123')
 }
 f1();
```
* 必须使用 var 声明变量，不允许使用未声明的变量

## eval()是一个函数
对传输的参数进行解析代码并执行
* 严格模式下面的eval产生的作用域会报错

## 函数内部不能有重复的形参
```js
function f1(a,b,c,a){
    console.log(a,b,c,a)
}
f1(10,20,30,40)
```
## 什么是对象？
* <span style="color:red">对象： 是值某个事物具有特征或者行为的就是对象</span> 

特征--->属性

行为--->方法

创建对象，根据一个类别来创建

遍历对象通过for(in)来遍历对象中的数据
## Object.create(prototype,[descriptors])

可以创建新的对象，新的对象是原创建对象的时候传入的对象的子对象（新对象和原对象是继承的关系）,所以obj2是obj1的子对象
```js
    var obj1 = {
        name:'卡卡西',
        age:20
    }
    var obj2 = Object.create(obj1);
```
语法：
    Object.create(prototype,[descriptors])
属性：
1. value 设置属性的默认值
2. writable 设置当前的属性是否可以被修改，默认是false 不能改变
3. configurable 设置当前的属性是否可以被删除，默认是false，不能被删除
4. enumerable 设置当前的属性是否可以被遍历，默认是false，不允许遍历
5. get 访问器，如果外部通过obj2.gender方法来获取当前这个gender属性值的时候，会自动的进入到get方法中
* get不能和value和writable一起使用，会出现冲突报错

* 什么时候使用Object.create()方法？
 如果向往单签的对象和其他对象是继承关系，同时大年的对象还需要添加一些可控制的属性

## Object.defineProperties(object,descriptors)
通过这个方法为当前对象添加一个新的属性
1. object 要操作的对象
2. descriptors 属性描述
3. get
```js
 var obj = {
     name: '强哥'
 }
 // 可以为当前的对象添加一个新属性
 Object.defineProperties(obj,{
     age:{
        value:20, //何止当前的age属性的默认值
        writable:false,//是否可以修改当前这个属性的值，fl

     }
 })
```
* Object.defineProperties方法与Object.create方法用法一致
总结：如果需要为当前的对象添加新的属性，可以使用Object.defineProperties方法,并且里面的配置对象的属性方法基本上一致。

## 面向对象
面向对象是一种编程思想，突出需求，抽象出对象，通过对象调用属性或者方法，实现相关的需求，注重的是结果
面向对象的特性：<span style='color:red'>封装，多态，继承，(抽象性)</span>
1. 封装：
2. 继承：
3. 多态：


## ES5 新增的数组方法

1. indexOf()  查询数组(对象)中的某个数据，如果存在则返回对应数组下标(索引值),如果不存在则返回-1
lastIndexOf()
2. map() 遍历数据
默认遍历数组中的每个数据，设置新的数据后，返回的是一个新的数组(需要声明变量来接受返回的数据)
```js
var arr2 = [10,20,30,40];
var arr3 = arr2.map(function(item,index){
    return item * 2
})
console.log(arr3)
```
3. filter() 过滤数组中的每个数据
    只要数组中的数据满足判断条件，则将满足的数据放入到一个新的数组中
```js
 var arr4 =[1,2,3,4];
 var arr5 = arr4.filter(function(item,index){
     return item % 2 === 0;
 })
```
## 关于this的指向
1.call
2.apply
3.bind
* 总结：**apply和call和bind方法都可以改变this的指向，但是，apply和call在调用的时候，会直接执行**
* bind 在调用的时候，需要接收，接收后的内容就是之前的函数(方法)，再次调用才能够执行
 call、apply、bind都属于大写Function的方法，只要是函数就会可以使用这三个方法
call的应用场景，借用构造函数，实现继承
```js
var Person(name,age){
    this.name = name;
    this.age = age;
}

function Student(name,age){
    //借用
    Person.call(this,name,age)
}
vat stu = new Student('小明',20)
```

## 原型回顾
原型：是对象，有两个原型对象
1. prototype 显示原型，浏览器的标准属性，程序猿专用
2. __proto__ 隐式原型，浏览器的非标签属性，浏览器专用
3. 对象中必然有__proto__
4. 函数中必然有prototype
5. 函数也是对象，所以，函数中有__proto__，也有prototype
6. 实例对象的__proto__指向必然是所在的构造函数中的prototype


## ES6介绍
let关键字
1.不允许重复声明
2.拥有块级作用域
> js中没有块级作用域，但let可以实现块级作用域

3.let不存在变量的提升---预解析
4.let不影响作用域链