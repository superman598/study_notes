数组Array的常用方法（22种）

[TOC]

## 关于数组的判断（1）isArray

**Array.isArray()**

```js
// 用于判断一个对象是否为数组
var f = [1,2,3];
console.log(Array.isArray(f)); //true
```

### 关于这个方法的实现

```js
Array.myIsArray = function(o){
    return Object.prototype.toString.call(Object(o) === '[Object Array]');
};
```



## 关于类数组对象转成数组（1）from

**Array.from()** 用于通过拥有length属性的对象或可迭代的对象来返回一个数组

```js
// 返回新的数组实例，对于原始解构不影响
let arr1 = Array.from("foo");
console.log(arr1); // ["f","o","o"];

// 该方法有三个参数：
// 1. 接受一个类数组或可迭代对象
// 2. 新数组中每个元素都会执行的回调函数
// 3. 指定第二个参数的this对象
var arr = Array.from([1,2,3],x=>x*10);
// arr[0] = 10;
// arr[1] = 20;
// arr[2] = 30;
```



## 关于多个数组的连接 & 数组转换成字符串（2）concat/join

**Array.prototype.concat()**  用于连接两个或多个数组，且不会改变现有数组

```js
var arr1 = ["苹果","香蕉","橘子"];
var arr2 = ["梨","西瓜","火龙果"]；
var arr = arr1.concat(arr2)
console.log(arr); // ["苹果","香蕉","橘子","梨","西瓜","火龙果"]
```

**Array.prototype.join()** 把数组的所有元素转换成一个字符串

```js
 var fruits = ["西瓜","哈密瓜","榴莲"];
 console.log(fruits.join()); // 西瓜,哈密瓜,榴莲
```



## 关于查找符合条件的数组（5）every/some/filter/find/includes

**Array.prototype.every()** 用于检测数值元素的每个元素是否都符合条件

```js
// 如果数组中检测到一个元素不满足条件，则整个返回false，剩余元素就不会再进行检测
// 不会对空数组进行检测，也不会改变原始数组
var ages = [33,20,16,50];
function checkAdult(age){
	return age >= 18;
}
ages.every(checkAdult); // false
```

**Array.prototype.some()** 检测数组元素中是否有元素符合指定条件

```js
// 不会对空数组进行检测，也不会改变原始数组
// 如果有一个元素满足条件，就返回true，剩余的元素不会再进行检测
var ages = [33,20,16,50];
function checkAdult(age){
	return age >= 18;
}
ages.some(checkAdult); // true
```

**Array.prototype.filter()** 用于检测数值元素，并返回符合条件的所有元素的数组

```js
// 不会对空数组进行检测，也不会改变原始数组
var ages = [33,20,16,50];
function checkAdult(age){
	return age >= 18;
}
ages.filter(checkAdult); // [33,20,50]
```

**Array.prototype.find()** 

```js
// 不会对空数组进行检测，也不会改变原始数组
// 返回符合传入测试（函数）条件的第一个元素的值，之后的值就不会再检测了
// 没有符合条件的值就返回undefined
var ages = [10,15,18,50];
function checkAdult(age){
	return age >= 18;
}
ages.filter(checkAdult); // 18
```

**Array.prototype.includes(searchElement,fromIndex)**

```js
// 判断一个数组是否包含一个指定的值,有就返回true，否则返回false
console.log([1,2,3].includes(2,1)); // true
```



## 关于数组的遍历（1）forEach

**Array.prototype.forEach()** 遍历方法，对数组中的每个元素都执行一次回调函数

```
// 对于空数组不会执行回调函数
var arr = [4,9,16,25];
arr.forEach(function(item,index){
	console.log(item)
});
```



## 关于数组index的查找（2）indexOf/lastIndexOf

**Array.prototype.indexOf()** 搜素数组中的元素，返回它所在的位置

```js
// 从头开始检索，找到item第一次出现的位置，返回它的索引值
// 没有就返回-1
var fruits = ["西瓜","哈密瓜","榴莲"];
console.log(fruits.indexOf("哈密瓜"));//1
```

**Array.prototype.lastIndexOf()** 返回指定内容最后出现的位置

