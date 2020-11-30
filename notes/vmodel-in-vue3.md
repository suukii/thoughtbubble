# V-Model In Vue3

- [V-Model In Vue3](#v-model-in-vue3)
  - [修改 1: 更改默认 prop 和 event 名称](#修改-1-更改默认-prop-和-event-名称)
  - [修改 2: 接收参数修改 prop 和 event 名称](#修改-2-接收参数修改-prop-和-event-名称)
  - [修改 3: 能够绑定多个 v-model](#修改-3-能够绑定多个-v-model)
  - [修改 4: 增加自定义修饰符](#修改-4-增加自定义修饰符)
    - [默认 v-model 的修饰符](#默认-v-model-的修饰符)
    - [命名 v-model 的修饰符](#命名-v-model-的修饰符)

## 修改 1: 更改默认 prop 和 event 名称

-   prop: `value` -> `modelValue`
-   event: `input` -> `update:modelValue`

## 修改 2: 接收参数修改 prop 和 event 名称

```html
<child v-model:title="test" />
```

-   prop: `modelValue` -> `title`
-   event: `update:modelValue` -> `update:title`

这个用法取代了 `v-bind` + `.sync` 语法。

## 修改 3: 能够绑定多个 v-model

利用命名 v-model 的特性可以实现在同一个组件上绑定多个 v-model 的功能。

```html
<child v-model:first-name="firstName" v-model:last-name="lastName" />
```

## 修改 4: 增加自定义修饰符

### 默认 v-model 的修饰符

给 `v-model` 增加的自定义修饰符会通过 `modelModifiers` prop 传递给组件。

```html
<my-component v-model.uppercase="test" />
```

```js
Vue.component('my-component', {
    props: {
        modelValue: String,
        modelModifiers: {
            default: () => ({}),
        },
    },
    methods: {
        onInput(e) {
            if (this.modelModifiers.uppercase) {
                // 判断是否提供了 `.uppercase` 修饰符
                // 对输出的值做一些处理
            }
        },
    },
});
```

### 命名 v-model 的修饰符

给命名 `v-model` 增加的自定义修饰符会通过 `[name]Modifiers` prop 传递给组件。

```html
<my-component v-model:title.uppercase="test" />
```

```js
Vue.component('my-component', {
    props: {
        title: String,
        titleModifiers: {
            default: () => ({}),
        },
    },
});
```
