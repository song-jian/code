### 事件委托
  #### 1. 原理是：利用依靠事件冒泡机制与事件捕获机
  #### 2. 事件委托又叫事件代理又叫事件代理，是指利用事件冒泡的特性将本该注册在子元素的事件注册在父元素上面，由父元素来处理子元素的事件，这样的好处是

   * 有利于降低DOM操作，提高性能
   * 增加扩展性，子元素可以随意增加、不需要再做额外操作，都在父元素上面处理
   * 有利于代码简洁、不需要为每个子元素增加事件程序、简化代码逻辑

   ```
   <!-- 委托示例 -->
    <ul class="ul">
        <li id="one">打球</li>
        <li id="two">跑步</li>
        <li id="three">游泳</li>
        <li id="four">俯卧撑</li>
    </ul>
    <script>
        document.querySelector('.ul').addEventListener('click', function(e){
            // 使用e.target获取目标元素
            console.log(`id为 '${e.target.id}' 对应的文本是 '${e.target.innerText}'`)
        })
        <!-- li元素的点击事件都委托给ul元素了，当点击某个li元素时，事件会冒泡到ul元素，执行ul元素绑定的事件监听函数。e.target值指向点击的目标元素，通过它可以处理子元素相关逻辑 -->
    </script>
   ```

### 事件模型
   
   #### 1.原始事件模型（DOM0级）
　　　　这是一种被所有浏览器都支持的事件模型，对于原始事件而言，没有事件流，事件一旦发生将马上进行处理，有两种方式可以实现原始事件：

1. 在html代码中直接指定属性值：
   ```<button id="demo" type="button" onclick="doSomeTing()" />　```

2. 在js代码中为　document.getElementsById("demo").onclick = doSomeTing()

  * 优点：所有浏览器都兼容
  * 缺点：1）逻辑与显示没有分离；2）相同事件的监听函数只能绑定一个，后绑定的会覆盖掉前面的，如：a.onclick = func1; a.onclick = func2;将只会执行func2中的内容。3）无法通过事件的冒泡、委托等机制（后面会讲到）完成更多事情。

  #### 2.DOM2事件模型
　　　　此模型是W3C制定的标准模型，现代浏览器（IE6~8除外）都已经遵循这个规范。W3C制定的事件模型中，一次事件的发生包含三个过程:

1. 事件捕获阶段
2. 事件目标阶段
3. 事件冒泡阶段

![blockchain](https://images2015.cnblogs.com/blog/982936/201607/982936-20160706150206796-2145702578.png "事件模型")

  * 事件捕获：当某个元素触发某个事件（如onclick），顶层对象document就会发出一个事件流，随着DOM树的节点向目标元素节点流去，直到到达事件真正发生的目标元素。在这个过程中，事件相应的监听函数是不会被触发的。
  * 事件目标：当到达目标元素之后，执行目标元素该事件相应的处理函数。如果没有绑定监听函数，那就不执行。

  * 事件冒泡：从目标元素开始，往顶层元素传播。途中如果有节点绑定了相应的事件处理函数，这些函数都会被一次触发。

  IE 5.5: div -> body -> document 

  IE 6.0: div -> body -> html -> document 

  Mozilla 1.0: div -> body -> html -> document -> window 

　　所有的事件类型都会经历事件捕获但是只有部分事件会经历事件冒泡阶段,例如submit事件就不会被冒泡。 

　　事件的传播是可以阻止的：
　　• 在W3c中，使用stopPropagation（）方法
　　• 在IE下设置eve.cancelBubble = true；
　　在捕获的过程中stopPropagation（）；后，后面的冒泡过程就不会发生了。