# 1. Vue实现对象响应式原理

- 响应式的特点

  - 响应式的特点是在数据发生改变的时候，会通知到视图、watch、计算属性等使用了响应式数据的地方。
  - 通知这个词语十分抽象，我们可以理解为当数据发生变化之后就执行某个函数。

- Object.defineProperty()定义访问器属性

  - 我们知道Vue2中data选项需要的是一个由函数返回的对象，每个响应式数据都是这个对象中的一个属性，所以我们想到也许可以使用Object.defineProperty()将每个属性定义为一个访问器属性。

    ```{js}
    function defineReactive(obj)
    {
      Object.keys().forEach((item) => {
        Object.defineProperty(obj,item,{
          enumerable: true,
          configurable: true,
          get: function(){
            return obj[item]
          },
          set: function(val){
            obj[item] = val
          }
        })
      })
    }
    ```

    现在我们已经将Vue2 data属性返回的对象中的每一个属性定义为了访问器属性。当读取响应式数据时，这个读取操作将被getter接管，当设置响应式数据时，这个设置操作将被setter接管（也就是数据发生修改时必然会运行setter）。

    但是我们还缺乏当对响应式数据进行设置修改时通知使用了这个数据的地方的能力。

- 加入通知依赖数据的地方的能力

  - 观察者模式（对于单个属性是观察者模式，但多个属性就必须借助数据中心将不同订阅者储存的回调分隔开）

    ```{js}
    function defineReactive(obj){
      let dep = {}
      Object.keys().forEach((item) => {
        Object.defineProperty(obj,item,{
          enumerable: true,
          configurable: true,
          get: function(){
           if(window.target)
          	{
              dep[item].push(window.target)
            }
            return obj[item]
          },
          set: function(val){
            for(const callBack of dep[item])
              {
                callBack(val)
              }
            obj[item] = val
          }
        })
      })
    }
    ```

    window.target是为了便于使用而储存的全局的可以用来通知使用了响应式数据的地方的函数。

    现在我们已经利用访问器属性实现在修改响应式数据的时候执行通知依赖了这个数据的地方的函数的能力。

    现在我们只需要实现这个通知函数，我们就实现了一个简单的响应式。

- 如何实现通知函数

  - 以监听器的通知函数为例

    - 如果一个数据被很多个地方使用，这些地方很有可能有模板，有监听器，对于这种情况我们需要抽象出一个适用于不同地方的Notify类。

      ```{js}
      class Notify{
        constructor(data,expOrFn,cb){
          this.data = data
          this.getter = parsePath(expOrFn)
          this.cb = cb
          this.value = this.get() //get会读取出data.a的值并返回
        }
        
        get(){
          window.target = this
          let value = this.getter.call(this.data,this.data) //触发data.a的getter，将window.target中的Notify的实例储存进dep
          window.target = undefined
          return value
        }
        
        update(){
          const oldValue = this.value
          this.value = this.get()
          this.cb.call(this.data,this.value,oldValue)// 触发监听器的回调函数
        }
      }
      
      const bailRE = /[^\w.$]/
      function parsePath(path){
        if(bailRE.test(path))
          {
            return
          }
        const segments = path.split('.')
        return function(obj){
          for(let i = 0;i < segments.length;i++)
            {
              if(!obj)
                {
                  return
                }
              obj = obj[segments[i]]
            }
          return obj
        }
      }
      ```

      Notify类的constructor函数接受3个参数（假设我们要通知依赖数据data.a的地方），data是指响应式对象，expOrFn是'.a'这个属性查询路径，cb是指watch传入的回调函数。

      parsePath函数将返回一个读取data.a的值的函数，由于我们已经为data.a设置了getter，因此这个时候必然会触发getter，也就会将储存在window.target中的Notify类的实例传入dep中储存。

- 整合起来的写法

  ```{js}
  function defineReactive(obj)
  {
      let dep = {}
      Object.keys(obj).forEach((item) => {
          Object.defineProperty(obj,item,{
              enumerable: true,
              configurable: true,
              get: function(){
                  if(window.target)
                  {
                      if(dep[item])
                      {
                          dep[item].push(window.target)
                      }
                      else{
                          dep[item] = [window.target]
                      }
                  }
                  return obj[item]
              },
              set: function(val){
                  for(const callback of dep[item])
                  {
                      callback.update()
                  }
                  obj[item] = val
              }
          })
      })
  }
  
  class Notify{
      constructor(data,expOrFn,cb){
          this.data = data
          this.getter = parsePath(expOrFn)
          this.cb = cb
          this.value = this.get()
      }
  
      get(){
          window.target = this
          let value = this.getter.call(this.data,this.data)
          window.target = undefined
          return value
      }
  
      update(){
          const oldValue = this.value
          this.value = this.get()
          this.cb.call(this.data,this.value,oldValue)
      }
  }
  
  function parsePath(path)
  {
      const segments = path.split('.')
      return function(obj){
          for(let i = 0;i < segments.length;i++)
          {
              obj = obj[segments[i]]
          }
          return obj
      }
  }
  ```

  