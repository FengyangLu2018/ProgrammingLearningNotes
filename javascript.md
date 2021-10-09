# 语法
## 输入输出语句
- `alert(msg)` 浏览器弹出警示框
- `console.log(msg)` 浏览器控制台打印输出信息
- `prompt(info)` 浏览器弹出输入框，用户可以输入

## 变量
### 变量提升
- 把所有变量声明提升到当前作用域最前面
- 不提升赋值操作
- 变量可以先使用再声明

### 变量声明
1. var声明         
- 变量提升
- 没有块级作用域
2. let声明
- 变量不提升
- 有块级作用域
3. const声明
- 变量不提升
- 有块级作用域
- 声明、赋值必须同时，简单数据类型值不可变，复杂数据类型地址不可变

### 变量作用域
1. 全局作用域：整个js文件；局部作用域：函数内部；块级作用域：{}
2. 全局变量的作用域是全局作用域，局部变量的作用域是局部作用域
3. 注意
- 在函数内部没有声明直接赋值的变量属于全局变量
- 函数形参也可以看做局部变量
- 作用域链：内部函数访问外部函数的变量采用链式查找的方式，就近原则

### 使用示例
```
一、var声明
1、由于存在变量提升，可以先使用再声明
console.log(a);
var a = 10;
//这段代码不会报错，但输出undefined，因为赋值操作不提升
2、在函数中声明的变量属于局部作用域，全局作用域访问不到
function f(){
    var a = 10;
}
f();
console.log(a);
//这段代码会报错'a is not defined'
3、变量在使用时，会寻找距离最近的作用域中的声明
var a = 20;
function f(){
    var a = 10;
    console.log(a);
}
console.log(a);
f();
这段代码输出20 10
4、循环变量会被提升为全局变量
console.log(a);
for(var a=0; a<2; a++){
    console.log(a);
}
console.log(a);
//这段代码输出0 1 2 undefined
5、可以重复声明，后面的声明会覆盖前面的
var a = 10;
var a = 20;
console.log(a);
//这段代码输出20

二、let声明
1、没有变量提升，不能先使用再赋值
console.log(a);
let a = 10;
//这段代码会报错'a is not defined'
2、循环变量属于块级作用域，不会提升到全局作用域
for(let a=0; a<2; a++){
    console.log(a);
}
console.log(a);
//这段代码会输出0 1之后报错
3、let声明的变量不可以重复声明
let a = 10;
let a = 20;
console.log(a);
//这段代码会报错'Identifier 'a' has already been declared'

三、const声明
1、const声明的变量，简单数据类型值不可变，复杂数据类型地址不可变
const a;
a = 10;
const b = 10;
b = 20;
const c = {x: 2, y: 3, z: 5};
c = {};
const d;
d = {x: 2, y: 3, z: 5};
//以上四种声明方式都会报错
2、const声明的复杂数据类型值可变
const a = {x: 2, y: 3, z: 5};
a.x = 10;
a.y = 20;
a.z = 50;
//这段代码没有语法错误
```
## 简单数据类型
### 数据类型
1. 数值型Number
- `isNaN()`可以判断变量是否是数值型，非数值型返回true
- `010`表示八进制，`0x10`表示十六进制
- `Number.MAX_VALUE`、`Number.MIN_VALUE`，表示数字型的最大值和最小值
- `Infinity`、`-Infinity`表示无穷大、无穷小
2. 布尔型Boolean
- 布尔型和数值型相加时，`true`的值为1，`false`的值为0
3. 字符串型String
- 引号嵌套：外双内单或外单内双
- 转义符：
   - `\n`  换行
   - `\\`  反斜杠
   - `\'`  单引号
   - `\"`  双引号
   - `\t`  tab缩进
   - `\b`  空格
- `str.length`获取字符串长度
- 字符串拼接，数值相加字符相连
4. 未定义型Undefined
- 与字符串相加，拼接字符串
- 与数字相加，返回`NaN`
5. 空值型Null
- 与字符串相加，拼接字符串
- 与数字相加，返回原数，这个变量在计算中相当于0

### 获取变量数据类型
- `typeof variable`获取变量数据类型

### 数据类型转换
1. 转换为字符串型
- `toString()`  `num.toString()`
- `String()`  `String(num)` 强制转换
- 加号拼接字符串  `num + ''` 隐式转换
2. 转换为数字型
- `parseInt(string)`     转为整数		
- `parseFloat(string)`   转为浮点数		
- `Number()`             强制转换
- `- * /`              隐式转换		
2. 转换为布尔型
- `Boolean()`函数

### 使用示例
```
一、数字运算
1、使用isNaN()判断变量是否为非数值型
console.log(isNaN(123));						//输出false
console.log(isNaN('123'));          			//输出false
console.log(isNaN('abc'));          			//输出true
console.log(isNaN(true));           			//输出false
console.log(isNaN(Infinity));       			//输出false
console.log(isNaN(NaN));            			//输出true
console.log(isNaN(undefined));      			//输出true
console.log(isNaN(null));           			//输出false
2、查看特殊数值			
console.log(1/0);								//Infinity
console.log(-1/0);                  			//-Infinity
console.log(Infinity);              			//Infinity
console.log(-Infinity);             			//-Infinity
console.log(Number.MAX_VALUE);      			//1.7976931348623157e+308
console.log(Number.MIN_VALUE);      			//5e-324
3、查看十六进制、八进制数值			
console.log(010);								//8
console.log(0x10);								//16
4、布尔值与数值、字符串相加			
console.log(1+true);							//2
console.log(typeof(1+true));					//number
console.log('1'+true);							//1true
console.log(typeof('1'+true));					//string
console.log(1+false);							//1
console.log(typeof(1+false));					//number
console.log('1'+false);							//1false
console.log(typeof('1'+false));					//string
5、数值与字符串相加			
console.log(1+'1');								//11
console.log(typeof(1+'1'));						//string
6、Undefined与数值、字符串相加		
console.log(undefined + 1);						//NaN
console.log(typeof(undefined + 1));				//number
console.log(undefined + '1');					//undefined1
console.log(typeof(undefined + '1'));			//string
7、Null与数值、字符串相加
console.log(null + 1);							//1
console.log(typeof(null + 1));					//number
console.log(null + '1');						//null1
console.log(typeof(null + '1'));				//string

二、数据类型转换
1、转为字符串型
var a = 10;
var s1 = a.toString()
var b = 20;
var s2 = String(b);
var c = 30;
var s3 = c + '';
var d = null;
var s4 = d.toString();							//报错Uncaught TypeError: Cannot read properties of null (reading 'toString')
var s5 = String(d);
var s6 = d + '';
var e = undefined;
var s7 = e.toString();							//报错Uncaught TypeError: Cannot read properties of null (reading 'toString')
var s8 = String(e);
var s9 = e + '';
var f = true;
var s10 = f.toString();
var s11 = String(f);
var s12 = f + '';
console.log(a);									//10
console.log(typeof a);			                //number
console.log(s1);                                //10
console.log(typeof s1);                         //string
console.log(b);                                 //20
console.log(typeof b);                          //number
console.log(s2);                                //20
console.log(typeof s2);                         //string
console.log(c);                                 //30
console.log(typeof c);                          //number
console.log(s3);                                //30
console.log(typeof s3);                         //string
console.log(d);									//null
console.log(typeof d);							//object
console.log(s4);								//报错
console.log(typeof s4);							//报错
console.log(s5);								//null
console.log(typeof s5);							//string
console.log(s6);								//null
console.log(typeof s6);							//string
console.log(e);									//undefined
console.log(typeof e);							//undefined
console.log(s7);								//报错
console.log(typeof s7);							//报错
console.log(s8);								//undefined
console.log(typeof s8);							//string
console.log(s9);								//undefined
console.log(typeof s9);							//string
console.log(f);									//true
console.log(typeof f);							//boolean
console.log(s10);								//true
console.log(typeof s10);						//string
console.log(s11);								//true
console.log(typeof s11);						//string
console.log(s12);								//true
console.log(typeof s12);						//string
2、转为数值型
console.log(parseInt('3.14'))  					//3
console.log(parseInt('3.94'))  					//3
console.log(parseInt('120px'))  				//120
console.log(parseInt('rem120px'))  				//NaN
console.log(parseInt(undefined))				//NaN
console.log(parseInt(null))						//NaN
console.log(parseInt(true))						//NaN
console.log(parseInt(false))					//NaN
console.log(parseFloat('120px'))  				//120
console.log(parseFloat('rem120px'))  			//NaN
console.log(parseFloat(undefined))				//NaN
console.log(parseFloat(null))					//NaN
console.log(parseFloat(true))					//NaN
console.log(parseFloat(false))					//NaN
console.log(Number('3.14')) 					//3.14
console.log(Number('3.94')) 					//3.94
console.log(Number('120px')) 					//NaN
console.log(Number('rem120px')) 				//NaN
console.log(Number(undefined))					//NaN
console.log(Number(null))						//0
console.log(Number(true))						//1
console.log(Number(false))						//0
console.log('12' - 0)    						//12
console.log('12' - '11')     					//1
console.log('12' *  1)    						//12
console.log('12' /  1)    						//12
console.log('12' - null)						//12
console.log('12' - undefined)					//NaN
console.log('12' - true)						//11
console.log('12' - false)						//12
console.log(typeof ('12' - 0))    				//number
console.log(typeof ('12' - '11'))     			//number
console.log(typeof ('12' *  1))    				//number
console.log(typeof ('12' /  1))    				//number
console.log(typeof ('12' - null))				//number
console.log(typeof ('12' - undefined))			//number
console.log(typeof ('12' - true))				//number
console.log(typeof ('12' - false))				//number
3、转为boolean型
console.log(Boolean(0))							//false
console.log(Boolean(''))						//false
console.log(Boolean(null))						//false
console.log(Boolean(undefined))					//false
console.log(Boolean(NaN))						//false
console.log(Boolean(1))							//true
console.log(Boolean('a'))						//true
```
## 运算符
### 算术运算符
- `+ - * / %`
### 递增递减运算符
- 前置，先自增自减后返回
- 后置，先返回后自增自减

