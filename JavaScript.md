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
结果，Uncaught ReferenceError: Cannot access 'tmp' before initialization.

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
- 解决方案1
```{javascript}
var a=[];
for(var i=0;i<10;i++){
    a[i]=function(num){
        return function(){
            return num;
        };
    }(i);
}
a[6]();
```
- 解决方案2
  - 把var改成let。
  
# 