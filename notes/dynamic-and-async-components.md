# Dynamic And Async Components

- [Dynamic And Async Components](#dynamic-and-async-components)
  - [动态组件](#动态组件)
  - [异步组件](#异步组件)
    - [用法 1](#用法-1)
    - [用法 2](#用法-2)
    - [用法 3](#用法-3)
    - [用法 4](#用法-4)

## 动态组件

`<components :is="componentName" />` 切换组件时会重复销毁/创建组件，如果想要缓存组件，可以包一层 `<keep-alive>`，不过要注意 `<keep-alive>` 要求组件必须有名称。

## 异步组件

### 用法 1

用工厂函数的方式来定义一个组件，Vue 会等到组件需要渲染的时候才触发函数。

```js
Vue.component('async-component', (resolve, reject) => {
    resolve(component);
});
```

### 用法 2

配合 webpack 的 code-splitting 功能一起使用

```js
Vue.component('async-component', (resolve, reject) => {
    // require 会告诉 webpack 自动将代码切割成多个包
    require(['./my-async-compoennt'], resolve);
});
```

### 用法 3

在工厂函数中返回一个 Promise

```js
Vue.component('async-component', () => import('./my-async-compoennt'));
```

### 用法 4

增加处理加载状态的情况，可以在工厂函数中返回如下格式的对象。

```js
Vue.component('async-component', () => ({
    // 需要加载的组件，提供一个 Promise
    component: import('./my-async-compoennt'),
    // 加载异步组件过程中使用的组件
    loading: LoadingComponent,
    error: ErrorComponent,
    // 展示加载时组件的延时时间。默认值是 200 (毫秒)
    delay: 200,
    // 如果提供了超时时间且组件加载也超时了，
    // 则使用加载失败时使用的组件。默认值是：`Infinity`
    timeout: 3000,
}));
```