### 比较运算符(关系运算符)
- `> >= < <=`
- `== !=` 只要求值相等
- `=== !==` 要求值和类型都相等

### 逻辑运算符
1. `&& || ！`
2. 短路运算
- 逻辑与
   - 表达式1为真，返回表达式2
   - 表达式1为假(0,'',undefined,NaN,null)，返回表达式1
- 逻辑或
   - 表达式1为真，返回表达式1
   - 表达式1为假，返回表达式2

### 赋值运算符
`= += -= *= %= /=`
### 运算符优先级
小括号 > 一元运算符(`++ -- !`) > 算数运算符(先`/ * %`后`+ -`) > 关系运算符(`> >= < <=`) > 相等运算符(`== != === !==`) > 逻辑运算符(先`&&`后`||`) > 赋值运算符(`=`) > 逗号运算符(`,`)
### 使用示例
```
var a = 123 && 456;
var b = NaN && 123;
var c = 123 || 456;
var d = NaN || 123;
console.log(a);									//456
console.log(b);									//NaN
console.log(c);									//123
console.log(d);									//123
```
## 流程控制
1. 顺序结构
2. 分支结构
- `if(条件表达式1){执行语句1}else if(条件表达式2){执行语句2}else{执行语句3}`
- `switch case` 表达式和value值必须是全等才匹配
- 三元表达式  `条件表达式 ？ 执行语句1 : 执行语句2;`
3. 循环结构
- `for(初始化变量;条件表达式;操作表达式){循环体}`
- `while(条件表达式){循环体}`
- `do{循环体}while(条件表达式)`
- `continue`跳出本次循环，开始执行下一次循环；`break`跳出循环，不再执行循环

## 复杂数据类型
### 数组
1. 创建数组
- `var arr = new Array();`
- `var arr = [1, 2, 'js', true];`
2. 获取数组元素
- 数组名[索引号]，例如`arr[0]`
- 索引号从0开始
3. 获取数组长度
- `arr.length`
4. 数组新增元素
- `arr.length += n;` 增加n个元素，元素是`undefined`
- `arr[arr.length] = value;` 增加一个元素，值为`value`

### 函数
1. 用法
- 声明
   - `function 函数名(形参1,形参2,...){函数体}`
   - `var 变量名 = function(形参1,形参2,...){函数体}`，称为匿名函数
- 调用
   - `函数名(实参1,实参2,...);`
   - `变量名(实参1,实参2,...);`
- 形参
   - 形参和实参不匹配的问题
      - 如果实参个数多于形参，会取形参个数的实参
      - 如果实参个数少于形参，多余的形参会是`undefined`
   - `arguments`的使用：`arguments`是当前函数的一个内置对象，存储了传递的所有实参，是一个伪数组
	  - 具有数组的`length`属性
      - 按索引方式存储
      - 没有真正数组的一些方法，例如`pop()`,`push()`
2. 返回值
- `return`会终止函数
- `return`只能返回一个值，可以通过返回数组返回多个值
- 如果没有`return`语句，则返回`undefined`
3. 函数预解析(函数提升)
- 把所有函数声明提升到当前作用域最前面

### 对象
1. 创建对象
- 字面量创建
   ```
	var obj = {
		uname: 'zhangsan',
		age: 18,
		sayHi: function(){
			console.log('hi');
		}
	}
	```
- new object创建
   ```
	var obj = new Object();
	obj.uname = 'zhangsan';
	obj.age = 18;
	obj.sayHi = function(){
			console.log('hi');
		}
	```
- 构造函数创建
   ```
	function 构造函数名(){
		this.属性 = 值;
		this.方法 = function(){
			
		}
	}
	new 构造函数名();
	```
- 注意：
   - 构造函数不需要`return`语句，返回一个对象
   - 使用构造函数必须加`new`
   - 属性和方法前面必须添加`this`
2. 使用对象
- 使用属性
   ```
	obj.uname
	obj['uname']
	```
- 调用方法
   ```
	obj.sayHi()
	```
3. 遍历对象属性
```
	for(var k in obj){
		console.log(k);//属性名
		console.log(obj[k]);//属性值
	}
```

### 使用示例
```
一、函数
1、arguments获取函数实参
function f(){
	for(let i=0; i<arguments.length; i++){
		console.log(arguments[i]);
	}
}
f(1, 2, 3, 4);
//输出1 2 3 4
2、函数形参与实参个数不一致
function f(a, b){
	console.log(a);
	console.log(b);
}
f(1, 2, 3);										//1 2
f(1);											//1 undefined
f(1, 2);										//1 2
3、如果没有return语句，函数的返回值为undefined
function f(a, b){
	console.log(a);
	console.log(b);
}
var c = f(1, 2);
console.log(c);									//undefined

二、数组
1、向空数组逐个添加元素
var arr = new Array();
for(let i=0; i<10; i++){
	arr[arr.length++] = i;
}
for(let i=0; i<arr.length; i++){
	console.log(arr[i]);
}

三、对象
1、创建对象
var obj1 = {
		uname: 'zhangsan',
		age: 18,
		sayHi: function(){
			console.log('hi');
		}
	}
var obj2 = new Object();
	obj2.uname = 'zhangsan';
	obj2.age = 18;
	obj2.sayHi = function(){
			console.log('hi');
		}
function MakeObj(uname, hello){
		this.uname = uname;
		this.hello = hello;
		this.sayHi = function(){
			console.log(this.hello);
		}
	}
var obj3 = new MakeObj('lisi', 'nihao');
2、遍历对象属性
for(var k in obj1){
		console.log(k);//属性名
		console.log(obj1[k]);//属性值
	}
for(var k in obj2){
		console.log(k);//属性名
		console.log(obj2[k]);//属性值
	}
for(var k in obj3){
		console.log(k);//属性名
		console.log(obj3[k]);//属性值
	}
3、使用对象
console.log(obj1.uname);
console.log(obj2['age']);
obj3.sayHi();
```

## 内置对象
### Math对象
不是一个构造函数，不需要`new`创建对象，直接使用
1. Math.PI
- 一个属性，圆周率
2. Math.max() Math.min()
- 方法，求最大值(最小值)
- 输入参数为一组数字
- 如果参数有非数字，返回`NaN`
- 如果没有参数，返回`-Infinity`和`Infinity`
1. Math.abs()
- 方法，求绝对值
- 有隐式转换，可以将字符串数字转为数字
- 如果是其他字符串，返回`NaN`
4. Math.floor()
- 向下取整
5. Math.ceil()
- 向上取整
6. Math.round()
- 四舍五入，`-3.5`结果是`-3`
7. Math.random()
- 返回一个随机的`[0, 1)`之间的小数

### Date对象
1. 构造函数，必须new创建对象后使用 var date = new Date(); 
- 如果没有参数，返回当前时间
- 参数常用的写法
   - 数字型 `var date1 = new Date(2019,10,01)` 创建的日期是`2019-11-1`，比输入的月份大1
   - 字符串型 `var date1 = new Date('2019-10-1 8:8:8')`
2. 日期格式化
- `date.getFullYear()`返回年
- `date.getMonth()`返回月，0~11，得到的月份比实际小一个月
- `date.getDate()`返回的是几号
- `date.getDay()`返回周几，周日是0
- `date.getHours()`返回当前小时
- `date.getMinutes()`返回当前分钟
- `date.getSeconds()`返回当前秒
3. 获取时间戳，即当前时间距1970-01-01过去的毫秒数
- `date.valueOf();`
- `date.getTime();`
- `var date1 = +new Date();` `date1`存储的就是过去的总毫秒数
- `Date.now()` H5新增的方法，不需要实例化

