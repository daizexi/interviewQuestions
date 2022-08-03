# 1.静态类型

> 类型系统按照类型检查的时机来分类，可以分为动态类型和静态类型。
>
> - 动态类型
>
>   - 动态类型是指在运行时才会进行类型检查，JavaScript是动态类型语言。（动态类型语言中出现类型错误会导致运行时错误）。
>
>     ```{javascript}
>     //JavaScript
>     let foo = 1;
>     foo.split(" "); //运行时会报错。
>     ```
>
>     
>
>   - 静态类型是指编译阶段就能确定每个变量的类型，TypeScript是静态类型语言。（静态类型语言的类型错误会导致语法错误）。
>
>     ```{typescript}
>     //Typescript，大多数TypeScript代码都只需要少量修改或不需要修改就可以得到JavaScript代码。
>     let foo = 1;
>     foo.split(" "); //编译时就会报错
>     ```
>
>     ```{typescript}
>     let foo: number = 1;
>     foo.split(" "); //编译时报错
>     ```

# 2. 强弱类型

> 类型系统按照是否允许隐式类型转换可以分成强类型和弱类型。
>
> - TypeScript是弱类型语言，ts可以进行隐式类型转换。
> - TypeScript和JavaScript的主要区别在于，添加了静态类型系统，使得当出现类型错误，会直接在编译阶段报错。

# 3.TypeScript指定变量类型的方法

> - 在TypeScript中，我们使用 : 来指定变量的类型。
>
>   ```{typescript}
>   function sayHello(person: string) //使用: 指定了变量person的类型。
>   {
>     return "hello" + person;
>   }
>   let user = "user1";
>   console.log(sayHello(user));
>   ```

# 4.接口

> ```{typescript}
> function printLabel(labelledObj: {label: string}){
>   console.log(labbelledObj.label);
> }
> let myObj = {size: 10, label: "Size 10 Object"};
> printLabel(myObj);
> ```
>
> 在这段TypeScript代码中，类型检查器，要求printLabel的参数是一个对象，并且这个对象包含一个名为label的属性，并且这个属性的类型是string。
>
> 在这种情况下（指定一个必须包含某个属性且属性类型也被指定的情况），我们可以使用接口。
>
> ```{typescript}
> interface LabelledValue{
>   label: string;
> }
> function printLabel
> ```
>
> 

# 5.tsconfig.json

> Tsconfig.json可以用来定义编译上下文，相当于告诉typescript当前哪些文件需要被编译，以及使用了哪些编译选项
>
> - 在tsconfig.json中选定编译选项，可以使用compilerOptions。
>
> ```{json}
> {
>  "compilerOptions": {
>    "target": "es5",
>    "module": "commonjs",
>    ...
>  }
> }
> ```
>
> - 使用"files"选定要编译的文件。
>
> ```{json}
> {
>   "files": [
>     "./some/file.ts"
>   ]
> }
> ```
>
> - 可以使用include和exclude选项来指定要包含的文件和要排除的文件
> ```{json}
> {
>   "include": [
>     "./folder"
>   ],
>   "exclude": [
>     "./folder/some.ts",
>     "./folder/someSubFolder"
>   ]
> }
> ```

# 6.声明空间

> 

# 7.模块

> - 全局模块
>
>   - 在默认情况下，当在一个文件如foo.ts中声明了一个变量foo，那么在同一个项目中的另一个文件bar.ts中也可以使用这个变量foo。
>
>     foo.ts中
>
>     ```{typescript}
>     const foo = 123;
>     ```
>
>     bar.ts中
>
>     ```{typescript}
>     const bar = foo; //不会报错
>     ```
>
>   - typescript类型系统使得变量foo全局可用。
>
>   - 因为使用默认的全局变量空间是很危险的，因为会导致变量命名冲突，所以更推荐使用文件模块。
>
> - 文件模块
>
>   - 文件模块也被称为外部模块，如果在一个typescript文件的根级别位置含有import或export，那么它会在这个文件创建一个本地作用域。
>   - 如果想要在bar.ts中使用foo.ts中的内容，那么必须使用import导入。
>
> - export和import关键字语法
>
>   - 使用export导出一个变量或类型
>
>     ```{typescript}
>     //foo.ts文件中的内容
>     export const someVar = 123;
>     export type someType = {
>       foo: string;
>     };
>     ```
>
>     ```{typescript}
>     //一起导出，和上一种并没有区别
>     const someVar = 123;
>     type someType = {
>       type: string;
>     };
>     export {someVar, someType};
>     ```
>
>     ```{typescript}
>     //导出某个变量并将其重命名
>     const someVar = 123;
>     export {someVar as aDifferentName};
>     ```
>
>   - 使用import导入
>
>     ```typescript
>     //从foo导入了一个变量和一个类型。
>     import {someVar, someType} from './foo' //可以不用加后缀，会自动补全
>     ```
>
>     ```{typescript}
>     //使用重命名方式导入变量或类型
>     import {someVar as a DifferentN}
>     ```
>
>     
>
>   - 默认导出
>
>     ```{typescript}
>     export default function someFunction(){} //默认导出函数
>     
>     export default class someClass{} //默认导出类
>     ```
>
>   - 默认导入（默认导入对应默认导出，默认导入只有在foo有默认导出才有效）
>
>     ```{typescript}
>     import someLocalName from './foo'
>     ```

