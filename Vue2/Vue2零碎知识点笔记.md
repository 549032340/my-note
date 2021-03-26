# vue零碎知识点笔记

#### ▲.复杂数据类型在栈中存贮的是指针,所以赋值给新的变量也会改变原始的变量值.那么应该咋整呢?答：复杂数据类型深度克隆 -- 对象

- Object.assign
  
  - 只会对只是一级属性复制，比浅拷贝多深拷贝了一层而已,无法达到深度克隆的目的.	
  
    ``````js
    let _data =  Object.assign({},obj);
    ``````
- JSON.stringify和JSON.parse
  - 先将这个复杂数据类型同过JSON.parse(obj)转换成string类型（简单数据类型），然后再通过JSON.stringify（obj）转换成对象，从而实现复杂数据类型的深度克隆
  	 ```js
  	 let _data = JSON.parse(JSON.stringify(obj));
     ```

### ▲.`You may have an infinite update loop in a component render function.`

- 在我将iview升级到view-design之后，所有的涉及let a = b;的地方都出现了问题，原因是let a = b;已经不是单纯的克隆b的值赋给a了，本质上对a的操作还是对b进行操作（此处我还不明白是什么原因造成的），就会出现各种问题，比如此处我用a进行for循环，结果报这个错，解决办法是let a = b.slice();使用赋值函数对b进行浅层次克隆然后赋值给a,才不会影响到b。如果是复杂数据类型需要按照**复杂数据类型深度克隆**方式处理。

### ▲.watch

- 当 watch 一个变量的时候，初始化时并不会执行，添加`immediate: true,`属性初始化的时候就会触发了
- watch 还有一个容易被忽略的属性`deep`。当设置为`true`时，它会进行深度监听。简而言之就是你有一个 `const obj={a:1,b:2}`，里面任意一个 key 的 value 发生变化的时候都会触发`watch`。应用场景：比如我有一个列表，它有一堆`query`筛选项，这时候你就能`deep watch`它，只有任何一个筛序项改变的时候，就自动请求新的数据。或者你可以`deep watch`一个 form 表单，当任何一个字段内容发生变化的时候，你就帮它做自动保存等等。

### ▲. computed

