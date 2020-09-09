# Less



## 介绍

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展

中文网址:  <http://lesscss.cn/>

![1572691610628](assets/1572691610628.png)



## 使用

### 编译器

因为浏览器不支持 less 语法，所以需要借助工具将 less 语法转换为 css，这里的工具有

- less.js （<https://less.bootcss.com/>）   								效率较低
- lessc   （<https://www.npmjs.com/package/lessc>）
  - 安装 nodejs （<https://npm.taobao.org/mirrors/node/latest-v10.x/>）
  - 全局安装 less 
  - lessc 命令解析 
- 考拉    （<http://koala-app.com/>）

webstorm 自动解析 less 文件

* 关闭 webstorm
* 安装 nodeJS 
  * 下载地址 http://nodejs.cn/download/
* CMD 命令行下全局安装 lessc
  * `npm install less -g`
* 启动 webstorm
* webstorm 下 File -> settings -> 搜索 File Watcher ->  右上角 + 选择less -> OK 确定👌

vscode 

* 安装插件 easy less

### 语法

#### 变量

##### 基本声明

```css
@name: value;
@base-color: 200;
```

* 变量名称要求： 字母、数字、下划线、中横线
* 首字母可以为数字，可以为纯数字
* 区分大小写
* 变量值随意

##### 动态特征

将一个变量的值作为选择器

```css
@header-selector: #header;

@{header-selector} {
    height:100px;
    background:#90a;
}
```

##### 字符串拼接

```css
@combine-selector: ~'@{header-selector},@{footer-selector}';
```

#### 运算

less 样式值支持四则运算 `+ - * /`

单位处理

- 如果只有一个有单位，则使用该单位
- 如果都有单位，则使用第一个作为单位

#### 注释

```
//
/**/ 
```

多行注释不能嵌套



#### 嵌套

less 支持嵌套书写形式

##### 普通嵌套

```css
#app{
	#header{
        //子孙元素
		.logo{
			
		}
		//子元素
		>.search{
		
		}
        //后边紧挨着的元素
        + div{
            
        }
        
        &:hover{
            
        }
	}
}
```

##### 媒体查询

```css
ul {
    width: 700px;
    margin: 50px auto;

    @media screen and (min-width: 700px) and (max-width: 800px) {
        background: red;
    }

    @media screen and (min-width: 800px) and (max-width: 900px) {
        background: yellow;
    }
}
```

##### 变量作用域

变量只能在当前代码段和下层代码段使用。



#### 混合 mixins

混合类似于自定义函数，可以减少 CSS 代码的重复书写

##### 普通混合

```css
.center-position {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
}

#box1{
    .center-position();
}

#box2{
    .center-position();
}
```

##### 不带输出混合

不输出选择器的单独样式设置

```css
.center-position() {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translateX(-50%) translateY(-50%);
}
```

##### 参数混合

```css
.box(@width, @height, @bg) {
    width: @width;
    height: @height;
    background: @bg;
}
```

> 两个混合，混合名相同，但是参数不同，这是两个不同的混合，根据参数的个数来决定执行哪个混合



##### 参数默认值

```css
.box2(@width,@height:100px, @bg:#090) {
    width: @width;
    height: @height;
    background: @bg;
}
```

> 注意： 具有默认值的参数一定要靠后声明，否则会有语法错误。

调用的时候有两种形式

按顺序传参

```css
.box2(100px, 100px, #000);
```

制定参数名传参

```css
.box2(@bg:#090, @width: 100px, @height:200px);
```

##### 条件混合

```css
.arrow(@size:10px, @color: #908, @direction) when(@direction=bottom) {
    width:0px;
    height:0px;
    border-style:solid;
    border-width: @size;
    border-color: @color transparent transparent transparent ;
}
```

#### 导入

less 支持组件化，允许将不同功能的代码放到不同的文件中。

- less 文件不需要加后缀
- css 文件需要加文件后缀

#### 函数

less 提供了内置函数来处理，文档地址 <https://less.bootcss.com/functions/>，函数和混合的区分

* `混合`相当于是自定义函数
* `函数`相当于是内置函数

数学函数

- ceil
- floor
- percentage    将小数转化为 『百分比』

颜色操作

* lighten       加亮
* darken       加暗
* fadein        提高透明度
* fadeout     降低透明度

#### Map

Map 相当于 JS 的对象，可以把一系列的数据保存在 Map 中 

```css
#color(){
    base: #098;
    darker: darken(#098, 10%);
    darkest: darken(#098, 30%);
}

footer{
    height:100px;
    background: #color[darker];
}
```



## 同类产品

- sass      <https://www.sass.hk/>
- stylus   <https://stylus.bootcss.com/>

