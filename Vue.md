# 1.声明式渲染

> ```{html}
> <!DCOTYPE html>
> <html>
>   <head>
>     <meta charset = "utf-8"/>
>     <style type = "text/css"></style>
>   </head>
>   <body>
>     <div id = "app">
>       {{ message }}
>     </div>
>   </body>
>   <script type = "text/javascript">
>     let app = new Vue({
>       el: "#app",
>       data: {
>         message: "Hello Vue."
>       }
>     })
>   </script>
> </html>
> ```
>
> 