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
```
# 文档对象模型DOM
# 浏览器对象模型BOM
# 查阅文档
	MDN：https://developer.mozilla.org/zh-CN/