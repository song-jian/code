#### http请求头
 - 1.Accept:浏览器能够处理的内容类型 application/json, text/plain, */*

 - 2.Accept-Encoding：浏览器能够处理的压缩编码  普通浏览器访问网页，之所以添加：
     "Accept-Encoding" = "gzip,deflate" 那是因为，浏览器对于从服务器中返回的对应的gzip压缩的网页，会自动解压缩，所以，其request的时候，添加对应的头，表明自己接受压缩后的数据。 

 - 3.Accept-Language：浏览器当前设置的语言 

 - 4.Connection：浏览器与服务器之间连接的类型  HTTP1.1一般只有6个TCP的并发链接,其他的连接会阻塞
     Connection:‘close’ 没有长连接，在每一次请求完毕之后关闭TCP连接，有新请求时创建一个新的TCP连接
     Connection:‘keep-alive’ 有长连接，在六个TCP连接中如果有一个连接完成数据传输，则将那个TCP连接给下一个请求用

 - 5.Cookie：当前页面设置的任何Cookie 

 - 6.Host： npi.dev.jwis.cn 发出请求的页面所在的域 

 - 7.Referer：http://npi.dev.jwis.cn/ 发出请求的页面的URL 

 - 8.Origin: http://npi.dev.jwis.cn  用于指明当前请求来自于哪个站点,不包含任何路径信息

 - 9.Content-Type 请求体的MIME类型 （用于POST和PUT请求中） 

 - 10.Content-Length 以8进制表示的请求体的长度



