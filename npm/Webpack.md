# [how2webpack](http://webpack.wuhaolin.cn/)

### 1.webpack是什么

- <font color=red>`webpack`</font> 是一个现代 <font color=red>`JavaScript`</font> 应用程序的静态模块打包器，当 <font color=red>`webpack`</font>处理应用程序时，会递归构建一个依赖关系图，其中包含应用程序需要的每个模块，然后将这些模块打包成一个或多个 <font color=red>`bundle`</font>，默认<font color=red>`webpack`</font>只能理解<font color=red>`JavaScript`</font>文件。

### 2.webpack核心概念

- <font color=red>`entry`</font>: 入口
- <font color=red>`output`</font>: 输出
- <font color=red>`loader`</font>: 模块转换器，用于把模块原内容按照需求转换成新内容
- <font color=red>`plugins`</font>: 扩展插件，在webpack构建流程中的特定时机注入扩展逻辑来改变构建结果或做你想要做的事情

### 3.webpack脑图



### 4.初始化一个项目

- `yarn init -y`
- `yarn add webpack webpack-cli -D`

### 5.webpack 当前版本号

### 6.开箱即用

- `npx webpack --mode=development`  // npx 会帮你执行依赖包里的二进制文件
- 默认是 `production` 模式，为了更清楚得查看打包后的代码，使用 `development` 模式

### 7.添加配置文件 

- 如果项目中有名叫`webpack.config.js`的配置文件，则打包的时候就会根据`webpack.config.js`中配置的参数进行打包编译


```js
module.exports={
    entry:{},  // 入口文件的配置项
    output:{}, // 出口文件的配置项
    devServer:{},  // 配置webpack开发服务功能
    module:{}, // 模块：例如解读CSS,图片如何转换，压缩
    plugins:[]  // 插件，用于生产模版和各项功能
}
```

### 8.配置入口出口文件

- path.resolve() 方法将路径或路径片段的序列解析为绝对路径

- <font color=red>**__dirname**</font>是`node.js`的一个全局变量，用于指向当前执行脚本（`dirname.js`）所在的目录路径，而且是绝对路径。

### 9.`mode`配置

### 10.`devServer`配置

- `yarn add webpack-dev-server -D`

- 修改 `package.json` 文件的 `scripts`

  ```js
  "scripts": {
      "dev": "webpack-dev-server",
      "build": "webpack"
  }
  ```

  

- 常用配置
  - port：指定服务端口号，默认端口号8080
  - host：指定监听
  - hot：  热更新

### devtool

|        **模式**         |                           **解释**                           |
| :---------------------: | :----------------------------------------------------------: |
|          eval           | 每个 module 会封装到 eval 里包裹起来执行，并且会在末尾追加注释 `//@ sourceURL`. |
|       source-map        |                   生成一个 SourceMap 文件.                   |
|    hidden-source-map    |      和 source-map 一样，但不会在 bundle 末尾追加注释.       |
|    inline-source-map    |           生成一个 DataUrl 形式的 SourceMap 文件.            |
|     eval-source-map     | 每个 module 会通过 eval() 来执行，并且生成一个 DataUrl 形式的 SourceMap . |
|    cheap-source-map     | 生成一个没有列信息（column-mappings）的 SourceMaps 文件，不包含 loader 的 sourcemap（譬如 babel 的 sourcemap） |
| cheap-module-source-map | 生成一个没有列信息（column-mappings）的 SourceMaps 文件，同时 loader 的 sourcemap 也被简化为只包含对应行的。 |

