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

> 每个构造函数都有一个原型对象，如果这个默认原型对象被重写为另一个构造函数的实例，那么这个原型内部就会有一个属性指向另一个原型。
>
> 原型链构成的基础是原型内部有一个属性指向另一个原型。

# 19.原型链的终点是什么

> 正常的原型链都会终止于Object的原型对象，因为Object原型的原型是null。
>
> 因为原型链的构成基础是构造函数的原型内部有一个属性指向另一个原型，Object的原型不再指向另一个原型，所以原型链终止了。

# 20.封装、继承、多态

> - 封装
>   - 封装是把抽象的事物封装成类，它的意义在于避免我们无意间破坏代码，并且阻止类外的代码访问不应被访问的变量。
> - 继承
>   - 继承就是让一个对象获得另一个对象的方法，它的意义在于重用代码，提升效率。
> - 多态
>   - 

# 21.Promise，看代码，说执行顺序

>  - 1.
>  ```{javascript}
>  let promise = new Promise((resolve,reject) =>{
>     console.log("promise");
>     resolve();})
>  promise.then(() => {
>     console.log("Fulfilled");
>  })
>  console.log("Hi")
>  ```
>
>  //promise
>
>  //Hi
>
>  //Fulfilled
>
>  因为promise实例一旦创建，就会立即执行，所以首先打印出promise，且因为.then方法指定的回调函数会在所有同步函数执行结束后执行，所以先打印出Hi，再打印出Fulfilled。
>
>  - 2.
>
>  ```{javascript}
>  let p1 = new Promise((resolve,reject) =>{
>     setTimeout(() => reject(new Error('fail')),1000);
>  })
>  let p2 = new Promise((resolve,reject) =>{
>     setTimeout(() => resolve(p1),1000);})
>  p2.then((value) => {console.log(value)}).catch((error) => console.log(error))
>  ```
>
>  //fail
>
>  因为如果p2的resolve()函数返回的是另一个Promise p1，那么p2的then方法都将变为针对p1的。
>
>  - 3.
>  ```{javascript}
>  new Promise((resolve,reject) =>{
>      resolve(1);
>      console.log(2);
>  }).then((value) => {
>      console.log(value);})
>  ```
>  //2
>  //1
>
>  因为resolve和reject函数都不会终结Promise对象的参数函数的运行，且因为立即resolve的Promise是在本次事件循环的末尾执行，总是晚于本轮循环的同步任务。
>
>  ```{javascript}
>  new Promise((resolve,reject) =>{
>      return resolve(1);
>      console.log(2);
>  }).then((value) => {
>      console.log(value);})
>  ```
>
>  // 1
>
>  因为加了return，Promise的参数函数就不会继续往后执行。
>
>  - 4.
>
>    ```{javascript}
>    setTimeout(() => {
>        console.log("three");
>    },0);
>    Promise.resolve().then(function(){
>        console.log("two");
>    });
>    console.log("one");
>    ```
>
>    //one
>
>    //two
>
>    //three
>
>    因为立即resolve的Promise的回调函数是在本轮事件循环末尾执行，所以晚于同步代码console.log("two");
>
>    因为setTimeout(fn,0)是在下一轮事件循环开始时执行，所以最晚。
>
>  - 

# 22.什么是Promise

> Prommise是一个容器，保存着异步操作的结果。
>
> Promise有2个特点，1. Promise有3种状态，Pending表示执行中，Fulfilled表示成功，Rejected表示失败。2.Promise的状态一旦改变，就不会再改变。
>
> ES6规定，Promise对象是一个构造函数，它可以生成Promise实例，Promise构造函数接收1个函数作为参数，这个函数有2个参数，resolve和reject，这是2个函数，resolve在Promise对象从Pending转换为Fulfilled时调用，它的参数将会被传给.then()方法，reject在Promise对象从Pending转换为Rejected时调用，它的参数将传给.then()方法的第二个参数函数。

# 23.Promise的8个方法

