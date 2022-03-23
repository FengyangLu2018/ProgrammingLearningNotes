# 配置文件manifest.json
## 示例
```json
{
    "app": {
        "background": {
            "scripts": ["background.js"]
        }
    },
	//配置文件版本
    "manifest_version": 2,
	//扩展程序名称
    "name": "My Extension",
	//扩展程序版本
    "version": "versionString",
	//默认语言
    "default_locale": "en",
	//简单描述
    "description": "A plain text description",
	//图标
    "icons": {
        "16": "images/icon16.png",
        "48": "images/icon48.png",
        "128": "images/icon128.png"
    },
    "browser_action": {
        "default_icon": {
            "19": "images/icon19.png",
            "38": "images/icon38.png"
        },
        "default_title": "Extension Title",
        "default_popup": "popup.html"
    },
    "page_action": {
        "default_icon": {
            "19": "images/icon19.png",
            "38": "images/icon38.png"
        },
        "default_title": "Extension Title",
        "default_popup": "popup.html"
    },
	//后台运行的脚本程序
    "background": {
        "scripts": ["background.js"]
    },
	//在网页中注入的脚本文件
    "content_scripts": [
        {
            "matches": ["http://www.google.com/*"],
            "css": ["mystyles.css"],
            "js": ["jquery.js", "myscript.js"]
        }
    ],
	//扩展程序用户配置选项
    "options_page": "options.html",
	//扩展程序申请的权限
    "permissions": [
        "*://www.google.com/*"
    ],
	//
    "web_accessible_resources": [
        "images/*.png"
    ]
}
```
## 必填属性
Chrome扩展的Manifest必须包含`name`、`version`和`manifest_version`属性，对于应用来说，还必须包含`app`属性。
# 打开页面时注入脚本
通过Chrome扩展我们可以对用户当前浏览的页面进行操作，实际上就是对用户当前浏览页面的DOM进行操作。通过Manifest中的`content_scripts`属性可以指定将哪些脚本何时注入到哪些页面中，当用户访问这些页面后，相应脚本即可自动运行，从而对页面DOM进行操作。
## 注入时机
- 如果URL匹配`mathces`值的同时也匹配`include_globs`的值，会被注入；如果URL匹配`exclude_matches`的值或者匹配`exclude_globs`的值，则不会被注入。
- `css`和`js`对应要注入的样式表和JavaScript。
- `run_at`定义了何时进行注入。
- `all_frames`定义脚本是否会注入到嵌入式框架中。

## 共享资源规则
`content_scripts`中的脚本只是共享页面的DOM，而并不共享页面内嵌JavaScript的命名空间。也就是说，如果当前页面中的JavaScript有一个全局变量`a`，`content_scripts`中注入的脚本也可以有一个全局变量`a`，两者不会相互干扰。当然你也无法通过`content_scripts`访问到页面本身内嵌JavaScript的变量和函数。

# 跨域请求
## 跨域定义
<table>
	<tr>
	    <th>URL</th>
	    <th>说明</th>
	    <th>是否允许请求</th>  
	</tr >
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">同域下</td>
		<td rowspan="2" align="center">允许</td>
	</tr>
	<tr>
	    <td>http://a.example.com/a.txt</td>
	</tr>
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">同域不同目录</td>
		<td rowspan="2" align="center">允许</td>
	</tr>
	<tr>
	    <td>http://a.example.com/b/a.txt</td>
	</tr>
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">同域不同端口</td>
		<td rowspan="2" align="center">不允许</td>
	</tr>
	<tr>
	    <td>http://a.example.com:8080/a.txt</td>
	</tr>
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">同域不同协议</td>
		<td rowspan="2" align="center">不允许</td>
	</tr>
	<tr>
	    <td>https://a.example.com/a.txt</td>
	</tr>
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">不同域</td>
		<td rowspan="2" align="center">不允许</td>
	</tr>
	<tr>
	    <td>http://b.example.com/a.txt</td>
	</tr>
	<tr>
	    <td>http://a.example.com/</td>
	    <td rowspan="2">不同域</td>
		<td rowspan="2" align="center">不允许</td>
	</tr>
	<tr>
	    <td>http://a.foo.com/a.txt</td>
	</tr>
