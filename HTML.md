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
