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
- element.setAttribute('属性', '值') 既能修改内置属性也能修改自定义属性，`element.setAttribute('class', 'red');`
4. 移除属性
- element.removeAttribute('index');
5. 修改元素样式
- element.style 行内样式操作 backgroundColor width
- element.className 类名样式操作
```
//1、先在css中声明一个类及其样式
.类名{
	color: pink;
	width: 100px;
}
//2、让元素类名等于第一步中定义的类，新增的类名是覆盖元素原来的类名的，可以通过this.className = '旧类名 新类名'定义多类名
this.className = '旧类名 新类名';
```

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
BOM概述
	标准
		javascript语法的标准化组织是ECMA
		DOM的标准化组织是W3C
		BOM最初是Netscape浏览器标准的一部分，兼容性比较差，DOM包含在BOM里面
	BOM的构成
		window对象是浏览器的顶级对象，它具有双重角色
			它是js访问浏览器窗口的一个接口
			它是一个全局对象，定义在全局作用域中的变量、函数会变成window对象的属性和方法，在调用时可以省略window，如alert()、prompt()等，window.name是window的内置属性，所以其它变量不要使用name这个名字
window对象的常见事件
	窗口加载事件		
		页面全部加载完毕才执行，这样就可以将js代码放到上方
		传统注册方式：
			window.onload = function(){
				
			}
			只能写一次，如果写多次，只有最后一个生效，前面的被覆盖
		添加监听事件方式：
			window.addEventListener('load', function(){
				
			})
		DOM加载事件
			document.addEventListener('DOMContentLoaded', function(){
				
			})
			DOM加载完成触发(不包括样式表、图片、flash等)，ie9以上支持
			用于图片很多的站，图片未加载完也可以有交互效果
	调整窗口大小事件
		只要浏览器大小发生变化就触发
		window.resize = fuction(){}
		window.addEventListener('resize', function(){})
	定时器
		window.setTimeout(调用函数, [延迟的毫秒数]);
			函数仅执行一次，类似定时炸弹
			经常给定时器取个名字区分不同的定时器
				var timer1 = setTimeout(callback1, 2000);
				var timer2 = setTimeout(callback2, 5000);
			window.clearTimeout(timeoutId);
				clearTimeout(timer1);
		window.setInterval(callback, [间隔的毫秒数]);
			函数可以执行很多次，类似定时中断
			window.clearInterval(intervalID);
this指向问题
	全局、普通函数内、定时器回调函数内，指向window
	对象的方法中，指向对象
	构造函数中，指向实例对象
js执行机制
	同步单线程，异步多线程
	同步任务
		同步任务都在主线程上执行，形成一个执行栈
	异步任务
		js中的异步是通过回调函数实现的，放到一个任务队列(消息队列)里执行
			普通事件，例如click、resize
			资源加载，例如load、error
			定时器，例如setinterval、settimeout
	执行机制
		1、先执行执行栈中的同步任务
		2、遇到回调函数，把回调函数放入任务队列中
		3、继续执行同步任务
		4、执行栈中同步任务执行完之后，系统按次序读取任务队列中的异步任务，被读取的异步任务结束等待状态，进入执行栈，开始执行
	事件循环
		主线程不断地重复获得任务、执行任务、再获取任务、再执行，这种机制称为事件循环
location对象
	url
		protocol://host [:port]/path/[?query]#fragment
			query 参数，以键值对形式，用&符号分隔开
			fragment 片段 常见于链接 锚点
	location对象属性
		location.href                   整个url
		location.host                   域名
		location.port                   端口号
		location.pathname               路径，对应url中的path
		location.search                 参数，对应url中的query
		location.hash                   片段，对应url中的fragment
	location对象方法
		location.assign(url)            跟href一样，可以跳转页面(重定向页面)，可以后退
		location.replace(url)           替换当前页面，因为不记录历史，所以不能后退
		location.reload()               重新加载页面，相当于刷新(F5)，如果参数未true强制刷新(ctrl+F5)，只刷新不强制刷新有可能读的是本地的缓存网页
navigator对象
	常用属性
		navigator.userAgent             包含浏览器相关信息，可用于判断用户使用哪个终端打开页面
history对象
	常用方法
		history.back()                  后退到上一页面
		history.forward()               前进功能
		history.go(参数)                前进后退功能，参数如果是1前进一个页面，参数如果是-1后退一个页面

# 查阅文档
- MDN：https://developer.mozilla.org/zh-CN/
- WebAPI：浏览器API，DOM和BOM https://developer.mozilla.org/zh-CN/docs/Web/API