</table>

## 申请跨域权限
在Manifest的`permissions`属性中声明需要跨域的权限，下例展示了一款获取维基百科数据并显示在其他网页中的扩展的权限申请。
```json
{
    ...
    "permissions": [
        "*://*.wikipedia.org/*"
    ]
}
```
## 异步请求
```js
function httpRequest(url, callback){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4) {
            callback(xhr.responseText);
        }
    }
    xhr.send();
}

var html;
httpRequest('test.txt', function(result){
    html = result;
    console.log(html);
});
```
使用回调函数才能将非阻塞函数的最终结果传递下去。在Chrome扩展应用的API中，大部分函数都是非阻塞函数。

# 常驻后台自动运行
## 需求
有时我们希望扩展不仅在用户主动发起时（如开启特定页面或点击扩展图标等）才运行，而是希望扩展自动运行并常驻后台来实现一些特定的功能，比如实时提示未读邮件数量、后台播放音乐等等。
## 配置方法
在Manifest中指定`background`域可以使扩展常驻后台，包含三种属性，分别是`scripts`、`page`和`persistent`。
- `scripts`属性，指定后台页面包含的所有脚本。
- `page`属性，指定后台页面。通常我们只需要使用`scripts`属性即可，除非在后台页面中需要构建特殊的HTML，一般情况下后台页面的HTML我们是看不到的。
- `persistent`属性定义了常驻后台的方式——当其值为true时，表示扩展将一直在后台运行，无论其是否正在工作；当其值为false时，表示扩展在后台按需运行，这就是Chrome后来提出的Event Page。Event Page可以有效减小扩展对内存的消耗，如非必要，请将persistent设置为false。persistent的默认值为true。

## 示例
```json
//manifest.json
{
    ......
    "background": {
        "scripts": [
            "js/status.js"
        ]
    },
    ......
```
```javascript
//status.js
function httpRequest(url, callback){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4) {
            callback(true);
        }
    }
	//onerror事件能捕捉到请求过程中是否发生了错误
    xhr.onerror = function(){
        callback(false);
    }
    xhr.send();
}

setInterval(function(){
    httpRequest('http://www.google.cn/', function(status){
        chrome.browserAction.setIcon({path: 'images/'+(status?'online.png':'offline.png')});
    });
},5000);
```
## 注意
如果想在用户打开浏览器之前就让扩展运行，可以在Manifest的`permissions`属性中加入`"background"`，但除非必要，否则尽量不要这么做，因为大部分用户不喜欢这样。

# 选项页面
## 需求
有一些扩展允许用户进行个性化设置，这样就需要向用户提供一个选项页面。
## 配置方法
- Chrome通过Manifest文件的`options_page`属性为开发者提供了这样的接口，可以为扩展指定一个选项页面，指定`options_page`属性后，扩展图标上的右键菜单会包含“选项”链接。
- 当用户在扩展图标上点击右键，选择菜单中的“选项”后，就会打开这个页面。对于没有图标的扩展，可以在chrome://extensions页面中单击“选项”。

