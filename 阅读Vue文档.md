# 1.为什么不要在Vue组件属性或回调中使用箭头函数

- 问题

  - 实际上这个问题应该是不要在Vue组件属性或回调中使用箭头函数并在箭头函数中使用this。

- 一个例子

  ```{vue}
  Vue.createApp({
  	data(){
  		return {
  			count: 1,
  		}
  	},
  	created() {
  		console.log(this.count);
  	}
  })
  ```

  上面给出了一个不使用箭头函数的版本，毫无疑问，created在调用的时候this将指向这个Vue组件，因此this.count访问将没有问题。

  ```{vue}
  Vue.createApp({
  	data(){
  		return {
  			count: 1,
  		}
  	},
  	created: () => {
  		console.log(this.count);
  	}
  })
  ```

  但是如果把created改成是箭头函数，由于箭头函数在使用this的时候会去引用它的上层词法作用域中的this，而不是发生隐式绑定，因此这时this将不指向这个Vue组件。

# 2. Vue计算属性和Methods的区别

- 计算属性
  - 计算属性的getter函数将只有在其中使用的响应式数据改变时，才会重新计算。
  - 如果一个计算属性没有使用响应式数据，那么将永远不会重新计算。
- Methods
  - 当在模板中使用Methods时，每当组件重新渲染（每次经过生命周期updated，而当响应式数据改变时，就会调用updated），都将重新执行一次。

# 3.Vue计算属性和watch监听器的区别

- 作用
  - watch的作用实际上和computed类似，都是用于对响应式数据进行复杂计算。
- watch
  - watch用于监听某个数据的变化，当监听数据发生变化时，就会执行给定的回调。
  - 只有在某个数据发生变化时需要进行异步操作才使用watch。
- 计算属性
  - 当getter中某个响应式数据发生变化，都将触发重新计算。

# 4.Vue2不能检测到哪些情况下的数组变化

- 原因
  - Vue2的数据响应式是基于Object.defineProperty()的getter、setter实现的。
- 通过索引直接设置项
  - this.books[3] = xxx
  - 解决方案
    - this.$set(this.books, 3, xxx); //
- 修改数组长度
  - this.books.length = 1
- 在对象上新加一个属性
  - obj.key = xxx
- 在对象上delete一个属性
  - delete obj.key   



# 5.Vue v-指令

1. v-once

   - 绑定v-once的结点，只会进行一次插值，当响应式数据更新时，插值处的内容不会更新。

     ```{vue}
     <span v-once>{{ Msg }}</span>
     ```

     当Msg数据改变时，视图也不会相应更新。

2. v-bind

   - v-bind用于在HTML属性进行数据插值。（也就是在HTML属性使用指令表达式动态绑定）

3. v-html

   - v-html用于渲染html

     ```{vue}
     <span v-html="rawHtml"></span>
     ```

     这个span的内容将被替换为rawHtml，并且将直接作为HTML加入渲染。

4. v-if

   - v-if用于条件渲染

     ```{vue}
     <span v-if="isShow"></span>
     ```

     v-if是惰性的，只有在isShow第一次为真的时候渲染元素、监听器及相应子组件/子元素。

     当v-if为绑定的指令表达式为false，那么将销毁结点/组件、监听器及相应子组件/子元素。

   - v-if结合template进行分组条件渲染

     ```{vue}
     <template v-if="isShow">
       <span></span>
       <p></p>
       <div></div>
     </template>
     ```

     template将作为一个不可见的元素，当isShow为真时，渲染出来的结点也不包含template。

5. v-show

   - v-show的表现上类似于条件渲染

     ```{vue}
     <span v-show="isShow"></span>
     ```

     不论v-show绑定的指令表达式取值为真还是为假，这些结点/组件、监听器及相应子元素/子组件都会被渲染，只是会通过display: none来设置是否可见。

6. v-for

   - v-for用于进行列表渲染

     ```{vue}
     <ul>
       <li v-for="(item,index) in items" :key="item.id"></li>
     </ul>
     ```

     上面是使用v-for基于数组渲染列表。item是数组中的元素的别名

     ```{vue}
     <ul>
       <li v-for="(value,key,index) in obj" :key="value.id"></li>
     </ul>
     ```

     上面是使用v-for基于对象渲染列表，value是对象属性的值，key是对象的属性的键，index是对象的第几个属性。

     这其实和基于数组的渲染有所对应，value相当于数组中的item，key相当于数组中的index（因为数组的key就是index的字符串）。

   - v-for必须和:key一起使用

   - v-for不要和v-if绑定在同一个结点

7. v-on

   - v-on用于为结点添加事件监听器。

     ```{vue}
     <span @click="doSomething"></span>
     ```

     span元素被绑定了一个doSomething的事件监听器。

   - 在v-on绑定的方法中访问原生DOM事件

     ```{vue}
     <input type="text" :value="something" @input="something=$event.target.value"/>
     ```

     可以使用$event来访问原生DOM事件。

6. 

# 6.为什么永远不要在一个结点上同时使用v-if和v-for

