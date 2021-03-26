# JavaScript 零碎知识点笔记

### Promise

```js
// 实战代码
    function getFormUrl() {
        return new Promise(function (resolve, reject) {
            //获取电子表单的地址
            $.ajax({
                type: 'post',
                url: form + 'getWebsite',
                dataType: 'JSON',
                contentType: "application/json",
                async: false,
                data: JSON.stringify({
                    matterId: window.taskCode,
                }),
                success: function (res) {
                    res.code == 200 && res.result ? resolve(res.result) : resolve('noForm');
                },
                error: function (res) {
                    reject('noForm');
                }
            })
        })

    }

    function getMaterial() {
        return new Promise(function (resolve, reject) {
            //获取电子表单的地址
            $.ajax({
                type: 'post',
                url: serverUrl + 'findMaterialInfoForConnectivity',
                dataType: 'json',
                async: false,
                contentType: "application/json;charset=utf-8",
                data: JSON.stringify({
                    implementId: window.taskCode,
                    handleId: "",
                    userId: sessionStorage.getItem('userId')
                }),
                success: function (res) {
                    if (res.code == 200) {
                        let arr = Object.keys(res.result.data);
                        console.log(res.result.data,'res.result.data');
                        console.log(arr,arr);
                        arr.length > 0 ? resolve('true') : resolve('未获取到电子材料数据');
                    } else {
                        reject('未获取到电子材料数据');
                    }
                },
                error: function (res) {
                    reject('未获取到电子材料数据');
                }
            })
        })

    }
    // 获取电子表单和材料数据，判断导航栏需要加载的导航数据
    function getFormAndMeratial() {
        Promise.all([getFormUrl(), getMaterial()])
            .then(function (results) {
                console.log(results);
                let formUrl = results[0];
                let material = results[1];
				...
            })
    }
```

### window.onresize 与 window.addEventListener(‘resize‘,...) 区别

- 问题：Vue 中，两个组件都使用的了 window.onresize 导致其中一个被覆盖。
- 解决：使用window.addEventListener()

### [es6的class继承和es5的继承有啥区别？](https://www.cnblogs.com/samsara-yx/p/14420660.html)

##### ES6 的class可以看作只是一个ES5生成实例对象的构造函数的语法糖。它参考了java语言，定义了一个类的概念，让对象原型写法更加清晰，对象实例化更像是一种面向对象编程。Class类可以通过extends实现继承。

##### 不同点：

- 类的内部定义的所有方法，都是不可枚举的。
- ES6的class类必须用new命令操作，而ES5的构造函数不用new也可以执行。
- ES6的class类不存在变量提升，必须先定义class之后才能实例化，不像ES5中可以将构造函数写在实例化之后。
- ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面
- ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。



### [js 五种绑定彻底弄懂this，默认绑定、隐式绑定、显式绑定、new绑定、箭头函数绑定详解](https://www.cnblogs.com/echolun/p/11962610.html)

##### this默认绑定

- 函数调用时无任何调用前缀，非严格模式下指向window,严格模式下指向undefined

##### this隐式绑定

- 如果函数调用时，前面存在调用它的对象，那么this就会隐式绑定到这个对象上
- 如果函数调用前存在多个对象，this指向距离调用自己最近的对象
- 隐式丢失：作为参数传递以及变量赋值

##### this显式绑定

- 显式绑定是指我们通过call、apply以及bind方法改变this的行为，相比隐式绑定，我们能清楚的感知 this 指向变化过程

  ```js
  let obj1 = {
      name: '听风是风'
  };
  let obj2 = {
      name: '时间跳跃'
  };
  let obj3 = {
      name: 'echo'
  }
  var name = '行星飞行';
  
  function fn() {
      console.log(this.name);
  };
  fn(); //行星飞行
  fn.call(obj1); //听风是风
  fn.apply(obj2); //时间跳跃
  fn.bind(obj3)(); //echo
  ```

- 如果在使用call之类的方法改变this指向时，指向参数提供的是null或者undefined，那么 this 将指向全局对象。(非严格模式下)

  ```js
  let obj1 = {
      name: '听风是风'
  };
  let obj2 = {
      name: '时间跳跃'
  };
  var name = '行星飞行';
  
  function fn() {
      console.log(this.name);
  };
  fn.call(undefined); //行星飞行
  fn.apply(null); //行星飞行
  fn.bind(undefined)(); //行星飞行
  ```

- call、apply与bind有什么区别？

  1.call、apply与bind都用于改变this绑定，但call、apply在改变this指向的同时还会执行函数，而bind在改变this后是返回一个全新的boundFcuntion绑定函数，这也是为什么上方例子中bind后还加了一对括号 ()的原因。

  2.bind属于**硬绑定**，返回的 boundFunction 的 this 指向无法再次通过bind、apply或 call 修改；call与apply的绑定只适用当前调用，调用完就没了，下次要用还得再次绑。

  3.call与apply功能完全相同，唯一不同的是call方法传递函数调用形参是以散列形式，而apply方法的形参是一个数组。在传参的情况下，call的性能要高于apply，因为apply在执行时还要多一步解析数组。

##### new绑定

- new一个函数究竟发生了什么呢，大致分为三步:

  1.以构造器的prototype属性为原型，创建新对象；