### 数组对象
1. 创建数组
- 字面量
  - `var arr1 = [2, 3];`
- newArray
  - `var arr1 = new Array(2);` 数组长度为2，里面有两个空的数组元素
  - `var arr1 = new Array(2, 3);` 等价于`var arr1 = [2, 3]`;
2. 检测是否为数组
- instanceof
  - `console.log(arr instanceof Array);`
- Array.isArray ie9以上才支持
  - `console.log(Array.isArray(arr));`
3. 添加删除元素
- push()
  - 在数组末尾添加一个或多个元素
    - `arr.push(4);` 添加一个元素
    - `arr.push(4, 'pink');` 添加两个元素
- 返回值是新数组的长度
  - unshift()
    - 在数组的开头添加元素
      - `arr.unshift(4, 'purple');`
  - 返回值是新数组的长度
- pop()
  - 删除数组最后的一个元素
    - `arr.pop()`
  - 返回值是删除的那个元素
- shift()
  - 删除数组第一个元素
    - `arr.shift()`
  - 返回值是删除的那个元素
4. 数组排序
- reverse()
  - 翻转数组
  - `arr.reverse();`
- sort()
  - 默认排序
    - `arr.sort()`按第一位数字升序排序，第一位相同再按第二位，13会排在7前面
  - 升序
  	```
	arr.sort(function(a, b){
  		return a - b;
  	});
	```
  - 降序
```
  	arr.sort(function(a, b){
  		return b - a;
  	});
```
5. 数组索引
- indexOf
  - `arr.indexOf('blue')`
  - 返回元素的索引号
  - 如果有两个相同元素，只返回第一个满足条件的索引号
  - 如果找不到匹配元素，返回-1
- lastIndexOf
  - `arr.lastIndexOf('blue')`
  - 从后往前查
6. 数组转为字符串
- toString()
  - `arr.toString()` 元素之间用逗号分隔
- join(分隔符)
  - `arr.join()`  如果不写分隔符，默认用逗号
7. 数组拼接截取
- concat()
  - 连接两个或多个数组 
  - 不影响原数组
  - 返回一个新的数组
- slice(begin, end)
  - 返回被截取项目的新数组，返回的数组是`[begin: end)`
  - 不影响原数组
- splice(begin, num)
  - 删除从`begin`开始的`num`个元素
  - 返回被删除元素组成的新数组
  - 影响原数组
  - 原数组减少了`num`个元素，新数组有`num`个元素

### 字符串对象
1. 基本包装类型：
- 将简单数据类型包装成为了复杂类型，相当于以下三步
```
var temp = new String('andy');
var str = temp;
temp = null;
```
- 基本包装类型包括Number, Boolean, String
2. 不可变性
- 因为字符创的不可变性，不要大量拼接字符串
3. 查找字符在字符串中索引
- `indexOf`
  - `str.indexOf('要查找的字符',[起始位置])`
    - 返回第一个匹配的位置
  - lastIndexOf
    - 从后往前查
4. 查找字符串中特定位置的字符
- `charAt`
  - `str.charAt(index)`
  - 返回特定位置的字符
- `charCodeAt(index)`
  - 返回特定位置字符的ascall码
- `str[index]`
  - 返回特定位置的字符
  - H5新增的，有兼容性问题
5. 拼接、截取字符串
- `concat(str1, str2, ...)`
  - 相当于`+`
- `substr(start, length)`
  - 截取从`start`位置开始`length`个字符组成的子字符串
- `slice(start, end)`
  - 截取`[start, end)`子字符串
- `substring(start, end)`
  - 与`slice`相比，不能取负值
6. 替换字符
- `str.replace('a','A')`
  - 只会替换查到的第一个匹配字符
7. 字符串转为数组
- `str.split('分隔符')`
8. 大小写转换
- `str.toUpperCase()`
  - 转为大写
- `str.toLowerCase()`
  - 转为小写

### 数据存储
- 简单数据类型(值类型)：
  - string number boolean undefined null
  - null的类型是对象
```
	var a = null;
	console.log(typeof a);
	输出是object
```
- 复杂数据类型(引用类型)：通过new关键字创建的对象(系统对象、自定义对象)，如Object、Array、Date等
- 堆和栈
  - 简单数据类型直接在栈里存放数据值：简单数据类型传参是值传递，函数里面对形参的操作不会影响实参的值
  - 复杂数据类型首先在栈里存放地址，地址指向堆里的数据值：复杂数据类型传参是地址传递，函数里面对形参的操作会影响实参的值

