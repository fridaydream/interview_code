###页面布局

> 假设高度已知，请写出三栏布局，其中左栏、右栏宽度各为300px，中间自适应

5种不同方法，float，position，flex，table，grid

[github代码地址](https://github.com/fridaydream/interview_code/blob/master/layout.html)

---

### css盒模型

>基本概念：标准模型+IE模型：

margin border padding content

>标准模型和IE区别

标准模型和IE宽高计算方式不同，标准模型中的宽高不包括border padding

>CSS如何设置这两种模型

```
box-sizing:border-box; /*IE*/
box-sizing:context-box; /*标准*/
```

>JS如何设置获取盒模型对应的宽和高

```
dom.style.width //只对行内设置的width起作用
dom.getCurrentStyle.width //IE计算之后的width
dom.getComputedStyle.width //非IE计算之后的width
dom.getBoundingClientRect().width //这个方法会返回top，left，right，bottom,width,height的值
```

>实例题(根据盒模型解释边距重叠)

子元素 margin-top：10px;height:100px;
父元素 height 100px;

设置父元素为BFC，如overflow:hidden;父元素height:110px;


父子，兄弟，空元素边距重叠


>BFC(边距重叠解决方案)

BFC概念：块级格式化上下文

BFC原理：BFC的垂直方向发生边距重叠

BFC的区域不会与浮动元素重叠(清除浮动)

BFC是个独立的容器，外部不会影响内部元素

BFC里面float也参与计算

创建BFC：

float不为none，position不为static或者relative，overflow不为visible，display的值为 “table-cell”, “table-caption”, or “inline-block”中的任何一个

BFC使用场景

BFC边距重叠，bfc之间不会重合，浮动元素参与计算高度

###DOM事件类

基本概念：DOM事件的级别

```
DOM0 element.onclick=function(){}
DOM2 element.addEventListener('click',function(){});
DOM3 element.addEventListener('keyup',function(){});
```

DOM事件模型

```
捕获(从上到下)和冒泡(从当前元素向上)
```

DOM事件流

```
捕获->从window到目标元素
冒泡->从目标元素到window
```

描述DOM事件捕获的具体流程

```
//window->document->document.documentElement(html)->body->ele
//true就是从上到下(捕获的监听事件)
window.addEventListener('click',function(){
 	console.log('window');
 },true);
 document.addEventListener('click',function(){
 	console.log('document');
 },true);
 document.documentElement.addEventListener('click',function(){
 	console.log('html');
 },true);
 document.body.addEventListener('click',function(){
 	console.log('body');
 },true);
 document.getElementById('ev').addEventListener('click',function(){
 	console.log('el');
 },true);
```

Event对象的常见应用

```
event.preventDefault()
event.stopPropagation()
event.stopImmediatePropagation() //监听click2个事件，用这个只执行一个click事件
event.currentTarget() //当前所绑定的事件->给父级元素的10个子元素绑定click事件，只给父级绑定事件，currentTarget为父级元素
event.target()
```
自定义事件

```
var eve=new Event('custome');
ele.addEventListener('custome',function(){
	console.log('ok');
});
ev.dispatchEvent(eve);

CustomEvent和Event一样，可以传第二个参数{}
```

###HTTP协议类

HTTP协议的主要特点

```
简单快速 ->uri 直接访问
灵活-> 每个http协议，可以设置请求头
无连接-> 连接一次就断开
无状态-> 客户端访问服务端不会记住服务端
```

HTTP报文的组成部分

```
请求报文

请求行                   请求头 空行 请求体
http协议，url，http版本     

响应报文

状态行 响应头 空行 响应体
```

HTTP方法

```
GET POST PUT DELETE HEAD
```

POST和GET的区别

```
1.GET在浏览器回退时是无害的，而POST会再次提交请求
GET不会在发请求，POST会

2.GET请求会被主动缓存，而POST不会，除非手动设置

3.GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留

4.GET有长度限制

5.GET参数通过url，POST在request body中


```

HTTP状态码

```
1xx：指示信息-表示请求已接受，继续处理
2xx：成功-表示请求已被成功接收
3xx：重定向-要完成请求必须进行更进一步的操作
4xx：客户端错误-请求有语法错误或请求无法实现
5xx：服务器错误-服务器未能实现合法的请求
```

什么是持久连接

```
HTTP协议采用“请求-应答”模式，当使用普通模式，即非Keep-Alive模式时，每个请求应答客户端服务器都要新建一个连接，完成之后立即断开连接(HTTP协议为无连接的协议)

当使用Keep-Alive模式(又称持久连接、连接重用)时，keep-alive功能使客户端到服务器端的连接持续有效，当出现对服务器的后续请求时，Keep-alive功能避免了建立或者重新建立连接。(>=1.1)
```

什么是管线化

```
在使用持久连接的情况下，某个连接上消息的传递类似于
请求1->响应1->请求2->响应2->请求3->响应3

某个连接上的消息变成了类似这样
请求1->请求2->请求3->响应1->响应2->响应3

只支持GET和HEAD
初次连接要看后台支不支持
```

###原型链类


创建对象有几种方法

```
var o1={name:'o1'};
var o11=new Object({name:'o11'});

var M=function(){this.name='o2'}
var o2=new M();

var P={name:'o3'}
var o3=Object.create(p);
```

![alt 原型链类关系](http://upload-images.jianshu.io/upload_images/8194969-8bf320973fcc1e8c..jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "原型链类关系")

```
var M= function(name){
	this.name=name;
}
var o3=new M('o3');

```
此时构造函数是M，实例是o3，原型对象是`M.prototype`

函数被声明的时候自动生成prototype属性

new 出实例的时候o3s属性`__proto__`指向M的原型对象，所以只有对象才有`__proto__`，函数也有这个属性是因为函数也是个对象，`M.__proto__===Function.prototype`


执行实例上的某个方法，先找本身有没这个方法，没有就找`__proto__`(即原型对象)有没这个方法，依次往上找，直到找到`Object`

如是：

```
M.prototype.constructor===M;
o3.__proto__===M.prototype;
o3 instanceof M===true;
o3 instanceof Object===true;
```

![alt 原型链instanceof](http://upload-images.jianshu.io/upload_images/8194969-3b7ce3f824fbfba3..jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "原型链instanceof")

`o3 instanceof M`这个判断的是实例对象的`__proto__`是不是构造函数prototype是不是引用同一个地址，构造函数的prototype也是个对象，所以有`__proto__`，所以这条链上的都是o3也是Object的实例


```
//instanceof出现这种问题的原因
o3.__proto__===M.prototype //M.prototype也是个对象
M.prototype.__proto__===Object.prototype
```

```
//判断o3是不是M的直接生成的实例
o3.__proto__.constructor===M
o3.__proto__.constructor===Object //false
```

---

new运算符

![alt new 运算符](http://upload-images.jianshu.io/upload_images/8194969-c85d8fd2e0914757..jpeg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "new 运算符")

```
var M= function(name){
	this.name=name;
}
var o3=new M('o3');
//模拟new 的效果
var new2=function(func){
	var o=Object.create(func.prototype); //创建Object
	var k=func.call(o); //执行func
	if(typeof k ==='object'){ //判断func返回值是不是object
		return k;
	}else{
		return o;
	}
}
var o6=new2(M);
o6 instanceof M===true
o6 instanceof Object===true
o6.__proto__.constructor===M
M.prototype.walk=function(){
	console.log('walk');
}
o6.walk();
o3.walk();
```

---

```
var p={name:'p'};
var o4=Object.create(p);
//o4的原型对象是p，name为什么不直接在o4对象上，因为js查找o4是个空对象，本身是没有name属性的，也就是说，在Object.create方法把参数中的对象作为新对象赋值给o4的，o4本身不具备name，通过原型链连接的
o4.__proto__===p
```

###面向对象类
>类与实例

类的声明

生成实例

>类与继承

如何实现继承

继承的几种方式

```
//借助构造函数实现继承 部分继承，原型链上的方法不被继承
 function Parent1(){
 	this.name='parent1';
 }
 Parent1.prototype.say=function(){

 }
 function Child1(){
 	Parent1.call(this);
 	this.type='child1';
 }
 // console.log(new Child1);
 //借助原型链实现继承
 function Parent2(){
 	this.name='parent2';
 	this.play=[1,2,3];
 }
 function Child2(){
 	this.type='child2';
 }
 Child2.prototype=new Parent2();
 console.log(new Child2());
 var s1=new Child2();
 var s2=new Child2();
 s1.play.push(3);
 console.log(s1.play,s2.play);//2个都push了

 //组合方式 父级构造函数执行了2次
 function Parent3(){
 	this.name='parent3';
 	this.play=[1,2,3];
 }
 function Child3(){
 	Parent3.call(this);
 	this.type='child3';
 }
 Child3.prototype=new Parent3();
 s3=new Child3();
 s4=new Child3();
 s3.play.push(4);
 console.log(s3.play,s4.play);

 // 组合继承方式优化1
 function Parent4(){
 	this.name='parent4';
 	this.play=[1,2,3];
 }
 function Child4(){
 	Parent4.call(this);
 	this.type='child4';
 }
 Child4.prototype=Parent4.prototype;
 s5=new Child4();
 s6=new Child4();
 s5.play.push(1);
 console.log(s5,s6)
 Child4.prototype.constructor===Parent4;//不对 应该是Child4，因为Child4.prototype.construtor的值直接取得是Parent4.prototype.construtor
 //组合继承优化2 完美
 function Parent5(){
 	this.name='parent5';
 	this.play=[1,2,3];
 }
 function Child5(){
 	Parent5.call(this);
 	this.type='child5';
 }
 var __proto=Object.create(Parent5.prototype);
 __proto.constructor=Child5;
 Child5.prototype=__proto;
 var s7=new Child5();
 var s8=new Child5();
 s7.play.push(4);
```

[oop 继承的几种方式 github代码地址](https://github.com/fridaydream/interview_code/blob/master/oop.html)

###通信类

什么是同源策略及限制

```
不同源之间不能相互访问，协议，域名，端口不同，

限制：Cookie、LocalStorage和IndexDB无法读取，
DOM无法获取，
AJAX请求不能发送
```

前后端如何通信

```
Ajax、WebSocket、CORS
```

如何创建Ajax

```
var getXMLRequest = function(opt) {
    var xhr = XMLHttpRequest ? new XMLHttpRequest() : new window.ActiveXObject('Microsoft.XMLHTTP');
    var data = opt.data,
        url = opt.url,
        type = opt.type.toUpperCase(),
        dataArr = [];
    for (var k in data) {
        dataArr.push(k + '=' + data[k]);
    }
    if (type === 'GET') {
        url = url + '?' + dataArr.join('&');
        xhr.open(type, url.replace(/\?$/g, ''), true);
        xhr.send();
    }
    if (type === 'POST') {
        xhr.open(type, url, true);
        xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
        xhr.send(dataArr.join('&'));
    }
    xhr.onreadystatechange = function() {
        if (xhr.readyState == 4 && xhr.status == 200) {
            var res;
            if (opt.success && opt.success instanceof Function) {
                res = xhr.responseText;
                if (typeof res === 'string') {
                    res = JSON.parse(res);
                    opt.success.call(xhr, res);
                }
            } else {
                if (opt.error && opt.error instanceof Function) {
                    opt.error.call(xhr, res);
                }
            }
        }
    }
}
getXMLRequest({
    data: {
        id: 1
    },
    url: 'a.php',
    type: 'get',
    success: function(data) {
        console.log(data);
    }
});
```

跨域通信的几种方式

JSONP

```
动态创建script标签，url用url+callback=jsonp,jsonp是回调方法,后台执行jsonp()返回数据{data:1}
```

[jsonp github代码地址](https://github.com/fridaydream/interview_code/blob/master/jsonp.html)

hash

```
//A中设置B hash变化
var B=document.getElementsByTagName('iframe');
B.src=B.src+'#'+data;
//B中监听onhashchange
window.onhashchange=function(){
	var data=window.location.hash;
}
```

postMessage

```
//postMessage
//窗口A(http:A.com)向跨域的窗口B(http:B.com)发送信息
Bwindow.postMessage('data','http://B.com');
//在窗口B中监听
window.addEventListener('message',function(event){
	console.log(event.origin); //http:A.com
	console.log(event.source); //http:B.com
	console.log(event.data); //data
},false)
```

WebSocket

`支持跨域`

CORS

`既可以同源也可以支持跨域`

###安全 csrf xss

[之前的记录链接](https://www.jianshu.com/p/3edbcefd6383 'csrf xss')

###算法

排序：快速，冒泡，希尔

快速：[快速排序-链接](https://segmentfault.com/a/1190000009426421 '快速排序')

选择：[快速排序-链接](https://segmentfault.com/a/1190000009366805 '选择排序')

https://segmentfault.com/a/1190000009461832
希尔：[希尔排序-链接](https://segmentfault.com/a/1190000009461832 '希尔排序')

堆栈、队列、链表

递归：函数内自己再次调用自己，要有打破循环的条件，引出尾递归，不会产生栈溢出

数组去重

波兰式和逆波兰式

[波兰式和逆波兰式-链接](http://www.cnblogs.com/chenying99/p/3675876.html '波兰式和逆波兰式')

###渲染机制

DOCTYPE的作用

```
html4有2中写法，严格模式是不包括展示性和弃用的元素，和传统模式，
告知浏览器的解析器用什么文档标准解析这个文档，用什么规范渲染页面
```

浏览器渲染过程

```
html->HTML parser ->  DOM Tree

 					        ||->attachment=>render=>layout(计算DOM的位置)
							
css -> css parser -> style rule
```

重排Reflow

```
定义：DOM结构中的各个元素都有自己的盒子(模型),这些都需要浏览器各种样式计算并根据计算结果将元素放到它该出现的位置，这个过程称之为reflow

触发Reflow：增加、删除DOM结点时，会导致Reflow或Repaint
移动DOM位置，（动画），transform不会
修改css样式的时候
当你Resize窗口的时候(移动端没这个问题) ，或是滚动的时候
当你修改网页默认字体
```

重绘Repaint

```
定义：页面把要呈现的内容渲染到页面上
触发Repaint

DOM改动
css改动

只能避免Repaint，如在页面中添加几个元素，放到一起添加

```

布局Layout

###JS运行机制

```
同步任务队列
异步任务队列：setTimeout和setInterval DOM事件 ES6中的Promise
```

###页面性能

提升页面性能的方法有哪些


* 资源压缩合并，减少http请求
* 非核心代码异步加载->异步加载的方式->异步加载的区别

defer async

* 利用浏览器缓存->缓存的分类->缓存原理

强缓存 http响应头
Expires   判断客户端时间是否过期
Cache-Control     相对时间 max-age=秒  

如果 Expires 和Cache-Control 都存在，以后者为准 

协商缓存
Last-Modified   if-Modified-since
Last-Modified是响应头服务器返回的，if-Modified-since是客户端请求头，2个值相同
这种有种问题就是如果文件修改了，但是内容没变，还是可以从缓存中读取

Etag          if-None-Match
//Etag是是响应头服务器返回的，是下次请求头里面if-None-Match 

* 使用CDN
* 预解析DNS

```
<meta http-equiv="x-dns-prefetch-control" content="on">
//浏览器的a标签默认是开启dns预解析DNS的，但是https里面有些浏览器不是的，加这句话去设置
<link rel="dns-prefetch" href="http://a.com">
//将http://a.com的域名预解析
```


###错误监控

>前端错误的分类和错误的捕获方式

```
>>> 即时运行错误捕获

1.try catch 

2.window.onerror

>>> 资源加载错误：1.object.onerror
script标签src引用其他资源，不会向上冒泡报错
 
2.performance.getEntries() 
performance.getEntries().forEach(function(item){console.log(item);})

document.getElementsByTagName('img')
对比查看img的资源数量，看有没加载少了

3.Error 事件捕获
阻止了冒泡，捕获可以拿到
window.addEventListener('error',function(e){
	console.log(e);
},true);
<img src="http://a.com/lll.png" />   //一个错误的地址

```

>跨域的js运行错误可以捕获吗，错误提示什么，应该怎么处理

1.在script标签增加crossorigin属性

2.设置js资源响应头Access-Control-Allow-Origin:*



> 上报错误的基本原理

1.采用Ajax上报

2.通过img对象上报

```
(new Image()).src('http://a.com/test?llll')
```