> 1. promise.prototype.then()
>
>    - promise.prototype.then()方法的作用是为Promise实例添加在对象改变时调用的回调函数。
>    - promise.prototype.then()方法接收2个函数作为参数，第一个函数是Promise实例状态从Pending改变为Fulfilled时的回调函数，第二个函数是Promise实例状态从Pending改变为Rejected时的回调函数。
>    - promise.prototype.then()方法返回的是一个Promise实例，所以可以采用链式写法。
>
> 2. promise.prototype.catch()
>
>    - promise.prototype.catch()方法的作用是指定发生错误时或Promise实例从Pending改变为Rejected时的回调函数。
>    - 建议使用promise.prototype.catch()定义Promise实例从Pending改变为Rejected时的回调函数，因为Promise对象的错误具有冒泡性质，会一直向后传递，这样catch方法可以捕获前面所有部分传来的错误。
>
> 3. Promise.all()
>
>    - Promise.all()方法的作用是把多个Promise实例打包成一个新的Promise实例。
>
>      ```{javascript}
>      let p = Promise.all([p1,p2,p3]);
>      ```
>
>      p1、p2、p3都是Promise实例，如果不是，那么会先调用Promise.resolve()。
>
>    - Promise.all()接收1个数组作为参数，也可以不是数组，但必须具有Iterator接口。
>
>    - Promise.all()得到的p的状态由p1、p2、p3决定，有2种情况
>
>      1. 只有p1、p2、p3的状态都变成Fulfilled，p的状态才会变成Fulfilled，这时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
>      2. 只要p1、p2、p3中有1个变为Rejected，p的状态就变为Rejected，这时第一个变为Rejected的实例的返回值会传递给p的回调函数。
>
> 4. Promise.race()
>
>    - Promise.race()方法的作用是将多个Promise实例包装成一个Promise实例。
>
>      ```{javascript}
>      let p = Promise.race([p1,p2,p3]);
>      ```
>
>    - Promise.race()和Promise.all()的不同在于，Promise.race()得到的p的状态，由p1、p2、p3中第一个发生改变的Promise实例决定，率先发生改变的Promise实例的返回值（resolve或reject函数中的值）就会传递给p的回调函数。
>
> 5. Promise.resolve()
>
>    - Promise.resolve()方法的作用是将现有对象转换为Promise实例。
>
> 6. Promise.reject()
>
>    - Promise.reject()方法的作用是返回一个Promise实例，状态为Rejected。
>
> 7. 

# 24.setTimeout()函数的执行

> setTimeout()函数接收2个参数，1.待加入消息队列的消息。2.一个时间值（默认是0）。这个时间值代表了消息被实际加入消息队列的最小延迟时间，如果队列中没有其他消息并且栈为空，那么在这段延迟时间过去之后，消息会被马上处理，但是如果队列中有其他消息，setTimeout()消息必须等待其他消息处理完。

# 25.执行上下文

> 执行上下文分为全局上下文、函数上下文和块级上下文。 
>
> 当一段JavaScript代码在运行的时候，它实际上是运行在执行上下文中，有4种类型的代码会创建一个执行上下文。
>
> 1. 全局上下文是为运行代码主体而创建的执行上下文，它是为了那些存在于函数之外的所有代码创建的。
> 2. 每个函数会在调用的时候创建自己的执行上下文。
> 3. 代码进入一个块也会创建一个上下文。
> 4. 使用eval()函数也会创建一个新的执行上下文。
>
> 每一个上下文本质上是一种作用域层级。

# 26.执行上下文栈