### 使用示例
```
//一、Math对象应用
//1. 查看Math.PI的值
console.log(Math.PI)
//输出3.141592653589793
console.log('-------------------spliter-------------------');
//2. 使用Math.max()、Math.min()方法
console.log(Math.max());
console.log(Math.max(0,1,2,3,4));
console.log(Math.max([0,1,2,3,4]));
console.log(Math.max(1,2,3,'red'));
//输出为-Infinity 4 NaN NaN
console.log(Math.min());
console.log(Math.min(0,1,2,3,4));
console.log(Math.min([0,1,2,3,4]));
console.log(Math.min(1,2,3,'red'));
//输出为Infinity 0 NaN NaN
console.log('-------------------spliter-------------------');
//3. 使用Math.abs()方法
console.log(Math.abs('-100'));
console.log(Math.abs('hello'));
//输出为100 NaN
console.log('-------------------spliter-------------------');
//4. Math.floor()向下取整
console.log(Math.floor(3.5));
console.log(Math.floor(-3.5));
console.log(Math.floor('3.5'));
//输出为3 -4 3
console.log('-------------------spliter-------------------');
//5. Math.ceil()向上取整
console.log(Math.ceil(3.5));
console.log(Math.ceil(-3.5));
//输出为4 -3
console.log('-------------------spliter-------------------');
//6. Math.round()四舍五入，`-3.5`结果是`-3`
console.log(Math.round(3.5));
console.log(Math.round(-3.5));
//输出为4 -3
console.log('-------------------spliter-------------------');
//7. Math.random()返回一个随机的`[0, 1)`之间的小数
console.log(Math.random());
console.log('-------------------spliter-------------------');
//8. 求两个整数之间的随机整数，并且包含这两个整数
var max = 10;
var min = 0;
var result = Math.floor(Math.random()*(max-min+1)) + min;
console.log(result);
console.log('-------------------spliter-------------------');
//二、Date对象使用
//1. 创建date对象，
var date = new Date();
var date2 = new Date(2019,10,01);
var date3 = new Date('2019-10-1 8:8:8');
console.log(date);
//没有参数返回当前时间：Fri Oct 08 2021 09:32:24 GMT+0800 (中国标准时间)
console.log(date2);
//数值型参数返回参数对应的时间，但月份加1：Fri Nov 01 2019 00:00:00 GMT+0800 (中国标准时间)
console.log(date3);
//字符串型参数返回参数对应的时间：Tue Oct 01 2019 08:08:08 GMT+0800 (中国标准时间)
console.log('-------------------spliter-------------------');
//2. 日期格式化
console.log(date.getFullYear());//返回年
console.log(date.getMonth());//返回月，0~11，得到的月份比实际小一个月
console.log(date.getDate());//返回的是几号
console.log(date.getDay());//返回周几，周日是0
console.log(date.getHours());//返回当前小时
console.log(date.getMinutes());//返回当前分钟
console.log(date.getSeconds());//返回当前秒
//依次输出2021 9 8 5 9 32 24
//3. 获取时间戳，即当前时间距1970-01-01过去的毫秒数
console.log(date.valueOf());
console.log(date.getTime());
console.log(+new Date());
console.log(Date.now());//H5新增的方法，不需要实例化
//依次输出：1633657049616 1633657049616 1633657049619 1633657049619
console.log('-------------------spliter-------------------');
//三、数组对象
//1. 创建数组
var arr = [2, 3, 4, 5, 6];
var arr2 = new Array(2);//数组长度为2，里面有两个空的数组元素
var arr3 = new Array(2, 3);//等价于var arr1 = [2, 3]
console.log(arr);
//输出：(5) [2, 3, 4, 5, 6]
console.log(arr2);
//输出：(2) [empty × 2]
console.log(arr3);
//输出：(2) [2, 3]
console.log('-------------------spliter-------------------');
//2. 检测是否为数组
console.log(arr instanceof Array);
console.log(arr2 instanceof Array);
console.log(arr3 instanceof Array);
console.log(Array.isArray(arr));//ie9以上才支持
console.log(Array.isArray(arr2));
console.log(Array.isArray(arr3));
//输出都是true
console.log('-------------------spliter-------------------');
//3. 添加删除元素
console.log(arr.push(7));//末尾添加一个元素
//输出：6
console.log(arr);
//输出：(6) [2, 3, 4, 5, 6, 7]
console.log(arr.push(8, 'pink'));//末尾添加两个元素，返回值是新数组的长度
//输出：8
console.log(arr);
//输出：(8) [2, 3, 4, 5, 6, 7, 8, 'pink']
console.log(arr.unshift(14, 15));//开头添加两个元素，返回值是新数组的长度
//输出：10
console.log(arr);
//输出：(10) [14, 15, 2, 3, 4, 5, 6, 7, 8, 'pink']
console.log(arr.pop());//删除数组最后的一个元素，返回值是删除的那个元素
//输出：pink
console.log(arr);
//输出：(9) [14, 15, 2, 3, 4, 5, 6, 7, 8]
console.log(arr.shift());//删除数组第一个元素，返回值是删除的那个元素
//输出：14
console.log(arr);
//输出：(8) [15, 2, 3, 4, 5, 6, 7, 8]
console.log('-------------------spliter-------------------');
//4. 数组排序
console.log(arr.reverse());//翻转数组，返回翻转后的数组
//输出：(8) [8, 7, 6, 5, 4, 3, 2, 15]
console.log(arr);
//输出：(8) [8, 7, 6, 5, 4, 3, 2, 15]
console.log(arr.sort());//按第一位数字升序排序，第一位相同再按第二位，13会排在7前面，返回排序后的数组
//输出：(8) [15, 2, 3, 4, 5, 6, 7, 8]
console.log(arr);
//输出：(8) [15, 2, 3, 4, 5, 6, 7, 8]
console.log(arr.sort(function(a, b){
  		return a - b;
  	}));//升序
//输出：(8) [2, 3, 4, 5, 6, 7, 8, 15]
console.log(arr);
//输出：(8) [2, 3, 4, 5, 6, 7, 8, 15]
console.log(arr.sort(function(a, b){
  		return b - a;
  	}));//降序
//输出：(8) [15, 8, 7, 6, 5, 4, 3, 2]
console.log(arr);
//输出：(8) [15, 8, 7, 6, 5, 4, 3, 2]
console.log('-------------------spliter-------------------');
//5. 数组索引
arr.push(15);
console.log(arr.indexOf(8));//从前往后查
//输出：1
console.log(arr.indexOf(15));//如果有两个相同元素，只返回第一个满足条件的索引号
//输出：0
console.log(arr.indexOf(9));//如果找不到匹配元素，返回-1
//输出：-1
console.log(arr.lastIndexOf(8));//从后往后查
//输出：1
console.log(arr.lastIndexOf(15));
//输出：8
console.log(arr.lastIndexOf(9));
//输出：-1
console.log('-------------------spliter-------------------');
//6. 数组转为字符串
console.log(arr.toString());//元素之间用逗号分隔
//输出：15,8,7,6,5,4,3,2,15
console.log(arr);
//输出：(9) [15, 8, 7, 6, 5, 4, 3, 2, 15]
console.log(arr.join('-'));//如果不写分隔符，默认用逗号
//输出：15-8-7-6-5-4-3-2-15
console.log(arr);
//输出：(9) [15, 8, 7, 6, 5, 4, 3, 2, 15]
console.log('-------------------spliter-------------------');
//7. 数组拼接截取
console.log(arr.concat(arr2,arr3));//连接两个或多个数组,不影响原数组,返回一个新的数组
//输出：(13) [15, 8, 7, 6, 5, 4, 3, 2, 15, empty × 2, 2, 3]
console.log(arr);
//输出：(9) [15, 8, 7, 6, 5, 4, 3, 2, 15]
console.log(arr2);
//输出：(2) [empty × 2]
console.log(arr3);
//输出：(2) [2, 3]
console.log(arr.slice(2, 5));//返回被截取项目的新数组，返回的数组是[begin: end),不影响原数组
//输出：(3) [7, 6, 5]
console.log(arr);
//输出：(9) [15, 8, 7, 6, 5, 4, 3, 2, 15]
console.log(arr.splice(2, 3));//删除从`begin`开始的`num`个元素,返回被删除元素组成的新数组,影响原数组,原数组减少了`num`个元素，新数组有`num`个元素
//输出：(3) [7, 6, 5]
console.log(arr);
//输出：(6) [15, 8, 4, 3, 2, 15]
console.log('-------------------spliter-------------------');
//四、字符串对象
//1. 查找字符在字符串中索引
var str = 'hello, my name is lufengyang. ';
console.log(str.indexOf('a', 5));//从5开始查找'a'在str中的索引，只返回第一个匹配
//输出：11
console.log(str.lastIndexOf('a', 5));//从后往前找
//输出：-1，没有找到匹配
console.log('-------------------spliter-------------------');
//2. 查找字符串中特定位置的字符
console.log(str.charAt(3));
//输出：l
console.log(str.charCodeAt(3));//返回特定位置字符的ascall码
//输出：108
console.log(str[3]);//H5新增的，有兼容性问题
//输出：l
console.log('-------------------spliter-------------------');
//3. 拼接、截取字符串
var str1 = 'nice to meet you. ';
var str2 = 'how are you?';
console.log(str.concat(str1, str2));
//输出：hello, my name is lufengyang. nice to meet you. how are you?
console.log(str);
//输出：hello, my name is lufengyang. 
console.log(str.substr(5, 8));//截取从5位置开始8个字符组成的子字符串
//输出：, my nam
console.log(str);
//输出：hello, my name is lufengyang. 
console.log(str.slice(5, -2));//截取[5, -2)子字符串
//输出：, my name is lufengyang
console.log(str);
//输出：hello, my name is lufengyang. 
console.log(str.slice(5, 8));//截取[5, 8)子字符串,与slice相比，不能取负值
//输出：, m
console.log(str);
//输出：hello, my name is lufengyang. 
console.log('-------------------spliter-------------------');
//4. 替换字符
console.log(str.replace('a','A'));//只会替换查到的第一个匹配字符
//输出：hello, my nAme is lufengyang. 
console.log(str);
//输出：hello, my name is lufengyang. 
console.log('-------------------spliter-------------------');
//5. 字符串转为数组
var arr = str.split(' ');
console.log(arr);
//输出：(6) ['hello,', 'my', 'name', 'is', 'lufengyang.', '']
console.log(str);
//输出：hello, my name is lufengyang. 
console.log('-------------------spliter-------------------');
//6. 大小写转换
console.log(str.toUpperCase());//转为大写
//输出：HELLO, MY NAME IS LUFENGYANG. 
console.log(str);
//输出：hello, my name is lufengyang. 
console.log(str.toLowerCase());//转为小写
//输出：hello, my name is lufengyang. 
```

# 文档对象模型DOM
## 概述
- 文档对象模型，是W3C推荐的处理可扩展标记语言的标准编程接口
- DOM树
  - document 文档(页面)
  - element 元素(标签)
  - node 节点(标签、属性、文本、注释)

## 元素操作
### 获取元素
0. console.dir(obj) 查看对象的详细信息
1. document.getElementByID('id名')
- 通过id获取元素
- 参数是字符串
- 返回的是元素对象
2. document.getElementsByTagName('标签名')
- 通过标签名获取元素集合
- 参数是字符串
- 返回的是元素对象集合，以伪数组形式存储
- 可以指定父元素，限定搜索范围，父元素必须是指定的单个元素
```
var ul = document.getElementsByTagName('ul')[0];
lis = ul.getElementsByTagName('li');
```
3. document.getElementsByClassName('类名')
- H5新增的方法，要考虑兼容性
- 通过类名获取元素集合
4. document.querySelector('选择器')
- H5新增的方法，要考虑兼容性
- 根据选择器获取元素，只返回匹配的第一个元素对象
```
var el = querySelector('.box');
var el = querySelector('#nav');
var el = querySelector('li');
```
5. document.querySelectorAll('选择器')
- H5新增的方法，要考虑兼容性
- 返回匹配的所有元素集合，以伪数组形式存储
6. document.body 获取body元素对象
7. document.documentElement 获取html对象

### 操作元素
1. 获取元素属性值
- element.属性 只能获取内置属性值
- element.getAttribute('属性') 能获取内置属性和自定义属性
- H5新增获取属性方法 
  - 自定义属性规范：以data-开头，例如data-index,data-list-name
  - 获取方法(ie11以上才兼容)：element.dataset.index, element.dataset.listName, element.dataset['index'], element.dataset['listName']
2. 修改元素内容
- element.innerText = '修改内容';
- element.innerHTML = '<strong>修改</strong>内容';
- 这两个元素都是可读写的，区别在于innerHTML可以识别标签，保留空格和换行
3. 修改元素属性
- 常见内置属性
  - img.src img.title
  - input.type value checked disabled
