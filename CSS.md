# 1. BFC是什么，怎么触发，一般用来做什么



# 2. CSS选择器优先级




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