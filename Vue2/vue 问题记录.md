### 由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
- 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
  - 可以使用vm.items.splice(indexOfItem,1,newValue)进行修改，此时vue即可检测到数据变动
- 当你修改数组的长度时，例如：vm.items.length = newLength
  - 对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。但是，可以使用 Vue.set(object, key, value)方法向嵌套对象添加响应式属性。Vue.set(vm.items, 'length', newLength)

### 公司基础模板报module.exports抛出的变量引用错误，但是用法是正确的，正好一起总结一下require和import的区别
- require是 AMD规范引入方式
- require是运行时调用，所以require理论上可以运用在代码的任何地方
```
某页面的引用
const module = require('module')

抛出页面
// module.js
function test(str) {
  console.log(str); 
}
module.exports = {
 test
}
```
- import是es6的一个语法标准，如果要兼容浏览器的话必须转化成es5的语法
- import是编译时调用，所以必须放在文件开头
```
// 引用
import fs from 'fs'
import {default as fs} from 'fs'
import * as fs from 'fs'
import {readFile} from 'fs'
import {readFile as read} from 'fs'
import fs, {readFile} from 'fs'

// 抛出
export default fs
export const fs
export function readFile
export {readFile, read}
export * from 'fs'
```
- 然而此次引用是正确的，那么为什么还会引用错误呢？
- 错误原因：vue项目的 文件中 import 和module.exports 不能混用