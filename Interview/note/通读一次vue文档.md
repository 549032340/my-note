# 通读一次vue2.x文档

##### Vue是不是MVVM模型

- Vue没有完全遵循MVVM模型，但是Vue的设计是有借鉴MVVM的，比如Vue中常使用变量vm（ViewModel的缩写）代表Vue的实例。

##### data

- 当Vue实例被创建时存在于data中的property才是响应式的。
  - 如果在实例创建之后再添加一个新的property`vm.b='hi';`，那么b的改动将不会触发任何视图的更新。**所以**，如果需要在实例创建之后使用一个property，但是一开始为空或者不存在，还是需要在data中设置一个初始值，比如：`vm.b='';`。
  - 唯一的例外是使用 `Object.freeze()`，这会阻止修改现有的 property，也意味着响应系统无法再*追踪*变化。

##### 生命周期

