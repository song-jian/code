## BFC

#### 1. 定义：BFC(Block Formatting context)块级格式化上下文，它是一个独立渲染的区域。

#### 2. 布局规则:
* 内部的Box会在垂直方向，一个接一个地放置。
* Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
* 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
* BFC的区域不会与float box重叠。
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
* 计算BFC的高度时，浮动元素也参与计算。

#### 3. BFC创建条件
* 根元素默认BFC
* float属性值不为none
* overfol属性值不为visible
* position属性值不为static or relative
* display属性值为flex inline-block table-cell table-caption inline-flex 

#### 4. BFC 作用
* 利用BFC解决margin重叠
  ```
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>解决margin重叠</title>
    </head>
    <style>
    *{
        margin: 0;
        padding: 0;
    }
    div {
       overflow:hidden;
    }
    p {
        color: red;
        background: yellow;
        width: 200px;
        line-height: 100px;
        text-align:center;
        margin: 30px;
    }
    </style>
    <body>
        <p>看看我的 margin是多少</p>
        <div>
           <p>看看我的 margin是多少</p>
        </div>
    </body>
    </html>

  ```
  
* 清楚浮动(计算BFC的高度时，浮动元素也参与计算)

```
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>清除浮动</title>
    </head>
    <style>
    .parent {
        border: 2px solid red;
        width: 300px;
        overflow: hidden;
    }

    .child {
        border: 1px solid pink;
        width:100px;
        height: 100px;
        float: left;
    }
    </style>
    <body>
    <div class="parent">
        <div class="child"></div>
        <div class="child"></div>
    </div>
    </body>
    </html>

```

* 自适应两栏布局（BFC的区域不会与float box重叠）
```
    <!DOCTYPE html>
    <html lang="en">
    <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    </head>
    <style>
    *{
        margin: 0;
        padding: 0;
    }
    body {
        width: 100%;
        position: relative;
    }

    .left {
        width: 100px;
        height: 150px;
        float: left;
        background: red;
        text-align: center;
        line-height: 150px;
        font-size: 20px;
    }

    .right {
        overflow: hidden;
        height: 300px;
        background: pink;
        text-align: center;
        line-height: 300px;
        font-size: 40px;
    }
    </style>
    <body>
    <div class="left">LEFT</div>
    <div class="right">RIGHT</div>
    </body>
    </html>

```