- [devtool里的7种SourceMap模式是什么鬼？](<https://juejin.im/post/58293502a0bb9f005767ba2f>)

### 11.常用的loader

- `babel-loader`  将`JS`转义为低版本

  - `yarn add babel-loader -D`

  -  `yarn add @babel/core @babel/preset-env @babel/plugin-transform-runtime -D`

  - `yarn add @babel/runtime @babel/runtime-corejs3`

- ### 处理样式文件

  - `yarn add style-loader less-loader css-loader postcss-loader autoprefixer less -D`
  - 修改配置文件

- ### 图片/字体文件处理

  - `yarn add url-loader -D` // 可以通过limit属性对图片分情况处理，当图片小于limit（单位：byte）大小时转base64，大于limit时调用file-loader对图片进行处理。
  - `yarn add file-loader -D` // 返回的是图片的url
  - `yarn add html-withimg-loader -D` //处理 html 中的本地图片
  - 修改配置文件

### 12.常用的plugin

- 生成html
  - `yarn add html-webpack-plugin -D`
  - 修改配置文件
    - 打包后的文件名
- 打包前清空dist目录
  - `yarn add clean-webpack-plugin -D `
  - 修改配置文件
    - 设置某些文件不被清空



### 13.resolve

- `alias` 配置项通过别名来把原导入路径映射成一个新的导入路径;

- `extensions` 在导入语句没带文件后缀时，Webpack 会自动带上后缀后去尝试访问文件是否存在;




### `webpack`常用变量

- <font color=red>**__dirname**</font>是`node.js`的一个全局变量，用于指向当前执行脚本（`dirname.js`）所在的目录路径，而且是绝对路径。
- output里面的<font color=red>**publicPath**</font>表示的是打包生成的index.html文件里面引用资源的前缀；
- `devServer`里面的<font color=red>**publicPath**</font>表示的是打包生成的静态文件所在的位置（若是`devServer`里面的`publicPath`没有设置，则会认为是output里面设置的`publicPath`的值）；
- [webpack中的path、publicPath、contentBase的区分](<https://blog.csdn.net/u012193330/article/details/83310924>)
- ...

### loader和plugin的区别

- `webpack `<font color=red>loader</font> 是用来加载文件的，`webpack`<font color=red> `plugin` </font>是用来扩展功能的
- <font color=red>loader</font> 主要是用来加载一个个文件的，比如它可以加载js文件，并把js文件转译成低版本浏览器可以支持的js文件；也可以用来加载css文件，可以把css文件变成页面上的style标签；还可以加载图片文件，还可以对文件进行优化
- <font color=red>plugin</font> 是用来加强webpack功能的，比如HTML webpack plugin 是用来生成一个html文件的；再比如mini css extract plugin 是用来抽取css代码并把它变成一个css文件的

### 观摩cli2.0下的webpack实际项目

- 利用`nodeJs`和`html-webpack-plugin`在打包的时候输出打包版本信息

  - [Javascript中<%=%>是客户端代码与服务器代码混合使用方式](https://www.cnblogs.com/iceflowerly/p/5149054.html)





### webpack --> vite

- **`webpack `**打包的时候会有两个阶段: 编译和打包，但打包之后会有一个问题，就是随着模块的增多，会造成打出的 bundle 体积过大，进而会造成热更新速度明显拖慢。
- **`vite`** 目前仅仅只是针对 **`Vue`** 项目**开发阶段**的工具
- **`vite`**重度依赖**`module sciprt`**的特性，**`module sciprt`**允许在浏览器中直接运行原生支持模块

- [是什么尤大选择放弃Webpack？——vite 原理解析](<https://mp.weixin.qq.com/s?__biz=MzU0MTU4OTU2MA==&mid=2247484097&idx=1&sn=670c116382134164afa71df402a8c36f&chksm=fb26eb96cc5162805e701db96072d71e765ae8680f529a1e2e93c80d1c396b289452671ea4da&scene=126&sessionid=1589340723&key=caf03409094d441cf4bf829be951e1030c94ad718c8c3be50b15b826504e6076edd808b4e0d06e3df89186758ca9110dae0f6100ca12a78e07eab56a226c3499185459a8c6fb416f486adb70c375461c&ascene=1&uin=MTMzODgyOTczMg%3D%3D&devicetype=Windows+10+x64&version=62090070&lang=zh_CN&exportkey=A8UOwR2gXZAyP3k6Yg6VnLk%3D&pass_ticket=JjmQFr3%2BKI9J526PmML9IibIoDnQJ7Wx%2BzLbA6rSCqUS%2Bw4T7A%2F7w7Gdr7e3vMBf>)



### ▲.webpack优化

#### [构建速度优化](https://www.jianshu.com/p/4f58b179c626)

#### 优化 Loader

##### 对于 Loader 来说，影响打包效率首当其冲必属 Babel 了。因为 Babel 会将代码转为字符串生成 AST，然后对 AST 继续进行转变最后再生成新的代码，项目越大，**转换代码越多，效率就越低**。

- 优化 Loader 的文件搜索范围

  ```js
  module.exports = {
    module: {
      rules: [
        {
          // js 文件才使用 babel
          test: /\.js$/,
          loader: 'babel-loader',
          // 只在 src 文件夹下查找
          include: [resolve('src')],
          // 不会去查找的路径
          exclude: /node_modules/
        }
      ]
    }
  }
  ```
#### HappyPack

##### 受限于 Node 是单线程运行的，所以 Webpack 在打包的过程中也是单线程的，特别是在执行 Loader 的时候，长时间编译的任务很多，这样就会导致等待的情况。

- **HappyPack 可以将 Loader 的同步执行转换为并行的**，这样就能充分利用系统资源来加快打包效率了

  ```js
  module: {
    loaders: [
      {
        test: /\.js$/,
        include: [resolve('src')],
        exclude: /node_modules/,
        // id 后面的内容对应下面
        loader: 'happypack/loader?id=happybabel'
      }
    ]
  },
  plugins: [
    new HappyPack({
      id: 'happybabel',
      loaders: ['babel-loader?cacheDirectory'],
      // 开启 4 个线程
      threads: 4
    })
  ]
  ```


#### DllPlugin

##### **DllPlugin 可以将特定的类库提前打包然后引入**。这种方式可以极大的减少打包类库的次数，只有当类库更新版本才有需要重新打包，并且也实现了将公共代码抽离成单独文件的优化方案。

- 如何使用

```js
// 单独配置在一个文件中
// webpack.dll.conf.js
const path = require('path')
const webpack = require('webpack')
module.exports = {
  entry: {
    // 想统一打包的类库
    vendor: ['react']
  },
  output: {
    path: path.join(__dirname, 'dist'),
    filename: '[name].dll.js',
    library: '[name]-[hash]'
  },
  plugins: [
    new webpack.DllPlugin({
      // name 必须和 output.library 一致
      name: '[name]-[hash]',
      // 该属性需要与 DllReferencePlugin 中一致
      context: __dirname,
      path: path.join(__dirname, 'dist', '[name]-manifest.json')
    })
  ]
}
```

- 然后我们需要执行这个配置文件生成依赖文件，接下来我们需要使用 `DllReferencePlugin` 将依赖文件引入项目中

```js
// webpack.conf.js
module.exports = {
  // ...省略其他配置
  plugins: [
    new webpack.DllReferencePlugin({
      context: __dirname,
      // manifest 就是之前打包出来的 json 文件
      manifest: require('./dist/vendor-manifest.json'),
    })
  ]
}
```



#### 代码压缩

##### 在 Webpack3 中，我们一般使用 `UglifyJS` 来压缩代码，但是这个是单线程运行的，为了加快效率，我们可以使用 `webpack-parallel-uglify-plugin` 来并行运行 `UglifyJS`，从而提高效率。

##### 在 Webpack4 中，我们就不需要以上这些操作了，只需要将 `mode` 设置为 `production` 就可以默认开启以上功能。代码压缩也是我们必做的性能优化方案，当然我们不止可以压缩 JS 代码，还可以压缩 HTML、CSS 代码，并且在压缩 JS 代码的过程中，我们还可以通过配置实现比如删除 `console.log` 这类代码的功能。



#### 一些小的优化点

##### 我们还可以通过一些小的优化点来加快打包速度

- `resolve.extensions`：用来表明文件后缀列表，默认查找顺序是 `['.js', '.json']`，如果你的导入文件没有添加后缀就会按照这个顺序查找文件。我们应该尽可能减少后缀列表长度，然后将出现频率高的后缀排在前面
- `resolve.alias`：可以通过别名的方式来映射一个路径，能让 Webpack 更快找到路径
- `module.noParse`：如果你确定一个文件下没有其他依赖，就可以使用该属性让 Webpack 不扫描该文件，这种方式对于大型的类库很有帮助



#### 减少 Webpack 打包后的文件体积

##### 按需加载

- 在给单页应用做按需加载优化时，一般采用以下原则：
  - 把整个网站划分成一个个小功能，再按照每个功能的相关程度把它们分成几类。
  - 把每一类合并为一个 Chunk，按需加载对应的 Chunk。
  - 对于用户首次打开你的网站时需要看到的画面所对应的功能，不要对它们做按需加载，而是放到执行入口所在的 Chunk 中，以降低用户能感知的网页加载时间。
  - 对于个别依赖大量代码的功能点，例如依赖 Chart.js 去画图表、依赖 flv.js 去播放视频的功能点，可再对其进行按需加载。
- 举个例子，现在需要做这样一个进行了按需加载优化的网页：
  - 网页首次加载时只加载 `main.js` 文件，网页会展示一个按钮，`main.js` 文件中只包含监听按钮事件和加载按需加载的代码。
  - 当按钮被点击时才去加载被分割出去的 `show.js` 文件，加载成功后再执行 `show.js` 里的函数。

##### 提取公共代码

-  [CommonsChunkPlugin](http://webpack.wuhaolin.cn/4%E4%BC%98%E5%8C%96/4-11%E6%8F%90%E5%8F%96%E5%85%AC%E5%85%B1%E4%BB%A3%E7%A0%81.html)