## 示例
本例共5个文件，`manifest.json`文件是扩展程序的配置文件；`popup.html`和`weather.js`两个文件构成了天气预报主页面，点击扩展程序图标弹出；`options.html`和`options.js`构成了选项页面，右击扩展程序图标--选项弹出。
```json
//manifest.json
{
    "manifest_version": 2,
    "name": "天气预报",
    "version": "1.0",
    "description": "查看未来两周的天气情况",
    "icons": {
        "16": "images/icon16.png",
        "48": "images/icon48.png",
        "128": "images/icon128.png"
    },
    "browser_action": {
        "default_icon": {
            "19": "images/icon19.png",
            "38": "images/icon38.png"
        },
        "default_title": "天气预报",
        "default_popup": "popup.html"
    },
    "options_page": "options.html",
    "permissions": [
        "http://api.openweathermap.org/data/2.5/forecast?q=*"
    ]
}
```
```html
<!-- options.html -->
<html>
    <head>
        <title>设定城市</title>
    </head>
    <body>
        <input type="text" id="city" />
        <input type="button" id="save" value="保存" />
        <script src="js/options.js"></script>
    </body>
</html>
```
```js
// options.js
var city = localStorage.city || 'beijing';
document.getElementById('city').value = city;
document.getElementById('save').onclick = function(){
    localStorage.city = document.getElementById('city').value;
    alert('保存成功。');
}
```
```html
<!-- popup.html -->
<html>
<head>
<style>
* {
    margin: 0;
    padding: 0;
}

body {
    width: 520px;
    height: 270px;
}

table {
    font-family: "Lucida Sans Unicode", "Lucida Grande", Sans-Serif;
    font-size: 12px;
    width: 480px;
    text-align: left;
    border-collapse: collapse;
    border: 1px solid #69c;
    margin: 20px;
    cursor: default;

}

table th {
    font-weight: normal;
    font-size: 14px;
    color: #039;
    border-bottom: 1px dashed #69c;
    padding: 12px 17px;
    white-space: nowrap;
}

table td {
    color: #669;
    padding: 7px 17px;
    white-space: nowrap;
}

table tbody tr:hover td {
    color: #339;
    background: #d0dafd;
}

</style>
</head>
<body>
<div id="weather"></div>
<script src="js/weather.js"></script>
</body>
</html>
```
```js
//weather.js
function httpRequest(url, callback){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4) {
            callback(xhr.responseText);
        }
    }
    xhr.send();
}

function showWeather(result){
    result = JSON.parse(result);
    var list = result.list;
    var table = '<table><tr><th>日期</th><th>天气</th><th>最低温度</th><th>最高温度</th></tr>';
    for(var i in list){
        var d = new Date(list[i].dt*1000);
        table += '<tr>';
        table += '<td>'+d.getFullYear()+'-'+(d.getMonth()+1)+'-'+d.getDate()+'</td>';
        table += '<td>'+list[i].weather[0].description+'</td>';
        table += '<td>'+Math.round(list[i].temp.min-273.15)+' °C</td>';
        table += '<td>'+Math.round(list[i].temp.max-273.15)+' °C</td>';
        table += '</tr>';
    }
    table += '</table>';
    document.getElementById('weather').innerHTML = table;
}

var city = localStorage.city;
city = city?city:'beijing';
var url = 'http://api.openweathermap.org/data/2.5/forecast/daily?q='+city+',china&lang=zh_cn';
httpRequest(url, showWeather);
```

# 扩展页面间的通信
## 需求
有时需要让扩展中的多个页面之间，或者不同扩展的多个页面之间相互传输数据，以获得彼此的状态。比如音乐播放器扩展，当用户鼠标点击popup页面中的音乐列表时，popup页面应该将用户这个指令告知后台页面，之后后台页面开始播放相应的音乐。
## 注意
Chrome提供的大部分API是不支持在`content_scripts`中运行的，但`runtime.sendMessage`和`runtime.onMessage`可以在`content_scripts`中运行，所以扩展的其他页面也可以同`content_scripts`相互通信。
## API
### chrome.runtime.sendMessage(extensionId, message, options, callback)
- `extensionId`为所发送消息的目标扩展，如果不指定这个值，则默认为发起此消息的扩展本身；
- `message`为要发送的内容，类型随意，内容随意，比如可以是`'Hello'`，也可以是`{action: 'play'}`、`2013`和`['Jim', 'Tom', 'Kate']`等等；
- `options`为对象类型，包含一个值为布尔型的`includeTlsChannelId`属性，此属性的值决定扩展发起此消息时是否要将TLS通道ID发送给监听此消息的外部扩展，`options`是一个可选参数；
- `callback`是回调函数，用于接收返回结果，同样是一个可选参数。

### chrome.runtime.onMessage.addListener(callback)
- 此处的`callback`为必选参数，为回调函数。`callback`接收到的参数有三个，分别是`message`、`sender`和`sendResponse`，即消息内容、消息发送者相关信息和相应函数。其中`sender`对象包含4个属性，分别是`tab`、`id`、`url`和`tlsChannelId`，`tab`是发起消息的标签

