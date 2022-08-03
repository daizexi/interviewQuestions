# 1. Proxy

- Proxy的作用

  - Proxy用于修改某些操作的默认行为。（由于相当于重载了某些操作，修改了语言特性，所以也被称为元编程）。
  - Proxy可以理解为在target对象外面架设一层代理，当JS引擎想要对这个对象进行操作的时候，实际上都是被代理劫持了，这个特点很想Object.defineProperty()访问器属性。访问器属性的get方法相当于劫持了目标对象的RHS查询，在JS引擎想要对这个对象进行RHS查询的时候实际上执行了访问器属性的gettter。

- Proxy对象

  - ES6提供了Proxy构造函数，用于提供Proxy实例。

  - Proxy构造函数接受2个参数，第一个是target表示要进行拦截操作的目标对象，第二个是handler，它也是一个对象，用于设置拦截操作。

  - 使用Proxy构造函数生成了Proxy对象之后，可以通过proxy对象来代替target对象，比如需要访问target上的属性，可以直接访问proxy上的相应属性就会触发拦截操作。

    也就是说对proxy对象进行操作就相当于对target对象进行操作，但是会多做一层拦截操作。通过这个拦截操作，我们就可以实现在触发原生操作的时候顺带执行其他操作。
    
    ```{js}
    function add(x,y){
        return x + y;
    }
    
    const proxy = new Proxy(add,{
        construct: function(){
            console.log('constructing')
            return {name: 'foo'}
        }
    })
    
    console.log(new proxy())
    ```
    
    handler对象的construct属性用于设置当将target对象以new操作符调用时的操作。

- Proxy的handler支持设置13种操作

- get

  - get用于设置拦截target对象的某个属性的读取操作。
  - get可以接受3个参数。
    1. 目标对象。
    2. 属姓名。
    3. proxy实例本身。（可选）
  - get操作的return值将作为读取的值返回。

- set

  - set用于拦截某个属性的赋值操作。
  - set可以接受4个参数
    1. 目标对象。
    2. 属性名。实际上当使用Proxy实例访问属性A时，这个参数就是'A'，访问属性B时，这个参数就是'B'。
    3. 属性值。
    4. Proxy实例本身。
  - set应该始终返回一个布尔值。（在严格模式，如果set操作没有返回true，就会报错）。



# 2. Reflect

- Reflect的作用

  - Reflect对象和Proxy对象一样，是ES6为了操作对象而添加的API。

  - Reflect主要有4个目的

    1. 将Object对象的一些明显属于语言内部的方法（如，Object.defineProperty()），放在Reflect对象上，也就是说可以从Reflect对象获取到语言内部的方法。

    2. 修改某些Object方法的返回结果，让其更加合理。如，Object.defineProperty()在严格模式下遇到不可配置的属性的时候会抛出错误，但是使用Reflect.defineProperty()只会返回false。

    3. 将Object的操作都变成声明式。现在一些Object的操作是命令式的，如name in obj、delete obj[name]，而Reflect.deleteProperty(obj,name)等可以声明式的进行这些操作。

    4. 由于Reflect上有Object操作的默认操作，所以当我们使用Proxy修改了默认操作之后，我们想要访问默认操作的话，可以通过Reflect对象。

       ```{js}
       const obj = {
           name: 'obj',
       }
       
       const proxy = new Proxy(obj,{
           get(target,key){
               if(key in target)
               {
                   console.log('getting');
                   return obj[key];
               }
               else{
                   console.log('not found');
               }
           },
           set(target,key,value){
               console.log('setting');
               Reflect.set(target,key,value); //相当于obj.name = value
           }
       })
       ```

       在Proxy中调用Reflect是为了使原生行为可以正常的执行。

- Refect支持的方法

  - Reflect支持13个方法，和Proxy的方法相对应。

- Reflect.get(target,name,receiver)

  - 作用

    - Reflect.get()方法用于查找target对象的name属性，如果target对象没有name属性，那么将返回undefined。

    - 如果name属性是一个访问器属性并定义了getter，那么将把这个getter中的this绑定为receiver。

      ```{js}
          foo: 1,
          bar: 1,
          get sum(){
              return this.foo + this.bar;
          }
      }
      
      const obj1 = {
          foo: 3,
          bar: 3,
      }
      
      console.log(Reflect.get(obj,'sum',obj1)); //6
      ```

      Reflect.get在访问访问器属性sum的时候将sum中的this绑定为receiver了。

- Reflect.set(target, name, value, receiver)

  - 作用

    - Reflect.set(target,name,receiver)用于将target对象的name属性设置为value。

    - 如果target对象上的name属性是一个访问器属性，并设置了setter，那么Reflect.set()将会把setter中的this绑定为receiver。

      ```{js}
      const obj = {
          _foo: 1,
          bar: 1,
          get sum(){
              return this._foo + this.bar;
          },
          set foo(val){
              this._foo = val
          }
      }
      
      const obj1 = {
          _foo: 3,
          bar: 3,
      }
      
      Reflect.set(obj,'foo',0,obj1)
      
      console.log(obj1._foo) //0
      ```

      Reflect.set(target,name,value,receiver)在访问访问器属性的时候将setter函数中的this绑定到了receiver上。

    