# 1. CJS和AMD

- 模块

  - 在ES6之前，JavaScript一直没有模块，这样就无法将一个大程序拆分成互相依赖的小文件，再用简单的方法拼接起来。
  - 没有模块系统对开发大型应用是很大的阻碍。

- CJS和AMD

  - 为了克服JavaScript没有Module的缺点，在ES6之前，社区制定了一些模块加载方案，主要用CommonJS和AMD两种。前者用于服务器，后者用于浏览器。

  - CJS

    ```{javascript}
    let { stat, exists, readfile } = require('fs');
    // 等同于
    let _fs = require('fs'); //引用模块本身
    let stat = _fs.stat;
    let exists = _fs.exists;
    let readfile = _fs.readfile;
    ```

    从上面代码可以看出，CJS的实质是完整加载fs模块，并将其生成一个对象，再使用对象解构赋值获取其中的方法。

    因此CJS实际上是一种在运行时加载的模块解决方案。这样的坏处是没办法在编译时拿到这个对象，因此不能做静态优化。

    静态优化是指只加载需要加载的，而不是生成一个对象。

  - AMD

- ES6模块

  - ES6模块不是对象

    ```{javascript}
    import { stat, exists, readFile } from 'fs';
    ```

    上面代码的实质是从fs模块加载这三个函数，其他都不需要加载。这种加载被称为静态加载。

    这样的缺点在于我们无法引用模块本身。

  - 严格模式

    - 不管有没有在文件头声明'use strict'，ES6模块都是严格模式。

- export命令

  - export命令用于定义模块的对外接口。如果想在模块以外访问到模块内部的变量，那么必须使用export命令将其输出。

  - export的写法有2种

    1. export接在语句最前面

       ```{javascript}
       export var firstName = 'a';
       export function multiply(x, y) {
         return x * y;
       };
       
       ```

    2. 使用大括号在模块底部统一导出

       ```{javascript}
       var firstName = 'a';
       function multiply(x, y) {
         return x * y;
       };
       export {firstName, multiply}
       ```

  - 常见错误写法

    ```{javascript}
    var firstName = 'a';
    export firstName; //错误
    function multiply(x, y) {
      return x * y;
    };
    export multiply; //错误
    ```

- import命令

  - import命令用于输入其他模块提供的功能。

  - 一个例子

    ```{js}
    import { firstName, lastName, year } from './profile.js';
    //等同于
    import * as profile from './profile.js'
    ```

    import的变量是只读的，但是这种只读的本质是栈内存中的储存的内容不允许改变，也就是如果import的是一个对象，是可以更改对象的属性的，但是这样是一种不好的方法，因为很难追溯错误。

- export default命令

  - 在不使用export default的时候，当想要在模块以外import的时候，需要知道要加载的变量的变量名。这对于库来说是不好的，因为用户不想要去一个一个查看变量的命名。

  - export default可以让在import的时候可以使用任意命名。

  - export default在一个模块中只能使用一次。

  - export default在import的时候不需要使用大括号将变量名括起来。

  - export default后面不能跟变量声明语句

    ```{javascript}
    export default var a = 1; //错误
    
    var a = 1;
    export default a; //正确
    ```

  - export default的实质

    ```{javascript}
    export default function add(x,y){
      return x + y;
    }
    //等价于
    function add(x,y){
      return x + y;
    }
    export { add as default };
    ```

    ```{javascript}
    import foo from './add.js'
    //等价于
    import { default as foo } from './add.js'
    ```

    因此我们可以看出export default就是一个语法糖。



# 2. Module的加载

- 浏览器环境
  - 
- 