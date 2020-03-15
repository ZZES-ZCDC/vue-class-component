# 扩展 和 Mixins

## 扩展

你可以扩展一个存在的Class组件, 类似于原生的Class继承. 想象你有下面的名为Super的Class组件:

```js
// super.js
import Vue from 'vue'
import Component from 'vue-class-component'

// 定义一个叫super的Class组件
@Component
export default class Super extends Vue {
  superValue = 'Hello'
}
```

你可以扩展他, 通过使用原生的Class继承语法:

```js
import Super from './super'
import Component from 'vue-class-component'

// 扩展  名为Super的Class组件
@Component
export default class HelloWorld extends Super {
  created() {
    console.log(this.superValue) // -> Hello
  }
}
```

注意 名为Super的Class组件必须是一个Class组件. 换句话说, 它需要继承作为原本的`Vue`构造器以及被 `@Component` 装饰器装饰.

## Mixins

Vue Class Component 提供 `mixins` 助手函数来在Class风格中使用 [mixins](https://vuejs.org/v2/guide/mixins.html). 通过使用`mixins`助手, TypeScript 可以推断mixin类型以及在组件类型中继承它们.

例子中定义了名为`Hello` 和 `World` 的 mixins :

```js
// mixins.js
import Vue from 'vue'
import Component from 'vue-class-component'

// 你可以定义和组件风格一样的 mixins .
@Component
export class Hello extends Vue {
  hello = 'Hello'
}

@Component
export class World extends Vue {
  world = 'World'
}
```

在Class组件中使用它们:

```js
import Component, { mixins } from 'vue-class-component'
import { Hello, World } from './mixins'

// 使用 `mixins` 助手函数代替 `Vue`.
// `mixins` 可以接收任何数量的参数.
@Component
export class HelloWorld extends mixins(Hello, World) {
  created () {
    console.log(this.hello + ' ' + this.world + '!') // -> Hello World!
  }
}
```

和名为Super的Class组件一样, 所有的mixins 必须被定义为一个 Class 组件.