```js
// 从后向前进行检索，找到item第一次出现的位置，返回它的索引值
// 没有就返回-1
var fruits = ["西瓜","哈密瓜","榴莲"];
console.log(fruits.indexOf("哈密瓜")); //1
```

## 关于数组的增删改（7）pop/shift/push/unshift/reverse/slice/splice

**Array.prototype.pop()** 删除数组的最后一个元素并返回删除的元素

```js
// 该方法会改变数组的长度
var arr1 = [10,20,30,40];
var arr2 = arr1.pop();
console.log(arr1); // [10,20,30]
console.log(arr2); // 40
```

**Array.prototype.shift()** 删除并返回数组的第一个元素

```js
// 该方法会改变数组的长度
var arr1 = [10,20,30,40];
var arr2 = arr1.shift();
console.log(arr1); // [20,30，40]
console.log(arr2); // 10
```

**Array.prototype.push()** 向数组末尾添加一个或多个元素，并返回新的长度

```js
// 该方法会改变数组的长度
var arr1 = [10,20,30,40];
var arr2 = arr1.push(50);
console.log(arr1); // [10,20,30，40,50]
console.log(arr2); // 5
```

**Array.prototype.unshift()** 向数组开头添加一个或多个元素，并返回新的长度

```js
// 该方法会改变数组的长度
var arr1 = [10,20,30,40];
var arr2 = arr1.unshift(50);
console.log(arr1); // [50,10,20,30，40]
console.log(arr2); // 5
```

**Array.prototype.reverse()** 反转数组的元素顺序

```js
var names = ["小明","小红","小黄"];
console.log(names.reverse()); // ["小黄","小红","小明"]
```

**Array.prototype.slice(start,end)** 选取数组的一部分，并返回一个新数组

```js
// 不会改变原始数组
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var re = fruits.slice(1,3);
console.log(re); // ["Orange","Lemon"]
```

**Array.prototype.splice(index，num)** 从数组中添加或删除元素

```js
// 该方法会改变原始数组
// 删除元素时,参数为开始索引和要删除元素的长度
var sites = ["Runoob","Google","Taobao"];
// 该方法返回的是被删除元素组成的数组
var re = sites.splice(2,1,"GGGGG");
console.log(sites); //["Runoob","Google","GGGGG"]
console.log(re); // ["Taobao"]
```



## 关于数组的回调处理和排序（3）sort/reduce/map

**Array.prototype.sort()** 对数组的元素进行排序

```js
// 该方法将改变原始数组
var points = [40,100,1,5,25,10];
points.sort(function(a,b){
	return a-b;
}); // 1，5，10，25，40，100 （升序排列）

// 也可以给字母排序
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort(); // ["Apple","Banana","Mango","Orange"];
```

**Array.prototype.reduce()** 将数组元素计算为一个值

```js
// 对于空数组不会执行回调函数的
// 接收一个函数作为累加器，对数组中的每个值进行累加计算
var num = [10,20,30,40,50];
var result = num.reduce(function(pre,cur){
	return pre + cur;
})
console.log(result); // 150
```

### 关于这个方法的实现

```js
Array.prototype.myReduce = function(callback, initialValue) {
  let accumulator = initialValue ? initialValue : this[0];
  for (let i = initialValue ? 0 : 1; i < this.length; i++) {
    let _this = this;
    accumulator = callback(accumulator, this[i], i, _this);
  }
  return accumulator;
};
```

**Array.prototype.map(callback,[thisObject])** 通过指定函数处理数组的每个元素，并返回处理后的数组

```js
// 返回一个新数组，新数组中的元素都是原始数组处理后的值
// 不会对空数组进行检测，也不会改变原始数组
var numbers = [4,9,16,25,36];
var newNum = numbers.map(Math.sqrt);
console.log(newNum); // [2,3,4,5,6]

var arr1 = [1,4,9,16];
const map1 = arr1.map(x=>{
// 只有当x=4时，才进行操作
	if(x == 4){
		return x*2;
	}
// 其他值时，就直接返回
	return x;
})
console.log(map1); // [1,8,9,16]
```

### 关于这个方法的实现

```js
Array.prototype.myMap = function(callback, thisArg) {
  let arr = [];
  for (let i = 0; i < this.length; i++) {
    arr.push(callback.call(thisArg, this[i], i, this));
  }
  return arr;
};
```











