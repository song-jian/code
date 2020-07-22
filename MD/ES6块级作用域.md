#### 块级作用域

  #### 1.JS中作用域有：全局作用域、函数作用域。没有块作用域的概念。ECMAScript 6(简称ES6)中新增了块级作用域。块作用域由 { } 包括，if语句和for语句里面的{ }也属于块作用域。

  ### 2.首先ES5在没有块级作用域时可能会出现的情况
  - 在if或者for循环中声明的变量可能会泄露为全局变量
    ```
    for(var i = 0; i < 5; i ++){
        console.log(i)
    }

    console.log(i) // 5
    ```
    
  - 内层变量可能覆盖外出变量
    ```
    var temp = 1;
    function  f(){
        console.log(temp);
        if(false){
            var temp = "hello";
        }
    }
    f(); //undefined
    不管最后是否执行if语句，都会输出undefined，因为temp会提升到函数顶部，因此覆盖了外部的变量temp。
    ```