# Get Input In Node

- [Get Input In Node](#get-input-in-node)
  - [`process.argv`](#processargv)
  - [`process.argv` + `minimist` 库简化操作](#processargv--minimist-库简化操作)
  - [`stdin` 内置模块](#stdin-内置模块)
  - [`get-stdin` npm 包](#get-stdin-npm-包)
  - [`process.env`](#processenv)

## `process.argv`

在 shell 中通过 `--` 和 `-` 传入的参数，在脚本中可以通过 `process.argv` 来获取。

`process.argv` 的返回值是一个数组，前两项是运行环境和脚本路径，一般我们不关心，从第三项开始就是用户传入的自定义参数。

e.g.

```shell
index.js --name=suukii -a=17

# ['--name=suukii', '-a=17'] 忽略前两项
```

## `process.argv` + `minimist` 库简化操作

`process.argv` 得到的数据太原始了，如果不想自己再处理参数，可以引入一个工具库 `minimist` 来帮我们处理。

e.g.

```js
const minimist = require('minimist');
const args = minimist(process.argv);

// {
//   name: 'suukii',
//   a: 17
// }
```

`minimist` 不仅把参数处理成了对象，还会对参数值的类型做一些猜测，比如把 `-a=17` 中的 `17` 转换成数字。

`minimist` 还能接收一个配置对象，通过这个配置对象我们可以告诉 `minimist`，我们希望收到的参数是什么类型的。

e.g.

```js
minimist(process.argv, {
    boolean: ['help'],
    string: ['file'],
});
```

-   `boolean: ['help']`: 把 `help` 参数处理成布尔值。
-   `string: ['file']`: 把 `file` 参数处理成字符串。

## `stdin` 内置模块

TODO

## `get-stdin` npm 包

`get-stdin` 提供两个方法：

-   `getStdin()` 以字符串形式获取 `stdin`。
-   `getStdin.buffer()` 以 Buffer 形式获取 `stdin`。

两个方法都会返回一个 promise。`stdin` 的 `end` 事件触发后，这个 promise 就会 resolve。

e.g.

```shell
# 输入1
echo suukii | node index.js
```

```shell
# 输入2
cat hello.txt | node index.js

# hello.txt
# suukii
```

```js
// 获取输入
const getStdin = require('get-stdin');
getStdin().then(data => {
    console.log(data); // suukii
});
```

## `process.env`

e.g.

```shell
# 设置环境变量 NAME
NAME=suukii node index.js
```

```js
// 获取环境变量
process.env.NAME; // suukii
```
