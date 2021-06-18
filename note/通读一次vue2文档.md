# 通读一次vue2.x文档

##### Vue是不是MVVM模型

- Vue没有完全遵循MVVM模型，但是Vue的设计是有借鉴MVVM的，比如Vue中常使用变量vm（ViewModel的缩写）代表Vue的实例。

##### data

- 当Vue实例被创建时存在于data中的property才是响应式的。
  - 如果在实例创建之后再添加一个新的property`vm.b='hi';`，那么b的改动将不会触发任何视图的更新。**所以**，如果需要在实例创建之后使用一个property，但是一开始为空或者不存在，还是需要在data中设置一个初始值，比如：`vm.b='';`。
  - 唯一的例外是使用 `Object.freeze()`，这会阻止修改现有的 property，也意味着响应系统无法再*追踪*变化。

##### 生命周期

##### computed

- 计算属性是基于它们的响应式依赖进行缓存的，只有在相关响应式依赖发生改变的时候才会重新求值。

##### watch

##### v-if

- 用`key`管理可复用元素

##### v-for

- v-if和v-for不可同时使用，原因：v-for比v-if优先级更高

- v-for可以遍历对象

  ```vue
  <ul id="v-for-object" class="demo">
    <li v-for="(value,key,index) in object">
      {{ index }} : {{ key }} : {{ value }}
    </li>
  </ul>
  ```

  ```vue
  new Vue({
    el: '#v-for-object',
    data: {
      object: {
        title: 'How to do lists in Vue',
        author: 'Jane Doe',
        publishedAt: '2016-04-10'
      }
    }
  })
  
  ```

##### 按键修饰符

- 按键修饰符

- 使用`v-on:keyup[按键名称或者code]='XX'`实现按键的监听

- 如果是监听封装的组件，比如element，这是需要在按键修饰符上添加.native。

  ```vue
  <el-input class="filter-item" v-model="phone" @keyup.enter.native="searchPhone"/>
  ```

##### 系统修饰键

- - `.ctrl`

  - `.alt`

  - `.shift`

  - `.meta`

    ```vue
    <!-- Alt + C -->
    <input v-on:keyup.alt.67="clear">
    
    <!-- Ctrl + Click -->
    <div v-on:click.ctrl="doSomething">Do something</div>
    ```
  
- `.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

##### 鼠标按钮修饰符

- `.left`
- `.right`
- `.middle`

#####  v-model 修饰符

- `.lazy`
- `.number`
- `.trim`

##### 组件

- **一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝

- 组件名大小写

  - 当使用 kebab-case (短横线分隔命名) 定义一个组件时，你也必须在引用这个自定义元素时使用 kebab-case，例如 `<my-component-name>`
  - 当使用 PascalCase (首字母大写命名) 定义一个组件时，你在引用这个自定义元素时两种命名法都可以使用。也就是说 `<my-component-name>` 和 `<MyComponentName>` 都是可接受的。注意，尽管如此，直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。

- 组件的自动化全局注册

  - 使用了webpack的`require.context`获取组件名称列表，然后统一循环注册到vue中

- 组件传参校验

  ```js
  props:{
  	// 自定义验证函数
    	// 当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。 
      propF: {
        validator: function (value) {
          // 这个值必须匹配下列字符串中的一个
          return ['success', 'warning', 'danger'].indexOf(value) !== -1
        }
      }
  }
  ```

- 组件上添加非Prop的Attribute

  - 比如给组件添加一个class类名

  - 对于绝大多数 attribute 来说，从外部提供给组件的值会替换掉组件内部设置好的值。

  - `class` 和 `style` attribute 会稍微智能一些，即两边的值会被合并起来

  - 如果你不希望组件的根元素继承 attribute，你可以在组件的选项中设置 `inheritAttrs: false`

    ```js
    Vue.component('my-component', {
      inheritAttrs: false,
      // ...
    })
    ```

    - `inheritAttrs: false` 选项不会影响 `style` 和 `class` 的绑定。

- 组件的自定义事件名称

    - `v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。
    - 推荐**始终使用 kebab-case 的事件名**
    
- 将原生事件绑定到组件 - `$listeners`

- 动态组件

- 异步组件

##### 插槽

- 具名插槽及缩写
- 插槽 prop及解构

##### 组件通信

- props

- provide/inject

  ```js
  // 发送数据的组件
  provide: function () {
    return {
      getMap: 'test'
    }
  }
  
  // 接受数据的组件
  injiect:['getMap']
  ```

  

##### 强制更新组件

- 通过改变v-for中的:key
- this.$forceUpdate

##### 进入/离开 & 列表过渡

- 封装组件 transition
- property: name(模式)
- 过渡的类名
  - v-enter
  - v-enter-active
  - v-enter-to
  - v-leave
  - v-leave-active
  - v-leave-to

