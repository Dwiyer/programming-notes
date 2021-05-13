Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

#### 组件

------

组件是可复用的 Vue 实例，且带有一个名字。我们可以在一个通过 `new Vue` 创建的 Vue 根实例中，把这个组件作为<u>自定义元素</u>来使用。因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。在 Vue 里，**一个组件本质上是一个拥有预定义选项的一个 Vue 实例。**

##### 组件复用

每用一次组件，就会有一个它的新**实例**被创建。

**一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝：

```vue
data: function () {
  return {
    count: 0
  }
}
```

如果 Vue 没有这条规则，点击一个按钮就可能会影响到*其它所有实例*

##### 组件的组织

通常一个应用会以一棵嵌套的组件树的形式来组织：

<img src="https://cn.vuejs.org/images/components.png" alt="Component Tree" style="zoom: 50%;" />

**为了能在模板中使用**，这些组件必须先注册（或者说定义）以便 Vue 能够识别。这里有两种组件的注册类型：**全局注册**和**局部注册**。

**全局注册**

至此，我们的组件都只是通过 `Vue.component` 全局注册的。全局注册的组件可以用在其被注册之后的任何 (通过 `new Vue`) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。**全局注册的行为必须在根 Vue 实例 (通过 `new Vue`) 创建之前发生**。

**局部注册**

全局注册往往是不够理想的。比如，如果你使用一个像 webpack 这样的构建系统，全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中。这造成了用户下载的 JavaScript 的无谓的增加。

在这些情况下，你可以通过一个普通的 JavaScript 对象来定义组件：

```vue
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

然后在 `components` 选项中定义你想要使用的组件：

```vue
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```

对于 `components` 对象中的每个 property 来说，其 property 名就是自定义元素的名字，其 property 值就是这个组件的选项对象。

##### 组件属性

**Prop 是你可以在组件上注册的一些自定义 attribute**。当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 property。我们能够在组件实例中访问这个值，就像访问 `data` 中的值一样。一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。

**单向数据流**

**Prop类型**

一般以字符串数组形式列出的 prop，也可以以对象形式列出 prop，这些 property 的名称和值分别是 prop 各自的名称和类型。

**传值**：传递静态或动态Prop，如数字、布尔值、数组、对象、对象所有property

###### Prop验证

我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。

为了定制 prop 的验证方式，你可以为 `props` 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：

```js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

**类型检查**

`type` 可以是下列原生构造函数中的一个：

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

额外的，`type` 还可以是一个自定义的构造函数，并且通过 `instanceof` 来进行检查确认。例如，给定下列现成的构造函数：

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

你可以使用：

```js
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

来验证 `author` prop 的值是否是通过 `new Person` 创建的。

###### 非Prop的Attribute

一个非 prop 的 attribute 是指传向一个组件，但是该组件并没有相应 prop 定义的 attribute。

因为显式定义的 prop 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。**这也是为什么组件可以接受任意的 attribute，而这些 attribute 会被添加到这个组件的根元素上。**





##### 组件模板

**每个组件必须只有一个根元素**



#### 生命周期

------







#### 路由

------



















































