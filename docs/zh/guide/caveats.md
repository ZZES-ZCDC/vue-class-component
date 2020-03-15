# Class 组件的注意事项

Vue Class Component 收集Class属性作为Vue实例的data, 通过在引擎下实例化原始的构造器. 当我们能像原生Class方法一样定义实例data时, 我们需要了解它时如何工作的.

## 在属性中初始化`this`的值

如果你在类的属性中定义一个箭头函数, 箭头函数中访问 `this` 时, 将无法获取实例. 这是因为当初始化Class属性时, `this`仅仅时Vue实例的代理:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class MyComp extends Vue {
  foo = 123

  // 不要这么做
  bar = () => {
    // 不能这样更新属性
    // 事实上`this` 不是Vue实例.
    this.foo = 456
  }
}
```

在下面的例子中, 你可以简单的定义一个方法代替一个Class属性, 因为Vue将自动绑定实例:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class MyComp extends Vue {
  foo = 123

  bar() {
    // 正确的更新属性
    this.foo = 456
  }
}
```

## 通常使用生命周期函数代替 `constructor`

由于原始构造函数被调用来收集初始组件数据, 建议不要自己声明 `constructor`:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class Posts extends Vue {
  posts = []

  // 不要这么做
  constructor() {
    fetch('/posts.json')
      .then(res => res.json())
      .then(posts => {
        this.posts = posts
      })
  }
}
```

上面的代码打算在组件初始化的时候用fetch来获取post列表, 但是fetch将会被调用两次, 因为Vue Class Component的运作

所以推荐写在生命周期函数里, 比如用`created` 代替 `constructor`:

```js
import Vue from 'vue'
import Component from 'vue-class-component'

@Component
export default class Posts extends Vue {
  posts = []

  // 这样做才对哦
  created() {
    fetch('/posts.json')
      .then(res => res.json())
      .then(posts => {
        this.posts = posts
      })
  }
}
```