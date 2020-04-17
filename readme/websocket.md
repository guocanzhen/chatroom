**SpringBoot 2.x 第二十五篇：打造属于你的聊天室（WebSocket）**

Webscoket 对浏览器有一定的要求，所以使用之前要考虑兼容性的问题….

1、Webscoket

WebSocket 是 HTML5 新增的一种在单个 TCP 连接上进行全双工通讯的协议，与 HTTP 协议没有太大关系….
在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。
浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
当你获取 WebSocket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过 onmessage() 事件来接收服务器返回的数据..

长连接
与 AJAX 轮训的方式差不多，但长连接不像 AJAX 轮训一样，而是采用的阻塞模型（一直打电话，没收到就不挂电话）；客户端发起连接后，如果没消息，就一直不返回 Response 给客户端。直到有消息才返回，返回完之后，客户端再次建立连接，周而复始。
在没有 WebSocket 之前，大家常用的手段应该就是轮训了，比如每隔几秒发起一次请求，但这样带来的就是高性能开销，都知道一次 HTTP 响应是需要经过三次握手和四次挥手，远不如 TCP 长连接来的划算

WebSocket 事件
D:\PDM\persiondemo\readme\WebSocket.png

2、本章目标
利用 Spring Boot 与 WebSocke 打造 一对一 和 一对多 的在线聊天….

导入依赖
依赖 spring-boot-starter-websocket…

工具类
为了减少代码量，此处就不集成 Redis、Mysql 之类的存储化依赖…

服务端点
@ServerEndpoint 中的内容就是 WebSocket 协议的地址，其实仔细看会发现与 @RequestMapping 也是异曲同工的…
HTTP 协议：http://localhost:8080/path
WebSocket 协议：ws://localhost:8080/path
@OnOpen、@OnMessage、@OnClose、@OnError 注解与 WebSocket 中监听事件是相对应的…

聊天室 HTML
onopen 建立 WebSocket 连接时触发。
message 客户端监听服务端事件，当服务端向客户端推送消息时会被监听到。
error WebSocket 发生错误时触发。
close 关闭 WebSocket 连接时触发。