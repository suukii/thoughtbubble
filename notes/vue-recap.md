# Vue Recap

## 组件 attribute 和 event 继承

-   只有一个根元素组件：根元素会自动继承组件的 attribute 以及 event。可以通过 `inheritAttrs: false` 来关闭自动继承，还可以通过 `v-bind="$attrs"` 将 attributes 绑定到组件内的其他元素上。
-   存在多个根元素的组件：不会默认继承 attribute，但也可以使用 `v-bind="$attrs"` 绑定到某个元素上，没有显式绑定则会出现 runtime warning。

> 事件绑定则是 `v-on="$listeners"`

## 自定义事件

-   跟原生事件不同的是，自定义事件名称没有驼峰式-横线式自动转换。推荐使用横线式命名。

### 事件验证

可以在 `emits` 选项中定义事件验证函数，类似于 `props` 的验证函数(不指定或者值为 `null` 表示无需验证)。

在函数体中返回布尔值表示事件是否有效，但返回值不影响事件的触发，也就是说，事件无论有效与否都会正常触发。

```js
app.component('custom-form', {
    emits: {
        submit(e) {
            return e === 'suukii';
        },
    },
    methods: {
        submitForm() {
            this.$emit('submit', 'suukii');
        },
    },
});
```

## slot

-   `v-slot` 指令只能用于 `<template>` 标签（除了 default slot 的简写，即只有 default slot 的情况下，`v-slot` 可以用在组件标签上，`v-slot:default="slotProps"` 或者 `v-slot="slotProps"` 都可以。）。
-   `v-slot` 的简写是 `#`，e.g. `v-slot:default` => `#default`
-   scoped slot: 在子组件中 `<slot name="item" :item="item"></slot>`，在父组件中 `<template #item="slotProps">{{ slotProps.item }}</template>` 可以获取子组件的数据。

## Provide & Inject

-   用于父组件向多层嵌套的子组件直接提供数据，不用一层层地传 props。
-   父组件使用 `provide` 字段(对象)声明要提供的数据，e.g. `provide: { user: 'suukii' }`，但使用这种语法不能获取组件示例上的数据；还有另一种语法，跟 `data` 一样在一个函数里返回对象，这种写法可以获取组件实例上的属性。
-   provide/inject 并不是默认响应式的，如果想要实现响应性，需要给 provide 提供 `ref` 或者 `reactive` 对象，e.g.

```js
app.component('todo-list', {
    // ...
    provide() {
        return {
            todoLength: Vue.computed(() => this.todos.length),
        };
    },
});
```

## <teleport>

-   使用场景：e.g. 全屏弹窗
-   如果在一个组件中嵌套了一个弹窗，我们会希望控制弹窗显隐的逻辑留在组件内，但是弹窗的 DOM 结构移到组件外面，一般是直接放到 body 中
-   这时候就可以使用 `<teleport>` 来指定某一段 HTML 希望挂载到的目标元素

```pug
teleport(to="body")
    .modal
        button 关闭
        p 这是一个弹窗
```

> teleport 中也可用于 Vue 组件。
