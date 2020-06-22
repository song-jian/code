## JS数组reduce方法详解及高级技巧
  #### 1. 语法  

  ```
  arr.reduce(callback,[initivalValue])
  ``` 
  reduce为数组中的每一个元素依次执行回调函数，不包括数组中被删除或者未赋值的元素；包含四个参数：
  初始值（或者上一次回调函数的返回值）、当前元素值、当前元素索引、调用reduce的数组; callback(init,current,index,arr)
  initvalValue:加入没有提供initvalValue,reduce会从数组索引1的地方开始执行callback,跳过第一个索引，如果提供初始值，从索引0开始

  #### 2. 用法
  * a.求和
    ```
    let arr = [1,2,3,4];
    let sum = arr.reduce((x,y)=>x+y)
    let mul = arr.reduce((x,y)=>x*y)

    ```

  * b.计算数组中没有元素出现的次数
    ```
    let arr = [1,2,3,4]
        let res = arr.reduce((pre,cur,index)=>{
            if(cur in pre){
                pre[cur]++
            }else{
                pre[cur]= 1
            }
            return pre
        },{})
    console.log(res);
    ```
* c.将二维数组转化为一维数组
    ```
    let arr1 = [
            [1, 2],
            [3, 2],
            [4, 4]
        ]
        let res1 = arr1.reduce((pre, cur) => {
            return pre.concat(cur)
        }, [])

    console.log(res1);
    ```
*  d.将多维数组转化为一维数组
   ```
   let arr2 = [
            [1, 2],
            [3, 2,[7,8]],
            [4, 4,[5,6]]
        ]

        const newArr = (arr)=>{
            return arr.reduce((pre,cur)=>{
              return pre.concat(Array.isArray(cur)?newArr(cur):cur)
            },[])
        }

    console.log(newArr(arr2));
    ```