- element.属性 = 属性值 修改内置属性属性值，`element.className = 'red';`
```
//1、先在css中声明一个类及其样式
.类名{
	color: pink;
	width: 100px;
}
//2、让元素类名等于第一步中定义的类，新增的类名是覆盖元素原来的类名的，可以通过this.className = '旧类名 新类名'定义多类名
this.className = '旧类名 新类名';
```
- element.setAttribute('属性', '值') 既能修改内置属性也能修改自定义属性，`element.setAttribute('class', 'red');`
4. 移除属性
- element.removeAttribute('index');
5. 修改元素样式
- element.style 行内样式操作 backgroundColor width
- element.className 类名样式操作


## 节点操作
### 节点属性
- nodeType 元素节点为1，属性节点为2，文本节点为3(包含文字、空格、换行等)
- nodeName
- nodeValue

### 获取节点
1. parentNode
- element.parentNode 离得最近的父级节点
- 如果没有父节点，返回null
2. childNode
- element.childNode 获取所有的子节点，可以通过nodeType筛选出元素节点
- element.children 只读，只返回所有的子元素节点
- element.firstChild 第一个子节点，不管是文本节点还是元素节点
- element.lastChild
- element.firstElementChild 第一个元素节点 IE9以上支持
- element.lastElementChild 最后一个元素节点
3. 兄弟节点
- nextSibling 返回当前元素的下一个兄弟节点，包括元素和文本，找不到返回null
- previousSibling 返回当前元素的上一个兄弟节点
- nextElementSibling 下一个兄弟元素节点，ie9以上可用
- previousElementSibling 上一个兄弟元素节点

### 操作节点
1. 创建节点 document.createElement('tagName')，
- 示例`var li = document.createElement('li');`
2. 添加节点 
- node.appendChild(child) 添加的节点在最后
```
var ul = document.querySelector('ul')
ul.appendChild(li)
```
- node.insertBefore(child, 指定元素) 在指定的子节点前添加节点
```
var lili = document.createElement('li');
ul.insertBefore(lili, ul.children[0]);	
```
3. 删除节点 node.removeChild(child) 
- 返回值是删除的节点
- 示例`ul.removeChild(ul.children[0])`

4. 复制节点 node.cloneNode()
- 如果参数为空或false，浅拷贝，只复制标签不复制内容
- 参数设为true，深拷贝，既复制标签也复制内容
- 示例
```
var lili = ul.children[0].cloneNode(true);
ul.appendChild(lili);
```

### 三种动态创建元素的方法对比
1. document.write()
- 示例 `document.write('<div>123</div>');`
- 将内容写入页面的内容流，但是文档流执行完毕后它会导致页面全部重绘，导致其它页面元素消失
2. innerHTML
- 示例`div.innerHTML = '<div>123</div>';`
3. document.createElement('tagName')
4. 注意：生成很多标签时，采用innerHTML拼接字符串的方法效率最低，采用数组拼接效率最高；采用createElement效率介于两者中间。

## 事件
### 常用事件
1. 常用的鼠标事件
- onclick            左键点击触发
- onmouseover        鼠标经过触发
- onmouseout         鼠标离开触发
- onfocus            获得鼠标焦点触发
- onblur             失去鼠标焦点触发
- onmousemove        鼠标移动触发
- onmouseup          鼠标弹起触发
- onmousedown        鼠标按下触发
- mouseover和mouseenter的区别
  - mouseover/mouseout向上冒泡：假如给父盒子添加mouseover事件，鼠标经过父盒子会触发，鼠标经过子盒子也会触发，这是由于子盒子没有监听，会将事件向上冒泡，触发父盒子的mouseover事件
  - mouseenter/mouseleave不冒泡：经过子盒子，即使子盒子没有监听，也不会触发父盒子的鼠标经过事件
2. 常用的键盘事件
- onkeyup            某个键盘按键被松开时触发
- onkeydown          某个键盘按键被按下时触发
- onkeypress         某个键盘按键被按下时触发，但不识别功能键，如ctrl shift 上下左右键
- 三个事件的执行顺序 down press up
- keydown和keypress在文本框里的特点：keydown事件触发的时候，文字还没有落入文本框中；keyup事件触发的时候文字已经落入文本框

### 注册事件
1. 传统方式
- btn.onclick = function(){}
- 注册事件的唯一性：如果一个事件绑定两个处理函数，后一个会将前一个覆盖
2. 方法监听方式
- eventTarget.addEventListener(type, listener[, useCapture])  
  - eventTarget：目标对象
  - type：事件类型字符串，click、mouseover，不带on
  - listener：事件处理函数，事件发生时会调用该监听函数
  - useCapture：可选参数，是一个布尔值，默认是false
  - 同一个元素的同一个事件可以绑定多个处理函数
  - ie9之前用attachEvent()代替 eventTarget.attachEvent(eventNameWithOn, callback)
    - eventNameWithOn：事件类型字符串，onclick、onmouseover，带on
    - callback：事件处理函数
- 示例
```
btn.addEventListener('click', function(){
	alert('hi');
})
```
	
### 删除事件(解绑事件)
1. 传统方式 
- btn.onclick = null;
2. 方法监听方式
- eventTarget.removeEventListener(type, listener[, useCapture])
- eventTarget.detachEvent(eventNameWithOn, callback)
- 考虑到删除，注册事件时不要用匿名函数

### DOM事件流
1. 捕获阶段 
- 捕获的顺序是document -> html -> body -> father -> son，如果上述元素都注册了事件，则从上往下触发
- 注册捕获阶段事件eventTarget.addEventListener(type, listener, true)
- js只能监听捕获、冒泡的一种，不可以都捕获
2. 当前目标阶段
3. 冒泡阶段
- 冒泡的顺序与捕获完全相反
- 注册冒泡事件eventTarget.addEventListener(type, listener, false)
- 实际开发中更关注事件冒泡
- 有些事件没有冒泡，例如onblur、onfocus、onmouseenter、onmouseleave
4. 阻止事件冒泡
- 标准写法，考虑兼容性
```
if(e && e.stopPropagation){
 e.stopPropagation();
}else{
 window.event.cancelBubble = true;
}
```

### 事件对象
1. 定义：
- event是一个事件对象，写在监听函数的形参位置
- 事件对象只有有了事件才会存在，是系统创建的
- 事件对象是有关事件的一系列数据的集合，比如鼠标点击事件对象包含了鼠标坐标等信息，键盘事件包含了按下了哪个按键等信息
- 可以自己命名，不一定非得叫event，例如evt、e
- ie6、7、8需要通过window.event获取事件对象，兼容写法：`e = e || window.event`
2. 常见事件对象属性和方法：
- e.target 触发事件的对象，this是绑定事件的对象
- e.currentTarget 与this相似
- e.type 返回事件类型 click、mouseover、mouseout
- 阻止默认行为，如链接跳转
  - e.preventDefault() 可用于事件监听注册方式，也可用于传统注册方式，有兼容性问题
```
1、禁止鼠标右键弹出菜单
//contextmenu主要控制应该何时显示上下文菜单，主要用于程序员取消默认的上下文菜单
document.addEventListener('contextmenu', function(e){
	  e.preventDefault();
	 })
2、禁止鼠标选中
//selectstart开始选中
document.addEventListener('selectstart', function(e){
	e.preventDefault();
	})
```
  - e.returnValue 只限于传统注册方式，用于低版本浏览器 
  - return false 只限于传统的注册方式，没有兼容性问题
3. 事件委托(代理、委派)
- 原理：不是每个子节点单独设置事件监听器，而是事件监听器设置在其父节点上，然后利用冒泡原理影响设置每一个子节点
- 作用：只操作一次DOM，提高了程序的性能
4. 鼠标事件对象MouseEvent
- e.clientX          返回鼠标相对于浏览器窗口可视区的x坐标
- e.clientY          返回鼠标相对于浏览器窗口可视区的y坐标
- e.pageX            返回鼠标相对于文档页面的x坐标 ie9+支持
- e.pageY            返回鼠标相对于文档页面的y坐标 ie9+支持
- e.screenX          返回鼠标相对于电脑屏幕的x坐标
- e.screenY          返回鼠标相对于电脑屏幕的y坐标
5. 键盘事件对象
- e.keyCode          相应键的ascall码值，keyup和keydown事件不区分字母的大小写，a和A都得到65;keypress区分大小写，a：97，A：65

# 浏览器对象模型BOM
## 概述
- javascript语法的标准化组织是ECMA
- DOM的标准化组织是W3C
- BOM最初是Netscape浏览器标准的一部分，兼容性比较差，DOM包含在BOM里面

