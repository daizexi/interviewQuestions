# 1.虚拟DOM

- 虚拟DOM有什么作用
  - 虚拟DOM所做的事情是提供VNode树和缓存中的旧VNode树进行比对，并根据比对结果更新DOM结构。（找出发生了改变的DOM结点，只针对这些改变了的DOM结点重新渲染）。
- 为什么需要虚拟DOM
  - 虚拟DOM本身的作用
    - 虚拟DOM可以通过比对VNode树找出发生了更改的结点并只更新这些结点，减少了对DOM的操作，可以提高性能。
  - Vue.js中为什么要引入虚拟DOM
    - 在某个响应式数据data发生改变之后，实际上Vue.js是知道需要通知哪些依赖了这个data的地方的，并且实际上Vue.js 1.0中也是这么做的。但是我们回忆Vue的响应式原理，我们可以知道，需要通知到依赖data的地方，那么必须为每一个依赖data的地方都注册一个Notify或者说Wather实例，等data发生改变之后就调用这个实例中的方法通知依赖。这个缺点就在于我们需要注册太多Watcher实例。
    - 因此在Vue.js 2.0中就改为了只为每个依赖响应式数据data的组件添加一个Watcher实例，当数据data发生改变的时候就需要调用这个组件注册在消息中心的Watcher实例通知这个组件，然后这个组件再运行虚拟DOM找出组件内部哪些结点需要更新。
- 虚拟DOM的过程
  1. 将模板编译成Render函数。
  2. 执行Render函数获取新的VNode树。
  3. 将新的VNode树和缓存中的旧VNode树进行比对（diff算法）。
  4. 更新DOM。（只更新发生改变的DOM结点）。
- 虚拟DOM的缺点
  1. 在第一次渲染DOM的时候，由于需要先执行Render函数获取VNode树，所以可能会更慢。
  2. 如果应用实际上只包含很少量的DOM，那么可能会负优化。但对于大多数应用而言，虚拟DOM因为减少了DOM操作而实际上提高了效率。



# 2. VNode

- 什么是VNode

  - VNode实际上是通过调用VNode类实例化的JavaScript对象。

- VNode的作用

  - VNode主要在新VNode树和旧VNode树比对时起作用。

- 6种VNode结点

  1. 注释结点
  2. 文本结点
  3. 元素结点
  4. 组件结点
  5. 函数式组件
  6. 克隆结点

- 注释结点

  - 注释结点的2个有效属性（不是undefiend或false的属性）

    1. text属性
    2. isComment属性

  - 真实注释结点和VNode注释结点对比

    ```{html}
    <!-- 注释 -->
    ```

    上面是一个真实的DOM的注释结点

    ```{js}
    {
      text: '注释',
      isComment: true,
    }
    ```

    上面是这个真实DOM注释结点对应的VNode。

  - 创建一个VNode注释结点

    ```{js}
    export const createEmptyVNode = text => {
      const node = new VNode();
      node.text = text;
      node.isComment = true;
      return node;
    }
    ```

    上面给出了一个创建VNode的工厂函数。

- 文本结点

  - 文本结点只有1个有效属性。
    1. text属性。

  - 创建文本结点

    ```{js}
    export function createTextVNode(val){
      return new VNode(undefined,undefined,undefined,String(val));
    }
    ```

    VNode类构造函数的前4个属性是tag，data，children，text。

- 克隆结点

  - 克隆结点的作用
    - 在每次组件中状态发生更新的时候，我们都需要重新执行一次Render函数以获取新的VNode树，但是有些结点是一直不会改变的，对于这些结点，我们可以使用对其进行克隆的方法获取它的克隆结点，这样我们就可以尽量少的运行Render函数。
  - isCloned属性
    - isCloned属性用于区分克隆结点和被克隆结点。克隆结点的isCloned属性是true。

