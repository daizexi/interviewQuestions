# 1.HTML、HTML5、XHTML的区别

> HTML是指HTML 4.01
>
> HTML5是指HTML 5
>
> XHTML是指更严格的HTML 4.01，如XHTML的标签必须闭合，XHTML的标签必须小写。



# 2.defer和script

> HTML5为script元素新增了2个属性，1.defer属性，2.async属性。这2个属性的作用都是加快页面的加载速度。
>
> - defer属性
>   - defer属性用于异步加载外部JavaScript，当异步加载完成之后，该JavaScript不会立即执行，会等到HTML文档加载完成才会执行。
> - async属性
>   - async属性用于异步加载外部JavaScript，当异步加载完成之后，该JavaScript就会立即执行，这时会阻塞HTML文档的加载。

# 3.HTML中DOCTYPE的作用

> <!DOCTYPE>声明是文档类型定义（DTD），DTD是一组机器可读的规则，它定义了HTML特定版本中允许存在什么，不允许存在什么，在浏览器解析文档过程中，将会使用这些规则检查页面的有效性，浏览器会通过分析<!DOCTYPE>来确定使用哪些规则来解析文档，也就确定了HTML的版本。

# 4.严格模式和混杂模式

> 严格模式和混杂模式都是浏览器的呈现模式。
>
> - 严格模式
>   - 严格模式下，浏览器按照W3C的规范来解析代码。
> - 混杂模式
>   - 混杂模式下，浏览器根据自己的规范来解析文档。
>     - 当DOCTYPE声明缺失或书写错误的时候，就会使用混杂模式。
> - 严格模式和混杂模式的区别
>   - 混杂模式下，盒模型的height和width包括padding和border，而W3C标准中设置width和height只包含content的高和宽。
>   - 混杂模式下，可以设置inline元素的height和width，W3C标准下不行。

# 5.<script>标签同时包含src和内部代码

> - 当<script>标签同时包含src和内部代码时，内部代码会被忽略。
> - 如果需要引入外部代码的同时使用内部代码，可以使用两个<script>标签。

# 6.为什么要使用事件委托

> - 给很多个DOM元素添加事件处理器，会很大影响网页的性能。
> - 如果需要动态删除某一个DOM元素，需要注意把这个DOM元素的事件处理器也删掉，否则会造成内存泄露。

# 7.Cookie、sessionStorage、localStorage的区别

- cookie将会在同源的Http请求中始终携带（除非服务器指定不需要cookie）
