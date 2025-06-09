---
title: JSONP跨域 ajax请求
date: 2017-09-03 12:01:26
tags: ajax
categories: 代码集
---

使用 JSONP 进行的跨域，链接的是聚合数据的手机号查询归属地 API，一开始的时候出现了问题，使用原生的 ajax 请求怎么样也是查询不到数据，后来了解到，使用 ajax 不能进行跨域请求。下面是通过使用 `jsonp` 实现的跨域请求

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<div id="div"></div>
<body>
<div>
    号码归属地查询<input id='phone' type='text' ><button id = 'btn'>查询手机号归属地</button>
    <p>您的手机号码归属地是  <span id='city'></span></p>
</div>
<script type="text/javascript">
   function doFunction (data) {
            if (!data.result) {
                return;
            } 
            document.getElementById('city').innerHTML = data.result.city;
        
        }
    window.onload = function () {
        var sendObj = {
            url: 'http://apis.juhe.cn/mobile/get',
            phone: '13429667914',
            key: '4ebdd2325a4ce36fa7ca55e910c185e9',
            callback: 'doFunction'
        };
        function sendLink (obj) {
            var link = obj.url;
            for (var key in obj) {
                if (key !== 'url') {
                    link = link + (~link.indexOf('?') ? '&' : '?' );
                    link = link + key + '=' + obj[key];
                }
            }
            return link;
        }
        document.getElementById('btn').addEventListener('click', function () {
            var body = document.getElementsByTagName('body')[0];
            var script = document.createElement('script');
            sendObj.phone = document.getElementById('phone').value;
            script.setAttribute('src', sendLink(sendObj));
            if (body.getElementsByTagName('script').length === 2) {
                body.replaceChild( script ,body.lastChild);
            } else {
                document.getElementsByTagName('body')[0].appendChild(script);
            }
        });
    }
</script>
<!--
   <script src="http://apis.juhe.cn/mobile/get?phone=13429667914&key=4ebdd2325a4ce36fa7ca55e910c185e9&callback=doFunction"></script>
-->
</body>
</html>
```

#### ajax 请求

原生的 ajax 请求如下:

```
var request;
if (window.XMLHttpRequest) {
	request = new XMLHttpRequest();  
} else {
// for IE
    request = new ActiveObject();
}
request.onreadystatechange = function () {
  if (request.readyState == 4 && request.status == 200) {
    console.log(request.responseText);
  }
};
request.open('get', url, false / true);
request.send();
```

使用 ajax 的目的在于在没有重新加载页面的时候进行页面部分数据的更新；

`open` 方法

使用 `open` 方法接收三个参数:

要发送的请求类型:  'get '    或者 'post'  

请求的url : 注意 `只能向同一个域中使用相同端口和协议 URL 发出请求`，如果请求的端口和本地页面不在同一个域的范围内，那么要使用跨域进行；

什么是跨域？

***

只要是协议，端口，域名当中存在一个不同的值，那么请求就会被认为是跨域:

一个 URL 地址如下:

`https://www.baidu.com:8080`

`https`  协议， 用来定义浏览器打开文件的形式

`www.baidu.com` 域名,  其中     `baidu.com`  主域     `www.baidu.com`  子域

`:8080` 端口

当两个 `URL` 的协议, 域名 ,端口中存在一个不同的时候，使用 ajax 是无法进行请求得出数据的  

但是这种情况:

`https:// www.a.com/javascript/a.js`

`https:// www.a.com/b.js`

这种情况下, 域名，协议，端口都是相同的, 因此可以进行访问到

https://segmentfault.com/a/1190000000718840



http://blog.csdn.net/seebetpro/article/details/51326260

***

布尔值：表示是否进行异步

在上面的代码中，请求成功之后，响应的数据会自动填充 request 对象的属性，表示请求成功之后的状态:

```
request.onreadystatechange = function () {
  if (request.readyState == 4 && request.status == 200) {
    // 表示请求成功之后，并且有返回数据之后要进行的动作
  }
}
```

`status` : 表示响应的 `http` 状态,对于  http 的状态码， 要记住几个重要的：[http状态码]('http://www.cnblogs.com/sprinng/p/6559431.html')

2XX :  表示请求成功  

`200` 请求成功， `201` 已创建  `202` 接收   `204` 无内容  

3XX: 表示重定向  

`300` 多路选择  `301` 永久转移  `302` 暂时转移  `304` 未修改

4XX: 客户方错误  

`400` 错误请求  `401` 未认证   `408` 请求超时  `410` 失败

