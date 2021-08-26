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