​       2.将this(可以理解为上句创建的新对象)和调用参数传给构造器，执行；

​       3.如果构造器没有手动返回对象，则返回第一步创建的对象;

##### this绑定优先级

- 显式绑定 > 隐式绑定 > 默认绑定

- new绑定 > 隐式绑定 > 默认绑定
- 为什么显式绑定不和new绑定比较呢？因为不存在这种绑定同时生效的情景，如果同时写这两种代码会直接抛错

### 事件处理程序在冒泡阶段执行（除非将 `useCapture` 设置为 `true`）。它从嵌套最深的元素向外传播。

```js
当您单击该段落时，日志输出是什么？答案： p div
<div onclick="console.log('div')">
  <p onclick="console.log('p')">
    Click here!
  </p>
</div>
```



### 导致事件的最深嵌套的元素是事件的 target。

```
当点击按钮时，event.target是什么？答案：button
<div onclick="console.log('first div')">
  <div onclick="console.log('second div')">
    <button onclick="console.log('button')">
      Click!
    </button>
  </div>
</div>
```



### 类的静态方法不能被类的实例使用



### call、apply、bind

##### call

- call 方法第一个参数是要绑定给this的值，后面传入的是一个参数列表。当第一个参数为null、undefined的时候，默认指向window。

##### apply

- apply接受两个参数，第一个参数是要绑定给this的值，第二个参数是一个参数数组。当第一个参数为null、undefined的时候，默认指向window。

##### bind

- 和call很相似，第一个参数是this的指向，从第二个参数开始是接收的参数列表。区别在于bind方法返回值是函数以及bind接收的参数列表的使用。

##### 区别

- apply 和 call 的用法几乎相同, 唯一的差别在于：当函数需要传递多个变量时, apply 可以接受一个数组作为参数输入, call 则是接受一系列的单独变量。

- 关于apply和call的使用区别，如果你的参数本来就存在一个数组中，那自然就用 apply，如果参数比较散乱相互之间没什么关联，就用 call。
- apply和call返回的是函数立即执行，bind方法不会立即执行，而是返回一个改变了上下文 this 后的函数。

##### 总结

- bind返回对应函数, 便于稍后调用； apply, call则是立即调用。
- 除此外, 在 ES6 的箭头函数下, call 和 apply 将失效, 对于箭头函数来说:
  - 箭头函数体内的 this 对象, 就是定义时所在的对象, 而不是使用时所在的对象;所以不需要类似于`var _this = this`这种丑陋的写法
  - 箭头函数不可以当作构造函数，也就是说不可以使用 new 命令, 否则会抛出一个错误
  - 箭头函数不可以使用 arguments 对象,，该对象在函数体内不存在. 如果要用, 可以用 Rest 参数代替
  - 不可以使用 yield 命令, 因此箭头函数不能用作 Generator 函数

### 立即执行函数表达式（IIFE）

- 在JavaScript里，**圆括号不能包含声明**，当圆括号为了包裹函数碰上了 `function`关键词，它便知道将它作为一个函数表达式去解析而不是函数声明。

- 如果你并不关心返回值，或者让你的代码尽可能的易读，你可以通过在你的函数前面带上一个一元操作符来存储字节

  ```js
  !function(){/* code */}();
  ~function(){/* code */}();
  -function(){/* code */}();
  +function(){/* code */}();
  ```



### [].slice.call()

##### [].slice.call() 常用来将类数组转化为真正的数组

- `[].slice() === Array.prototype.slice(); // true`

- `Array.prototype.slice.call()`能将具有length属性的对象转成数组，除了IE下的节点集合（因为ie下的dom对象是以com对象的形式实现的，js对象与com对象不能进行转换）

  ```js
   function test(a,b,c,d) 
     { 
        var arg = Array.prototype.slice.call(arguments,1); 
        alert(arg); 
     } 
     test("a","b","c","d"); //b,c,d
  ```


### 0.1+0.2>0.3

##### 使用Number.EPSILON进行判断

- Number.EPSILON === 2^-52（这个值非常非常小，在底层计算机已经帮我们运算好，并且无限接近0，但不等于0。）

- 只要判断(0.1+0.2)-0.3小于 Number.EPSILON，在这个误差的范围内就可以判定0.1+0.2===0.3为true。

  ```js
  Number.EPSILON=(function(){   //解决兼容性问题
      return Number.EPSILON?Number.EPSILON:Math.pow(2,-52);
  })();
  //上面是一个自调用函数，当JS文件刚加载到内存中，就会去判断并返回一个结果，相比下面这种方式，这种代码更节约性能，也更美观。
  //if(!Number.EPSILON){
  //   Number.EPSILON=Math.pow(2,-52);
  //}
  function numbersequal(a,b){ 
      return Math.abs(a-b)<Number.EPSILON;
    }
  //接下来再判断   
  var a=0.1+0.2, b=0.3;
  console.log(numbersequal(a,b)); //这里就为true了
  ```


### 闭包

##### 特点

- 可以让外部访问函数内部的变量
- 局部变量会常驻在内存中
- 可以避免使用全局变量，防止全局变量被污染
- 这个变量会一直被引用，不会被删除，一直占用内存空间，换种方式描述就是在垃圾回收机制中一直被标记，无法被释放，也就是所谓的内存泄露。

