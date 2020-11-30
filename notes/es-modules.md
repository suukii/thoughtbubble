# ES Modules

## ES5 模块

虽然语言本身并没有模块系统，但社区有两种用的比较广的解决办法：

-   CommonJS: Node.js 实现了这种模块模式
    -   语法简洁
    -   同步加载，适用于服务端
-   AMD: RequireJS 实现了这种模块模式
    -   语法稍复杂一点，但不依赖 `eval()` 或者编译步骤
    -   异步加载，适用于客户端

[Writing Modular JavaScript With AMD, CommonJS & ES Harmony](http://addyosmani.com/writing-modular-js/)

## 模块解决的问题

在原本的 JS 代码管理模式中，如果我们想要引入共享的变量，一般是把他们放在全局作用域，这种做法会带来两个问题：

1. 所有 JS 文件必须按一定顺序引入。
2. 把变量放在全局除了会引起命名冲突问题，更麻烦的是，任何代码都可以修改全局变量。

## 为什么要用模块系统

提供了更好的管理变量和函数的方法

## 语法

### 默认导出

`export default` 后面跟的是函数声明、generator 函数声明或者 class 声明(但在这里这些声明可以是匿名的)，匿名函数在 `export default` 后面会被认为是函数声明而不是表达式(如果想让匿名函数被解析成表达式，需要加一个括号)。

`export default` 后面也可以跟表达式，e.g. `export default 'abc'`。

默认导出还有第二种写法：

```js
const name = 'suukii';
export { name as default };
```

这种写法是因为 `export default const foo = bar = 'suukii'` 这样的语法不合法，无法确定哪一个才是默认导出。

### 提升

导入语句会被“提升”。

## 与普通 JS 脚本的区别

### general

-   默认运行在严格模式下。
-   不支持 HTML 注释语法。
-   有属于自己的 top-level 词法作用域，在模块中声明变量不会在 window 上添加属性。
-   `this` 指向 `undefined` (如果想要获取全局 `this`，需要访问 `globalThis`)
-   `import` 和 `export` 语句只能在模块中使用
-   支持 top-level `await`，而且在 `async` 函数外也不能使用 `await` 作变量名。

> 因为有这些不同，所以 JS 运行时必须要知道哪些脚本是模块，哪些是普通 JS 脚本，以区别对待。

-   指定 `Content-Type`: [推荐](http://tools.ietf.org/html/rfc4329#section-7)使用 `application/javascript` (`text/javascript` 是给老古董 IE8 用的，不用管它)
-   指定 `<script>` 的 `type` 属性为 `module` (如果 `<script>` 包含或者指向外部普通 JS 脚本，则 `type` 属性一般是忽略的)。

### browser-specific

-   普通 JS 在 HTML 中引入多少次就执行多少次，而模块无论引入多少次都只执行一次。
-   模块加载受到同源策略限制，必须通过 CORS 获取，而普通 JS 不受此限制。
-   `async` 对于内联的普通 JS 脚本不起作用，但是对于内联模块脚本却有用。(?怎么起作用的)

## 在 HTML 中使用模块

使用 `type="module"` 属性告诉浏览器哪些脚本是模块。

```html
<script type="module" src="module.js"></script>
<script nomodule src="fallback.js"></script>
```

支持模块系统的浏览器会忽略 `nomodule` 属性。

模块引入顺序并不重要。

```html
<script type="module">
    import toUpperCase from './index.js';
    console.log(toUpperCase('suukii'));
</script>
<script src="./index.js" type="module"></script>
```

`import` 语句的引入路径只能是绝对路径或者 `./`, `../` 和 `/` 开头的路径，并不支持裸路径 `scripts/index.js`。

因为模块加载默认是 `defer` 的，所以一般也给 `nomodule` 脚本加上 `defer` 属性。

```js
<script nomodule defer src="fallback.js"></script>
```

## 模块文件扩展名

V8 的建议是使用 `.mjs` 扩展名，因为：

1. 开发时能轻易分辨哪个文件是 ES 模块。因为模块和普通 JS 文件不一样，所以这个区别还很重要的。
2. 能保证模块文件能被 Node.js 和 d8 等运行时和 babel 之类的构建工具正确解析。

> 但 `.mjs` 扩展名并不是必须的，`type="module"` 已经告诉浏览器这个文件是模块文件了。另外使用 `.mjs` 扩展名的话，还需要配置好响应头部 `Content-Type: text/javascript`。

## 静态引入

ES6 模块是静态引入的，指的是在编译阶段就可以知道导出/引入了哪些依赖，不需要等执行代码才知道。在语法上的限制就是 `import`/`export` 语句必须是 top level 的，而且语句中不能出现动态成分，比如用变量来指定模块路径名，这种情况是不允许的。

### 好处

#### 方便在打包阶段删除无用代码

-   在开发中，为了便于管理，代码是分成很多小模块的。
-   在发布时，这些小模块一般会被打包成几个相对大一点的文件。

**为什么要打包**

1. 减少下载模块的次数(对于 HTTP/1 来说比较重要，对于 HTTP/2 这已经不算理由了)
2. 压缩打包好的文件会比压缩若干个没打包的文件要高效一点
3. 在打包时可以分析出没有用到的 `export`，减少包的体积(这个理由更有力一点)

#### 打包后的代码就是 ES6 模块，不用引入新语法

-   ES6 模块是静态引入，打包工具不需要执行代码就能分析出依赖树的关系。
-   `import` 是 read-only views，所以不用复制导出，直接指向它们就好。

## 动态引入

-   静态 `import` 的话，要等全部模块依赖都下载并执行完才能执行程序的其他代码。
-   动态 `import()` 函数则是在需要时再加载模块，而且跟静态 `import` 不一样的是，`import()` 可以在普通 JS 文件中使用，函数调用返回一个 Promise。

```js
document.addEventListener('click', async function () {
    const { shout } = await import('./index.js');
    shout('suukii');
});
```

-   只有在点击页面的时候浏览器才会去下载 `./index.js` 文件并执行。

> webpack 有自己版本的 [import()](https://developers.google.com/web/fundamentals/performance/webpack/use-long-term-caching)，还会自动 code splitting。

## `import.meta`

提供当前模块的信息，但并没有在 ES 规范中规定，具体信息取决于运行环境。

使用场景 e.g. 通过 `import.meta.url` 获取当前模块的路径，然后根据相对路径获取其他资源。

## [性能建议](https://v8.dev/features/modules#performance)

### 还是建议使用打包工具

使用 JS 原生模块的场景：

-   本地开发。
-   比较小的程序，模块总共不超过 100 个，依赖嵌套不超过 5 层。

生产环境中还是建议使用打包工具，原因之一是打包工具会分析静态 `import`/`export` 语句，删掉没有用到的代码，提高程序性能。

-   [ ] [DevTools 检测有没有给用户推送没用的代码](https://developers.google.com/web/updates/2017/04/devtools-release-notes#coverage)
-   [ ] [code splitting](https://developers.google.com/web/fundamentals/performance/webpack/use-long-term-caching#lazy-loading)

### preload modules

一般浏览器是先下载 A 模块，解析，然后再下载 A 模块所依赖的 B 模块，这样构建依赖树消耗时间比较长。但我们可以通过 `<link rel="modulepreload" href="lib.mjs">` 来声明所有依赖模块，尤其是在依赖树比较大的时候很有用。

### 使用 HTTP/2

单是 HTTP/2 的[多路复用](https://developers.google.com/web/fundamentals/performance/http2/#request_and_response_multiplexing)特性就很适合模块的加载。

不过客户端推送特性可能[不是那么好用](https://jakearchibald.com/2017/h2-push-tougher-than-i-thought/)。

## 循环依赖

可以的话应该要避免依赖间循环引用，但因为有些情况必须要循环引用，所以支持这个特性也是很有必要的。

## Resources

-   [JavaScript modules - v8](https://v8.dev/features/modules#performance)

## 拓展文章

-   [ ] [deploying-es2015-code-in-production-today](https://philipwalton.com/articles/deploying-es2015-code-in-production-today/)
-   [ ] [dynamic-import](https://v8.dev/features/dynamic-import)
