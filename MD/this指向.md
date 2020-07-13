#### 首先我们要了解：

 1. 全局变量默认是挂载在window下
 2. 一般情况this指向它的调度者
 3. es6箭头函数中 this指向它的创建者 而不是调度者
 4. 通过call 和 apply可以改变this的指向
 
 #### this遇到的情况

 - **作为函数调用：this指向全局对象（window）**
```
var name = 'this is window'; //定义window的name属性 
function getName(){ 
var name = 
 console.log(this); //控制台输出: Window //this指向的是全局对象--window对象 
 console.log(this.name); //控制台输出: this is window / 
} 
getName();
```
- **作为对象方法调用：指向调用该方法的对象**

```
var name = 'this is window'; //定义window的name属性,看this.name是否会调用到 
var testObj = { 
 name:'this is testObj', 
 getName:function(){ 
 console.log(this); //控制台输出:testObj //this指向的是testObj对象 
 console.log(this.name); //控制台输出: this is testObj 
 } 
} 
testObj.getName();
```
- **使用new调用构造函数：指向新生成的对象**

```
function Newobj(){
   console.log(this) //输出newobj
}
new Newobj
```

- **内部函数**

```
var name = "this is window"; //定义window的name属性,看this.name是否会调用到 
var testObj = { 
 name : "this is testObj", 
 getName:function(){ 
 //var self = this; //临时保存this对象 
 var handle = function(){ 
 console.log(this); //控制台输出: Window //this指向的是全局对象--window对象 
 console.log(this.name); //控制台输出: this is window 
 //console.log(self); //这样可以获取到的this即指向testObj对象 
 } 
 handle(); 
 } 
} 
testObj.getName();
```

- **call和apply**

```
var name = 'this is window'; //定义window的name属性,看this.name是否会调用到 
var testObj1 = { 
 name : 'this is testObj1', 
 getName:function(){ 
 console.log(this); //控制台输出: testObj2 //this指向的是testObj2对象 
 console.log(this.name); //控制台输出: this is testObj2 
 } 
}
var testObj2 = { 
 name: 'this is testObj2'
}
testObj1.getName.apply(testObj2); 
testObj1.getName.call(testObj2);
note：apply和call类似，只是两者的第2个参数不同：
[1] call( thisArg [，arg1，arg2，… ] );  // 第2个参数使用参数列表：arg1，arg2，...
[2] apply(thisArg [，argArray] );     //第2个参数使用 参数数组：argArray
运行结果分析：使用call / apply  的函数里面的this指向绑定的对象。
```