# 1.async和await

- async和Generator函数对比

  - async函数实际上是Generator函数的语法糖。它的目的是使异步操作看起来更像同步操作。

- 基本用法

  - async函数会返回一个Promise对象，也就是说调用async函数会立即获取一个Promise对象（但状态在await全部执行完之前都是PENDING）。

  - async关键字放在function关键字前用于声明async函数。对于箭头函数，async放在()之前。

    ```{js}
    async function foo(){}
    
    const bar = async () => {}
    ```

  - await后面可以跟原始值或Promise对象（跟原始值的话相当于调用一个Promise.resolve()）。

- 语法

  - 返回的Promise对象
    - async函数内部return的值将成为then方法回调函数的参数。
    - async函数内部的错误会导致返回的Promise对象变为reject状态。抛出的错误可以被Promise.prototype.catch()捕捉到。
    - async函数返回的Promise对象必须等到async函数内部所有await命令都执行完毕，这个Promise的status才会改变。也就是说除非执行到return语句或抛出错误，async函数返回的Promise对象的status都会是PENDING。
  - await
    - await命令后面可以接一个Promise对象或原始值，如果是原始值的话，那么对这个原始值调用Promise.resolve()将其转换为Promise对象。await命令会返回Promise对象中的value实例属性。
    - await命令后面也可以接一个thenable的对象（定义了then方法的对象），那么会先对这个对象调用Promise.resolve。Promise.resolve()会立即将这个对象转化为一个Promise对象，并且立即执行它的then方法，最终返回它的then方法返回的Promise对象。
    - 在async函数中借助await命令可以实现暂停执行代码的作用（只能暂停async内部的代码执行，是一种伪暂停）。
    - await后面的promise对象如果status变为reject（执行出错或返回reject），那么都将视为async函数执行抛出错误，将被async函数返回的Promise对象定义的catch方法捕捉。因此如果async函数中有多个await操作，如果第一个变成reject或执行出错，剩下的await命令都不会被执行。
    - 如果想要即使前面的await操作失败也不要影响后面的异步操作，我们可以将前面的await放在try catch结构中。（这也是为什么请求接口的时候最好将它用try catch包起来，这样就可以不影响后面的异步操作）。

- 使用注意点

  1. 由于await后面的Promise对象可能变成rejected导致中断整个async函数的运行，因此建议将await命令放在try catch结构中。

  2. 如果async函数中的多个await命令不存在依赖关系，那么最好将它们同时触发。

     ```{js}
     async function demo(){
       let [foo, bar] = await Promise.all([getFoo(),getBar()]);
     }
     ```

     数组的解构赋值以index来进行，等式右边必须具有Iterator接口。

  3. await和yield类似，await只能用在async函数中，yield只能用在Generator函数中。

- async的实现原理

  - async的等价写法

    ```{js}
    async function fn() {}
    
    //等价于
    
    function fn(){
      return run(genFn) //genFn是对应的Generator函数。
    }
    ```

  - 找出对应的Generator

    - 将async改成function*

    - 将await改成yield

    - 一个例子

      ```{js}
      async function fn(){
          let [res1,res2] = await Promise.all([someAsyncThings(1000),someAsyncThings(5000)]);
          console.log(res1,res2);
      }
      ```

      上面是一个async函数，那么它对应的Generator函数就是

      ```{js}
      function* fn(){
          let [res1,res2] = yield Promise.all([someAsyncThings(1000),someAsyncThings(5000)]);
          console.log(res1,res2);
      }
      ```

  - run函数的实现方法

    ```{js}
    function run(gen){
        return new Promise((resolve,reject) => { //因为async函数返回的是一个Promise对象。
            const h = gen(); //获取Generator函数返回的迭代器对象。
            function step(nextFn)
            {
                let next;
                try{
                    next = nextFn(); //在回调函数中执行迭代器的next()方法，这里加一个try catch是因为，async函数中任何一个await执行出错或返回reject都将触发async返回的Promise对象reject。
                }catch(err){
                    reject(err);
                }
                if(next.done){
                    resolve(next.value); //如果遇到return或遍历完成，async函数返回的Promise对象变为resolevd
                    return; //终止递归
                }
                Promise.resolve(next.value).then( //使用Promise.resove()处理是因为await后面也可以接原始值、thenable对象、Promise对象。
                    value => step(() => h.next(value)) //向next中传入值是为了当await后的异步操作得到值的时候，将值赋予其等式左边的变量。
                ).catch(reason => step(() => h.next(reason))) //在await后面的异步操作reject之后，抛出错误，这将被上面的try catch捕获。
            }
            step(() => h.next())
        })
    }
    ```

- 