## 示例
本例共包含4个文件，点击扩展图标后`popup.js`向扩展自身发送`'Hello'`，`background.js`收到信息之后回复`Hello from background.'`，`popup.js`收到回复之后将回复内容写在`popup.html`页面上。
```json
// manifest.json
{
    "manifest_version": 2,
    "name": "扩展内部通信Demo",
    "version": "1.0",
    "description": "扩展内部通信Demo",
    "browser_action": {
        "default_popup": "popup.html"
    },
    "background": {
        "scripts": [
            "js/background.js"
        ]
    }
}
```
```html
<!-- popup.html -->
<script src="js/popup.js"></script>
```
```js
// popup.js
chrome.runtime.sendMessage('Hello', function(response){
    document.write(response);
});
```
```js
// background.js
chrome.runtime.onMessage.addListener(function(message, sender, sendResponse){
    if(message == 'Hello'){
        sendResponse('Hello from background.');
    }
});
```

# 存储
## localStorage
### 限制
- 运行在不同域的JavaScript无法调用其他域localStorage的数据；
- 单个域在localStorage中存储数据的大小通常有限制，对于Chrome这个限制是5MB；（通过声明unlimitedStorage权限，Chrome扩展和应用可以突破这一限制）
- localStorage只能储存字符串型的数据，无法保存数组和对象，但可以通过join、toString和JSON.stringify等方法先转换成字符串再储存。

### 用法
- 增改查：`localStorage.namespace`或`localStorage['namespace']`
- 删除：`localStorage.removeItem('namespace')`

## Chrome存储API
### 特点
- 提供了2种储存区域，分别是`sync`和`local`。两种储存区域的区别在于，`sync`储存的区域会根据用户当前在Chrome上登陆的Google账户自动同步数据，当无可用网络连接可用时，`sync`区域对数据的读写和`local`区域对数据的读写行为一致；
- `content_scripts`可以直接读取数据，而不必通过background页面。localStorage是基于域名的，`content_scripts`是注入到用户当前浏览页面中的，如果直接读取localStorage，所读取到的数据是用户当前浏览页面所在域中的。通常的解决办法是`content_scripts`通过`runtime.sendMessage`和background通信，由background读写扩展所在域（通常是chrome-extension://extension-id/）的localStorage，然后再传递给`content_scripts`。
- 在隐身模式下仍然可以读出之前存储的数据；
- 读写速度更快；
- 用户数据可以以对象的类型保存；
- 使用Chrome存储API必须要在Manifest的`permissions`中声明`"storage"`

### 用法
#### get读取数据
```js
chrome.storage.StorageArea.get(keys, function(result){
    console.log(result);
});
```
keys可以是字符串、包含多个字符串的数组或对象。如果keys是字符串，则和localStorage的用法类似；如果是数组，则相当于一次读取了多个数据；如果keys是对象，则会先读取以这个对象属性名为键值的数据，如果这个数据不存在则返回keys对象的属性值（比如keys为{'name':'Billy'}，如果name这个值存在，就返回name原有的值，如果不存在就返回Billy）。如果keys为一个空数组（[]）或空对象（{}），则返回一个空列表，如果keys为null，则返回所有存储的数据。
#### getBytesInUse获取数据占用空间
```js
chrome.storage.StorageArea.getBytesInUse(keys, function(bytes){
    console.log(bytes);
});
```
此处的keys只能为null、字符串或包含多个字符串的数组。
#### set写入数据
```js
chrome.storage.StorageArea.set(items, function(){
    //do something
});
```
items为对象类型，形式为键/值对。items的属性值如果是字符型、数字型和数组型，则储存的格式不会改变，但如果是对象型和函数型的，会被储存为“{}”，如果是日期型和正则型的，会被储存为它们的字符串形式。
#### remove删除数据
```js
chrome.storage.StorageArea.remove(keys, function(){
    //do something
});
```
其中keys可以是字符串，也可以是包含多个字符串的数组。
#### clear删除所有数据
```js
chrome.storage.StorageArea.clear(function(){
    //do something
});
```
#### onChanged监听存储区变化
```js
chrome.storage.onChanged.addListener(function(changes, areaName){
    console.log('Value in '+areaName+' has been changed:');
    console.log(changes);
});
```
callback会接收到两个参数，第一个为changes，第二个是StorageArea。changes是词典对象，键为更改的属性名称，值包含两个属性，分别为oldValue和newValue；StorageArea为local或sync。
# Browser Actions
## 图标
### 设定默认图标
```json
"browser_action": {
    "default_icon": {
        "19": "images/icon19.png",
        "38": "images/icon38.png"
    }
}
```
Chrome会选择使用19像素的图片显示在工具栏中，但如果用户正在使用视网膜屏幕的计算机，则会选择38像素的图片显示；如果只指定一种尺寸的图片，在另外一种环境下，Chrome会试图拉伸图片去适应，这样可能会导致图标看上去很难看；default_icon也不是必须指定的，如果没有指定，Chrome将使用一个默认图标。
### 动态更换图标
`chrome.browserAction.setIcon(details, callback)`
- `details`的类型为对象，可以包含三个属性，分别是`imageData`、`path`和`tabId`
  - `imageData`的值可以是`imageData`，也可以是对象。如果是对象，其结构为`{size: imageData}`，比如`{'19': imageData}`，这样可以单独更换指定尺寸的图片。`imageData`是图片的像素数据，可以通过HTML的canvas标签获取到。
  - `path`的值可以是字符串，也可以是对象。如果是对象，结构为`{size: imagePath}`。`imagePath`为图片在扩展根目录下的相对位置。
  - `tabId`的值限定了浏览哪个标签页时，图标将被更改
  - 不必同时指定`imageData`和`path`，这两个属性都是指定图标所要更换的图片的。
