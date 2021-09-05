# 1. BFC是什么，怎么触发，一般用来做什么

> BFC是block formatting context，块级格式上下文，它是一个独立的渲染区域，不会对外部造成影响，只有块级盒子参与BFC，它规定了内部元素的布局方式。
>
> BFC的5条规定
>
> 1. 在同一个BFC中，相邻的margin会发生叠加。
> 2. 计算一个BFC高度时，浮动元素的高度也会计算在其中。
> 3. 在一个BFC中，盒子会在垂直方向上一个接一个的排列。
> 4. BFC不会与外部的float元素发生重叠。
> 5. 在一个BFC中，每一个盒子的margin-left都会贴着包含盒子的元素的border-left，即使存在浮动元素，也是这样。
>
> BFC有5种触发条件
>
> 1. 根元素
>
> 2. float属性是left或right。
>
> 3. position属性是absolute或fixed。
>
> 4. overflow属性是auto或hidden或scroll。
>
> 5. 元素类型（display属性）是inline-block或table-caption或table-cell。
>
> BFC可以控制内部的块级元素的排列方式，我们可以使用BFC来实现常见3种效果
>
> 1. 创建BFC避免相邻的margin重叠。
> 2. 创建BFC清除浮动带来的影响。
> 3. 创建BFC实现自适应两栏布局。

# 2. CSS选择器优先级

> **CSS选择器优先级一共有5个档次**
>
> 行内样式 > id选择器 > 属性选择器 === 伪类选择器 === class选择器 > 元素选择器 === 伪元素选择器 > 通配符选择器
>
> **CSS选择器只针对指定同一个元素的同一个属性时使用。**


# 3. flex代码题
## 回答a和b的宽度是多少

100px和200px。

## 如果把container的宽度改成50px会怎样

a的宽度是16.6，b的宽度是16.6 * 2。

## flex这个属性是哪几个属性的简写

flex-grow、flex-shrink、flex-basis。

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
    flex:1; //flex是flex-grow、flex-shrink、flex-basis的简写，因为默认值设置了flex-basis: auto;所以width不再起效果。
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

