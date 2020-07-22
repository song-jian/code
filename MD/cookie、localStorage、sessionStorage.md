####  webStorage和cookie
###   共同点:都是保存在浏览器端,且同源的
 
    cookie有什么缺点？
    Cookie数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB
    安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。
###  区别：
        
        1、cookie数据始终在同源的http请求中携带(即使不需要)，即cookie在浏览器和服务器间来回传递
 
        2、cookie数据还有路径（path）的概念，可以限制。cookie只属于某个路径下、
 
        3、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie，所以cookie只适合保存很小的数据，如回话标识。
 
        4、webStorage虽然也有存储大小的限制，但是比cookie大得多，可以达到5M或更大
 
        5、数据的有效期不同
                        sessionStorage：仅在当前的浏览器窗口关闭有效；
                        localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；
                        cookie：只在设置的cookie过期时间之前一直有效，即使窗口和浏览器关闭
 
        6、作用域不同
 
                        sessionStorage：不在不同的浏览器窗口中共享，即使是同一个页面；
                        localStorage：在所有同源窗口都是共享的；
                        cookie：也是在所有同源窗口中共享的
 
 
        7、webStorage支持事件通知机制，可以将数据更新的通知发生给监听者
 
        8、webStorage的API借口使用更方便 。setItem  getItem clearItem
            window。sessionStorage/window。 localStorage
            setItem（key,val）设置
            getItem（key）获取
            webStorage。removeItem（key）删除单个
            webStorage.clear（）清除所有

            webStorage只能操作字符串对象，所有的存储值都会为字符串数据

### 2.再分析：
        cookie的优点：具有极高的扩展性和可用性
        通过良好的编程，控制保存在cookie中的session对象的大小。
        通过加密和安全传输技术，减少cookie被破解的可能性。
        只有在cookie中存放不敏感的数据，即使被盗取也不会有很大的损失。
        控制cookie的生命期，使之不会永远有效。这样的话偷盗者很可能拿到的就是一个过期的cookie。

        cookie的缺点：
        cookie的长度和数量的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB。否则会被截掉。
        安全性问题。如果cookie被人拦掉了，那个人就可以获取到所有session信息。加密的话也不起什么作用。
        有些状态不可能保存在客户端。例如，为了防止重复提交表单，我们需要在服务端保存一个计数器。若吧计数器保存在客户端，则起不到什么作用


        在html5中web storage包括两种存储方式：sessionstorage和localstorage
        sessionstorage用于本地存储一个会话(session)中的数据，这个数据只有在同一个会话中的页面才能访问并且当会话结束后数据也随之销毁。因此sessionstorage不是一种持久化的本地存储，仅仅是会话级别的存储。
        而localstorage用于持久化的本地存储，除非主动删除数据，否则数据是永远也不过期的。
        web storage的概念和cookie相似，区别是它是为了更大的容量存储设计的。cookie大小受限，并且每次都请求一个新的页面的时候后cookie都会被发送过去，另外cookie还需要指定作用域，不可以跨域调用。

        cookie的作用是与服务器进行交互，作为HTTP规范的一部分存在，而web storage仅仅是为了在本地存储数据而生。

        localstorage和sessionstrage都具有相同的操作方法，例如setItem,getItem和removeItem等。