5XX: 服务器错误

`500` 服务器内部错误  `501` 未实现   `504` 网关超时  

关于这些请求的具体情况：

2XX: 表示已经接受到了请求

| 状态码  | 含义                   |
| ---- | -------------------- |
| 200  | 表示请求已经成功             |
| 201  | 表示请求已经被创建            |
| 202  | 服务器已经接收到了请求， 但是尚未处理  |
| 204  | 服务器已经处理了请求， 但是没有返回任何 |

3XX:资源重定向

| 状态码  | 含义                                       |
| ---- | ---------------------------------------- |
| 301  | 表示永久性重定向， 请求的资源分配到新的 url                 |
| 302  | 表示临时性重定向，表示请求的资源已经分配到新的url, 希望用户本次能够使用新的url 访问 |
| 304  | 表示请求的资源已经找到， 但是不符合条件要求                   |

4XX:客户端请求错误

| 状态码  | 说明                                |
| :--- | --------------------------------- |
| 400  | 表示客户端发给服务器的请求存在语法错误， 服务器无法理解这个请求。 |
| 401  | 表示发送的请求需要通过使用 HTTP 认证             |
| 403  | 表示客户端想要请求的资源被服务器拒绝访问              |
| 404  | 表示在服务器上没有找到请求的资源                  |

5XX: 服务器错误

| 状态码        | 说明                    |
| ---------- | --------------------- |
| 500（服务器错误） | 表示服务器在执行请求的时候发生了错误    |
| 503（服务器正忙） | 表示现在服务器正在处于超负载状态，无法处理 |

`readyState` 表示 `request` 对象的  `readyState` 对象，属性可取的值如下：

`0` : 未初始化，尚未调用 open() 方法

`1` : 启动， 已经调用 open  方法，尚未调用 send() 方法

`2` : 发送，已经调用 send() 方法，但是尚未接收到响应

`3`: 接收，表示已经接受到部分数据

`4`: 完成, 已经接收到全部的响应数据

> 使用 `get` 和 `post` 请求的区别

##### 1. 两种方式传递参数的方式不同

`Get` 请求用于向服务器发送请求查询信息，查询字符串参数添加到 url 的末尾 

`post` 请求是将作为 HTTP  消息的实体内容发送给服务器

##### 2. 两者缓存不同

使用 `get` 方法的数据会被浏览器进行缓存起来，因此其他人可以通过使用浏览器的历史记录进行读取到这些数据

##### 3.服务器端 针对这两种方式的获取参数是不同的

在客户端通过使用 `get` 请求的时候，服务器端通过使用 `Request.QueryString`  来进行获取参数, 在客户端通过使用 post 进行请求的时候，服务器端通过使用 `Request.Form` 来进行获取到参数

为什么要使用两种方式:

当请求没有副作用的时候， 例如进行搜索数据， 可以使用 `get` 方法， 当请求存在副作用的时候，使用 `post `进行请求

使用 `post ` 的情况:

* 请求的结果存在副作用，例如，向数据库内添加新的数据行
* 使用 GET 方法, 使用表单上添加的数据使得 URL太长
* 进行传送的数据不是7 位的 ASCII 编码

使用 `get` 的情况

* 请求是为了查找资源, HTML 上的表单数据仅仅用来进行搜索
* 请求结果无持续性的副作用
* 收集到的数据及HTML 表单内的输入字段名称的总长不超过 1024 字符



### 如何使用 ajax 实现跨域？

对于使用原生的 ajax 而言,是无法实现进行跨域请求的,如何使用 ajax 实现跨域呢？

#### 1.使用跨源资源共享(CORS)

跨源资源共享定义了当进行访问跨源资源的时候，浏览器和服务器之间应该如何进行通信, 基本的思想是:

浏览器向服务器发送请求的时候，添加一个额外的 `origin` 头部，这个头部包含了请求页面的源信息(协议， 域名， 端口)

如果服务器认为请求可以接受，可以在 `Access-Control-Allow-Origin` 中回发相同的源信息,如果在服务器端设置 为 * 表示公共资源

![](JSONP跨域-ajax请求/跨源资源共享Snipaste_2017-09-09_10-35-03.png)

如上图所示，实现了公共资源的共享

#### 2.使用 JSONP

使用 jsonp 技术实现的，原理是将请求的参数放入到 js 中，通过使用动态 js 来实现资源的动态请求

缺点: 无法处理请求失败之后的动作,并且使用 jsonp 请求到的数据只能使用 `get` 请求进行