- 为什么不要

  - 因为当Vue处理指令的时候，v-if比v-for的优先级更高，v-if将会被首先执行，而在v-if指令执行的时候迭代的变量往往还不存在，所以会抛出错误。

- 涉及场景

  1. 当我们想要对列表中的某个项进行过滤的时候。
  2. 为了避免渲染应该隐藏的整个列表。

- 对列表中某个项进行过滤

  - 一个例子

    ```{vue}
    <ul>
      <li v-for="item in items" v-if="item.isShow" :key="item.id"></li>
    </ul>
    ```

    在v-if执行的时候，因为v-for还没有执行，因此迭代的变量别名item就还不存在，因此会抛出错误。

  - 解决方法

    1. 可以通过使用一个计算属性（计算属性本来就是对响应式数据进行复杂逻辑处理的）。

       ```{vue}
       <ul>
         <li v-for="item in visiableItem" :key="item.id"></li>
       </ul>
       
       export defineComponents({
       	setup(){
       		const visiableItem = computed(() => {
       			return items.value.filter((item) => item.isShow)
       		})
       		return {
       			visiableItem,
       		}
       	}
       })
       ```

       相当于在计算属性中对列表做了筛选，这样避免了v-for和v-if一起使用。

    2. 也可以使用template来包裹v-if

       ```{vue}
       <ul>
         <template v-for="item in items" :key="item.id">
       		<li v-if="item.isShow"></li>
         </template>
       </ul>
       ```

       这样的思路是使用一个不可见的template避免了v-for和v-if在同一个元素上使用。

- 控制整个列表的可见性

  - 一个例子

    ```{vue}
    <ul>
      <li v-for="item in items" v-if="isShow"></li>
    </ul>
    ```

  - 解决方法

    1. 将v-if放到v-for绑定的元素的父元素

       ```{vue}
       <ul v-if="isShow">
         <li v-for="item in items"></li>
       </ul>
       ```

       

# 7. 为什么要在v-for的绑定元素上绑定一个唯一的数据key

- v-for

  - v-for一般用于进行列表渲染，这个列表可能数据量很大，而每一个列表项都是一个li元素，如果列表渲染的数据一旦改变就对整个列表重新渲染，那么开销会很大。

- 冲突

  1. 我们想要每当列表数据发生改变的时候，视图上可以正确的更新。
  2. 我们也想在视图正确更新的基础上，尽量的减少对DOM的操作，也就是说我们并不想每当列表数据一改变，就重新渲染整个列表。

- v-for的默认策略

  - v-for默认使用就地复用策略。
    - 就地复用策略是当列表数据发生更改时，Vue会根据key值去判断某个列表项是否发生改变，如果发生改变那么就重新渲染这一项，否则就复用之前的。（这样可以正常工作的前提是，新数据绑定的一个新的key，这样Vue发现出现了一个新的key，就会知道需要渲染一个新数据）
    - Vue默认使用v-for中的index作为就地复用时的key。

- 默认key不起效的情况

  - 假设我们有一个列表渲染数组，items。我们在向items中添加元素的时候不使用push，而是使用unshift，也就是说新的元素将使用旧的index。

    ```{vue}
    <ul>
      <li v-for="item in items"></li>
    </ul>
    <button @click="addItem">add</button>
    
    export defineComponent({
    	setup(){
    		const items = ref(xxx)
    		const addItem = () => {
    				items.value.unshift(xxx)
    		}
    		return {
    			items,
    			addItem,
    		}
    	}
    })
    ```

    如果Vue在就地复用的时候使用index来作为跟踪数据的标准，那么新的数据因为它的index使用的旧的index，而不会被重新渲染。

# 8. 事件修饰符

- 为什么要有事件修饰符

  - 很多时候我们会在事件监听器的回调中使用event.preventDefault()（阻止原生DOM事件），event.stopPropagation()（停止冒泡，但是无法阻止绑定在本结点上的其他事件监听器），event.stopImmediatePropagation()（立刻停止冒泡，阻止绑定在本结点上的后续相同类型事件监听器）。

- 事件修饰符

  1. .stop，等同于event.stopProagation()。

  2. .prevent，等同于event.preventDefault()。

  3. .capture，等同于Element.addEventListener('xxx',function(){},true);

  4. .self，只有当event.target是绑定了事件监听器的元素本身时，才触发事件处理函数。

  5. .passive，等同于Element.addEventListener('xxx',function(){},{passive: true})（passive选项的作用是，当passive选项被设置为true，那么即使回调函数中有event.preventDefault()也不会运行）

     为什么要有passive选项，在一些页面滚动场景，由于浏览器并不知道回调函数中有没有event.preventDefault()，因此就只能等待回调函数执行完毕再响应滚动，但是如果回调函数执行时间稍长，那么就很可能造成难以忍受的延迟，因此加入passive选项是为了告诉浏览器回调函数中是否存在event.preventDefault()。

  6. .once，等同于Element.addEventListener('xxx',function(){},{once: true})（once表示在这个事件监听器注册之后最多调用一次，在调用之后就会被移除）。

  7. .native，这个修饰符用于在父组件模板中为子组件绑定原生事件，如果不使用.native就直接在组件上使用v-on绑定事件，那么事件将不会触发。（.native不能绑定在HTML元素上）