- `callback`为回调函数，此函数没有可接收的回调结果。

### 示例
图标不停旋转。
```json
{
	// manifest.json
	...
	"browser_action": {
	    "default_icon": {
	        "19": "images/icon19_0.png",
	        "38": "images/icon38_0.png"
	    },
	    "default_title": "Turtle"
	},
	"background": {
	    "scripts": [
	        "js/background.js"
	    ]
	}
	...
}
```
```js
// background.js
function chgIcon(index){
    if(index === undefined){
        index = 0;
    }
    else{
        index = index%20;
    }
    chrome.browserAction.setIcon({path: {'19': 'images/icon19_'+index+'.png'}});
    chrome.browserAction.setIcon({path: {'38': 'images/icon38_'+index+'.png'}});
    setTimeout(function(){chgIcon(index+1)},50);
}

chgIcon();
```
## popup页面
- Popup页面是当用户点击扩展图标时，展示在图标下面的页面。
- 每次打开popup页面，所有的DOM和js空间变量都将被重新创建。因此popup页面更多地是用来作为结果的展示，而不是数据的处理。通常情况下，如果需要扩展实时处理数据，而不是只在用户打开时才运行，我们需要创建一个在后端一直运行的页面或者脚本。

## 标题
将鼠标移至扩展图标上，片刻后所显示的文字就是扩展的标题。
### 设置默认标题
```json
"browser_action": {
    "default_title": "Extension Title"
}
```
### 动态设置标题
```js
chrome.browserAction.setTitle({title: 'This is a new title'});
```

## badge
### 应用场景
- Badge是扩展为用户提供有限信息的另外一种方法，这种方法较标题优越的地方是它可以一直显示，其缺点是只能显示大约4字节长度的信息。
- 比较典型的应用是音乐播放器，它们使用badge显示当前音乐播放的时间；另一些内容类的应用，如邮件、微博、RSS阅读器等，则显示未读条目。

### 设置
```js
chrome.browserAction.setBadgeBackgroundColor({color: '#0000FF'});
chrome.browserAction.setBadgeText({text: 'Dog'});
chrome.browserAction.setBadgeBackgroundColor({color: [0, 255, 0, 128]});
```

