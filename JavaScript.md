# 1.用set实现去重。
```{javascript}
let arr = [1,2,4]
let set = new Set(arr)
arr = Array.from(set)
```

# 2.var和let，看代码说输出
```{javascript}
var tmp = 123;
if(true){
    tmp = 'abc';
    let tmp;
    console.log(tmp);
}
```
报错，Uncaught ReferenceError: Cannot access 'tmp' before initialization

> 因为暂时性死区，在找tmp变量的时候，JavaScript会沿着作用域链，而作用域链的最前面是块级作用域，所以JavaScript先找到用let声明的tmp，然后发现在声明之前使用了这个变量，所以触发暂时性死区。

# 3.闭包问题
```{javascript}
var a=[];
for(var i=0;i<10;i++){
    a[i]=function(){
        console.log(i);
    };
}
a[6](); //10
```
如何让a[i]输出i，a[1]输出1。

> 因为匿名函数引用了外部作用域的变量i，所以匿名函数构成了闭包。

- 解决方案1
```{javascript}
var a=[];
for(var i=0;i<10;i++){
    a[i]=function(i){
        return () => console.log(i);
    }(i);
}
a[6](); //6
```
> ```{javascript}
> a[i]=function(i){
        return () => console.log(i);
    }(i);
> ```
> 会立即调用匿名函数，所以a[i]储存的是返回的箭头函数，因为箭头函数是闭包，所以维持着对i的引用，所以等到a[6]/()调用这个箭头函数的时候，会打印出6。

- 解决方案2

  > 把var改成let

```{javascript}
var a=[];
for(let i=0;i<10;i++){
    a[i]=function(){
        console.log(i);
    };
}
a[6](); //6
```

# 4. JavaScript中的数值存储方式

> ECMAScript中数值以IEEE 754方式存储，所以都是64位，但是当我们对数值做位运算时，后台会自动把64位转换成32位，等完成了位操作，再转换成64位，这个过程对于开发者是不可见的，所以就相当于ECMAScript是以32位储存数值。
>
> **但是这个过程也会导致一些问题，如，NaN被当成0处理，Infinity被当成0处理。**

# 5.JavaScript中的位运算

> ECMAScript一共有7种位运算符。
>
> 1. ~，表示按位非，最终得到的数值是操作数的负值减1。
> 2. &，表示按位与，实际上，它是把2个操作数的每一位对齐，并做与操作。
> 3. |，表示按位或，实际上，它是把2个操作数的每一位对齐，并做或操作。
> 4. ^，表示按位异或，它也是把2个操作数的每一位对齐，在只有1位是1时，返回1。
> 5. <<，左移，它会把所有位向左移指定位，左移不会改变符号位。
> 6. \>>，无符号右移，它会把除符号位外31位向右移指定位。
> 7. \>>>，有符号右移，有符号右移对于正数来说和无符号右移没有区别，但是对于负数，由于负数是使用补码保存的，它会把补码直接当成正常的二进制右移（整个32位），负数做有符号右移之后，会变为正数，且会变得非常大。

# 6.防抖和节流

> - 什么时候用到防抖和节流
>
>   - 当某一个事件的触发频率非常高，如，用户点击按钮、用户移动鼠标，为了节省浏览器资源，就需要对事件的执行做出限制。
>
> - 防抖
>
>   - 防抖，就是在用户点击按钮、用户移动鼠标触发事件后，需要等待一段时间再执行事件函数，如果在等待的过程中，这个事件再次触发，那么重新计时等待。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css"></style>
>         </head>
>         <body>
>             <h1>防抖</h1>
>             <input id = "btn" type = "button" value = "add"/>
>             <h2>数字</h2>
>             <span id = "num">0</span>
>         </body>
>         <script>
>             let button = document.getElementById('btn');
>             let number = document.getElementById('num');
>     
>             let m = 0;
>             let time;
>             let isOk = true;
>     
>             //不使用闭包，思路清晰。
>             button.onclick = function()
>             {
>                 console.log("xxxxxx");
>     
>                 clearTimeout(time);
>     
>                 time = setTimeout(() => {
>                     number.innerHTML = ++m;
>                 },1000);
>             }
>     
>             //使用闭包，使用闭包的好处是把防抖封装成了函数。
>             function debounce(fn,delay)
>             {
>                 let timer = null;
>                 return function()
>                 {
>                     console.log("xxxxx");
>                     if(timer !== null)
>                     {
>                         clearTimeout(timer);
>                     }
>                     timer = setTimeout(fn,delay);
>                 }
>             }
>     
>             button.onclick = debounce(() => {number.innerHTML = ++m},1000);
>     
>         </script>
>     </html>
>     ```
>
>     
>
> - 节流
>
>   - 节流，是指在触发事件之后，只执行第一次，然后在一段时间内，剩下的触发都不会执行事件。（用于即使用户频繁触发事件，也要求有一定反馈的场景）。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css"></style>
>         </head>
>         <body>
>             <h1>节流</h1>
>             <input id = "btn" type = "button" value = "add"/>
>             <h2>数字</h2>
>             <span id = "num">0</span>
>         </body>
>         <script>
>             let button = document.getElementById('btn');
>             let number = document.getElementById('num');
>     
>             let m = 0;
>             let time;
>             let isOk = true;
>     
>             //不使用闭包，思路清晰。
>             button.onclick = function()
>             {
>                 console.log("xxxxxx");
>                 if(!isOk)
>                 {
>                     return;
>                 }
>                 number.innerHTML = ++m;
>                 isOk = false;
>                 time = setTimeout(() => {isOk = true;clearTimeout(time);},1000);
>             }
>     
>             //使用闭包，把节流封装成函数
>             function throttle(fn,delay)
>             {
>                 let isOk = true;
>                 return function()
>                 {
>                     if(!isOk)
>                     {
>                         return;
>                     }
>                     isOk = false;
>                     fn();
>                     time = setTimeout(() => {
>                         isOk = true;
>                         clearTimeout(time);
>                     },delay);
>                 }
>             }
>     
>             button.onclick = throttle(() => {number.innerHTML = ++m;},1000);
>     
>         </script>
>     </html>
>     ```
>
>     
