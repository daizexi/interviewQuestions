# 1. BFC是什么，怎么触发，一般用来做什么

 

# 2. CSS选择器优先级

> **CSS选择器优先级一共有5个档次**
>
> 行内样式 > id选择器 > 属性选择器 === 伪类选择器 === class选择器 > 元素选择器 === 伪元素选择器 > 通配符选择器
>
> **CSS选择器只针对指定同一个元素的同一个属性时使用。**


# 3. flex代码题
## 回答a和b的宽度是多少
## 如果把container的宽度改成50px会怎样
## flex这个属性是哪几个属性的简写
```{html}
<template>
    <div class="container">
        <div class="a"></div>
        <div class="b"></div>
    </div>
</template>

<style lang="less">
.container{
    display:flex;
    width:300px;
}
.a{
    flex:1;
    width:50px;
}
.b{
    flex:2;
    width:50px;
}
</style>
```

 

# 4.px、em和rem的区别

> - px是指以像素为单位，如果一台电脑的屏幕分辨率是800*600，说明屏幕宽800px，高600px。
> - em是以当前元素的font-size值为单位，如果当前元素font-size值是10px，那么1em等于10px。如果当前元素没有设置font-size，那么会去寻找祖先元素的font-size，如果祖先元素也没有设置，那么会直接使用浏览器默认font-size: 16px;。
> - rem和em相似，但是rem是使用根元素（html元素）的font-size值，html的font-size值默认是16px。



# 5.什么时候使用em

> - 首行缩进使用text-indent: 2em来实现。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>       </head>
>       <body>
>           <p style = "font-size: 14px; text-indent: 2em;">aaa</p>
>       </body>
>   </html>
>   ```
>
>   
>
> - 使用em继承浏览器默认font-size: 16px;来设置
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <style type = "text/css">
>               body{
>                   font-size: 62.5%;
>               }
>           </style>
>       </head>
>       <body>
>           <p style = "font-size: 1em;">aaa,10px</p>
>           <p style="font-size: 2em;">bbb,20px</p>
>           <p style="font-size: 1.5em;">ccc,15px</p>
>       </body>
>   </html>
>   ```
>
>   
>
> - 使用em作为字体大小单位，这时会去寻找祖先元素的font-size值，如果祖先元素都没有设置font-size，那么会直接继承浏览器默认font-size值（16px）。

# 6.CSS层叠性
> 当我们对于一个**相同的元素**的**同一个属性**，重复定义了多个样式，就会触发CSS层叠性。CSS的层叠性指的就是样式的覆盖。
> CSS将首先选择CSS选择器优先级高的显示，如果设置的多个样式优先级相同，那么显示后定义的样式。
>
> 1.当具有相同优先级，后定义的样式覆盖先定义的样式。
>
> ```{html}
> <!DOCTYPE html>
> <html>
>  <head>
>      <meta charset = "utf-8"/>
>      <style type="text/css">
>          .container{
>              color: red;
>          }
>          .container{
>              color: blue
>          }
>      </style>
>  </head>
>  <body>
>      <div class="container">
>          aaa
>      </div>
>  </body>
> </html>
> ```
> 2.当优先级不同，选择优先级更高的。
>
> ```{html}
> <!DOCTYPE html>
> <html>
>     <head>
>         <meta charset = "utf-8"/>
>         <style type="text/css">
>             .container{
>                 color: red;
>             }
>             #containerId{
>                 color: blue;
>             }
>         </style>
>     </head>
>     <body>
>         <div id = "containerId" class="container">
>             aaa
>         </div>
>     </body>
> </html>


# 7.CSS继承性
> CSS继承性是指子元素会继承父元素的某些属性，但是一些属性如padding、margin、border属性是不会继承的，CSS也可以设置强制继承
> ```{html}
> <!DOCTYPE html>
> <html>
>     <head>
>         <meta charset = "utf-8"/>
>         <style type="text/css">
>             html{
>                 font-size: 62.5%;
>             }
>             .container{
>                 color: red;
>                 font-size: 1em;
>                 width: 1em
>             }
>             .container div{
>                 color: inherit;
>             }
>         </style>
>     </head>
>     <body>
>         <div class="container">
>             aaa
>             <a href = "https://www.baidu.com">aaa</div> <!-- 由于a元素本身具有颜色样式，所以不会继承，但可以通过color: inherit;来强制继承。 -->
>         </div>
>     </body>
> </html>

