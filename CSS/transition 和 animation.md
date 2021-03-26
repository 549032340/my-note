# transition 和 animation
### [Vue 2.0学习笔记：Vue的transition](https://www.codercto.com/a/44499.html)

### transition元素

- 在Vue中有一个 `<transition>` 元素（即一个容器），它主要用来处理元素或组件上的 `transition` 动效，CSS和 [JavaScript](http://www.codercto.com/category/javascript.html) 的 `animation` 动效，而且会让你处理这些动效变得简地多。而在CSS的 `transition` 动效中， `<transition>` 元素主要负责应用和取消类（元素的类名）。而你所要做的就是定义元素在 `transition` 动效期间元素的样式。

- vue中动画效果都是在`<transition >`中实现的，`<transition >`的动画执行完毕之后就会移除，`animation-fill-mode:forwards`属性是保持动画的最后一帧，在此失效。**需要给`<transition >`设置一个时间，然后再设置`animation-fill-mode:forwards`**。代码：

  ```css
  .font-enter-active {
      transition: all 3.2s;
      animation: 0.6s title ease-in-out;
      animation-delay: 1.4s;
      animation-fill-mode: forwards;
  }
  ```

  之所以强调这一点是因为我之前已经这样设置过，但是并没有生效，动画结束后还是一样被移除了，罪魁祸首是我在`<transition>`中设置了`type="animation`导致了`<transition >`设置的时间没有生效。代码：```<transition name='font' type='animation'>```

