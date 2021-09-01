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

# 7.改变this指向的方法

> 使用call()、apply()、bind()函数。
>
> - call()函数接收多个参数，第一个参数是this值，之后是传给调用call()的函数的参数，这些参数必须逐个传递。
> - apply()函数接收2个参数，第一个参数是this值，之后是传给调用apply()的函数的参数列表，可以是Array的实例，也可以是arguments对象。
> - bind()函数接收this值，它会先创建一个调用bind()的函数的实例，这个新函数实例中的this对象会被绑定到bind()的传入参数设置的this值。

# 8.什么是this

> this是ES5新增的函数体内的特殊对象。
>
> this对象在普通函数和箭头函数中有不同的表现。
>
> - 在普通函数中，this对象引用的是调用this所在函数的作用域的活动对象，在普通函数中，this到底指向哪个作用域的活动对象，只能到函数被调用时才能确定。
> - 在箭头函数中，this对象引用的是定义箭头函数的作用域。

# 9.什么是作用域

> 作用域是一套规则，定义了如何查找变量（标识符）。
>
> 在存在块或函数嵌套的时候，会产生作用域嵌套，引擎在需要某个变量时，会对这个变量做RHS查询，RHS查询会首先询问当前执行作用域是否存在引擎想要的变量，如果不存在，引擎就会去上一级作用域寻找，如果一直到全局作用域都没有找到，引擎会停止查询，并抛出ReferenceError。

# 10.什么是RHS和LHS

> RHS和LHS是引擎向作用域查找变量的2种方法。
>
> RHS应用在发生需要找到某个变量（标识符）的值的时候。
>
> LHS应用在发生需要对某个变量进行赋值的时候。
>
> 在非严格模式下，LHS查询过程中，如果一直到全局作用域都没有找到变量的声明，那么全局作用域会生成一个对应的变量的声明。  
>
> 使用RHS查询，如果查找到了引擎所需的变量声明，但是对这个变量保存的值做了不合法的操作，那么会抛出TypeError。

# 11.修改对象属性的默认特性的方法

> Object.defineProperty()。
>
> ```{javascript}
> let person = {};
> Object.defineProperty(person,"name",{
>     writable: false,
>     value: "a"
> })
> //这样为person对象定义了一个不可修改的name属性。
> ```

# 12.读取属性特性的方法

> Object.getOwnPropertyDescriptor()。
>
> ```{javascript}
> let person = {};
> person.name = "a";
> let descriptor = Object.getOwnPropertyDescriptor(person,"name");
> ```

# 13.合并对象

> ES6新增了合并对象的方法Object.assign()。
>
> Object.assign()接收一个目标对象和一个或多个源对象，然后将源对象种所有可枚举（Object.propertyIsEnumerable()返回true）的属性和所有自有（Object.hasOwnProperty()返回true）的属性复制到目标对象。

# 14.ES6新增特性

> 1. Object.is()可以用来弥补 === 的不足，如，Object.is(NaN,NaN)返回true。
> 2. 解构赋值，ES6运行以一定的模式从数组或对象中提取值。
> 3. Object.assign()用于对象合并。
> 4. 类和继承

# 15.new操作符创建对象的步骤

> new操作符创建对象一共有5步。
>
> 1. 在内存中创建一个新对象。
>
> 2. 新对象内部的[[Prototype]]属性被赋值为构造函数的prototype属性。
> 3. 构造函数内部的this值指向这个新对象。
> 4. 执行构造函数内部的代码。
> 5. 返回该对象。

# 16.创建对象的3种模式

> 1. 工厂模式
>
>    - 工厂模式是一种设计模式，它解决了使用Object构造函数或对象字面量创建多个特定对象需要重复大量代码的问题，但是它没有解决对象标识的问题（所有由工厂模式创建的对象的类型都是Object）。
>
>      ```{javascript}
>      function createPerson(name,age)
>      {
>          let o = new Object();
>          o.name = name;
>          o.age = age;
>          return o;
>      }
>      let person1 = createPerson("a",1);
>      typeof person1 //"Object"
>      person1 instanceof Object //true
>      ```
>
>      
>
> 2. 构造函数模式
>
>    - 构造函数模式解决了工厂模式没有解决对象标识的问题，通过构造函数创建的对象都是特定类型的，但它没有解决每使用构造函数生成一个实例，构造函数中定义的函数就会被创建一遍，而实际上所有由构造函数生成的实例可以共享一个函数。
>
>      ```{javascript}
>      function Person(name,age)
>      {
>          this.age = age;
>          this.name = name;
>          this.sayName = function()
>          {
>              console.log(this.name);
>          }
>      }
>      let person1 = new Person("a",1);
>      person1 instanceof Person; //true
>      ```
>
>      
>
> 3. 原型模式
>
>    - 原型模式解决了构造函数模式中，每个实例拥有一个函数的缺点，它把构造函数中定义的函数放到构造函数的原型对象中，这样所有实例可以共享这些方法。
>
>      ```{javascript}
>      function Person(name,age)
>      {
>          this.age = age;
>          this.name = name;
>      }
>      Person.prototype.sayName = function()
>      {
>          console.log(this.name);
>      }
>      ```

# 17.什么是原型

> 每个函数体内都会有一个prototype属性，这个属性指向一个对象，这个对象就是函数的原型对象。
>
> 只要创建一个函数，就会为这个函数创建一个原型对象，所有原型对象默认会获得一个属性constructor指向它对应的函数。（其他方法都因原型链继承自Object对象）。
>
> 从构造函数创建的实例，会有一个[[Prototype]]属性指向构造函数对应的原型对象，但这个特性不能被直接访问，所以在Chrome、Safari、FireFox中定义了一个\__proto__属性来指向原型对象。

# 18.什么是原型链

> 

# 19.原型链的终点是什么

> 正常的原型链都会终止于Object的原型对象，因为Object原型的原型是null。

# 20.封装、继承、多态

> - 封装
>   - 封装是把抽象的事物封装成类，它的意义在于避免我们无意间破坏代码，并且阻止类外的代码访问不应被访问的变量。
> - 继承
>   - 继承就是让一个对象获得另一个对象的方法，它的意义在于重用代码，提升效率。
> - 多态
>   - 