# 8.@import和link引入样式表的区别
> 1. @import方式先加载HTML再加载CSS，link方式CSS和HTML会同时加载。
>
> 2. @imort是CSS提供的语法，link是HTML标签。

# 9.border:0和border:none的区别
> **两者之间只有性能差异。**
> border:0表示浏览器需要把边框渲染为一个0px的边框，虽然看不见，浏览器还是需要渲染的。
> border:none表示浏览器不用渲染边框。

# 10.文本垂直居中的方法
> 1. 设置line-height
>
>    ```{html}
>    <!DOCTYPE html>
>    <html>
>        <head>
>            <meta charset = "utf-8"/>
>            <style type = "text/css">
>                html{
>                    font-size: 62.5%;
>                }
>                .wrapper>div{
>                    width: 10rem;
>                    height: 10rem;
>                    text-align: center;
>                    font-size: 1em;
>                    font-weight: bold;
>                    color: black;
>                    line-height: 10em; <!-- line-height实际上是文字上下所组成的一行的高度，所以叫行高，所以当line-height设置等于div的height之后，文字垂直居中了。 -->
>                }
>                #first{
>                    background-color: khaki;
>                }
>                #second{
>                    background-color: lavenderblush;
>                }
>                #third{
>                    background-color: lightcoral;
>                }
>            </style>
>        </head>
>        <body>
>            <div class = "wrapper">
>                <div id = "first">a</div>
>                <div id = "second">b</div>
>                <div id = "third">c</div>
>            </div>
>        </body>
>    </html>
>    ```
>
> 
>
> 2. 

# 11.负margin技术
> 负margin技术对于普通文档流元素和对浮动元素影响是不一样的。
> - 对于普通文档流元素
>   1. 当元素的margin-top或margin-left为负数时，会把当前元素往上或左方向拉扯。
>   ```{html}
>   `<!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <style type = "text/css">
>               html{
>                   font-size: 62.5%;
>               }
>               .wrapper{
>                   font-size: 0;}
>               .wrapper>div{
>                   width: 10rem;
>                   height: 10rem;
>                   text-align: center;
>                   font-size: 10px;
>                   font-weight: bold;
>                   color: black;
>                   line-height: 10em;
>                   display: inline-block;
>               }
>               #first{
>                   background-color: khaki;
>               }
>               #second{
>                   background-color: lavenderblush;
>                   margin-left: -1em;
>               } 
>               #third{
>                   background-color: lightcoral;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div id = "first">a</div>
>               <div id = "second">b</div>
>               <div id = "third">c</div>
>           </div>
>       </body>
>   </html>
>   ```
>   2. 当元素的margin-bottom或margin-right为负数时，会把临近元素往当前元素上拉扯。
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <style type = "text/css">
>               html{
>                   font-size: 62.5%;
>               }
>               .wrapper>div{
>                   width: 10rem;
>                   height: 10rem;
>                   text-align: center;
>                   font-size: 1em;
>                   font-weight: bold;
>                   color: black;
>                   line-height: 10em;
>               }
>               #first{
>                   background-color: khaki;
>                   margin-bottom: -1em;
>               }
>               #second{
>                   background-color: lavenderblush;
>               } 
>               #third{
>                   background-color: lightcoral;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div id = "first">a</div>
>               <div id = "second">b</div>
>               <div id = "third">c</div>
>           </div>
>       </body>
>   </html>
>   ```
>
> 

# 12.实现块元素垂直居中的方法
> 