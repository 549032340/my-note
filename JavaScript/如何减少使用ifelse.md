# 如何减少使用ifelse

### 参考博文

- [这些优化技巧可以避免我们在 JS 中过多的使用 IF 语句](<https://juejin.im/post/5e964e616fb9a03c320bb74f>)
- [虾扯蛋之条件判断的极致优化](<https://juejin.im/post/5c0a3c2de51d4540da7f1c93>)

- [在JS开发中，如何减少if...else...循环嵌套](jianshu.com/p/ea22123d4f62)

### 总结

##### ▲最基本的 || 运算符

```js
if(a为真){
     a=a
 }else{
     a=b
 }
// 重构
a = a || b
```

##### ▲三元运算符

```js
if(a==b){
     a=c
 }else{
     a=d
 }
 // 重构
 a = (a==b)?c:d
```

##### ▲后台返回的数据通常会使这样的`task: 0 // 0=紧急，1=日常，2=临时`

```js
var _task = ['紧急','日常','临时']
task_type = _task[task]
```

##### ▲使用Array的方法或者Map等数据结构

- 单一水果颜色

```js
function test(fruit){
      if(fruit == 'apple' || fruit == 'strawberry'){
          console.log('red');
      }
  }
// 重构
function test(fruit){
    const redFruit = ['apple','strawberry','cherry','cranberry'];
    if(redFruit.includes(fruit)){
        console.log('red');
    }
}
```

- 如果有多个水果颜色 ：`map`登场

```js
const fruitColor1 = new Map();
fruitColor1.set('red',['apple','strawberry']);
fruitColor1.set('yellow',['banana','pineapple']);
fruitColor1.set('purple',['grape','plum']);
function test(color){
    return fruitColor1.get(color)||[];
}
test('yellow')// ["banana", "pineapple"]
```

- 简单来讲，Map类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```js
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
```

- 在上面我们可以看到Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

```js
const m = new Map();
m.set('title','zty');
m.set('content','very good');
m.get('title');//zty
m.get('content');//very good
```

##### ▲函数委托

```js
function itemDropped(item, location) {
    if (!item) {
        return false;
    } else if (outOfBounds(location) {
        var error = outOfBounds;
        server.notify(item, error);
        items.resetAll();
        return false;
    } else {
        animateCanvas();
        server.notify(item, location);
        return true;
    }
}
// 重构
function itemDropped(item, location) {
    const dropOut = function() {
        server.notify(item, outOfBounds);
        items.resetAll();
        return false;
    }

    const dropIn = function() {
        server.notify(item, location);
        animateCanvas();
        return true;
    }

    return !!item && (outOfBounds(location) ? dropOut() : dropIn());
}
```

##### ▲非分支策略

- 此技巧尝试避免使用`switch`语句，相反是用键/值创建一个映射并使用一个函数访问作为参数传递的键的值。

```js
switch(breed){
    case 'border':
      return 'Border Collies are good boys and girls.';
      break;  
    case 'pitbull':
      return 'Pit Bulls are good boys and girls.';
      break;  
    case 'german':
      return 'German Shepherds are good boys and girls.';
      break;
    default:
      return 'Im default';
}
// 重构
const dogSwitch = (breed) =>({
  "border": "Border Collies are good boys and girls.",
  "pitbull": "Pit Bulls are good boys and girls.",
  "german": "German Shepherds are good boys and girls.",  
})[breed]||'Im the default';
dogSwitch("border xxx");

// 实战优化
const doSwitch = (breed) => ({
    "1": "申请人承诺后，部门即给予办理，无需再提交证明",
    "2": "申请人承诺后，通过部门自行核查，无需申请人提交证明材料",
    "3": "申请人承诺后，需要在60工作日内补齐证明材料",
})[breed] || '无';
let promiseForm = doSwitch(item.promiseForm);
```

##### ▲作为数据的函数

- 把代码分割成一个函数对象

```js
const calc = {
    run: function(op, n1, n2) {
        const result;
        if (op == "add") {
            result = n1 + n2;
        } else if (op == "sub" ) {
            result = n1 - n2;
        } else if (op == "mult" ) {
            result = n1 * n2;
        } else if (op == "div" ) {
            result = n1 / n2;
        }
        return result;
    }
}

calc.run("sub", 5, 3); //2

// 重构
const calc = {
    add : function(a,b) {
        return a + b;
    },
    sub : function(a,b) {
        return a - b;
    },
    mult : function(a,b) {
        return a * b;
    },
    div : function(a,b) {
        return a / b;
    },
    run: function(fn, a, b) {
        return fn && fn(a,b);
    }
}

calc.run(calc.mult, 7, 4); //28
```



##### ▲短路运算符

```js
const isOnline = true;
const makeReservation= ()=>{};
const user = {
    name:'Damian',
    age:32,
    dni:33295000
};

if (isOnline){
    makeReservation(user);
}
// 重构
const isOnline = true;
const makeReservation= ()=>{};
const user = {
    name:'Damian',
    age:32,
    dni:33295000
};

isOnline&&makeReservation(user);
```

