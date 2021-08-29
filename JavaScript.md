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

