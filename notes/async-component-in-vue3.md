# Async Component In Vue3

## 旧语法

将组件定义成一个返回 Promise 的函数：

```js
const asyncPage = () => import('./NextPage.vue');
```

带选项的异步组件定义：

```js
const asyncPage = {
    component: () => import('./NextPage.vue'),
    delay: 200,
    timeout: 3000,
    error: ErrorComponent,
    loading: LoadingComponent,
};
```

## 3.x 语法

因为函数式组件被定义成了纯函数，所以异步组件需要一个新语法，就是包一层 `defineAsyncComponent` (新 API) 函数。

```js
import { defineAsyncComponent } from 'vue';

const asyncPage = defineAsyncComponent(() => import('./NextPage.vue'));
```

带选项：

```js
const asyncPageWithOptions = defineAsyncComponent({
    loader: () => import('./NextPage.vue'),
    delay: 200,
    timeout: 3000,
    errorComponent: ErrorComponent,
    loadingComponent: LoadingComponent,
});
```

> 原先的 component 字段更换成了 loader 字段

另一个改动是，3.x 语法中，工厂函数不再提供 resolve 和 reject 参数，而是要求必须返回一个 Promise。

```js
// 2.x
const oldAsyncComponent = (resolve, reject) => {
    /* ... */
};

// 3.x version
const asyncComponent = defineAsyncComponent(
    () =>
        new Promise((resolve, reject) => {
            /* ... */
        }),
);
```

> 2.x 版本是上面两种写法都支持的。