## 常用对象
### window对象
1. 简介
- window对象是浏览器的顶级对象，它具有双重角色
- 它是js访问浏览器窗口的一个接口
- 它是一个全局对象，定义在全局作用域中的变量、函数会变成window对象的属性和方法，在调用时可以省略window，如alert()、prompt()等，`window.name`是window的内置属性，所以其它变量不要使用name这个名字
2. 常见事件
- 窗口加载事件		
  - 页面全部加载完毕才执行，这样就可以将js代码放到上方
  - 传统注册方式：`window.onload = function(){}` 只能写一次，如果写多次，只有最后一个生效，前面的被覆盖
  - 添加监听事件方式：`window.addEventListener('load', function(){})`
- DOM加载事件
  - DOM加载完成触发(不包括样式表、图片、flash等)，ie9以上支持
  - 用于图片很多的站，图片未加载完也可以有交互效果
  - `document.addEventListener('DOMContentLoaded', function(){})`	
- 调整窗口大小事件
  - 只要浏览器大小发生变化就触发
  - 传统注册方式：`window.resize = fuction(){}`
  - 添加监听事件方式：`window.addEventListener('resize', function(){})`
3. 定时器
- window.setTimeout(调用函数, [延迟的毫秒数])
  - 函数仅执行一次，类似定时炸弹
  - 经常给定时器取个名字区分不同的定时器
```
var timer1 = setTimeout(callback1, 2000);
var timer2 = setTimeout(callback2, 5000);
```
  - window.clearTimeout(timeoutId)清除
- window.setInterval(callback, [间隔的毫秒数])
  - 函数可以执行很多次，类似定时中断
  - window.clearInterval(intervalID)清除

### location对象
1. url
- protocol://host [:port]/path/[?query]#fragment
  - query 参数，以键值对形式，用&符号分隔开
  - fragment 片段 常见于链接 锚点
2. 常用属性
- location.href                   整个url
- location.host                   域名
- location.port                   端口号
- location.pathname               路径，对应url中的path
- location.search                 参数，对应url中的query
- location.hash                   片段，对应url中的fragment
3. 常用方法
- location.assign(url)            跟href一样，可以跳转页面(重定向页面)，可以后退
- location.replace(url)           替换当前页面，因为不记录历史，所以不能后退
- location.reload()               重新加载页面，相当于刷新(F5)，如果参数未true强制刷新(ctrl+F5)，只刷新不强制刷新有可能读的是本地的缓存网页

### navigator对象
1. 常用属性
- navigator.userAgent             包含浏览器相关信息，可用于判断用户使用哪个终端打开页面

### history对象
1. 常用方法
- history.back()                  后退到上一页面
- history.forward()               前进功能
- history.go(参数)                前进后退功能，参数如果是1前进一个页面，参数如果是-1后退一个页面

## this指向问题
- 全局、普通函数内、定时器回调函数内，指向window
- 对象的方法中，指向对象
- 构造函数中，指向实例对象

## js执行机制
1. 同步任务 
- 同步任务都在主线程上执行，形成一个执行栈
2. 异步任务
- js中的异步是通过回调函数实现的，放到一个任务队列(消息队列)里执行
  - 普通事件，例如click、resize
  - 资源加载，例如load、error
  - 定时器，例如setinterval、settimeout
3. 执行机制
- 先执行执行栈中的同步任务
- 遇到回调函数，把回调函数放入任务队列中
- 继续执行同步任务
- 执行栈中同步任务执行完之后，系统按次序读取任务队列中的异步任务，被读取的异步任务结束等待状态，进入执行栈，开始执行
4. 事件循环
- 主线程不断地重复获得任务、执行任务、再获取任务、再执行，这种机制称为事件循环

# js动画
## 元素动态属性
1. 元素偏移量offset
- 常用属性
  - element.offsetParent                      返回该元素具有定位属性的父级元素，如果父级都没有定位则返回body
  - element.offsetTop                         返回元素相对带有定位的父级元素上方的偏移
  - element.offsetLeft                        返回元素相对带有定位的父级元素左方的偏移
  - element.offsetWidth                       返回自身包括padding、border、content的宽度，返回值没有单位
  - element.offsetHeight                      返回自身包括padding、border、content
- 与style的区别
  - style只能获得行内样式表中的样式值        	offset可以得到任意样式表中的样式值
  - style.width获得的是带单位的字符串        	offsetWidth获得的是没有单位的数字
  - style.width不包含padding和border        	offsetWidth包含padding、border、content
  - style.width可读可写                     	offsetWidth只读的高度，返回值没有单位
- 获取用offset，设置用style
2. 元素可视区client
- 常用属性
  - element.clientTop                          返回元素上边框的大小
  - element.clientLeft                         返回元素左边框的大小
  - element.clientWidth                        返回元素padding和content的宽度，不含边框，没有单位
  - element.clientHeight                       返回元素padding和content的高度，不含边框，没有单位
3. 元素滚动区scroll
- 常用属性
  - element.scrollTop                          返回被卷去的上册距离，没有单位
  - element.scrollLeft                         返回被卷去的左侧距离，没有单位
  - element.scrollWidth                        返回自身实际宽度，不含边框，没有单位
  - element.scrollHeight                       返回自身实际高度，不含边框，没有单位
- 用法
  - 给盒子设置overflow: auto; 内容超出盒子范围时盒子会多出滚动条
  - 滚动条滚动时会触发onscroll事件
- 页面滚动
  - 事件源是document
  - 页面卷去的头部  window.pageYOffset document.documentElement.scrollTop(声明了DTD) document.body.scrollTop(未声明DTD)
  - 页面卷去的左侧  window.pageXOffset
- 与client的区别
  - 如果内容超出盒子边框，scrollHeight会大于盒子高度，而clientHeight依然等于盒子高度

## 动画核心原理
- 通过定时器定时移动一定的距离
- 缓慢动画原理 
  - 步进值 = (目标值 - 现在位置) / 10
  - 向左移动计算步长值需要向上取整，如果向下取整当距离target小于10就不会动了，向右移动向下取整
- 注意
  - 获取元素位置用offsetLeft，更改元素位置用style.left
  - 同时被很多对象调用的函数中声明的局部变量最好声明为对象的一个属性(定时器)，这样不容易引起歧义
  - 函数中给对象添加新属性之前应判断这个属性(定时器)是否已经添加过，避免频繁调用导致对象有大量同名属性
- 小技巧
  - 手动调用事件函数 obj.click()
  - 节流阀 flag控制事件函数的执行时机，防止点击过于频繁
  - 回调函数写法 callback && callback();
  - 窗口滚动 `window.scroll(x, y);` 参数没有单位，00滚动到最顶最左
- mouseover和mouseenter的区别
  - mouseover/mouseout向上冒泡：假如给父盒子添加mouseover事件，鼠标经过父盒子会触发，鼠标经过子盒子也会触发，这是由于子盒子没有监听，会将事件向上冒泡，触发父盒子的mouseover事件
  - mouseenter/mouseleave不冒泡：经过子盒子，即使子盒子没有监听，也不会触发父盒子的鼠标经过事件