- 事件修饰符的顺序

  1. @click.prevent.self，将阻止当前元素及所有子元素的默认事件。
  2. @click.self.prevent，将阻止当前元素的默认事件。

# 9. Provide / Inject

- Provide / Inject的作用

  - Provide / Inject相当于长距离的props，Provide可以用于在祖先组件中传递数据到后代组件（Inject）
  - provide / inject克服了props只能从父组件向子组件传数据的不足，provide / inject允许跨越式的传递数据。

- 一个例子

  - 祖先组件

    ```{vue}
    export defineComponent({
    	provide(){
    		return {
    			todoLength: len
    		}
    	},
    	setup(){
    		const todos = ref(['Feed a cat', 'Buy tickets'])
    		const len = computed(() => {
    			return todos.value.length
    		})
    		return {
    			len
    		}
    	}
    })
    ```

    如果想要在Provide中传入一些组件实例的property，那么必须将Provide转换成函数返回的对象（和data一样）。

  - 后代组件

    ```{vue}
    export defineComponent({
    	inject: ['todoLength'],
    })
    ```

    在后代组件中inject有点类似于props的书写方式。

- provide / inject的响应式

  - 一般provide / inject传递的数据并不是响应式的，除非传入ref、reactive、计算属性等。

# 10. v-model

- v-model的作用

  - v-model用于提供双向绑定
    - 在表单元素上表现为表单元素的值发生变化之后，响应式数据也会变化；在响应式数据发生变化时，表单上的值也会变化。
    - 在组件上表现为，在父组件中的使用v-model双向绑定到子组件的数据，当这个数据变化时，通过props传入子组件的数据也会变化；当我们在子组件中触发input事件（默认的绑定事件），父组件中的值也会变化。

- 在表单元素上使用v-model

  - v-model将忽略表单元素属性的初始值

    - 当在表单元素input、select、textarea上使用v-model时，这些元素的value属性设置的初始值将会被忽略。

  - v-model在不同的表单元素绑定不同的属性和事件

    - 对于\<input type='text'>和\<textarea>，将绑定value属性和input事件。

      - 一个例子

        ```{vue}
        <input v-model="bindValue" type='text'></input>
        ```

        等同于

        ```{vue}
        <input :value="bindValue" @input="bindValue=$event.target.value" type='text'>
        ```

        所以说v-model相当于一个语法糖。

    - 对于checkbox和radio，将绑定checked属性和change事件。

    - 对于select，将绑定value属性和change事件。

  - 在\<input>和\<textarea>使用v-model进行双向绑定时输入需要使用输入法的语言

    - 对于需要使用输入法的语言（中文），v-model并不会使绑定的响应式数据在输入法组织语言的时候得到更新。
    - 如果想要在输入法组织语言的时候也更新响应式数据，那么可以显式绑定@input事件和bindValue。

  - v-model的修饰符
    - .lazy
      - 将text和textarea中的v-model转化为绑定change事件。
    - .number
      - 将用户输入的值自动转化为Number。
    - .trim
      - 自动过滤用户输入的首尾空白字符。

- 在自定义组件上使用v-model

  - 一个基础例子

    - 父组件

      ```{vue}
      <template>
      	<Sub v-model="ParentData"/>
      </template>
      
      export defineComponent({
      	setup(){
      		const ParentData = ref(1)
      		retrun {
      			ParentData,
      		}
      	}
      })
      ```

      在父组件的模板中将ParentData绑定在了子组件上。

    - 子组件

      ```{vue}
      <template>
      	<div>{{ value }}</div>
      	<button @click="change">Change Parent Data</button>
      </template>
      
      export defineComponent({
      	props: {
      		value: {
      			type: Number,
      			required: true,
      		}
      	},
      	setup(props,{ emit }){
      		const change = () => {
      			emit('input',123)
      		}
      		return {
      			change,
      		}
      	}
      })
      ```

      我们在子组件中的props中声明了一个value，这是因为v-model在组件上使用的时候，默认绑定值是value，默认事件是@input

      因此，我们也可以通过在子组件中触发input事件来修改父组件中的响应式数据。

      这相当于父子组件间通信，但是用v-model做成了一个语法糖。

  - 修改v-model在组件上使用时的默认属性和默认事件

    - 在子组件中使用model选项

      ```{vue}
      <template>
      	<div>{{ parentData }}</div>
      	<button @click="handleClick">change Parent Data</button>
      </template>
      
      export defineComponent({
      	model: {
      		prop: 'parentData',
      		event: 'changeParentData'
      	},
      	props: {
      		parentData: {
      			type: Number,
      			required: true,
      		}
      	},
      	setup(props,{ emit }){
      		const handleClick = () => {
      			emit('changeParentData',123)
      		}
      	}
      })
      ```

      我们在子组件的model选项中，通过prop属性定义了v-model在父组件模板中绑定的属性值，通过event属性定义了在父组件模板中绑定的自定义事件名。