# 右键菜单
## 应用场景
把自己所编写的扩展功能放到右键菜单中。
## 配置方法
首先要在Manifest的`permissions`域中声明`contextMenus`权限，同时还要在`icons`域声明16像素尺寸的图标，这样在右键菜单中才会显示出扩展的图标。
```json
"permissions": [
    "contextMenus"
],
"icons": {
    "16": "icon16.png"
}
```
## 操作菜单
### chrome.contextMenus.create(details)创建菜单
details是个对象，主要有以下属性：
- `type`：普通菜单(normal)、复选菜单(radio)、单选菜单(checkbox)和分割线(separator)，其中普通菜单还可以有下级菜单。
- `title`：在菜单中显示的文字。
- `id`：菜单id。
- `contexts`：值是数组型的，也就是说我们可以定义多种情况下显示自定义菜单，完整的选项包括all、page、frame、selection、link、editable、image、video、audio和launcher，默认情况下为page，即在所有的页面唤出右键菜单时都显示自定义菜单。其中launcher只对Chrome应用有效，如果包含launcher选项，则当用户在chrome://apps/或者其他地方的应用图标点击右键，将显示相应的自定义菜单。需要注意的是，all选项不包括launcher。
- `documentUrlPatterns`：限定页面的URL，比如我们可以限定只在Google的网站上显示自定义菜单。
- `targetUrlPatterns`：限定诸如图片、视频和音频等资源的URL。
- `onclick`：菜单被点击后就会调用onclick指定的函数。调用的函数会接收到两个参数，分别是点击后的相关信息和当前标签信息。点击后的相关信息包括菜单id、上级菜单id、媒体类型（image、video或audio）、超级链接目标、媒体URL、页面URL、框架URL、选择的文字、是否可编辑（只针对text input和textarea等控件）、用户点击前是否被选中和当前是否被选中（只针对checkbox或radio）。

### 更改、删除菜单
update方法可以动态更改菜单属性，指定需要更改菜单的id和所需要更改的属性即可。remove方法可以删除指定的菜单，removeAll方法可以删除所有的菜单。

## 示例
```json
// manifest.json
{
    "manifest_version": 2,
    "name": "Google Translate",
    "version": "1.0",
    "description": "Translate what you select with Google",
    "background": {
        "scripts": [
            "js/background.js"
        ]
    },
    "icons": {
        "16": "images/icon16.png"
    },
    "content_scripts": [
        {
            "matches": ["*://*/*"],
            "js": ["js/content.js"]
        }
    ],
    "permissions": [
        "contextMenus"
    ]
}
```
```js
// background.js
chrome.contextMenus.create({
    'type':'normal',
    'title':'使用Google翻译……',
    'contexts':['selection'],
    'id':'cn',
    'onclick':translate
});

function translate(info, tab){
    var url = 'http://translate.google.com.hk/#auto/zh-CN/'+info.selectionText ;
    window.open(url, '_blank');
}

chrome.runtime.onMessage.addListener(function(message, sender, sendResponse){
    chrome.contextMenus.update('cn',{
        'title':'使用Google翻译“'+message+'”'
    });
});
```
```js
// content.js
window.onmouseup = function(){
    var selection = window.getSelection();
    if(selection.anchorOffset != selection.extentOffset){
        chrome.runtime.sendMessage(selection.toString());
    }
}
```

