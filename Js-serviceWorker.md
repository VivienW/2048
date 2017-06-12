Js技术报告——Service Worker
==================

                                                                        软件工程1401
                                                                        31401284
                                                                        王维维

----------


一．Service worker介绍
-------------
        一个Service Worker是一段运行在浏览器后台进程里的脚本，他独立于当前页面，提供了那些不需要与web页面交互的功能在网页背后悄悄执行的能力。在将来，基于它可以实现消息推送，静静更新以及地理围栏等服务，但是目前它首先要具备的功能是拦截和处理网络请求的功能，包括可编程的消息缓存管理能力。
        Service Worker 类似提供了一个前端的 HTTP 服务器，我们可以注册一些 URL 让这些请求在 Service Worker 中处理并响应。
        
二．Service worker作用
-------------

> - 网络代理，转发请求，伪造响应

> - 离线缓存

> - 消息推送

> - 后台消息传递

三．Service worker 的安装
-------------
 > - 3.1 首先先在chrome中运行代码：
       if ('serviceWorker' in navigator) {  
       navigator.serviceWorker.register('/sw.js').then(function(registration) {  
       console.log('ServiceWorker registration successful with scope: ',    registration.scope);  }).catch(function(err) {  
       console.log('ServiceWorker registration failed: ', err);  });  }  
 > - 3.2  注册成功后，则可以打开 chrome://serviceworker-internals/查看浏览器的Service Worker信息。 以上步骤只是注册了一个Service Worker脚本，注册完成后将会安装，安装过程在Service Worker脚本中进行    sw.js：
        var CACHE_NAME = "my_cache";  
        var urlsToCache = [  
        '/index.html',  
        '/css/style.css',  
        '/js/script.js'  
       ];   
        self.addEventListener('install', function(event) {   
        event.waitUntil(  
        caches.open(CACHE_NAME).then(function(cache) {  
             console.log('Opendhe : ',cache);  
            return cache.addAll(urlsToCache);  
      })  
        );  
        });  
        
 

四．Service worker API设计理念
-------------------
    Service Worker 是「 Promise 友好」的设计。比如上面例子中的 register 方法，它是一个异步方法，返回一个 Promise 对象，于是后续程序放在 then 的回调中执行。还有上面的 respondWith 方法，由于是演示同步返回，所以直接传入它需要的参数。然而它也能接受 Promise 对象.
    
五．与service worker相关的事件
-------------------

> - 5.1 fetch事件：
<i> 在页面发起http/https请求时，Service Worker可以通过fetch事件拦截请求，并且给出自己的相应。w3c提供了一个新的fetch API可用于取代XMLHttpRequest，与XMLHttpRequest最大的不同就是：fetch方法返回的是promise对象，可通过then方法进行连续调用，减少嵌套。</i>
> - 5.2 message事件：
<i>页面和ServiceWorker质检可以通过postMessage方法发送消息，发送的消息可以通过message事件接收到。这是一个双向的过程，页面可以发消息给Service Worker，Service Worker也可以发送消息给页面，由于这个特性，可以将Service Worker作为中间纽带，使得一个域名或者子域名下的多个页面自由通信页可以实现服务器消息推送的功能 </i>

六．浏览器的实现情况
-------------------

Item     | Chrome    |    firefox  |   IE
-------- | --------- | ----------- | ------
基本功能  | 40.0      | 44.0        | 不支持
Fetch    | 40.0      | 44.0        | 不支持





