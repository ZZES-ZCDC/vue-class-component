# 自定义装饰器

你可以通过创建你自己的装饰器来扩展功能性的库. Vue Class Component 提供 `createDecorator` 帮助创建自定义装饰器. `createDecorator` 接受一个回调函数作为第一个参数, 回调将接受下面的参数:

- `options`: Vue 组件选项. 改变这个对象将影响所提供的组件.
- `key`: 装饰器所需要的属性或方法的键.
- `parameterIndex`: 如果自定义装饰器用于参数，则装饰参数的索引.

下面例子是创建一个 `Log` 装饰器, 当修饰器方法被调用时, 打印log信息, 包括方法名和参数:

```js
// decorators.js
import { createDecorator } from 'vue-class-component'

// 定义 Log 装饰器.
export const Log = createDecorator((options, key) => {
  // 保存原始方法.
  const originalMethod = options.methods[key]

  // 覆盖方法.
  options.methods[key] = function wrapperMethod(...args) {
    // 打印一个 log.
    console.log(`Invoked: ${key}(`, ...args, ')')

    // 调用原始方法
    originalMethod.apply(this, args)
  }
})
```

作为方法修饰器使用它:

```js
import Vue from 'vue'
import Component from 'vue-class-component'
import { Log } from './decorators'

@Component
class MyComp extends Vue {
  // 当`hello`方法被调用, 打印一个log
  @Log
  hello(value) {
    // ...
  }
}
```

在上面代码中, 当 `hello` 被调用, 并传入 `42`, 将会打印处下面的log:

```
Invoked: hello( 42 )
```