# js零碎知识点笔记
## [js常用的数组操作内置函数](https://www.cnblogs.com/hellozg/p/7193648.html)

`let arr = [];`

#### 向数组中添加值

- 在数组尾部添加值
  - `arr.push(XX);`
- 在数组开头添加数据
  - `arr.unshit(XX)`

#### 删除数组中的值

- 删除数组中末尾的值 返回删除的值

  - `arr.pop();`

- 删除数组中开头的值 返回删除的值 与pop()相反

  - `arr.shift();`

- 删除数组中的某个值

  - `arr.remove(XX);`


#### 数组转换字符串

- 间隔的字符串，默认为“，”
  - `arr.join('-');`

#### 数组排序

- 反转排序
  - `arr.reverse();`
- 转换字符串,再按照字典顺序进行比较之后排序
  - `arr.sort();`
- 升序
  - `arr.sort(function(a,b){return a-b});`
- 降序
  - `arr.sort(function(a,b){return b-a});`

#### 连接数组

- `console.log(arr);//输出 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11]`

```js
let arr1=[1,2,3];
let arr2=[4,5,6];
let arr3=[7,8,9];
let arr=arr1.concat(arr2,arr3,[10,11]);
```

#### splice()

- 删除

  ```js
  var arr=new Array(1,2,3,4,5);
  var arr2=arr.splice(1,2);//删除开始下标为1的值（2）开始的2个值 （2和3） 返回删除的值
  console.log(arr,arr2);//输出 [1, 4, 5] [2, 3]
  ```

- 插入

  ```js
  var arr=new Array(1,2,3,4,5);
  var arr2=arr.splice(1,0,11,111);//在下标为1的值（2）之前插入值 ，参数第二个为0不删除，插入11,111
  console.log(arr,arr2);// [1, 11, 111, 2, 3, 4, 5] [] arr2返回为空数组 因为不删除
  ```

- 替换

  ```js
  var arr=new Array(1,2,3,4,5);
  var arr2=arr.splice(1,2,11,111);//在下标为1的值（2）之前替换值 ，参数第二个为2删除2个值，再插入11,111
  console.log(arr,arr2);// [1, 11, 111, 4, 5] [2, 3]   //也就是 先删除再添加
  ```

- 已有数组中返回选定元素

  ```js
  var arr=new Array(1,2,3,4,5);
  var arr2=arr.slice(2);
  //第一个参数 start 开始选取的index 下标值 2为第三个值（0,1,2） 数字3开始 end 为可选 默认为到数组的末尾
  console.log(arr2);//输出 [3, 4, 5]
  //注意 end 参数为 该参数是数组片断结束处的数组下标 4 为 数字5的下标 截取5之前，也可以理成截取到end-1
  var arr2=arr.slice(2,4);
  console.log(arr2);//输出 [3, 4]  不是[3,4,5]
  ```

  

- 截取开始为负数

  ```js
  var arr2=arr.slice(-2,4);
  //如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。
  // 也可以转换成 数组长度（5）加 负数的值 （-2） 相当于arr.slice(3,4);
  console.log(arr2);//输出 [4]
  ```

## 编码与解码
### 文字编码与解码
- encodeURIComponent() 函数可把字符串作为 URI 组件进行编码
- decodeURIComponent() 函数可对 encodeURIComponent() 函数编码的 URI 进行解码
- [escape、encodeURI和encodeURIComponent的区别](https://www.cnblogs.com/qlqwjy/p/9934706.html)

