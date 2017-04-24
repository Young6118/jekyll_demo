---
title: WebSocket 学习 （一）
date: 2017-04-23
tags: WebSocket
---
## WebSocket 学习 (一)
### 一、简介
#### 1. WebSocket 是基于 TCP 的一种 **新** 的网络协议，是 HTML5 中的一种协议，作为基于 TCP 的套接字 API 的占位符。它实现了浏览器和服务器全双工通信（数据可以同时沿相反的两个方向传送，也就是双向通信）。当然，它只是一个协议，并不是具体的实现，在不同的平台下，该协议有不同的实现方式。
#### 2. HTTP 协议是一种无状态协议，服务端没有识别客户端的机制，所以需要通过 session 和 cookie 来和特定的客户端保持对话。它只能实现单向的通信，不支持持久连接。这多少有些不方便，有其在聊天室这样的持续交换数据的场景下，更不适用。其中的轮询（polling）技术是指间隔一定时间客户端或浏览器向服务器发送 HTTP 请求，然后服务器响应返回最新数据。由于客户端和服务器之间的连接需要频繁验证身份，频繁发送重复信息，浪费了时间和性能。HTTP 的实现方式中还有长连接 （long polling） 方式是服务端把所需要的信息准备好再通知客户端接收，也即是阻塞方式模拟双向通信。但是实际上，客户端和服务器之间的连接仍旧是一次性的，服务器不会发出请求，因此具有 HTTP 的被动特征。在一个 HTTP 连接中，一个 Request 只能有一个 Response。
#### 3. 而 WebSocket 是真正意义上的双向通信（full-duplex），是一个持久化的协议，它完全减少了频繁的身份验证等不必要的开销。WebSocket 可以说是 HTTP 的一个扩展，用到了它的内容，但与它却又有本质上的不同。
#### 4. WebSocket 就像俩人打电话，服务端和客户端可以同时发数据，而 HTTP 就像是发信息，发出去以后得等别人回信，有没有还不一定。常用的 WebSocket 工具包有 [Socket.IO](https://socket.io/)。
### 二、 实现
#### 1. 握手
浏览器发出  WebSocket  连线请求，服务器响应，这个过程称作”握手“(handshaking)。在 WebSocket API，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。在此WebSocket 协议中，为我们实现即时服务带来了两大好处：
> 1. Header
互相沟通的Header是很小的-大概只有 2 Bytes
>2. Server Push
服务器的推送，服务器不再被动的接收到浏览器的request之后才返回数据，而是在有新数据时就主动推送给浏览器。

这是一个 WebSocket 头，我们来具体分析下：
```
GET ws://localhost/socket.io/?EIO=3&transport=websocket&sid=1RmWjx0UGWJ7pvVwAAAA HTTP/1.1
Host: localhost
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
Upgrade: websocket
Origin: http://localhost
Sec-WebSocket-Version: 13
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.81 Safari/537.36
DNT: 1
Accept-Encoding: gzip, deflate, sdch, br
Accept-Language: zh-CN,zh;q=0.8
Cookie: _ga=GA1.1.104692147.1493020339; io=1RmWjx0UGWJ7pvVwAAAA
Sec-WebSocket-Key: K/mADV4wzQ5nyUsClLH1Vg==
Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```

> ws 表示是 WebSocket 协议
wss 是加密的 WebSocket 协议 对应HTTPs 协议

分析：
```
Connection: Upgrade
```

HTTP 头是 Upgrade，HTTP1.1协议规定，Upgrade 头信息表示将通信协议从HTTP/1.1转向该项所指定的协议。
```
Connection: Upgrade
```

就表示浏览器通知服务器，如果可以，就升级到webSocket协议。
```
Origin: http://localhost
```

Origin 用于验证浏览器域名是否在服务器许可的范围内。
```
Sec-WebSocket-Key: K/mADV4wzQ5nyUsClLH1Vg==
```

Sec-WebSocket-Key则是 **用于握手协议的密钥**，是base64编码的16字节随机字符串。

这是该请求服务端的响应：
```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: jI5blupmAg6R6TvIrG01dppJJj4=
Sec-WebSocket-Extensions: permessage-deflate
```

服务端用
```
Connection: Upgrade
```

来通知浏览器，需要改变协议。
```
Upgrade: websocket
```

```
Sec-WebSocket-Accept: jI5blupmAg6R6TvIrG01dppJJj4=
```

Sec-WebSocket-Accept 是服务器加在浏览器提供的 Sec-WebSocket-Key 后面，然后对它取 [sha-1](https://zh.wikipedia.org/zh/SHA-1) 算法的 [hash](https://zh.wikipedia.org/zh-hans/%E5%93%88%E5%B8%8C%E8%A1%A8) 值。浏览器对这个值进行验证，用来证明确实是目标服务器回应了请求。

完成握手之后，WebSocket 协议就在第四层传输层 [TCP](https://zh.wikipedia.org/wiki/%E4%BC%A0%E8%BE%93%E6%8E%A7%E5%88%B6%E5%8D%8F%E8%AE%AE) 协议上建立连接传输数据。

#### 2. 具体实现
##### 2.1 检查支持、建立和断开
###### 2.1.1 检查支持
查看 BOM （浏览器对象模型）中 Window 对象 是否支持 WebSocket

```
if(window.WebSocket != undefined) {
// WebSocket 代码
}
```

###### 2.1.2 建立、断开

```
if(window.WebSocket != undefined) {
var webSocket = new WebSocket('ws://localhost:9000')
}
```

WebSocket 的实例对象 connection 有一个 [readyState 属性](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

| Constant	| Value |	Description |
| ------- | ----: | :----: |
| CONNECTING |	0 |	The connection is not yet open.（正在连接）|
| OPEN | 1 | The connection is open and ready to communicate. （连接成功）|
| CLOSING |	2	| The connection is in the process of closing. （正在关闭）|
| CLOSED |	3	| The connection is closed or couldn't be opened. （连接关闭）|

握手协议成功，readyState 从 0 变为 1，同时触发 open 事件，此时可以向服务器发送信息了，可以指定 open 事件的回调函数。

```
webSocket.onopen = wsOpen

var wsOpen = function(event) {
console.log('Connected to: ' + event.currentTarget.URL)
}
```

关闭连接，会触发 close 事件：
```
webSocket.onclose = wsClsoe

var wsClose = function() {
console.log("closed")
}

webSocket.close()
```

##### 2.2 发送、接收数据
```
webSocket.send(message)
```

message 可以是字符串、ArrayBuffer 或 Blob 对象。
>抛出异常

```
webSocket.onerror = wsError

var wsError = function(event) {
console.log("Error: " + event.data)
}
```


参考：
> * [MDN WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)
> * [阮一峰 WebSocket](http://javascript.ruanyifeng.com/htmlapi/websocket.html)
> * [Poker_Facer 1小时教你理解HTTP，TCP，UDP，Socket，WebSocket](http://www.jianshu.com/p/42260a2575f8)
> * [维基百科 OSI模型](https://zh.wikipedia.org/wiki/OSI%E6%A8%A1%E5%9E%8B)