# 12.div中实现元素垂直居中的方法
> - 图片垂直居中的方法（也可以把块元素转换成inline-block元素，那么就和图片居中一样了）。
>
>   1. 结合display: table-cell;和vertical-align: middle;
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <style type = "text/css">
>               div{
>                   display: table-cell;
>                   width: 40em;
>                   height: 40em;
>                   font-size: 10px;
>                   border: 0.1em solid red;
>                   vertical-align: middle;
>                   text-align: center;
>               }
>               img{
>                   width: 10em;
>                   height: 10em;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div id = "wrapper-first"><img src = "8C52A09A073693FCC58E4E646220AE35.jpg"/></div>
>           </div>
>       </body>
>   </html>
>   ```
>
> - 把块元素使用display属性设置为inline-block类型，再把块元素的父元素设置text-align: center;属性，这样可以使得块元素水平居中。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset="utf-8"/>
>           <style type = "text/css">
>               .wrapper{
>                   border: 0.1em solid red;
>                   font-size: 10px;
>                   width: 10em;
>                   height: 10em;
>                   text-align: center;
>               }
>               .wrapper>#wrapper-first{
>                   background-color: cadetblue;
>                   width: 5em;
>                   height: 5em;
>                   display: inline-block;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div id = "wrapper-first"></div>
>           </div>
>       </body>
>   </html>
>   ```
>
> - 使用margin: 0 auto;可以直接设置块元素水平居中。（margin: 0 auto;是对当前元素设置的）。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset="utf-8"/>
>           <style type = "text/css">
>               .wrapper{
>                   border: 0.1em solid red;
>                   font-size: 10px;
>                   width: 10em;
>                   height: 10em;
>                   text-align: center;
>               }
>               .wrapper>#wrapper-first{
>                   background-color: cadetblue;
>                   width: 5em;
>                   height: 5em;
>                   margin: 0 auto;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div id = "wrapper-first"></div>
>           </div>
>       </body>
>   </html>
>   ```
>
>   

# 13.<img/>是什么类型的元素

> <img/>是行内块元素，它可以设置width属性和height属性，并可以和其他行内元素在显示效果上共占一行。
>
> <input/>也是行内块元素。

# 14.行内元素和块元素的区别
> - 行内元素
>
>   1. 行内元素可以和其他行内元素共占一行。
>
>   2. 行内元素内可以容纳其他行内元素。
>
>   3. 行内元素不能设置width属性和height属性。
>
>   4. 行内元素只能设置marigin-left属性和margin-right属性。
> - 块元素
>   1. 块元素排斥其他元素和它共占一行。
>   2. 块元素内部一般容纳其他块元素或行内元素。
>   3. 块元素可以设置width属性和height属性。
>   4. 块元素可以设置4个方向上的margin属性。

# 15.子元素width之和大于父元素width之和，设置了inline-block也不能共占一行。

> 正确
>
> ```{html}
> <!DOCTYPE html>
> <html>
>     <head>
>         <meta charset = "utf-8"/>
>         <!-- <meta http-equiv = "refresh" content = "6;url = https://www.baidu.com"/> -->
>         <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>         <style type = "text/css">
>             html{
>                 font-size: 62.5%;
>             }
>             .wrapper{
>                 width: 10em;
>                 height: 10em;
>                 font-size: 1em;
>             }
>             .wrapper>div{
>                 background-color: blanchedalmond;
>                 display: inline-block;
>                 width: 10em;
>                 height: 10em;
>             }
>             #wrapper-first{
>                 background-color: cornflowerblue;
>             }
>             #wrapper-second{
>                 background-color: cornsilk;
>             }
>         </style>
>     </head>
>     <body>
>         <div class = "wrapper">
>             <div id = "wrapper-first">aaa</div>
>             <div id = "wrapper-second">bbb</div>
>             <div id = "wrapper-third">ccc</div>
>         </div>
>     </body>
> </html>
> ```
>
> 

# 16.display: none;和visibility: hidden;的区别
> display: none;使得元素被隐藏，并且元素不再占据之前的位置。
>
> visibility: hidden;使得元素被隐藏，但是只是看不见了，元素还占据着之前的位置。

# 17.回流和重绘的区别
> 



# 18.进行SEO（搜索引擎优化）技巧

> - 使用text-indent: -9999px;实现SEO
>    	1. 因为在SEO中，h1是很重要的标签，一般来说，我们会把网站的LOGO放在h1标签中，但是搜索引擎只能识别文字，不能识别图片，为了SEO，我们可以使用text-indent: -9999px隐藏文字。（因为搜索引擎会把使用display: none;的部分忽略，所以不能使用sidplay: none;）。

# 19.如何使用CSS画三角形

> CSS盒子模型中，在两条border的相交处，浏览器会绘制一条接合线，我们只需要把元素的width和height属性设置为0，再把border-width设置为较大的值，任选2条或3条border，把它们的颜色设置为transparent。
>
> ```{html}
> <!DOCTYPE html>
> <html>
>  <head>
>      <meta charset = "utf-8"/>
>      <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>      <style type="text/css">
>          div{
>              width: 0em;
>              height: 0em;
>              border-width: 3em;
>              border-color: red red transparent transparent;
>              border-style: solid;
>              font-size: 10px;
>          }
>      </style>
>  </head>
>  <body>
>      <div></div>
>  </body>
> </html>
> ```
>
> 它的原理实际上是隐藏了一些border边，完整的图形应该是四条border边（每条边看上去都是一个三角形）组成的矩形。
>
> ```{html}
> <!DOCTYPE html>
> <html>
>  <head>
>      <meta charset = "utf-8"/>
>      <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>      <style type="text/css">
>          div{
>              width: 0em;
>              height: 0em;
>              border-width: 3em;
>              border-color: rebeccapurple royalblue indianred wheat;
>              border-style: solid;
>              font-size: 10px;
>          }
>      </style>
>  </head>
>  <body>
>      <div></div>
>  </body>
> </html>
> ```
>
> - 实现带边框的三角形，我们一般使用2个三角形叠加的方法。（使用定位布局来实现2个三角形的层叠）。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>           <style type="text/css">
>               .wrapper{
>                   width: 0em;
>                   height: 0em;
>                   border-width: 3em;
>                   border-color: transparent transparent black transparent;
>                   border-style: solid;
>                   font-size: 10px;
>                   position: relative;
>               }
>               .wrapper>.inside{
>                   width: 0;
>                   height: 0;
>                   border-width: 2.9em;
>                   border-style: solid;
>                   border-color: transparent transparent burlywood transparent;
>                   position: absolute;
>                   top: -28px;
>                   left: -29px;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div class = "inside"></div>
>           </div>
>       </body>
>   </html>
>   ```
>   
> - 使用带边框的三角形实现一个对话气泡框。（绝对布局的元素是相对于祖先元素中最近一个设置了position: relative;或position: absolute;或position: fixed;来定位的）。
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>           <style type="text/css">
>               .wrapper{
>                   position: relative;
>                   width: 16em;
>                   height: 10em;
>                   font-size: 10px;
>                   background-color: burlywood;
>                   margin-top: 10em;
>                   border: 0.1em solid black;
>                   text-align: center;
>                   line-height: 10em;
>               }
>               .wrapper>.bottom{
>                   position: absolute;
>                   width: 0;
>                   height: 0;
>                   border-width: 3em;
>                   border-style: solid;
>                   border-color: transparent transparent black transparent;
>                   margin-left: 5em;
>                   top: -6em;
>               }
>               .wrapper .top{
>                   position: absolute;
>                   width: 0;
>                   height: 0;
>                   border-width: 2.9em;
>                   border-style: solid;
>                   border-color: transparent transparent burlywood transparent;
>                   left: -2.85em;
>                   top: -2.8em;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               这是一个对话气泡框。
>               <div class = "bottom">
>                   <div class = "top"></div>
>               </div>
>           </div>
>       </body>
>   </html>
>   ```
> 
> 

# 20.使用CSS实现半圆和圆

> - 实现半圆
>
>   - 我们把div的height设置为width的一半，再把左上角和右上角的border-radius属性值设置为等于height，再把左下角和右下角的border-radius设置为0。
>
>   - 实际上，原理是因为border-radius设置的是圆角的半径。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css">
>                 div{
>                     width: 20em;
>                     height: 10em;
>                     font-size: 10px;
>                     border-radius: 10em 10em 0 0;
>                     border: 0.1em solid red;
>                 }
>             </style>
>         </head>
>         <body>
>             <div></div>
>         </body>
>     </html>
>     ```
>
> - 实现圆
>
>   - 我们把div的height和width设置为相等的值，再把四个角的border-radius设置为height的一半。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css">
>                 div{
>                     width: 10em;
>                     height: 10em;
>                     font-size: 10px;
>                     border-radius: 5em;
>                     border: 0.1em solid red;
>                 }
>             </style>
>         </head>
>         <body>
>             <div></div>
>         </body>
>     </html>
>     ```
>
> - 实现椭圆
>
>   - 实现椭圆的语法是border-radius: x/y，x是圆角的水平半径，y是圆角的垂直半径。
>   
>   - 我们可以把div的width和height设置为不同的值，再把水平半径x设置为width的一半，把垂直半径设置为height的一半。
>   
>   - border-radius: 30px实际上就表达的是，border-radius: 30px/30px;，所以我们实现圆和圆角的写法只是border-radius属性的缩写。 
>   
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css">
>                 div{
>                     width: 10em;
>                     height: 5em;
>                     font-size: 10px;
>                     border: 0.1em solid red;
>                     border-radius: 5em/2.5em;
>                 }
>             </style>
>         </head>
>         <body>
>             <div></div>
>         </body>
>     </html>
>     ```
>   
>     
>

# 21.CSS选择器的匹配效率

> id选择器 > class选择器 > 元素选择器 > 相邻选择器 > 子选择器 > 后代选择器 > 通配符选择器 > 属性选择器 > 伪类选择器。
>
> 原理是，浏览器解析选择器是从右向左的，如果最右面的选择器需要匹配大量元素，那么就会导致解析效率很低，如，#column .content div{}选择器，浏览器会先去页面中匹配所有div元素，再去匹配所有div元素中祖先元素有class名是content的，然后再在这个基础上去寻找祖先元素中id是column的元素。

# 22.垂直居中

> - 文本的垂直居中
>
>   - 单行文本的垂直居中
>
>     ​		对于单行文本，我们可以直接设置line-height和父元素height值相等。
>
>   - 多行文本的垂直居中
>
>     ​		对于多行文本，我们可以使用span元素把文本包含起来，然后使用dispaly: inline-block;把span转换成inline-block元素，这样就可以使用inline-block元素的垂直居中方式了。
>
> - 元素的垂直居中
>
>   - 对于block元素，我们需要先给父元素和子元素设置width和height（因为position只对设置了width和height的元素有效），然后我们对父元素设置position: relative;对子元素设置position: absolute;，再对子元素设置top: 50%和left: 50%，把子元素的margin-left设置为子元素width值得负值的一半，margin-top设置为子元素height负值的一半。**这样我们就实现了block元素的水平垂直居中。**
>
>   - 如果只想设置垂直居中，可以把margin-top和top元素删掉，如果只想设置水平居中，可以把left和margin-left属性删掉。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css">
>                 .wrapper{
>                     border: 0.1em solid red;
>                     font-size: 10px;
>                     width: 10em;
>                     height: 10em;
>                     position: relative;
>                     margin-top: 20em;
>                 }
>                 div{
>                     position: absolute;
>                     width: 5em;
>                     height: 5em;
>                     border: 0.1em solid red;
>                     top: 50%;
>                     left: 50%;
>                     margin-left: -2.5em;
>                     margin-top: -2.5em;
>                 }
>             </style>
>         </head>
>         <body>
>             <div class = "wrapper">
>                 <div></div>
>             </div>
>         </body>
>     </html>
>     ```
>
> - flex实现块元素在父元素中垂直水平居中
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset = "utf-8"/>
>           <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>           <style type = "text/css">
>               .wrapper{
>                   font-size: 10px;
>                   border: 0.1em solid red;
>                   width: 20em;
>                   height: 20em;
>                   display: flex;
>                   justify-content: center;
>                   align-items: center;
>               }
>               .wrapper div:first-child{
>                   flex-basis: 10em;
>                   height: 10em;
>                   background-color: dodgerblue;
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "wrapper">
>               <div></div>
>           </div>
>       </body>
>   </html>
>   ```
>
>   
>
>   - 对inline-block元素设置垂直居中
>
>     对父元素设置display: table-cell;和vertical-align: middle;，这样可以使得inline-blcok元素垂直居中。
>
>     ```{html}
>     <!DOCTYPE html>
>     <html>
>         <head>
>             <meta charset = "utf-8"/>
>             <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>             <style type = "text/css">
>                 .wrapper{
>                     border: 0.1em solid red;
>                     font-size: 10px;
>                     width: 10em;
>                     height: 10em;
>                     margin-top: 20em;
>                     display: table-cell;
>                     vertical-align: middle;
>                 }
>                 div{
>                     width: 5em;
>                     height: 5em;
>                     border: 0.1em solid red;
>                     display: inline-block;
>                 }
>             </style>
>         </head>
>         <body>
>             <div class = "wrapper">
>                 <div></div>
>             </div>
>         </body>
>     </html>
>     ```
>
> 
>
> 

# 23.水平居中

> - 文本的水平居中
>
>   - 单行文本的水平居中
>
>     对于单行文本的水平居中，我们可以使用text-align: center;的方法。（text-align: center对于文本、inline元素、inline-block元素、inline-flex元素等都有效）。
>
> - 元素的水平居中
>
>   - block元素的水平居中
>
>     对于block元素，我们可以使用margin-left: auto和margin-right: auto;的方法来设置水平居中。（等价于margin: 0 auto;）
>
>   - inline-block元素设置水平居中
>   
>     对于inline-block元素，我们可以设置text-align: center;来使得inline-block元素水平居中。
>

# 24.使用BFC避免外边距叠加问题

> 因为BFC规定，在同一BFC中，相邻的外边距会发生叠加，所以我们可以通过触发创建一个新的BFC来避免。
>
> ```{html}
> <!DOCTYPE html>
> <html>
>     <head>
>         <meta charset="utf-8"/>
>         <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>         <style type = "text/css">
>             .wrapper{
>                 border: 0.1em solid red;
>                 font-size: 10px;
>                 width: 20em;
>                 overflow: hidden;
>                 text-align: center;
>             }
>             .first,.second{
>                 line-height: 5em;
>                 height: 5em;
>                 width: 5em;
>                 background-color: coral;
>             }
>             .first{
>                 margin-bottom: 3em;
>             }
>             .second{
>                 background-color: cornflowerblue;
>                 margin-top: 2em;
>             }
>             .createNewBFC{
>                 overflow: hidden; <!-- 创建了一个新的BFC，这样second盒子和first盒子就不在一个BFC了，就不会发生相邻外边距叠加的问题。 -->
>             }
>         </style>
>     </head>
>     <body>
>         <div class = "wrapper">
>             <div class = "first">aaa</div>
>             <div class = "createNewBFC">
>                 <div class = "second">bbb</div>
>             </div>
>         </div>
>     </body>
> </html>
> ```
>
> 

# 25.触发创建新BFC的5种情况

> 1. 根元素
>
> 2. float属性设置为left或right。
>
> 3. position属性设置为absolute或fixed。
>
> 4. overflow属性设置为hidden或auto或scroll。
>
> 5. display属性为inline-block、table-caption、table-cell。
>
> **根元素会创建一个BFC，所以在默认情况下，一个页面中所有块级盒子都在一个BFC中。**

# 26.实现自适应两栏布局

> 自适应两栏布局是指，在左右两列中，有一列的宽度自适应，另一列的宽度固定。
>
> - 使用BFC实现自适应两栏布局
>
>   ```{html}
>   <!DOCTYPE html>
>   <html>
>       <head>
>           <meta charset="utf-8"/>
>           <link type = "text/css" rel = "stylesheet" href = "a.css"/>
>           <style type = "text/css">
>               .first,.second{
>                   background-color: coral;
>                   font-size: 10px;
>               }
>               .first{
>                   float: left;
>                   width: 20em;
>                   height: 20em;
>               }
>               .second{
>                   background-color: cornflowerblue;
>                   height: 30em;
>                   overflow: hidden; <!-- second元素变成了一个新的BFC。 -->
>               }
>           </style>
>       </head>
>       <body>
>           <div class = "first"></div>
>           <div class = "second"></div>
>       </body>
>   </html>
>   ```
>
> 
>

# 26.CSS3中浏览器的私有前缀

> 因为一些CSS3标准还没有成为W3C的标准，因此对于这些属性，每种浏览器有不同的实现，为了在不同浏览器中兼容效果，我们必须加上浏览器的私有前缀。
>
> - Chrome、Safari、Edge
>   - 使用 -webkit- 前缀。
> - Firefox
>   - 使用 -moz- 前缀。
> - Opera
>   - 使用 -o- 前缀。

# 27.常见的浏览器内核

> - Chrome、Safari使用Webkit内核。
> - FireFox使用Gecko内核。
> - IE使用Trident内核。
> - Opear和部分版本Chrome使用blink内核。

# 28.会发生继承的样式

> 1. width
> 2. height
> 3. font-size
> 4. line-height（当line-height使用百分比作为单位时，子元素继承利用百分比乘以当前元素font-size得到的值，当line-height使用数字不带单位时，子元素继承这个倍数）
> 5. text-align
> 6. color
