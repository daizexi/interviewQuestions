# 1. $router和$route

- router和route的区别

  - $router访问的是全局的路由实例(是所有路由的管理器)，$route访问的是当前的路由。
  - $router.currentRoute就是当前的$route

- Vue2中访问router、route

  ```{vue}
  this.$router
  this.$route
  ```

- Vue3中访问router、route

  ```{vue}
  const router = useRouter();
  const route = useRoute();
  ```

# 2. router-link

- router-link用法

  ```{vue}
  <RouterLink to="/rules"><span>rules</span></RouterLink>
  ```

  /表示根目录，/rules表示跳转到www.example.com/rules。

- router-link和a标签的区别

  - router-link将呈现出一个带有正确href的a标签。
  - 但是router-link不像a标签将完全刷新页面，router-link可以只局部刷新。

# 3. 带参数的动态路由匹配

- 使用场景

  - 有时候我们有一个User组件来展示所有用户的主页，但是由于用户id不同，路由会有一个不同的路径参数，用户A的路由是/user/A，用户B的路由是/user/B，对于这种路由不同但是映射的组件相同的情况，Vue Router提供了动态路径参数的方法。（要明确是动态路径参数）

- 在路由的路径中使用动态字段

  - routes

    const routes = [{path: 'user:id', component: User}]，在这里我们使用了 : 来表示动态字段。

- 在组件中访问动态路径参数的方法

  - 使用$route.params
    - $route.params将表现为一个对象，动态字段的名称就是这个对象的key，动态字段的取值就是这个对象的对应key的value。
    - /users/:username/posts/:postId，那么$route.params这个对象就会有2个属性，第一个的key是username，第二个的key是postId。

# 4. 嵌套路由

- 什么是嵌套路由

  - 业务上有一种场景是在某一个父组件中，需要根据路由切换子组件。如某个路由/users下有一个侧边栏，点击侧边栏的不同item会对应切换到不同的路由，并在主要内容中展示该路由映射的组件。一般可以使用\<router-view>进行适配。

- 使用router-view

  - 在父组件User中

    ```{vue}
    <template>
    	<router-link to='profile'>
        <span>To profile</span>
      </router-link>
    	<router-view></router-view>
    </template>
    ```

    这里是使用router-view的父组件，其中定义了一个router-link，用以声明式的导航到当前路径下的profile（使用/profile就是表示导航到根目录下的profile路径，使用profile就是相对路径，也就是当前路径的profile子路径）

  - 配置routes

    ```{js}
    const routes = [
      {
        path: '/users',
        component: '@/components/User',
        children: [
          {
            path: 'profile',
            component: '@/components/User/components/profile.vue'
          }
        ]
      }
    ]
    ```

    我们可以看到的是children就配置了/users/profile路径的映射组件。

    同时children属性指向的只是另一个routes数组。



# 5. Vue Router支持的2种切换路由的方法

- router-link
  - router-link是一个声明式的方式，它被渲染为一个a标签，但可以不完全刷新页面就切换URL。
  - 如果router-link跳转的路由是一个变量，那么可以对to属性使用v-bind。（v-bind可以对HTML属性进行插值）
- $router.push
  - $router.push()会向history栈中添加一个新的记录，所以用户点击浏览器后退按钮时，会回到之前的URL。
  - $router.push()可以接受一个path，$router.push('users/A')
  - $router.push()可以接受一个对象，$router.push({path: 'users/A'})
  - $router.push()接受使用命名路由代替path，$router.push({name: 'user', params: {username: 'A'}})
  - 如果同时提供了path和params，那么params将会被忽略，可以使用字符串插值解决动态路径参数的问题，$router.push({path: \`users/${userName}`})
  - $router.push()可以为当前路由添加查询参数，$router.push({query: {plan: 'private'}})



# 6. 为什么要操作history栈

- history栈的作用
  - histroy栈用于保存历史记录，这使得用户可以通过点击回退回到上一个页面状态。
  - 用户回退操作是通过读取history栈中的信息实现的。
- 为什么要操作history栈
  - 可以通过操作history栈修改用户回退操作的结果。
  - 如果在跳转路由的时候不将失活的路由压进history栈，那么点击回退之后会退到上上次的路由对应页面。（可以把router-link的replace属性设置为true看看效果）

# 7. 命名视图

- router-view的作用

  - router-view用于在切换路由的时候局部刷新使用了router-view的地方。也就是说router-view用于在切换路由的时候局部刷新。

- 命名视图的作用

  - 命名视图用于存在需要使用多个router-view的情况，如一个页面（父组件）上有2个不同区域需要在切换路由的时候局部刷新。
  - 命名视图的特点是在定义了命名视图的路由之间切换时，父组件内容始终展示，其他组件对应渲染到routes中配置的相应的router-view处。
  - 在routes数组中某一个路由配置对象中定义了components就说明会应用命名视图，并且component不再生效。
  - 命名视图也不能和嵌套路由一起使用。 

- 使用命名视图

  - 在父组件中使用

    ```{vue}
    <template>
    	<RouterLink to='profile'>
        <span>To profile</span>
      </RouterLink>
    	<RouterLink to='/'>
        <span>To root directory</span>
      </RouterLink>
      <router-view></router-view>
      <router-view name='profile'></router-view>
    </template>
    ```

    父组件中存在2个部分需要在组件触发切换之后发生局部刷新。

  - routes数组配置

    ```{js}
    const routes = [
      {
        path: '/'
        components: {
        	default: '@/components/SideBar',
        	profile: '@/components/Profile'
      	},
      },
      {
        path: '/profile',
        components: {
          default: '@/components/Nav',
          profile: '@/components/NewProfile',
        }
      }
    ]
    ```

    