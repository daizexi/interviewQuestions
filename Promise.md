# 1. Promise.prototype.finally

- finally()方法的作用

  - finally()用于指定不管Promise对象最后状态如何，都会执行的操作。

- Promise.prototype.finally()的实现

  ```{js}
  Promise.prototype.finally = function(fn){
    const P = this.constructor
    return this.then(
      value => P.resolve(fn()).then(() => value),
      reason => P.resolve(fn()).then(() => {throw reason})
    )
  }
  ```

  我们可以看到Promise.prototype.finally()方法返回的还是一个Promise对象，并且在执行一定执行的代码之后，finally返回的Promise实例的value/reason还是上一个Promise实例传递过来的value/reason。



# 2. Promise.resolve()

- Promise.resolve()的作用

  - Promise.resolve()用于将一个对象转换为Promise对象。
  - Promise.resolve(obj) 等价于 new Promise(resolve => resolve(obj))

- Promise.resolve()方法会有4种情况

  1. 参数是一个Promise实例

     - 那么Promise.resolve()不做任何操作，直接返回这个Promise实例。

  2. 参数是一个thenable对象

     - thenable对象是指有then方法的对象

       ```{js}
       const obj = {
         then: function(resolve,reject) {
           resolve(111)
         }
       }
       ```

       obj就是一个定义了then方法的对象。

     - Promise.resolve(obj)会首先将obj转为Promise对象，并立即调用obj中定义的then方法，最终会获得调用then方法之后返回的Promise实例。

  3. 参数不是具有then()方法的对象，或只是原始值

     - 那么Promise.resolve()将返回一个立即resolved的Promise对象，并且这个Promise对象的实例属性value是传入的对象或原始值。

  4. 不带有任何参数

     - Promise.resolve()将直接返回一个resolved的Promise实例，但是它的value和reason都是undefined。

# 3. Promise.all()

- Promise.all()的作用

  - Promise.all()用于将多个Promise实例包装成一个Promise实例。
  - Promise.all()接受一个可迭代对象（有Symbol.iterator属性的对象）作为参数，可迭代对象的元素都是Promise实例，否则会调用Promise.resolve()转换为Promise实例。

- Promise.all()返回的Promise实例的状态

  - Promise.all()返回的Promise实例的状态分成2种情况
    1. 只有参数数组中的所有元素都变成Fulfilled，Promise.all()返回的Promise实例才会变成Fulfilled。
    2. 只要有一个元素的状态变成Rejected，那么Promise.all()返回的Promise实例就会变成Rejected。

- 一些特殊情况

  - 当作为Promise.all()参数元素的Promise实例定义了自己的catch方法，那么一旦它被Rejected，也并不会触发Promise.all()定义的catch方法。

    ```{js}
    const p1 = new Promise((resolve, reject) => {
      resolve('hello');
    })
    .then(result => result)
    .catch(e => e);
    
    const p2 = new Promise((resolve, reject) => {
      throw new Error('报错了');
    })
    .then(result => result)
    .catch(e => e);
    
    Promise.all([p1, p2])
    .then(result => console.log(result))
    .catch(e => console.log(e));
    // ["hello", Error: 报错了]
    ```

    p1会resolved，p2会rejected，但由于p2后面使用了catch处理了这个异常，最终p2将指向catch返回的Promise实例，并且这个Promise实例的value是报错原因且状态是Fulfilled。

    在分析Promise的执行的时候要记得Promise相关方法返回的都是Promise实例。



# 4. Promise.race()

- Promise.race()的作用

  - Promise.race()用于将多个Promise实例包装成一个新的Promise实例。
  - 可迭代对象的元素都是Promise实例，否则会调用Promise.resolve()转换为Promise实例。
  - 只要有一个参数元素改变状态，那么Promise.race()返回的Promise实例就会立即跟着改变。

- Promise.race()的应用

  - Promise.race()可以用于为Ajax请求设置timeout。

    ```{js}
    const p = Promise.race([
      fetch('/resource-that-may-take-a-while'),
      new Promise(function (resolve, reject) {
        setTimeout(() => reject(new Error('request timeout')), 5000)
      })
    ]);
    
    p
    .then(console.log)
    .catch(console.error);
    ```

    