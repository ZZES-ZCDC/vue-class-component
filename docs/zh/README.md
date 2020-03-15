# 总览

Vue Class Component 是一个可以让你使用Class风格语法编写Vue组件的库. 例如, 下面是一个通过Vue Class Component编写的简单的计数器组件的例子:

```vue
<template>
  <div>
    <button v-on:click="decrement">-</button>
    {{ count }}
    <button v-on:click="increment">+</button>
  </div>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

// 使用Class风格定义组件
@Component
export default class Counter extends Vue {
  // Class的属性将是组件的data
  count = 0

  // Methods will be component methods
  increment() {
    this.count++
  }

  decrement() {
    this.count--
  }
}
</script>
```

正如例子展现的, 你可以使用通过`@Component`装饰器标注Class, 来用直观和标准的Class语法定义组件的data和方法. 你可以简单地使用Class风格的组件代替组件定义, 因为它等价于普通的使用对象定义的组件.

通过使用Class风格定义的组件, 你不但要改变语法, 还要利用一些ECMAScript语法特性, 比如Class继承和装饰器. Vue Class Component 也提供了一个[`mixins` 帮助](guide/extend-and-mixins.md#Mixins) 来继承mixin, 以及一个 [`createDecorator` 方法](guide/custom-decorators.md)来简单地创建你自己的修饰器.

你或许也需要使用  [Vue Property Decorator](https://github.com/kaorun343/vue-property-decorator) 提供的 `@Prop` 和 `@Watch` 装饰器.
