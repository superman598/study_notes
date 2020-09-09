[TOC]

# 字符串（String）的常用方法

### 1. str.charAt(index）动态方法

```js
// 返回子字符串
```

### 2. str.charCodeAt(index) 动态方法

```js
// 返回子字符串的Unicode编码
```

### 3. String.fromCharCode(num1,num2,...)

```js
// 根据Unicode编码返回字符串
```

### 4. str.indexOf(searchString,startIndex)

```js
// 返回子字符串第一次出现的位置，从startIndex开始查找，找不到返回-1
```

### 5. str.lastIndexOf(searchString,startIndex)

```js
// 从右往左开始查找，找不到返回-1
```

### 6. 截取字符串

#### 1) str.substring(start,end)

```js
// 两个参数都为正数，返回值就是[start,end-1]这段的字符串
```

#### 2) str.slice(start,end)

```js
// 两个参数可正可负，返回值就是[start,end-1]这段的字符串
```

### 7. str.split(separator,limit)

```js
// 参数1指定字符串或正则，参数2指定数组的最大长度
// 字符串变数组
str.split(""); // 每个字符被分割["","",""]；
// 数组变成字符串
arr.join(分隔符)
```

### 8. str.replace(rgExp/substr,replaceText)

```js
// 返回替换后的字符串
```

### 9. str.match(rgExp)

```js
// 正则匹配
```



# JS中获取数据类型的四种方法

JS中获取数据类型常用的方法有四种：

### 1. typeof

```js
// 判断js中基本数据类型，但是无法判断对象的具体类型
var a = new Object();
var b = null;
console.log("a:"+typeof(a)); // a: object
console.log("b:"+typeof(b)); // b: object

var arr = [];
var arr1 = new Array();
console.log("arr:"+typeof(arr)); // arr: object
console.log("arr1:"+typeof(arr1)); // arr1: object

// 注意：当使用基本包装类型创建字符串、数组或者布尔值时，使用typeof返回的是Object
var s = new String("123");
console.log(typeof s); // Object
```



### 2. Object.prototype.toString.call( )

```js
// 可以判断具体的对象类型，包括正则等，但是无法判断自定义对象类型
console.log("a:"+Object.prototype.toString.call(a)); // [object Null]
console.log("b:"+Object.prototype.toString.call(b)); // [object Object]

console.log("arr:"+Object.prototype.toString.call(arr)); // [object Array]
console.log("arr1:"+Object.prototype.toString.call(arr1)); // [object Array]
// 对于自定义的对象类型，无法判断
function A(){
    this.a = 1;
}
var x = new A();
console.log(Object.prototype.toString.call(x)); // [object Object]
```



### 3. instanceof

```js
// 变量 instanceof对象，返回值为boolean,用来判断一个变量是否为某个对象的实例
// 仅能判断对象的具体类型，但可以拥有判断自定义对象类型
var a = 123;
var b = true;
console.log(a instanceof Number); // false
console.log(b instanceof Boolean); // false

function A(){
    this.a = 1;
}
var x = new A();
if(x instanceof A){
    console.log("x is A");
}
// x is A
```

### 4. constructor

```js
// 查看对象对应的构造函数
// object的每个实例都具有属性constructor，保存着用于创建当前对象的函数

function A(){
    this.a = 1;
}
var x = new A();
console.log(x.constructor); // f A(){this.a = 1}
if(x.constructor === A){
    console.log("x is A");
}
// x is A

// 但是， undefined和Null类型不能判断
```

