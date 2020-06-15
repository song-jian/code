## 跨域

   ### 1. 这里说的js跨域是指通过js在不同的域之间进行数据传输或通信，比如用ajax向一个不同的域请求数据，或者通过js获取页面中不同域的框架中(iframe)的数据。只要协议、域名、端口有任何一个不同，都被当作是不同的域(当一个请求url的协议、域名、端口三者之间任意一个与当前页面url不同即为跨域)

## 跨域资源共享

   ### 1. 开发网站时经常会用到跨域资源共享（简称cors）来解决跨域问题，但是在使用cors的时候，http请求会被划分为两类，简单请求和复杂请求，而这两种请求的区别主要在于是否会触发cors预检请求。

   ### 2. cors原理:跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求），浏览器必须首先使用 OPTIONS 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨域请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 Cookies 和 HTTP 认证相关数据）
    
   ### 3.从上面的文字中我们得到如下信息
   1. 跨域资源共享标准新增了一组 HTTP 首部字段，服务器通过这些字段来控制浏览器有权访问哪些资源。
   2. 为了安全起见请求方式分为两类，一类不会预先发送options请求，一些会预先发送options请求。
   3. GET 以外的 HTTP 请求，或者搭配某些 MIME 类型的 POST 请求会触发options请求。
   4. 服务器验证OPTIONS完成后才会允许发送世界的http请求。

   **不会触发http预检请求的便是简单请求，想法能够触发http预检请求的便是复杂请求。**

   ### 4. 那么有哪些简单请求呢？以下是来自MDN官方引用

   1. 使用下列方法之一：
    * GET
    * POST
    * HEAD

   2. 不得人为设置该集合之外的其他首部字段。该集合为：
    * Accept
    * Accept-Language
    * Content-Language
    * Content-Type

   3. Content-Type 的值仅限于下列三者之一：
    * text/plain
    * multipart/form-data
    * application/x-www-form-urlencoded

   4. 请求中的任意XMLHttpRequestUpload 对象均没有注册任何事件监听器；XMLHttpRequestUpload 对象 可以使用 XMLHttpRequest.upload 属性访问

   5. 请求中没有使用 ReadableStream 对象


   ### 5. 简单请求的部分响应头及解释如下：

        Access-Control-Allow-Origin（必含）- 不可省略，否则请求按失败处理。该项控制数据的可见范围，如果希望数据对任何人都可见，可以填写"*"。 Access-Control-Allow-Credentials（可选） – 该项标志着请求当中是否包含cookies信息，只有一个可选值：true（必为小写）。如果不包含cookies，请略去该项，而不是填写false。这一项与XmlHttpRequest2对象当中的withCredentials属性应保持一致，即withCredentials为true时该项也为true；withCredentials为false时，省略该项不写。反之则导致请求失败。 Access-Control-Expose-Headers（可选） – 该项确定XmlHttpRequest2对象当中getResponseHeader()方法所能获得的额外信息。通常情况下，getResponseHeader()方法只能获得如下的信息： Cache-Control Content-Language Content-Type Expires Last-Modified Pragma 当你需要访问额外的信息时，就需要在这一项当中填写并以逗号进行分隔

        如果仅仅是简单请求，那么即便不用CORS也没有什么大不了，但CORS的复杂请求就令CORS显得更加有用了。简单来说，任何不满足上述简单请求要求的请求，都属于复杂请求。比如说你需要发送PUT、DELETE等HTTP动作，或者发送Content-Type: application/json的内容。

        复杂请求表面上看起来和简单请求使用上差不多，但实际上浏览器发送了不止一个请求。其中最先发送的是一种"预请求"，此时作为服务端，也需要返回"预回应"作为响应。预请求实际上是对服务端的一种权限请求，只有当预请求成功返回，实际请求才开始执行。

        预请求以OPTIONS形式发送，当中同样包含域，并且还包含了两项CORS特有的内容

        Access-Control-Request-Method – 该项内容是实际请求的种类，可以是GET、POST之类的简单请求，也可以是PUT、DELETE等等。 Access-Control-Request-Headers – 该项是一个以逗号分隔的列表，当中是复杂请求所使用的头部。

        显而易见，这个预请求实际上就是在为之后的实际请求发送一个权限请求，在预回应返回的内容当中，服务端应当对这两项进行回复，以让浏览器确定请求是否能够成功完成。

	### 5. 复杂请求的部分响应头及解释如下：

				Access-Control-Allow-Origin（必含） – 和简单请求一样的，必须包含一个域。 Access-Control-Allow-Methods（必含） – 这是对预请求当中Access-Control-Request-Method的回复，这一回复将是一个以逗号分隔的列表。尽管客户端或许只请求某一方法，但服务端仍然可以返回所有允许的方法，以便客户端将其缓存。 Access-Control-Allow-Headers（当预请求中包含Access-Control-Request-Headers时必须包含） – 这是对预请求当中Access-Control-Request-Headers的回复，和上面一样是以逗号分隔的列表，可以返回所有支持的头部。这里在实际使用中有遇到，所有支持的头部一时可能不能完全写出来，而又不想在这一层做过多的判断，没关系，事实上通过request的header可以直接取到Access-Control-Request-Headers，直接把对应的value设置到Access-Control-Allow-Headers即可。 Access-Control-Allow-Credentials（可选） – 和简单请求当中作用相同 Access-Control-Max-Age（可选） – 以秒为单位的缓存时间。预请求的的发送并非免费午餐，允许时应当尽可能缓存。