# 8.命名空间

> ```{typescript}
> namespace Utility{
>   export function log(msg){
>     console.log(msg);
>   }
>   export function error(msg){
>     console.log(msg);
>   }
> }
> 
> Utility.log("Call me");
> Utility.error("maybe");
> ```
>
> namespace可以确保创建的变量不会泄露至全局命名空间。
>
> ```{javascript}
> (function (Utility)){
>   Utility.log = function(msg)
>   {
>     console.log(msg);
>   }
> 	Utility.error = function(msg)
>   {
>     console.log(msg);
>   }
> })(Utility || Utility = {});
> ```
>
> 命名空间是可以嵌套的，我们可以在Utility命名空间下嵌套一个命名空间Messaging。

# 9.接口

> 接口是TypeScript用来合并多个类型声明到一个类型声明。
>
> 接口使得我们可以对每个成员进行强制类型检查。
>
> ```{typescript}
> interface Name{
>   first: string;
>   second: string;
> }
> let name: Name;
> name = {
>   first: "a",
>   second: "b"
> };
> name = {
>   //Error: 'second is missing'
>   first: "a"
> };
> name = {
>   //Error: 'second is the wrong type'
>   first: "a",
>   second: 123
> };
> ```

# 10.内联类型注解

> 我们可以使用内联类型注解，快速提供一个类型注解。
>
> ```{typescript}
> let name: {
>   first: string;
>   second: string;
> };
> name = {
>   first: "a",
>   second: "b"
> };
> name = {
>   //Error: 'second is missing'
>   first: "a"
> }
> name = {
>   //Error: 'second is the wrong type'
>   first: 'a',
>   second: 123
> };
> ```
>
> - 内联类型和接口的不同
>   - 当需要多次使用相同的内联注解的时候，可以把它重构为接口。
>   - 接口类似于内联注解的升级。

# 11.泛型

> ```{typescript}
> function reverse<T>(items: T[]): T[]{
>   const toreturn = [];
>   for(let i = items.length - 1;i >= 0;i--)
>     {
>       toreturn.push(items[i]);
>     }
>   return toreturn;
> }
> 
> const sample = [1,2,3];
> let reversed = reverse(sample);
> ```
>
> function reverse\<T>(items: T[]): T[]，第一个T[]用于约束输入参数的类型，第二个T[]用于约束函数返回的类型。
>
> reverse函数接受一个类型为泛型T[]的数组，并且返回一个泛型T[]的数组。
>
> 泛型的作用在于，当传入数组是number[]类型的时候，它针对number[]进行类型约束，当传入数组是string[]的时候，函数针对string[]进行类型约束。

# 12.联合类型

> ```{typescript}
> function formatCommand(command: string[] | string){
>   let line = '';
>   if(typeof command === 'string')
>     {
>       line = command.trim();
>     }
>   else{
>     line = command.join(" ").trim();
>   }
> }
> ```
>
> 联合类型使用 ｜ 标记，函数formatCommand可以接受string[]的输入或string的类型。

# 13.交叉类型

> 交叉类型支持从两个对象创建一个新对象。
>
> ```typescript
> function extend<T, U>(first: T,second: U): T & U
> {
>   const res = <T & U>{};
> }
> 
> ```
>

# 14.d.ts文件的作用

> d.ts文件是类型声明文件，用来提供类型声明。
>
> 由于许多第三方库是用JavaScript开发的，并不支持类型系统，当我们需要引入第三方库的时候，我们可以编写一个d.ts文件，为使用JavaScript开发的第三方库添加类型定义。