## 示例
```
//animate.js
function animate(obj, target, callback){
	clearInterval(obj.interval);
	obj.interval = setInterval(function(){
		if(obj.offsetLeft == target){
			clearInterval(obj.interval);
			if(callback){
				callback();
			}
		}else{				
			var step = (target - obj.offsetLeft) / 10;
			step = step>0 ? Math.ceil(step) : Math.floor(step);
			obj.style.left = obj.offsetLeft + step + 'px';
		}
	}, 15);
}	
//轮播图.js
window.addEventListener('load', function(){
	var box = document.querySelector('.box');
	var pictures = document.querySelector('.pictures');
	var dots = document.querySelector('.dots');
	var leftArrow = document.querySelector('.leftarrow');
	var rightArrow = document.querySelector('.rightarrow');
	var iniPos = 0;
	var index = 0;
	var stepLength = pictures.children[0].offsetWidth;
	var flag = true;
	
	//动态生成小圆点，使小圆点的数量与图片的数量一致
	for(var i=0; i<pictures.children.length; i++){
		var li = document.createElement('li');
		dots.appendChild(li);
		var a = document.createElement('a');
		a.href = 'javascript:;';
		a.setAttribute('indexVal', i);
		li.appendChild(a);
	}
	dots.children[0].children[0].className = 'current';
	
	//将第一张图复制一份放到最后，实现无缝滚动效果
	var first = pictures.children[0].cloneNode(true);
	var last = pictures.children[pictures.children.length-1].cloneNode(true);
	pictures.appendChild(first);
	pictures.insertBefore(last, pictures.children[0]);
	iniPos = -stepLength;
	pictures.style.left = iniPos + 'px';
	
	//设置定时器，2s切一次图
	var interval = setInterval(function(){
		index++;
		// index = index>dots.children.length-1 ? 0 : index; 
		setPage();
	}, 2000);
	
	//如果使用mouseover，鼠标经过box的子元素(pictures、leftArrow)也会触发事件，导致的结果是：尽管鼠标一直在box内，却有可能多次触发事件
	//故此处使用mouseenter
	box.addEventListener('mouseenter', function(){
		// console.log('jingguole');
		leftArrow.style.display = 'block';
		rightArrow.style.display = 'block';
		clearInterval(interval);
	})
	
	box.addEventListener('mouseleave', function(){
		// console.log('likaile');
		leftArrow.style.display = 'none';
		rightArrow.style.display = 'none';
		interval = setInterval(function(){
			index++;
			// index = index>dots.children.length-1 ? 0 : index; 
			setPage();
		}, 2000);
	})
	
	leftArrow.addEventListener('click', function(){
		if(flag){
			flag = false;
			index--;
			// index = index>dots.children.length-1 ? 0 : index; 
			setPage();
		}		
	})
	
	rightArrow.addEventListener('click', function(){
		if(flag){
			flag = false;
			index++;
			// index = index<0 ? dots.children.length-1 : index;
			setPage();
		}		
	})
	
	dots.addEventListener('click', function(e){
		index = e.target.getAttribute('indexVal');
		setPage();
	})
	
	function setPage(){
		// console.log(iniPos-index*stepLength);
		
		if(index == dots.children.length){
			animate(pictures, iniPos-index*stepLength, function(){
				index = 0;
				// setDots(index);
				pictures.style.left = iniPos-index*stepLength + 'px';
				flag = true;
			});		
			setDots(0);
		}else if(index == -1){
			animate(pictures, iniPos-index*stepLength, function(){
				index = dots.children.length-1;
				setDots(index);
				pictures.style.left = iniPos-index*stepLength + 'px';
				flag = true;
			});
			setDots(dots.children.length-1);
		}else{
			animate(pictures, iniPos-index*stepLength, function(){
				flag = true;
			});
			setDots(index);
		}
		// console.log(index);
	}

	function setDots(index){
		for(var i=0; i<dots.children.length; i++){
			dots.children[i].children[0].className = '';
		}
		dots.children[index].children[0].className = 'current';
	}
})
```

# 移动端事件、属性
## 触屏事件
1. 事件
- touchstart 手指触摸DOM对象
- touchmove  手指在DOM对象上移动
- touchend   手指离开DOM对象
2. 事件对象
- e.touches            正在触摸屏幕的所有手指的一个列表
- e.targetTouches      正在触摸当前DOM元素上的手指的一个列表，列表中每个元素有以下属性
  - clientX、clientY
  - pageX、pageY
  - screenX、screenY
  - target
- e.changedTouches     手指状态发生了改变的列表，从无到有，从有到变化

## 拖动元素
1. 拖动元素过程中依次触发的事件
- 触摸元素touchstart：获取手指初始坐标，同时获得盒子原来的位置
- 移动手指touchmove：计算手指的滑动距离，并且移动盒子
- 手指离开touchend
2. 注意：
- 手指移动也会触发屏幕滚动，所以这里要阻止默认的屏幕滚动e.preventDefault();

## click延时解决方案
1. 移动端click事件会有300ms的延时，原因是移动端屏幕双击会缩放(double tap to zoom)页面
2. 解决方案
- 禁用缩放
- 利用touch事件自己封装
  - 当手指触摸屏幕，记录当前触摸时间
  - 当手指离开屏幕，用离开时间减去触摸时间
  - 如果时间小于150ms并且没有滑动过屏幕，就定义为点击
- 使用fastclick.js
  - 引入js文件
  - 复制usage代码放到代码头部

## 插件
1. fastclick.js
- 解决click延时问题
- 用法：引入js文件，复制usage代码放到代码头部
2. swiper插件
- 引入轮播图
- 用法
  - 引入相关文件 swiper.min.css、swiper.min.js
  - 按照规定语法使用
    - 复制示例的结构到页面中，更改内容
    - 复制示例样式到css文件中，更改内容
    - 复制示例js到js文件中，更改内容
  - 参数更改 查看官网API文档，可以查看js代码的参数的意义以及如何更改
3. zy.media.js  
- 使视频组件在不同浏览器中显示的外观相同
4. 其它插件
	superslide.js
	iscroll.js

## 本地存储
1. 特性
- 数据存储在用户浏览器中
- 设置、读取方便、页面刷新不丢失数据
- 容量较大，sessionStorage约5M，localStorage约20M
- 只能存储字符串，可以将对象JSON.stringify()编码后存储
2. window.sessionStorage
- 特征：
  - 生命周期为关闭浏览器窗口
  - 在同一个窗口(页面)下数据可以共享
  - 以键值对的形式存储使用
- 存储数据：`sessionStorage.setItem(key, value)`
- 获取数据：`sessionStorage.getItem(key)`
- 删除某个数据：`sessionStorage.removeItem(key)`
- 删除全部数据：`sessionStorage.clear()`
- 更改数据：`sessionStorage.setItem(key, value)`
3. window.localStorage
- 特征：
  - 生命周期永久生效
  - 可以多窗口共享(同一浏览器)
  - 以键值对形式存储使用
- 存储数据：`localStorage.setItem(key, value)`
- 获取数据：`localStorage.getItem(key)`
- 删除某个数据：`localStorage.removeItem(key)`
- 删除全部数据：`localStorage.clear()`
- 更改数据：`localStorage.setItem(key, value)`
4. 记住用户名案例
```
//html
<input type="checkbox" name="" id="">
//js,可以通过change事件监听状态是否变化
input.addEventListener('change',function(){

})
```
	
# ES5语法进阶
## 构造函数和原型
1. ES5创建对象的三种方式
- 字面量 
```
var obj = {
	uname: '刘德华',
	age: 18
	sing: function(){
		console.log('我会唱歌');
	}
};
```
- new Object() 
```
var obj = new Object();
obj.uname = 'zhangsan';
	obj.age = 18;
	obj.sing = function(){
		console.log('我会唱歌');
	}		
```
- 自定义构造函数
```
function Star(uname, age){
	this.uname = uname;
	this.age = age;
	this.sing = function(){
		console.log('我会唱歌');
	}
}
var ldh = new Star('刘德华', 18);
```
- new构造函数会做四件事情：
  - 在内存中创建一个新的空对象
  - 让this指向这个新的对象
  - 执行构造函数里面的代码，给这个新对象添加属性和方法
  - 返回这个新对象(所以构造函数里面不需要return)
2. 静态成员和实例成员
- 实例成员
  - 构造函数内部通过this添加的成员，例如uname、age、sing
  - 它们只能通过实例化的对象来访问(ldh.uname)，不可以通过构造函数访问(Star.uname)
- 静态成员
  - 直接在构造函数本身上添加的成员
    - `Star.sex = '男';`sex就是静态成员
    - 静态成员只能通过构造函数来访问，不可以通过实例化的对象来访问
3. 构造函数原型对象prototype	
- 构造函数的问题：浪费内存，每一个对象的成员函数会各自占用内存空间
- 构造函数原型对象prototype
  - 每一个构造函数都有一个prototype属性，指向另一个对象
  - 构造函数通过原型分配的函数是所有对象共享的
  - 把那些不变的方法直接定义在prototype对象上，这样所有对象的实例就可以共享这些方法
```
Star.prototype.sing = function(){
		console.log('我会唱歌');
	}
ldh = new Star();
ldh.sing();
```
4. 对象原型`__proto__`
- 每一个对象都会有一个`__proto__`属性指向其构造函数的原型对象`prototype`，所以对象实例可以使用构造函数原型定义的方法
- 方法的查找规则
  - 首先看对象实例身上是否有sing方法，如果有就直接执行
  - 如果没有，再去查看构造函数原型对象prototype身上查找
5. constructor
- 原型对象`prototype`和对象原型`__proto__`都有一个constructor属性，指向创建对象的构造函数
- 如果以对象的形式向`prototype`对象赋值，constructor属性会被覆盖，所以需手动添加constructor属性
6. 原型链
- `ldh.__proto__ === Star.prototype`                     true
- `Star.prototype.__proto__ === Object.prototype`        true
- `Object.prototype.__proto__ === null`                  true
- `Star.prototype.constructor === Star`                  true
- `Object.prototype.constructor === Object`              true
- 实例对象的方法寻找顺序按照原型链顺次向上，直到找到为止
![js原型链](./imgs/js原型链.png)
7. this指向问题
- 构造函数中this指向对象实例
- 原型对象中的函数中this也指向对象实例
- call()调用函数，并且改变函数运行时this的指向
```
fun.call(thisArg, arg1, arg2, ...)
	thisArg：当前调用函数this的指向对象
```
8. 扩展内置对象
- 给内置对象Array添加求和方法
```
Array.prototype.sum = function(){
	var sum = 0;
	for(var i=0; i<this.length; i++){
		sum += this[i];
	}
	return sum;
}
var arr = {1, 2, 3};
console.log(arr.sum());
```


	组合继承
		父构造函数
			function Father(uname, age){
				// this指向父构造函数的对象实例
				this.uname = uname;
				this.age = age;
			}
			Father.prototype.money = function(){
				console.log(100000);
			}
		子构造函数
			function Son(uname, age, score){
				// this指向子构造函数的对象实例
				Father.call(this, uname, age);
				this.score = score;
			}
			// 将Son的原型对象指向一个Father构造函数实例化的对象，从而间接访问Father.prototype中的函数
			Son.prototype = new Father();
			// 重新指定Son.prototype.constructor，否则它会指向Father(因为Son.prototype是一个Father的实例对象)
			Son.prototype.constructor = Son;
			// 如果直接让Son.prototype = Father.prototype，相当于两个实例对象指向同一地址，Son.prototype中定义的函数Father的对象实例也能访问
			Son.prototype.exam = function(){
				console.log(100);
			}
		实例化
			var son = new Son('ldh', 18, 100);
