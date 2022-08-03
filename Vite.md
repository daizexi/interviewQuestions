# Vite

## Vite用来做哪些工作

### Vite是一种新型的前端构建工具，它的功能类似于webpack，但是服务启动速度和更新速度优于webpack。

- 为什么需要构建工具

	- 有5个方向上的需求，使我们需要构建工具

		- 预处理

			- 在大型的项目中，我们经常使用SCSS、TS等需要经过编译才可以执行的语言，构建工具可以帮助我们完成这些工作。

		-  eslint

			- 构建工具可以帮助完成eslint的检查。

		- 资源压缩

			- 在大型项目中，我们一般将业务逻辑单独写在以一个文件中，但是如果到了生产环境，用户仍然需要加载这么多的文件，毫无疑问会影响性能。所以在发布到生产环境之前，需要使用构建工具将逻辑压缩合并。

		- 静态资源替换

			- 在生产环境中，静态资源的地址很可能和开发环境中不一致，可能是由于在资源压缩的过程中的JS合并、CSS合并导致，也可能是因为应用了CDN，这些大量重复的静态资源的url替换都可以借助构建工具帮助我们完成替换。

## Vite的使用方法

### 安装Vite

- 2种方法用于安装Vite（npm和yarn，适用于安装的同时初始化一个vite项目）

	- npm create vite@latest
	- yarn create vite（可以完成安装并创建一个Vite+Vue/React的项目）

- 如果想为项目下载vite依赖

	- npm install vite@latest

### 使用Vite

- 想要运行一个vite项目需要经过下面几步

	- 安装依赖

		- 安装vite并创建一个vite项目之后，由于已经初始化了package.json文件，但是没有node_modules文件夹，所以，我们需要执行npm install命令。

			- 执行完成npm install命令之后，会将下载的依赖放在node_modules文件夹下，并且创建package-lock.json文件用于锁定依赖版本。

	- 启动vite服务

		- 执行npm run dev命令。

### 配置vite

- vite的配置文件

	- vite的默认配置文件

		- 类似于webpack的webpack.config.ts，vite配置文件是vite.config.ts

	- 为vite指定一个配置文件

		- 执行vite --config my-config.ts，将会把vite的配置文件设置为my-config.ts。

- 构建配置

	- 构建配置由一个对象build提供。
	- 常用的构建选项

		- build.sourcemap

			- 用于决定构建后是否生成source map文件。（默认值是false）。

				- 接受的类型

					- boolean 

						- true

							- 将会在构建后创建一个独立的source map文件。

						- false

							- 不会创建source map文件。

					- 'inline'（TS字面量类型）

						- source map将作为一个data URL附加在输出文件。

					- 'hidden'（TS字面量类型）

						- 将会创建一个独立的source map文件，只是bundle文件中的注释将不被保留。（工作原理和true类似）。

				- 默认值

					- false

		- build.outDir

			- 用于指定输出路径（是一个相对路径，相对于项目根目录而言）。

				- 接受类型

					- string

				- 默认值

					- 'dist'

		- build.target

			- 用于处理浏览器兼容性的问题，设置最终构建的浏览器兼容目标。

				- 接受类型

					- string
					- string[]

				- 默认值

					- 'modules'

						- 表示构建会兼容支持原生ES模块的浏览器。

		- build.assetsDir

			- 用于指定生成静态资源的存放路径。（是一个相对路径，相对于build.outDir）。

				- 接受类型

					- string

				- 默认值

					- 'assets'

		- build.assetsInlineLimit

			- 用于指定一个阈值，当小于这个值的import或引用资源将内联为base64编码。（用来减少http请求）。

				- 接受的类型

					- number

				- 默认值（单位字节，Byte）

					- 4096（4KB）

		- build.rollupOptions

			- 用于定义底层的Rollp打包配置。

				- 接受类型

					- RollupOptions

				- 默认值

					- 无

- 开发服务器配置

	- 开发服务器配置由server对象提供。
	- 常用的开发服务器选项

		- server.proxy

			- 用于为开发服务器配置自定义代理规则。（使用http-proxy库）。

				- 接受类型（一个{key: options}的对象）

					- Record<string, string | ProxyOptions>
					- 如果key以 ^ 开头，那么将被视为正则表达式。

- 共享配置

	- root

		- 用于指定项目根目录，也就是index.html文件所在的位置。（可以是绝对路径，也可以是相对于配置文件的相对路径）。

			- 接受类型

				- string

			- 默认值

				- process.cwd()

	- base

		- 用于指定开发或生产环境的公共基础路径。（构建后所有静态资源的路径都将根据base提供的路径重写）。

			- 接受类型

				- string

			- 默认值

				- /

	- publicDir

		- 用于存放静态资源服务的文件夹，该目录在文件开发期间在根目录处，在构建后，将会复制到outDir的根目录。