- 元素结点

  - 什么是元素结点

    ```{html}
    <span></span> //就是一个元素结点。
    ```

  - 元素结点的4个有效属性

    1. tag，tag就是一个结点的名称，如p、ul、li、div等。
    2. data，data包含的是结点上的属性，如class、style等。
    3. children，children是当前结点的子结点数组。
    4. context，context是这个结点所属的Vue组件的实例。

  - 真实元素结点和VNode元素结点对比

    ```{html}
    <p>
      <span>aaa</span>
      <span>bbb</span>
    </p>
    ```

    下面是p元素的VNode

    ```{js}
    {
      children: [VNode,VNode],
      context: {}, //表示当前组件实例的对象。
      data: {}, //一个储存所有结点属性的对象
      tag: 'p',
    }
    ```

- 组件结点

  - 组件结点在元素结点的基础还有2个独有的属性
    1. componentOptions，这个是组件结点的选项参数，包含propsData、tag和children信息。
    2. componentInstance，组件的实例。

- 函数式组件

- VNode有很多类型，但是实际上它们都是VNode类的实例，只是它们的有效属性不同。

- 常用的VNode结构

  ```{js}
  //<div id="v" class="classA"><div>对应的VNode
  {
    el: div, //对真实DOM结点的引用
    tag: 'DIV', //元素名
    sel: 'div#v.classA', //选择器
    data: null, //属性
    children: [], //子结点
    text: null, //如果是文本结点text属性才是非null对应文本结点的textContent。
  }
  ```

  

# 3. Patch

- Patch的作用
  - 在Vue.js中Patch用于比较旧的VNode树和新的VNode树，找出其中不同的地方再对不同的地方触发DOM更新。
  - 实际上Patch就是Vue.js中的diff算法部分。
- 朴素的树比较算法（树的编辑距离算法）
  - 朴素diff算法的时间复杂度
    - 对于给定的两棵树结构，如果我们想对它们进行比较找出其中不同的地方。那么时间复杂度是O(n^3)。
  - 朴素diff算法的流程
    - diff算法或者说patch是为了找出从旧VNode代表的旧DOM结构转换为新VNode代表的新DOM结构所需的最小操作次数。而操作主要有3种
      1. 插入DOM结点
      2. 删除DOM结点
      3. 替换DOM结点
    - 首先朴素diff算法会比较旧VNode树的每个结点和新VNode树的每个结点，假设旧VNode树中的结点数是m，新VNode树中的结点数是n，那么在比较阶段至少需要O(mn)的时间复杂度。
    - 
- 两大假设
  1. 相同的组件将产生类似的DOM结构，不同的组件产生不同的DOM结构。
  2. 对于同一层的一组子结点，它们可以通过唯一的id进行区分。
- O(n)的diff算法
  - 编辑操作
    - 我们知道diff算法的编辑操作一般有3种，创建DOM结点（在第一次渲染时触发），删除DOM结点（在旧VNode中的结点和新VNode中的结点完全不同时触发），更新DOM结点（在旧VNode中的结点和新VNode中的结点相同但属性不同时触发）。
    - 我们可以把这3种情况归纳为2种
      1. 当旧VNode和新VNode结点类型不同时。
      2. 当旧VNode和新VNode结点类型相同，但属性不同时。
  - diff的比较操作
    - 当旧VNode结点和新VNode结点类型不同时，那么直接删除旧VNode结点对应的DOM结点及其子结点。（这是基于第一个假设的）。
    - 对于不在同一层的结点比较，diff算法只会对同一层的DOM结点进行比较（也就是只会对同一个父结点下面的所有子结点），如果发现某个VNode结点在新VNode树中不存在，diff算法就会提示Vue将这个结点及所有子结点删除。（虽然有可能是这个旧VNode结点移动到了另一个父结点下）。
    - 对于类型相同的结点且选择器相同的结点Vue会去更新，或者删除重建。
    - 对于同一层结点的比较。
      - 列表往往是同一层结点比较的常见场景，在进行同一层结点比较的时候key是区分结点的重要手段。（这也是为什么v-for要配合key使用，否则Vue将默认采用index作为key，但是这样往往不能很好的追踪变化，Vue就只能采取全部重新渲染的方式处理）。但是如果每个同层次的结点绑定一个key，那么Vue就可以通过这个key区分出哪些VNode结点是旧的，哪些VNode结点是新插入的。
  - 源码分析
    - 
  - 