> 每个执行上下文在创建时会被推入一个执行上下文栈，当退出的时候它会从栈中退出并销毁。
>
> ```{javascript}
> let outputElem = document.getElementById("output");
> 
> let userLanguages = {
>   "Mike": "en",
>   "Teresa": "es"
> };
> 
> function greetUser(user) {
>   function localGreeting(user) {
>     let greeting;
>     let language = userLanguages[user];
> 
>     switch(language) {
>       case "es":
>         greeting = `¡Hola, ${user}!`;
>         break;
>       case "en":
>       default:
>         greeting = `Hello, ${user}!`;
>         break;
>     }
>     return greeting;
>   }
>   outputElem.innerHTML += localGreeting(user) + "<br>\r";
> }
> 
> greetUser("Mike");
> greetUser("Teresa");
> greetUser("Veronica");
> ```
>
> - 程序开始运行时，全局上下文就会被创建好。
>
>   - 当执行到 
>
>     ```
>     greetUser("Mike")
>     ```
>
>      的时候会为 
>
>     ```
>     greetUser()
>     ```
>
>      函数创建一个它的上下文。这个执行上下文会被推入执行上下文栈中。
>
>     - 当 `greetUser()` 调用 `localGreeting()`的时候会为该方法创建一个新的上下文。并且在 `localGreeting()` 退出的时候它的上下文也会从执行栈中弹出并销毁。 程序会从栈中获取下一个上下文并恢复执行, 也就是从 `greetUser()` 剩下的部分开始执行。
>     - `greetUser()` 执行完毕并退出，其上下文也从栈中弹出并销毁。
>
>   - 当 
>
>     ```
>     greetUser("Teresa")
>     ```
>
>      开始执行时，程序又会为它创建一个上下文并推入栈顶。
>
>     - 当 `greetUser()` 调用 `localGreeting()`的时候另一个上下文被创建并用于运行该函数。 当 `localGreeting()` 退出的时候它的上下文也从栈中弹出并销毁。 `greetUser()` 得到恢复并继续执行剩下的部分。
>     - `greetUser()` 执行完毕并退出，其上下文也从栈中弹出并销毁。
>
>   - 然后执行到 
>
>     ```
>     greetUser("Veronica")
>     ```
>
>      又再为它创建一个上下文并推入栈顶。
>
>     - 当 `greetUser()` 调用 `localGreeting()`的时候，另一个上下文被创建用于执行该函数。当 `localGreeting()`执行完毕，它的上下文也从栈中弹出并销毁。
>     - `greetUser()` 执行完毕退出，其上下文也从栈中弹出并销毁。
>
> - 主程序退出，全局执行上下文从执行栈中弹出。此时栈中所有的上下文都已经弹出，程序执行完毕。

# 27.执行上下文和作用域链的关系

> 执行上下文保存着自己的作用域链，每个执行上下文都有自己的作用域链，执行上下文在被销毁时，作用域链也会被销毁。

# 28.作用域链

> 每个执行上下文都会有一个包含其中变量的对象，全局上下文中的叫变量对象，函数局部上下文中的叫活动对象。
>
> 作用域链是一个包含着指针的列表，每个指针指向一个变量对象。
>
> 作用域链引用着从当前执行上下文一直到全局上下文的所有变量对象，这样作用域链就保证了可以从当前执行上下文可以找到包含执行上下文中的变量。

# 29.作用域

> JavaScript中的作用域是指词法作用域。
>
> 红宝书说执行上下文等于作用域。

# 30.map和forEach的区别

> map和forEach都是ECMAScript为数组定义的5个迭代方法之一。
>
> map会对数组的每一项运行传入的函数，并返回结果组成的数组。
>
> forEach会对数组的每一项运行传入的函数，但是并不会返回结果。
>
> map和forEach都不会改变原数组。
>
> 本质上，forEach()相当于使用for循环遍历数组。

# 31.JavaScript的垃圾回收

> JavaScript的垃圾回收有2种。
>
> 1. 标记清理
>    - 标记清理的策略
>      - 垃圾回收程序在运行的时候，会标记内存中所有变量，然后会为所有处于还在上下文中的变量和所有被还在上下文中的变量引用的变量的标记删除，然后会销毁剩下还带有标记的变量。
>    - 标记清理的方法（方法并不重要，重要的是策略）
>      -  维护2个列表，一个是在上下文中，一个不在上下文中。
> 2. 引用计数
>    - 引用计数的策略（因为循环引用的问题，已经逐渐不常用了）
>      - 引用计数不是那么常用，引用计数会为变量设置它的引用值是1，然后每多一个变量引用该变量，引用值都会加1，如果一个变量的引用值等于0，这个变量就会被回收。
>    - 循环引用
>      - 当2个对象互相引用对方的时候，即使已经退出上下文，内存也不会被回收。只有引用值可能出现循环引用，原始值不会出现。

# 32.Object的方法

> 1. Object.defineProperty()，修改属性的默认特性。
> 2. Object.getOwnPropertyDescriptor()，获取属性的特性。
> 3. Object.assign()，合并对象。
> 4. Object.is()，弥补===的不足。
> 5. Object.getPrototypeOf()，获取传入参数的原型对象。
> 6. Object.setPrototypeOf()，可以修改传入参数的原型对象。
> 7. Object.create()，使用传入的参数作为原型对象创建一个新的对象，可以避免Object.setPrototypeOf()带来的性能下降。（原型式继承）。

# 33.JavaScript的6种继承方式