-  [Vue中的computed属性](https://www.cnblogs.com/gunelark/p/8492468.html)

  1. computed用来监控自己定义的变量，该变量不在data里面声明，直接在computed里面定义，然后就可以在页面上进行双向数据绑定展示出结果或者用作其他处理；
2. computed比较适合对多个变量或者对象进行处理后返回一个结果值，也就是数多个变量中的某一个值发生了变化则我们监控的这个值也就会发生变化，举例：购物车里面的商品列表和总金额之间的关系，只要商品列表里面的商品数量发生变化，或减少或增多或删除商品，总金额都应该发生变化。这里的这个总金额使用computed属性来进行计算是最好的选择；
  3. 除了可以缓存计算属性外，它在处理传入数据和目标数据格式不一致的时候也是很有用的。

- 与watch之间的区别

  - watch一般用于监控路由、input输入框的值等简单数据类型的处理，但是如果是复杂数据类型watch 处理起来就比较麻烦了

    1. 举例
    
    ```js
    	workWelfareList: {
          handler (val) {
            this.workWelfareObject = val;
          },
          // 代表在wacth里声明了workWelfareList这个方法之后立即先去执行handler方法
          immediate: true
        }
    ```
    比如workWelfareList是一个数组对象，他是通过父组件传递过来的，然后子组件由于不能修改父组件参数的值需要copy出来一份，这个时候要是用watch监听数据过来之后复制给workWelfareObject的话需要进行深度监听。
    
    2. 但是如果用computed计算属性进行处理就没有这么麻烦了
    
     ```js
       computed: {
           workWelfareObject: {
             get () {
               return this.workWelfareObject = this.workWelfareList;
             },
             set (val) {}      
           }
         },
     ```
   workWelfareObject 不需要在data里进行声明，直接就可以在template里面引用，用computed处理
    
  	即节省性能又方便。
  	
  - watch比较适合的场景是一个数据影响多个数据 
  
-  严格模式computed需要添加set()方法，不然会报错`...was assigned to but it has no setter`

### ▲. sync修饰符

- [深入理解vue 修饰符sync【 vue sync修饰符示例】](https://www.cnblogs.com/musicbird/p/10130238.html)

### ▲. 数据类型

- 基本数据类型：**Number、String、Boolean、Null、 Undefined、Symbol（ES6）**
- 引用数据类型：**Object（在JavaScript中除了基本数据类型以外的都是对象，数据是对象，函数是对象，正则表达式是对象）**

### ▲. 自动注册全局组件

- 一些常用的组件每次都需要手动引入还是很麻烦的，放在`main.js`全局引入的话，数量少还可以接受，如果需要引入的组件很多，就会产生大量重复代码，所以，常用的组件来一个自动全局注册就很省心。

- 专门新建一个存放全局注册组件的文件夹`GlobalComponents`，然后在这个文件夹里新建一个`global.js`，通过这个`global.js`实现自动全局注册，贴代码：

  - 版本一

  ```js
  import Vue from 'vue'
  
  function changeStr(str) {
    // charAt 取字符的第一个字节 abc => Abc
    return str.charAt(0).toUpperCase() + str.slice(1);
  }
  
  const requireComponent = require.context('.', false, /\.vue$/);
  // 第一个参数：哪个目录下（当前目录下）;
  // 第二个参数：是否使用下面的子目录，如果下面新建子目录，并且里面也有组件需要使用，则改成true;
  // 第三个参数：正则表达式，用于匹配.vue的文件
  requireComponent.keys().forEach(fileName => {
    const config = requireComponent(fileName);
    const componentName = changeStr(
      fileName.replace(/^\.\//, '').replace(/\.\w+$/, '')
    )
    // console.log(componentName)
    Vue.component(componentName, config.default || config)
  });
  
  ```

  - 版本二
  ```js
  function registerAllComponents(requireContent) {
  	return requireContent.keys().forEach(comp =>{
          const vueComp = requireContent(comp)
          const compName = vueComp.name ? vueComp.name.toLowerCase() : 
      })
  }
  ```
- 最后在`main.js`中进入`global.js`即可实现在此文件夹下的组件自动全局注册。

- 当然也不要为了省事，啥组件都往全局注册，这样会让初始化页面的时候初始`init bundle`很大

### ▲.[为什么 Vue 中不要用 index 作为 key？（diff 算法详解）](https://juejin.im/post/5e8694b75188257372503722)

- 总结：用id等具有唯一性的变量作为key

### ▲. [Vue基础语法之@click、时间修饰符@click.stop与@click.prevent、按键修饰符（如@keyup.enter）](https://blog.csdn.net/wangchaohpu/article/details/84575144)

- @click.stop [阻止点击事件继续传播](https://www.cnblogs.com/limengyao/p/9267254.html)
- @click.prevent 阻止事件的默认行为
- @keyup.enter 按键修饰符

### ▲. vue组件的具名插槽 solt



### ▲. vue中使用web worker

- 第一步，安装worker-loader,用于解析web worker

  ```
  yarn add worker-loader -D
  ```

- 第二步，在`vue.config.js`中配置

  ```js
  chainWebpack: config => {
  	// worker-loader
      config.module
        .rule('worker')
        .test(/\.worker\.js$/)
        .use('worker-loader')
        .loader('worker-loader')
        .options({
          inline: 'fallback'
        });
      // worker-loader
      config.module.rule('js').exclude.add(/\.worker\.js$/);
  }
  ```

- 第三步，新建worker文件，`test.worker.js`

  ```js
  // 接收信息
  onmessage = e => {
    console.log('接收到主线程发来的消息', e.data);
    postMessage('发送消息给主线程');
  };
  
  // 错误信息
  onerror = e => {
    console.log(e.message);
  };
  ```

- 第四步，在组件中引用

  ```vue
  <template>
    <div class="test">
      <el-button @click="handlerWorker">TEST</el-button>
    </div>
  </template>
  <script>
  import Worker from './test.worker';
  export default {
    name: 'Test',
    methods: {
      handlerWorker() {
        // 创建 worker 实例
        let worker = new Worker();
  
        // 向工作线程发送消息
        worker.postMessage({
          mas: 'msg',
          test: [1, 2, 3]
        });
  
        worker.onmessage = e => {
          // 主线程接收到工作线程的消息
          console.log(e.data);
          // 关闭线程
          worker.terminate();
        };
      }
    }
  };
  
  ```


### ▲. axios的onprogress无法触发解决

- 在上传文件时要做进度显示 需要用到xhr.upload.onprogress事件，此时如果用到mock.js模拟数据的话 则无法触发onprogress事件

  - 原因：mockjs会重新声明一个XMLHttpRequest导致el-upload的progress失效

  - 解决方法:关闭mockjs模拟数据即可


### ▲. vue中首页白屏优化

- 路由懒加载，在router.js中修改
- cdn缓存图片的等 文件
- vue.config.js中关闭map文件生成，能减少好多map文件，`productionSourceMap:false`
- vue 组件尽量不要全局引入
-  使用更轻量级的工具库
-  开启gzip压缩
-  首页单独做服务端渲染
-  把不常改变的库放到 index.html 中，通过 cdn 引入



