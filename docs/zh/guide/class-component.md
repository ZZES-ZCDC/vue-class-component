# Class 组件

`@Component` 装饰器可以让你能创建一个基于Class的Vue组件:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

// HelloWorld class will be a Vue component
@Component
export default class HelloWorld extends Vue {}
```

## Data

使用Class属性来初始化 `data`:

```vue
<template>
  <div>{{ message }}</div>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  // 定义 component 的 data
  message = 'Hello World!'
}
</script>
```

The above component renders 上面的组件会在`<div>`中的组件data `message`中渲染`Hello World!`

注意如果初始化的值是 `undefined`, Class属性将不是响应式的, 意思就是当其发生修改后, 将不会被侦测到:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  // `message` 将不是响应式数据
  message = undefined
}
```

为了防止这种情况, 你需要使用 `null` 来赋值, 或者使用 `data` 钩子来代替:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  // `message` 将是响应式
  message = null

  data() {
    return {
      // `hello` 将是响应式的, 因为在data钩子里
      hello: undefined
    }
  }
}
```

## Methods

组件 `methods` 将直接定义在Class方法属性中:

```vue
<template>
  <button v-on:click="hello">Click</button>
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  // 定义一个组件方法
  hello() {
    console.log('Hello World!')
  }
}
</script>
```

## Computed Properties

计算属性可以通过Class属性的 getter / setter 定义:

```vue
<template>
  <input v-model="name">
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  firstName = 'John'
  lastName = 'Doe'

  // 定义计算属性的 getter
  get name() {
    return this.firstName + ' ' + this.lastName
  }

  // 定义计算属性的 setter
  set name(value) {
    const splitted = value.split(' ')
    this.firstName = splitted[0]
    this.lastName = splitted[1] || ''
  }
}
</script>
```

## Hooks

`data`, `render` 以及所有Vue生命周期钩子可以直接在Class属性方法中直接定义, 但是你不能在实例本身上调用他们. 当定义自定义方法时, 你应该回避这些保留名

```jsx
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class HelloWorld extends Vue {
  // 定义生命周七钩子函数
  mounted() {
    console.log('mounted')
  }

  // 定义渲染函数
  render() {
    return <div>Hello World!</div>
  }
}
```

## 其他选项

对于其他的选项, 使用修饰器来配置它们:

```vue
<template>
  <OtherComponent />
</template>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import OtherComponent from './OtherComponent.vue'

@Component({
  // 查看 Vue.js 文档, 来了解所有的选项:
  // https://vuejs.org/v2/api/#Options-Data
  components: {
    OtherComponent
  }
})
export default class HelloWorld extends Vue {}
</script>
```