> 1. 原型链继承
>
>    - 原型链继承的策略
>      - 原型链继承是利用了原型链实现子类的实例可以通过原型链访问到父类的原型上的方法。
>
>    - 原型链继承的缺点
>      - 当父类构造函数中存在引用值，如数组时，因为子类的原型是父类的实例，所以子类的实例将会共享原型上的引用值，这使得instance1对于引用值的修改，也会体现到instance2上。
>
> 2. 盗用构造函数继承
>
>    - 盗用构造函数的策略
>      - 盗用构造函数会在子类的构造函数中调用父类的构造函数。通过使用call()或apply()方法，父类构造函数中的this对象被绑定到子类构造函数的this对象，当在使用new操作符调用子类构造函数时，子类构造函数的this对象将会指向创建的新对象，因此父类的构造函数的this对象也会指向创建的新对象，因此新对象会拥有父类构造函数中定义的属性和方法。
>    - 盗用构造函数的缺点
>      - 盗用构造函数实际上是构造函数模式创建对象，它不能继承父类构造函数原型中的方法。
>
> 3. 组合继承
>
>    - 组合继承的策略
>      - 组合继承是结合了原型链继承和盗用构造函数继承，在子类构造函数中通过call()或apply()调用构造函数，并且把子类构造函数的原型重置为父类的实例，这样通过原型链继承了父类构造函数原型中的方法，并且因为每个子类的实例都相当于调用了一遍父类构造函数，所以每个子类实例中都会有父类构造函数中定义的属性，这样就会遮蔽子类的原型中的引用值。
>    - 组合继承的优点
>      - 组合继承的优点是保留了instanceof操作符和isPrototypeOf()识别合成对象的能力。
>
> 4. 原型式继承
>
>    - 原型式继承的策略
>
>      - 原型式继承的策略是即使不自定义类型，也可以通过原型实现多个对象间的信息共享。
>
>        ```{javascript}
>        function Object(o)
>        {
>            function F()
>            {}
>            F.prototype = o;
>            return new F();
>        }
>        ```
>        原型式继承适用于已经有一个对象的基础上，想在这个对象的基础上创建一个新对象。ES5使用Object.create()函数将原型式继承规范化了。
>
>    - Object.create()
>
>      - Object.create()函数接收2个参数，第一个参数是传入作为新对象原型的对象，第二个参数是给新对象额外定义属性的对象（第二个参数可选）。
>
>        ```{javascript}
>        function Person(name)
>        {
>            this.name = name;
>        }
>        
>        let person1 = new Person("name");
>        const instance1 = Object.create(person1,{age : {value : 1,writable: false}})
>        instance1.age;
>        instance1.name;
>        Object.getOwnPropertyDescriptor(instance1,"age");
>        ```
>
> 5. 寄生式继承
>
>    - 寄生式继承的策略
>      - 寄生式继承类似于原型式继承，只是它使用了一个工厂函数为新对象定义新的方法。
>    - 寄生式继承的缺点
>      - 它的缺点类似于构造函数模式，函数无法重用。
>
> 6. 寄生式组合继承
>
>    - 寄生式组合继承的策略
>
>      - 寄生式组合继承解决了组合继承的效率问题，因为组合继承需要调用2次父类的构造函数，但是使用寄生式组合继承只需要调用一次父类构造函数。
>
>      - 寄生式组合继承是使用寄生式继承的方法来继承父类的原型对象。
>
>        ```{javascript}
>        function object(o)
>        {
>            function F(){}
>            F.prototype = o.prototype;
>            return new F();
>        }
>        function inheritPrototype(SubType,SuperType) //通过这个函数减少了一次调用父类构造函数，但是也将子类构造函数的原型重写为了父类构造函数的实例，SubType是子类构造函数，SuperType是父类构造函数。
>        {
>            let prototype = object(SuperType);
>            prototype.constructor = SubType;
>            SubType.prototype = prototype;
>        }
>        ```

# 34.instanceof操作符的原理

> instanceof操作符用来判断引用值类型，因为typeof操作符对于引用值只能得到Object，想要判断哪一类的引用值就要使用instanceof操作符。
>
> instanceof操作符的原理是判断目标对象是否存在于左边操作数的原型链上。
>
> ```{javascript}
> function SuperType()
> {}
> 
> function SubType()
> {}
> 
> SubType.prototype = new SuperType();
> 
> let instance1 = new SubType();
> 
> instance1 instanceof SuperType; //true
> instance1 instanceof SubType; //true
> ```

