# CSS零碎知识点

### margin重叠

- 何为margin重叠
  - 边界重叠: 是指两个或多个盒子(可能相邻也可能嵌套)的相邻边界(其间没有任何非空内容、补白、边框)重合在一起而形成一个单一边界。 

- 为什么会重叠？

  - 两个或多个块级盒子的垂直相邻边界会重合。结果的边界宽度是相邻边界宽度中最大的值。如果出现负边界，则在最大的正边界中减去绝对值最大的负边界。如果没有正边界，则从零中减去绝对值最大的负边界。注意：相邻的盒子可能并非是由父子关系或同胞关系的元素生成。

  - 在规范文档中，2个或2个以上的块级盒模型相邻的垂直margin重叠的计算方法:

    1.全部都为正值，取最大者；
    2.不全是正值，则都取绝对值，然后用正值减去最大值；
    3.没有正值，则都取绝对值，然后用0减去最大值。

- 防止外边距重叠解决方案

  1.外层元素使用padding代替内层元素的margin; // 测试生效

  2.外层元素` overflow:hidden;` // 测试生效

  3.内层元素绝对定位 `postion:absolute;` // 测试生效

  4.内层元素 加`float:left;`或`display:inline-block; ` // 测试生效

  5.内层元素`padding:1px; ` // 测试未生效

  6.内层元素透明边框` border:1px solid transparent;` // 测试未生效

### 数组对象去重

- 数组去重

  - 通过Set去重，通过解构赋值将类数组转成数组

    ```js
    const arr = ['张三','张三','三张三']
    let set = [...new Set(arr)]; // set 自带去重，解构赋值将set集合转化为数组
    // 或者
    let set = Array.from([...new Set(arr)]); // set 自带去重，Array.from()将set集合转化为数组
    
    console.error(set); // [ '张三', '三张三' ]
    ```

- 数组对象去重

  - 通过reduce去重

    ```js
    let person = [
         {id: 0, name: "小明"},
         {id: 1, name: "小张"},
         {id: 2, name: "小李"},
         {id: 3, name: "小孙"},
         {id: 1, name: "小周"},
         {id: 2, name: "小陈"},   
    ];
    
    let obj = {};
    let peon = person.reduce((cur,next) => {
        obj[next.id] ? "" : obj[next.id] = true && cur.push(next);
        return cur;
    },[]) //设置cur默认类型为数组，并且初始值为空的数组
    ```

    