# 桌面通知
## manifest配置
声明notifications权限，声明图片资源
```json
"permissions": [
    "notifications"
],
"web_accessible_resources": [
    "images/*.png"
]
```
## 应用桌面提醒
### 创建桌面提醒
只需指定标题、内容和图片即可。
```js
var notification = webkitNotifications.createNotification(
    'icon48.png',//图片
    'Notification Demo',//标题
    'Merry Christmas'//内容
);
```
### 显示桌面提醒
```js
notification.show();
```
### 定时关闭桌面提醒
```js
setTimeout(function(){
    notification.cancel();
},5000);
```
### 桌面提醒监听事件
ondisplay、onerror、onclose和onclick
# 多功能框omnibox
## manifest配置
要使用omnibox需要在Manifest的omnibox域指定keyword，同时最好指定一个16像素的图标，当用户键入关键字后，这个图标会显示在地址栏的前端。Chrome会自动将这个图标渲染成灰度图标，而无需开发者指定一个灰度的图标，由于右键菜单等其他地方也会用到16像素的图标，所以应该指定一个彩色的图标。
```json
"omnibox": { "keyword" : "hamster" },
"icons": {
    "16": "icon16.png"
}
```
## setDefaultSuggestion设置默认建议
默认建议会在用户输入keyword之后一直显示在地址栏下方并且紧挨着地址栏，所以设定一个默认建议是必要的，否则简单地显示“运行 XXX 命令：XXX”会让用户摸不到头脑。
## Omnibox事件监听
Omnibox有四种事件：onInputStarted、onInputChanged、onInputEntered和onInputCancelled，分别用于监听用户开始输入、输入变化、执行指令和取消输入行为。其中执行指令是指用户敲击回车键或用鼠标点击建议结果。
### onInputStarted
```js
chrome.omnibox.onInputStarted.addListener(function(){
	console.log('Input started.');
});
```
### onInputCancelled
```js
chrome.omnibox.onInputCancelled.addListener(function(){
	console.log('Input cancelled.');
});
```
### onInputChanged
onInputChanged事件所承接的只有一个function类型的参数，这个function参数又有两个承接参数，第一个参数是字符串型，值为用户当前的输入值，第二个参数还是function型，用于返回建议结果，建议的结果为数组型数据，数组中的元素是建议结果对象。
```js
chrome.omnibox.onInputChanged.addListener(function(text, suggest){
    suggest([{
        content: text,
        description: 'Search '+text+' in Wikipedia'
    }]);
});
```
### onInputEntered
onInputEntered事件同样只有一个function类型的承接参数，这个function有两个承接参数，第一个是用户输入的值，字符串型，第二个是对结果的建议打开方式，字符串型，但取值范围固定。
```js
chrome.omnibox.onInputEntered.addListener(function(text, disposition){
    switch(disposition){
        case 'currentTab': //do something in the current tab
                 break;
        case 'newForegroundTab': //do something in a new tab and active it
                 break;
        case 'newBackgroundTab': //do something in a new tab
                 break;
    }
});
```
## 示例
```json
// manifest.json
{
    "manifest_version": 2,
    "name": "USD Price",
    "version": "1.0",
    "description": "查询美元当日价格",
    "background": {
        "scripts": [
            "js/background.js"
        ]
    },
    "icons": {
        "16": "images/icon16.png"
    },
    "omnibox": {
        "keyword": "usd"
    },
    "permissions": [
        "*://query.yahooapis.com/*"
    ]
}
```
```js
// background.js
function httpRequest(url, callback){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4) {
            callback(xhr.responseText);
        }
    }
    xhr.send();
}

function updateAmount(amount, exchange){
    amount = Number(amount);
    if(isNaN(amount) || !amount){
        exchange([{
            'content': '$1 = ¥'+price,
            'description': '$1 = ¥'+price
        },{
            'content': '¥1 = $'+(1/price).toFixed(6),
            'description': '¥1 = $'+(1/price).toFixed(6)
        }]);
    }
    else{
        exchange([{
            'content': '$'+amount+' = ¥'+(amount*price).toFixed(2),
            'description': '$'+amount+' = ¥'+(amount*price).toFixed(2)
        },{
            'content': '¥'+amount+' = $'+(amount/price).toFixed(6),
            'description': '¥'+amount+' = $'+(amount/price).toFixed(6)
        }]);
    }
}

function gotoYahoo(text, disposition){
    window.open('http://finance.yahoo.com/q?s=USDCNY=X');
}

var url = 'http://query.yahooapis.com/v1/public/yql?q=select%20Rate%20from%20yahoo.finance.xchange%20where%20pair%20in%20(%22USDCNY%22)&env=store://datatables.org/alltableswithkeys&format=json';
var price;

httpRequest(url, function(r){
    price = JSON.parse(r);
    price = price.query.results.rate.Rate;
    price = Number(price);
});

chrome.omnibox.setDefaultSuggestion({'description':'Find current USD price.'});

chrome.omnibox.onInputChanged.addListener(updateAmount);

chrome.omnibox.onInputEntered.addListener(gotoYahoo);
```
# Page Actions
Page Actions与Browser Actions非常类似，除了Page Actions没有badge外，其他Browser Actions所有的方法Page Actions都有。另外的区别就是，Page Actions并不像Browser Actions那样一直显示图标，而是可以在特定标签特定情况下显示或隐藏，所以它还具有独有的show和hide方法。
```js
chrome.pageAction.show(integer tabId);
chrome.pageAction.hide(integer tabId);
```