类的本质
	类的本质还是一个函数，可以认为类是构造函数的另一种写法
		类有原型对象prototype
		类原型对象prototype里面有constructor属性，指向类本身
		类可以通过原型对象添加方法
		类实例化的对象也有__proto__属性，指向类的原型对象
	ES6的类绝大部分功能ES5都可以做到，ES6的类其实就是语法糖
ES5中的新增方法
	数组方法
		迭代(遍历)方法
			forEach()
				arr.forEach(function(value, index, array){
					
				})
					value: 每一个数组元素的值
					index：每一个数组元素的索引号
					array：数组本身
					arr.forEach(function(value, index, array){
						sum += value;
					})
					回调函数中return不会终止迭代
			map()
			filter()
				array.filter(function(value, index, arr){
					
				})
					filter方法创建一个新数组，数组中元素是通过检查指定数组中符合条件的所有元素
					它直接返回一个新数组
					var newArr = array.filter(function(value, index, arr){
						return value >= 20;
					})
					遇到return不会终止迭代
			some()
				array.some(function(value, index, arr){
					
				})
					检测数组中有没有满足条件的元素，如果有返回true，否则返回false
					如果找到第一个满足条件的元素则终止循环
					var flag = array.some(function(value, index, arr){
						return value >= 20;
					})
					回调函数中遇到return true就会终止迭代
			every()
	字符串方法
		var str1 = str.trim();
			trim方法可以去除字符串两侧的空格
	对象方法
		Object.defineProperty()
			定义对象新属性或修改原有属性的值
			Object.defineProperty(obj, prop, descriptor)
				obj：对象
				prop：属性
				dexcriptor：说明，以对象形式{}书写
					value：属性值，默认undefined
					writable：值是否可以重写，默认false
					enumerable：目标属性是否可以被枚举，默认为false，即不允许遍历(bj.keys()取不到)
					configurable目标属性是否可以被删除或是否可以再次修改特性，默认false
函数进阶
	函数的定义方式
		1、function fn(){}
		2、var fn = function(){}
		3、var f = new Function('参数1', '参数2', '函数体');
		所有函数都是Function的实例对象
	函数的调用方式
		普通函数
			function fn(){}
				1、fn();
				2、fn.call();
		对象的方法
			var o = {
				sayHi: function(){}
			}
				o.sayHi();
		构造函数
			function Star(){}
				var ldh = new Star();
		绑定事件函数
			btn.onclick = function(){}
				点击按钮调用
		定时器函数
			setInterval(function(){}, 1000)
				定时器每隔1s调用一次
		立即执行函数
			(function(){})()
				自动调用
	函数中this指向问题
		普通函数
			指向window
		对象中函数
			指向对象o
		构造函数
			指向ldh这个实例对象
			构造函数原型对象中的函数中this也指向ldh这个实例对象
		绑定事件函数
			指向函数的调用者btn，即绑定事件的对象
		定时器函数
			指向window
		立即执行函数
			指向window
	改变函数内部this指向
		call方法
			fn.call(thisArg, arg1, arg2, ...)
			主要应用：类继承
		apply方法
			fn.apply(thisArg, [argArray])
			调用函数，改变this指向
			参数必须是数组
			主要应用：借助Math内置对象求最大值
				var arr = [1, 2, 3, 4, 5];
				var max = Math.max.apply(Math, arr);
		bind方法
			fn.bind(thisArg, arg1, arg2, ...);
			不调用函数，只改变函数内部this的指向；返回由指定的this值和初始化参数改造的原函数拷贝
				var f = fn.bind(o, 1, 2);
				f();
			典型应用：发送验证码按钮，将定时器函数中this指向btn
				btn.onclick = function(){
					this.disabled = true;
					setTimeout(function(){
						this.disabled = false;
					}.bind(this), 3000)
				}
	严格模式
		开启严格模式
			整个脚本
				在脚本最上面写上'use strict';
			某个函数
				function fn(){
					'use strict';
					...
				}
		严格模式中的变化
			变量：
				变量必须先声明再使用
				不能随意删除已经声明好的变量 delete num
			this：
				全局作用域中的this是undefined，非严格模式是window
				如果构造函数不加new调用，this会报错
				定时器中this还是指向window
			函数：
				不允许函数参数有重名情况
				不允许在非函数代码块中声明函数
	高阶函数
		传递过来的参数是函数，或返回值是函数
	闭包
		变量作用域
			全局
			局部
		闭包
			某个作用域有权访问另一个函数作用域中变量，被访问变量所在作用域称为闭包，fn就是闭包
			闭包的作用是延申了变量的作用范围
			function fn(){
				var num = 0;
				function fun(){
					console.log(num);
				}
				// fun();
				return fun;
			}
			fn();
			var f = fn();
			f();
		闭包的应用
			for(var i=0; i<lis.length; i++){
				(function(i){
					lis[i].onclick = function(){
						console.log(i);
					}
				})(i);
			}
			立即执行函数也称为小闭包，因为立即执行函数中的任何一个函数都可以使用它的i变量
	递归
		自己调用自己的函数
		栈溢出 stack overflow
			必须要加退出条件，利用return语句退出
		利用递归求阶乘
			function fn(n){
				if(n == 1){
					return 1;
				}
				return n * fn(n-1);
			}
			fn(100);
		利用递归求斐波那契数列
			function fn(n){
				if(n == 1){
					return 1;
				}
				return fn(n-1)+fn(n-2);
			}
			fn(100);
	浅拷贝和深拷贝
		浅拷贝
			对象级别的数据只是把地址拷贝过去，新数据和旧数据指向内存同一地址
				直接赋值
				Object.assign(o, obj); 把obj浅拷贝给o
		深拷贝
			对象级别的数据在内存中开辟新空间，新数据和旧数据指向内存不同地址
				递归赋值
正则表达式
	创建
		1、var regexp = new RegExp(/123/);
		2、var regexp = /123/;
	测试
		regexObj.test(str);
		只要包含123就符合
		符合返回true，不符合返回false
	特殊字符(元字符)
		边界符
			^abc                     以abc开头
			abc$                     以abc结尾
			^abc$                    只有'abc'才匹配
		字符类                       
			[abc]                    a、b、c出现任意一个就匹配
			[a-z]                    任意一个小写字母
			[A-Z]                    任意一个大写字母
			[0-9]                    任意一个数字
			[a-zA-Z0-9_-]            判断用户名
			[^a-z]                   不能包含小写字母
		量词符                       
			*                        0~n次
			+                        1~n次
			?                        0、1次
			{n}                      n次
			{n,}                     大于等于n次
			{m,n}                    大于等于m次小于等于n次,中间不能有空格
			^[a-zA-Z0-9_-]{6,16}$    判断用户名
		预定义类
			\d                       [0-9]数字
			\D                       [^0-9]非数字
			\w                       [a-zA-Z0-9_]字母数字下划线
			\W                       [^a-zA-Z0-9_]
			\s                       [\t\r\n\v\f]空格、换行符、制表符
			\S                       [^\t\r\n\v\f]
		或符号 |
			var reg = /^\d{3}-\d{8}|\d{4}-\d{7}$/
	括号总结
		中括号                  字符集合，匹配方括号中的任意字符
		大括号                  量词符，里面表示重复次数
		小括号                  表示优先级
	菜鸟工具
		https://c.runoob.com/
	正则替换
		newStr = stringObject.replace(regEx, 替换文本);
		如果正则表达式不加参数，只替换第一个
		正则表达式参数
			g                   全局匹配    /123/g
			i                   忽略大小写  /123/i
			gi                  全局匹配且忽略大小写    /123/gi
# 查阅文档
- MDN：https://developer.mozilla.org/zh-CN/
- WebAPI：浏览器API，DOM和BOM https://developer.mozilla.org/zh-CN/docs/Web/API