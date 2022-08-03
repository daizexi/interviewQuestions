# 1. Vue组件

> # 1.1 组件用法
>
> ```{html}
> <!DOCTYPE html>
> <html>
> <head>
> </head>
> <body>
>  <div id="app">
>    <mycompoent></mycompoent> <!-- 在Vue实例中使用Vue组件必须在创建实例之前注册组件。 -->
>  </div>
> </body>
> <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
> <script>
>  Vue.component('my-component',{
>    //这时组件还是空白的，因为在这里我们没有添加任何内容。
>  }); //对组件进行全局注册
> 
>  const app = new Vue({
>    el: "#app"
>  })
> </script>
> </html>
> ```
>
> - 当我们给组件选项中添加 template 就可以显示组件内容了
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>     <head>
>     </head>
>     <body>
>       <div id="app">
>         <my-component></my-component>
>       </div>
>     </body>
>     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>     <script>
>       Vue.component('my-component',{
>         template: '<div>这里是组件内容</div>'
>       });
>   
>       const app = new Vue({
>         el: '#app'
>       })
>     </script>
>   </html>
>   ```
>
> 
>
> - template的DOM结构必须被一个元素包裹（否则无法渲染），在上面是 <div></div>
>
> - Vue组件在某些情况下会受到HTML的限制，如<table>中规定只允许是<tr>、<td>、<th>等表格元素，所以在<table>中直接使用组件是无效的，在这种情况下，可以使用特殊的is属性来挂载组件。
>
>   - 使用is属性之后，tbody在渲染时，会被替换成组件内容。
>   - 常见的限制元素还有\<ul>、\<ol>、\<select>
>   - ul和ol中只能使用\<li>，如果要把\<li>替换为组件内容，也可以使用is属性。
>   - \<select>只能是\<option>，如果要把\<li>替换为组件内容，可以使用is属性。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>     <head>
>     </head>
>     <body>
>       <div id="app">
>         <table>
>           <tbody is="my-component"></tbody>
>         </table>
>       </div>
>     </body>
>     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>     <script>
>       Vue.component('my-component',{
>         template: '<div>这是组件内容</div>'
>       })
>       const app = new Vue({
>         el: '#app'
>       })
>     </script>
>   </html>
>   ```
>
> 
>
> - 除了使用template属性外，组件中还可以像Vue实例那样使用其他选项，如data、computed、methds等。
>
>   - 在使用data属性的时候，和实例有些区别，data必须是函数，并把数据return出去。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>       <head>
>       </head>
>       <body>
>         <div id="app">
>           <my-component></my-component>
>         </div>
>       </body>
>       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>       <script>
>         Vue.component('my-component',{
>           template: '<div>{{ message }}</div>',
>           data: ()=>{
>             return {
>               message: '这里是组件内容'
>             }
>           }
>         })
>     
>         const app = new Vue({
>           el: '#app'
>         })
>       </script>
>     </html>
>     ```
>
> # 1.2 使用props传递数据
>
> - 通过props可以实现父组件向子组件传递数据。
>
>   - props可以是2种
>
>     - 字符串数组
>
>       ```{html}
>       <!DOCTYPE html>
>       <html>
>         <head>
>         </head>
>         <body>
>           <div id="app">
>             <my-component message="这里放来自父组件的数据，一般可以结合v-bind使用"></my-component>
>           </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>           Vue.component('my-component',{
>             props: ['message'], //表示我们接收一个来自父组件的数据message
>             template: '<div>{{ message }}</div>'
>           })
>       
>           const app = new Vue({
>             el: '#app'
>           })
>         </script>
>       </html>
>       ```
>
> 
>
>     - 对象
>     
>       - 当需要对props中的变量规定类型的时候，就需要使用对象形式的props
>     
>         ```{html}
>         <!DOCTYPE html>
>         <html>
>             <head></head>
>             <body>
>                 <div id="app">
>                     <input type="text" v-model="initCount"/>
>                     <p>{{ initCount }}</p>
>                     <my-component :init-count="initCount"></my-component>
>                 </div>
>             </body>
>             <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>             <script>
>                 Vue.component('my-component',{
>                     template: '<div>{{ count }}</div>',
>                     props: {
>                         initCount: {
>                             type: Number, //规定props变量的类型
>                             default: 0, //规定默认值
>                         }
>                     },
>                     data: function(){
>                         return{
>                             count: this.initCount
>                         }
>                     }
>                 })
>     
>                 const app = new Vue({
>                     el: '#app',
>                     data: {
>                         initCount: 0,
>                     }
>                 })
>             </script>
>         </html>
>         ```
>
> 
>
> 
>
> 
>
> - props中声明的数据与组件data函数return的数据的主要区别是props中的数据来自父组件，而data中的数据是组件本身的数据，作用域是组件本身，这两种数据都可以在template、computed、methods中使用。
>
>   - 另外如果父组件需要传输多个数据到子组件，那么向props字符串数组中添加项就可以。
>
> - 由于HTML特性不区分大小写，当使用DOM模板时，驼峰命名需要转成短横分隔命名。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>     <head>
>     </head>
>     <body>
>       <div id="app">
>         <my-component warning-text="这里放从父组件传过来的warning text"></my-component> //如果这里使用warningText，那么会报错。
>       </div>
>     </body>
>     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>     <script>
>       Vue.component('my-component',{
>         props: ['warningText'],
>         template: '<div>{{ warningText }}</div>'
>       })
>   
>       const app = new Vue({
>         el: '#app'
>       })
>     </script>
>   </html>
>   ```
>
> 
>
> - 有时候props传递的数据不是写死的，而是来自父组件的动态数据，这时我们需要使用v-bind来动态绑定props的值。（作用是当父组件数据的值发生变化的时候，也会传递给子组件）。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>     <head>
>     </head>
>     <body>
>       <div id="app">
>           <input type="text" v-model="text"/> //v-model对表单元素的value数据做了双向绑定
>           <my-component :warning-text='text' v-show="isExist"></my-component>
>           <input type="button" value="add" @click="handleVisble"/>
>       </div>
>     </body>
>     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>     <script>
>       Vue.component('my-component',{
>         props: ['warningText','isExist'],
>         template: '<div>{{ warningText }}</div>'
>       })
>   
>       const app = new Vue({
>         el: '#app',
>         data: {
>             isExist: true,
>             text: 'aaaa'
>         },
>         methods: {
>             handleVisble(){
>                 this.isExist = !this.isExist;
>             }
>         }
>       })
>     </script>
>   </html>
>   ```
>
> 
>
> - 当想要使用props传递数字、布尔值、数组、对象等，并且没有使用v-bind，那么传递的只是字符串。
>
>   ```{html}
>   //这是一个使用props传递数组，但没有使用v-bind使传递的只是字符串的例子
>   <!DOCTYPE html>
>   <html>
>     <head>
>     </head>
>     <body>
>       <div id="app">
>           <my-component :warning-text='[1,2,3]'></my-component>
>           <my-component warning-text='[1,2,3]'></my-component>
>       </div>
>     </body>
>     <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>     <script>
>       Vue.component('my-component',{
>         props: ['warningText'],
>         template: '<div>{{ warningText.length }}</div>'
>       })
>   
>       const app = new Vue({
>         el: '#app'
>       })
>     </script>
>   </html>
>   ```
>
> 
>
> - 在子组件中修改props传入的数据的方法
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head></head>
>       <body>
>           <div id="app">
>               <input type="text" v-model="initCount"/>
>               <p>{{ initCount }}</p>
>               <my-component :init-count="initCount"></my-component>
>           </div>
>       </body>
>       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>       <script>
>           Vue.component('my-component',{
>               template: '<div>{{ count }}</div>', //子组件的count因为是data中的变量，所以并不会跟随表单元素的initCount而改变。（因为只有props被动态绑定了，当父组件中表单元素中的value改变时，父组件的data中的initCount会改变，因为props被v-bind绑定了，所以子组件中的initCount也会动态改变）
>               props: ['initCount'],
>               data: function(){
>                   return{
>                       count: this.initCount
>                   }
>               }
>           })
>   
>           const app = new Vue({
>               el: '#app',
>               data: {
>                   initCount: 0,
>               }
>           })
>       </script>
>   </html>
>   ```
>
> 
>
> - 父子组件通信
>
>   - 使用v-on与$emit()结合自定义事件，从子组件向父组件传递数据。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head></head>
>       <body>
>           <div id="app">
>               <p>父组件中的计数器: {{ outerCount }}</p>
>               <my-component
>                   @increase="handleGetTotal"
>                   @decrease="handleGetTotal"
>               ></my-component>
>           </div>
>       </body>
>       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>       <script>
>           Vue.component('my-component',{
>               template: '\
>                   <div>\
>                       <input type="button" value="add" @click="handleIncrease"/>\
>                       <input type="button" value="decrease" @click="handleDecrease"/>\
>                   </div>',
>               data: ()=> {
>                   return{
>                       count: 0
>                   }
>               },
>               methods: {
>                   handleIncrease(){
>                       this.count++;
>                       this.$emit('increase',this.count);
>                   },
>                   handleDecrease(){
>                       this.count--;
>                       this.$emit('decrease',this.count);
>                   }
>               }
>           })
>   
>           const app = new Vue({
>               el: '#app',
>               data: {
>                   outerCount: 0,
>               },
>               methods: {
>                   handleGetTotal(count){
>                       this.outerCount = count
>                   }
>               }
>           })
>       </script>
>   </html>
>   ```
>
>   - 使用ref来实现父子组件间通信
>     - 父组件通过this.$refs来访问指定的子组件。
>     - this.$refs在组件渲染完成后才填充
>     - this.$refs不是响应式的
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head></head>
>       <body>
>           <div id="app">
>               <p>{{ message }}</p>
>               <button @click="handleGetMessage">通过ref获取子组件message</button>
>               <my-component ref="comA"></my-component>
>           </div>
>       </body>
>       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>       <script>
>           Vue.component('my-component',{
>               template: '\
>                   <div>\
>                       <input type="text" v-model="message"/>\
>                   </div>',
>               data: ()=> {
>                   return{
>                       message: 'initMessage',
>                   }
>               }
>           })
>   
>           const app = new Vue({
>               el: '#app',
>               data: {
>                   message: '',
>               },
>               methods: {
>                   handleGetMessage(){
>                       this.message = this.$refs.comA.message; //在父组件中可以通过this.$refs来访问指定子组件/子实例
>                   }
>               }
>           })
>       </script>
>   </html>
>   ```
>
> 
>
> - Vue2使用发布订阅模式来实现非父子组件间通信，主要是使用一个空的Vue实例 bus 作为事件主线（bus），非父子组件间通过这个bus通信。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head></head>
>       <body>
>           <div id="app">
>               <p>{{ count }}</p>
>               <my-component></my-component>
>           </div>
>       </body>
>       <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>       <script>
>           const bus = new Vue({}); //消息中心
>   
>           Vue.component('my-component',{ //my-component组件相当于发布者
>               template: '\
>                   <div>\
>                       <input type="button" value="add" @click="handleEventAdd"/>\
>                       <input type="button" value="decrease" @click="handleEventDecrease"/>\
>                   </div>',
>               data: ()=> {
>                   return{
>                       count: 0,
>                   }
>               },
>               methods: {
>                   handleEventAdd(){
>                       this.count++;
>                       bus.$emit('message',this.count);
>                   },
>                   handleEventDecrease(){
>                       this.count--;
>                       bus.$emit('message',this.count);
>                   }
>               }
>           })
>   
>           const app = new Vue({ //在这个例子中，这个实例相当于订阅者
>               el: '#app',
>               data: {
>                   count: 0,
>               },
>               mounted(){
>                   const _this = this;
>                   bus.$on('message',(count) => { //箭头函数的this引用的是上一级作用域的this，也就是bus.$on()的this，而它是一个普通函数，普通函数的this取决于调用它的对象，也就是app这个实例，而如果在这里回调函数使用普通函数，就需要上面保存的_this。
>                       this.count = count
>                   }
>                   )
>               }
>           })
>       </script>
>   </html>
>   ```
>
> 
>
> - 作用域
>
>   - 父组件模板中的内容是在父组件作用域中编译
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <my-component v-show="isVisble"></my-component> //这里的isVisble实际上引用的是父组件中的isVisble
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('my-component',{
>                 template: '<div>子组件</div>',
>                 data: () => {
>                     return{
>                         isVisble: false,
>                     }
>                 }
>             })
>     
>             const app = new Vue({
>                 el: '#app',
>                 data: {
>                     isVisble: true,
>                 }
>             })
>         </script>
>     </html>
>     ```
>
> 
>
>   - 如果想要在上面的例子中使用子组件中的数据 isVisble，那么必须在子组件的作用域内编译。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <my-component></my-component>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('my-component',{
>                 template: '<div v-show="isVisble">子组件</div>', //这样isVisble就引用的是子组件的isVsible，因为这时使用isVisble时是在子组件作用域。
>                 data: () => {
>                     return{
>                         isVisble: true,
>                     }
>                 }
>             })
>     
>             const app = new Vue({
>                 el: '#app',
>                 data: {
>                     isVisble: false,
>                 }
>             })
>         </script>
>     </html>
>     ```
>
> 
>
> - 使用slot
>
>   - 单个slot使用
>
>     - 在子组件中使用特殊的\<slot>元素就可以为子组件开启一个slot，这样就可以在父组件模板中在子组件内部插入内容替代子组件中\<slot>元素内的内容。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <my-component>
>                     <p>{{ message }}</p>
>                 </my-component>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('my-component',{
>                 template: '\
>                     <div>\
>                         <slot>\ //注意这里必须加入<slot>元素，才可以在父组件模板中在子组件内部插入内容，如果子组件中不包含<slot>元素，那么在父组件模板中插入子组件标签内部的内容是不会渲染的\
>                             <p>如果父组件模板中没有在子组件标签内部插入内容，这里的内容将会作为默认内容被渲染</p>\
>                         </slot>\
>                     </div>',
>                 data: () => {
>                     return {
>                         message: '子组件中的message',
>                     }
>                 }
>             })
>     
>             const app = new Vue({
>                 el: '#app',
>                 data: {
>                     message: '父组件中的message',
>                 }
>             })
>         </script>
>     </html>
>     ```
>
> 
>
>   - 具名slot（解决当有多个slot时，不知道选取哪个slot的问题）
>
>     - 在子组件的\<slot>元素指定一个name可以分发多个内容，同时具名slot可以与不具名slot共存。
>
>     - 在父组件模板中插入子组件标签内部的内容，如果没有加入 slot="name"，那么会全部默认插入不具名slot
>
>       ```{html}
>       <!DOCTYPE html>
>       <html>
>           <head></head>
>           <body>
>               <div id="app">
>                   <my-component>
>                       <nav slot="header">父组件的header内容</nav> //插入slot name对应header的slot
>                       <p>父组件的container内容1</p> //会插入不具名slot
>                       <p>父组件中的container内容2</p> //会插入不具名slot
>                       <p slot="footer">父组件中的footer内容</p>
>                   </my-component>
>               </div>
>           </body>
>           <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>           <script>
>               Vue.component('my-component',{
>                   template: '\
>                       <div>\
>                           <slot name="header">\
>                               <nav>这里是nav部分的默认内容</nav>\
>                           </slot>\
>                           <slot>\
>                               <p>这里是container的默认内容</p>\
>                           </slot>\
>                           <slot name="footer">\
>                               <p>这里是footer的默认内容</p>\
>                           </slot>\
>                       </div>',
>               })
>       
>               const app = new Vue({
>                   el: '#app',
>               })
>           </script>
>       </html>
>       ```
>
> 
>
>   - 作用域插槽（作用域插槽可以是不具名的slot，也可以是具名的slot）
>
>     - 作用域插槽是一种特殊的slot，使用一个可以复用的模板替换已渲染元素。
>
>     - 在子组件模板中的\<slot>元素上有一个类似props传递数据给组件的写法。
>
>     - 父组件模板中在子组件标签内部使用了\<template>，并且\<template>上有1个scope="props"的特性。在这个template中，可以使用临时变量props来访问子组件模板中的类似props="msg"的这个数据。
>
>       ```{html}
>       <!DOCTYPE html>
>       <html>
>           <head></head>
>           <body>
>               <div id="app">
>                   <my-component>
>                       <template scope="props">
>                           <p>父组件msg</p>
>                           <p>{{ props.msg }}</p>
>                       </template>
>                   </my-component>
>               </div>
>           </body>
>           <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>           <script>
>               Vue.component('my-component',{ //上面的组件标签名应该和这里注册组件时的组件名一致，否则会报错。
>                   template: '\
>                       <div>\
>                           <slot msg="子组件msg"></slot>\
>                       </div>'
>               })
>
>               const app = new Vue({
>                   el: '#app',
>               })
>           </script>
>       </html>
>       ```
>
>     - 作用域插槽主要用于列表组件的情况。（作用域插槽的使用场景就是既可以复用子组件的\<slot>，又可以使slot内容不一致）
>
>       ```{html}
>       <!DOCTYPE html>
>       <html>
>           <head></head>
>           <body>
>               <div id="app">
>                   <my-component :books="books">
>                       <template scope="props" slot="bookList">
>                           <li>{{ props.book }}</li>
>                       </template>
>                   </my-component>
>               </div>
>           </body>
>           <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>           <script>
>               Vue.component('my-component',{
>                   template: '\
>                       <div>\
>                           <ul>\
>                               <slot name="bookList" v-for="book in books" :book="book.name"></slot>\
>                           </ul>\
>                       </div>',
>                   props: {
>                       books: {
>                           type: Object,
>                           default: {},
>                       }
>                   }
>               })
>       
>               const app = new Vue({
>                   el: '#app',
>                   data: {
>                       books: [
>                           {name: 'booka'},
>                           {name: 'bookb'},
>                           {name: 'bookc'},
>                       ],
>                   }
>               })
>           </script>
>       </html>
>       ```
>
> 
>
> - 组件高级用法
>
>   - 递归调用组件（必须设置递归终止条件，通过v-if设置，可以通过props传递计数器）
>
>     - 递归组件一般用于组成级联菜单、树形菜单等组件
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <my-component :count="1"></my-component>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('my-component',{
>                 name: 'my-component', //name和上面用于注册组件的组件名一致
>                 template: '\
>                     <div>\
>                         <my-component :count="count + 1" v-if="count < 3"></my-component>\ //这里的v-if用来设置递归终止条件，props用来传递计数器，当递归调用了3次组件后，终止递归。
>                     </div>',
>                 props: {
>                     count: {
>                         type: Number,
>                         default: 1,
>                     }
>                 },
>             })
>     
>             const app = new Vue({
>                 el: '#app'
>             })
>         </script>
>     </html>
>     ```
>
> 
>
>   - 内联模板
>
>     - 组件的模板一般写在template选项内，但是内联模板支持把模板写在外面。
>
>     - 在使用组件时，在组件标签上使用inline-template特性，这样组件就会把这部分（组件标签的内容）当成模板。
>
>       ```{html}
>       <!DOCTYPE html>
>       <html>
>           <head></head>
>           <body>
>               <div id="app">
>                   <my-component :msg="message" inline-template>
>                       <div>
>                           {{ msg }}
>                       </div>
>                   </my-component>
>               </div>
>           </body>
>           <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>           <script>
>               Vue.component('my-component',{
>                   props: {
>                       msg: {
>                           type: String,
>                           defalt: 'defalt message',
>                       }
>                   },
>                   name: 'my-component'
>               })
>       
>               const app = new Vue({
>                   el: '#app',
>                   data: {
>                       message: '父组件的msg',
>                   }
>               })
>           </script>
>       </html>
>       ```
>



# 2. Vue自定义指令

> - 基本用法
>
>   - 自定义指令的注册方法和组件很像，分为2种
>
>     - 全局注册，注册一个v-focus指令。
>
>       ```{javascript}
>       Vue.directiv('focus',{
>       	//指令选项
>       })
>       ```
>
>       
>
>     - 局部注册，注册一个v-focus指令。
>    
>       ```{javascript}
>       const app = new Vue({
>         el: '#app',
>         directives: {
>           focus: {
>             //指令选项
>           }
>         }
>       })
>       ```
>
>       
>
>     - 
>
> - 

# 3. Virtual DOM

> - 介绍
>   - virtual DOM实际上是一个JavaScript对象，它相比于DOM运算会小很多。
>     - 当状态发生变化的时候，前后的virtual DOM会进行diff运算，这样可以只更新部分改变了的DOM，而不是全部重绘。
> - 作用
>   - Vue2使用了virtual DOM来更新DOM，这样减小le开销。
>   - 虽然我们一般将组件模板写在template中，但是当Vue编译的时候，都会解析为virtual DOM。
> - VNode
>   - virtual DOM是使用VNode描述的，可以理解成VNode就是virtual DOM。
> - VNode的创建
>   - VNode要映射到真实的DOM需要经过一系列create、diff、patch等过程。
>   - VNode的创建是通过createElement方法完成的。

# 4. Render函数

> - virtual dom
>
>   - 在状态发生变化时，Vue会创建virtual dom，virtual dom实际上是一个javasc对象（这是为了只更新发生变化的部分，而不用全部重绘）
>   - 对于正常的DOM节点
>
>   ```{html}
>   <div id="main">
>     <p>文本</p>
>   </div>
>   ```
>
>   对应的JavaScript对象（也就是virtual dom会是这样）
>
>   ```{javascript}
>   var vNode = {
>     tag: 'div',
>     attributes: {
>       id: 'main',
>     },
>     children: [
>       //子节点（p标签）
>     ],
>   }
>   ```
>
>   - vNode对象通过一些特定选项描述了真实的DOM结构。
>
> - 为什么要使用render函数
>
>   - 有时候template不如render函数简洁，尤其当有大段逻辑重复的HTML的时候
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <anchor :level="2" :title="特性"></anchor> <!-- 点击这个组件会跳转到页面中对应锚点位置 -->
>                 <script type="text/x-template" id="anchor"> <!-- x-template写法，这样可以把组件的template部分拿出来写 -->
>                     <div>
>                         <h1 v-if="level === 1">
>                             <a :href="'#' + title"> <!-- 这里使用的是锚点链接 -->
>                                 <slot></slot>
>                             </a>
>                         </h1>
>                     </div>
>                     <div>
>                         <h2 v-if="level === 2">
>                             <a :href="'#' + title">
>                                 <slot></slot>
>                             </a>
>                         </h2>
>                     </div>
>                     <div>
>                         <h3 v-if="level === 3">
>                             <a :href="'#' + title">
>                                 <slot></slot>
>                             </a>
>                         </h3>
>                     </div>
>                 </script>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('amchor',{
>                 template: '#anchor',
>                 props: {
>                     level: {
>                         type: number,
>                         required: true,
>                     },
>                 }
>             })
>     
>             const app = new Vue({
>                 el: '#app',
>             })
>         </script>
>     </html>
>     ```
>
>   - render函数与template的对比（使用template和render写的同一个组件）
>
>     - 在这个例子中，template明显比render易读，所以绝大多数情况我们还是选择template，只有少数情况使用render。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <ele></ele>
>                 <script type="text/x-template" id="ele">
>                     <div id="element" :class="{show: isVisible}" @click="handleClick">
>                         文本内容
>                     </div>
>                 </script>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('ele',{
>                 template: '#ele',
>                 data: () => {
>                     return{
>                         isVisible: true,
>                     }
>                 },
>                 methods: {
>                     handleClick(){
>                         console.log('click');
>                     },
>                 },
>             })
>     
>             const app = new Vue({
>                 el: '#app',
>             })
>         </script>
>     </html>
>     
>     <!DOCTYPE html>
>     <html>
>         <head></head>
>         <body>
>             <div id="app">
>                 <ele></ele>
>             </div>
>         </body>
>         <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
>         <script>
>             Vue.component('ele',{
>                 render: function(createElement){
>                     return createElement(
>                         'div',
>                         {
>                             class: { //作用类似于v-bind:class的API
>                                 show: isVisible,
>                             },
>                             attrs: { //HTML的属性
>                                 id: 'element',
>                             },
>                             on: { //绑定事件
>                                 click: this.handleClick
>                             },
>                         },
>                         '文本内容' //第3个参数实际上是子节点（也包括标签内的内容）
>                     )
>                 },
>                 data: () => {
>                     return {
>                         isVisible: true,
>                     }
>                 },
>                 methods: {
>                     handleClick(){
>                         console.log('click')
>                     },
>                 },
>             })
>     
>             const app = new Vue({
>                 el: '#app'
>             })
>         </script>
>     </html>
>     ```