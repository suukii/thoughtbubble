# Package Json Cheatsheet

[What's what? - Package.json cheatsheet!](https://areknawo.com/whats-what-package-json-cheatsheet/)

## Basic

### name

-   string, 必填。
-   如果是要发布的 npm 包，名字必须是唯一的。
-   如果注册了命名空间，包名称只需在命名空间内保持唯一即可，e.g. @scope/package。
-   包括命名空间在内，名称长度不超过 214 个字符。
-   不能有大写字母，不能以 `_` 和 `.` 开头。
-   只能使用 URL 安全字符。

### version

-   string, 必填。
-   e.g. `0.0.0`
-   with tag e.g. `0.0.0-beta.0` (tags: next, beta, alpha, etc.)
-   [versioning guide](https://semver.org/)

## Information

### description

-   string
-   描述信息。

### keywords

-   string
-   用来提高 SEO 的。

### license

-   string
-   [SPDX identifiers](https://spdx.org/licenses/)。
-   [选证书指南](https://tldrlegal.com/)。
-   传统的做法：把你选的证书的正文放到项目根目录下的 `LICENSE.md` 文件里。

### homepage

-   string
-   如果你的 npm 包有 landing page 的话，可以把 URL 放在 homepage 字段，网址会在 npm 网页上显示。

### repository

源码仓库地址，它的值可以有两种格式：

1. object

```json
{
    "type": "git", // svn
    "url": "https://example.com",
    "direcotry": "" // 选填
}
```

2. string

`provider:user/repo`, provider 可以是 `github`, `gitlab`, `bitbucket`, `gist:id`。

### bugs

-   一般是指向 **issues** 页面，或者其他可以报告 bug 的地方。

### author

两种格式：

1. object

```json
{
    "name": "",
    "email": "",
    "url": ""
}
```

2. string

`name<email>(url)` 可以省略 email 或者 url。

### contributors

-   object/string 数组，格式和 author 字段一样。
-   或者也可以在项目根目录下提供一个 `AUTHORS.md` 用 string 格式列出所有 contributors，一行一个，这个文件会被当做 `contributors` 字段的默认值。

## Files

### files

-   用来指定哪些文件要发布到 npm，格式是 string 数组(支持指定文件/文件夹/\*)。
-   另一个选择是提供一个 `.npmignore` 文件，类似于 `.gitignore`。
-   有些文件不受这些设置的约束，比如 `README.md` 和 `LICENSE.md` 无论如何都会被包括；而 `node_modules` 和 `.git` 无论如何都不会被包括。

### main

-   指定包的入口文件，这个文件也不受 `files` 字段设置的影响，一定会被包含在最后的打包中。

## browser

<!-- TODO: 没理解这段话。 -->

With browser property, we're getting to different variations of main files for your package. It can be used when e.g. you use some kind of module bundler which outputs different formats (like IIFE or UMD). Browser field should point to file, that may be used in browsers 🖥 and be dependent on global variables of this environment (e.g. window).

### unpkg

<!-- TODO: 这个字段还不太知道怎么用。 -->

-   [UNPKG](https://unpkg.com/) 是一个非官方的 npm-based CDN。
-   `unpkg` 字段用来指定一个文件，这个文件用来提供 CDN 支持。

### module

<!-- TODO -->

If you have one, `module` property should point out the file that is an entry point for your modular (not-bundled) code base. It's targeted towards more modern environments.

### typings

-   `typings` 或者 `types` 指向 TS 声明文件的入口文件，比单独把声明文件上传到如 [DefinitelyTyped](https://definitelytyped.org/) 之类的网站方便。

### bin

-   如果你的 npm 包是一个可执行文件，就必须包含这个字段。
-   指向可执行文件 `#!/usr/bin/env node`。

### man

-   string/string 数组
-   如果你有 [man pages](https://en.wikipedia.org/wiki/Man_page) 格式的文档，可以使用 `man` 字段来指定。

### directories

用来提供 meta-info。

-   `lib` - 指定 package 中 library 的位置。
-   `bin` - 指定可执行文件所在文件夹，不用在 `bin` 字段中一个个文件路径地指定，这个属性跟 `bin` 字段只能选其一。
-   `man` - man pages 所在的文件夹，可以用来替换 `man` 字段，不用在 `man` 字段中指定所有 man pages 文件的路径。
-   `doc` - markdown doc 所在地文件夹。
-   `example` - 示例代码所在文件夹。
-   `test` - 测试文件所在文件夹。

## Tasks

### scripts

-   [complete list](https://docs.npmjs.com/misc/scripts)

### config

-   object
-   在这里指定的配置可以在 scripts 中使用。

e.g.

```json
{
    "config": {
        "port": "8080"
    }
}
```

-   设置了 `port` 属性，在 scripts 中可以通过 `npm_package_config_port` 来获取配置值。
-   这些选项可以通过 `npm config set [package]:[prop] [value]` 来修改。

## Dependencies

### dependencies

-   可以通过 URL, Git URL, GitHub URL, linked packages 和 local path 来安装依赖。

### devDependencies

-   e.g. testing framework, module bundler, transplier

### peerDependencies

Peer dependencies (this time not configured automatically) allow you to specify compatibility of your package with some other ones. This should have a form of an object with compatible packages names as keys and their respective versions (following node-semver, e.g. 0.x.x) as values. Since NPM v3 these dependencies aren't installed by default.

### optionalDependencies

-   有用但非必须的依赖，条件允许的情况下就安装。
-   e.g. [fsevents](https://www.npmjs.com/package/fsevents) 只能在 Mac OS 上安装。

### bundledDependencies

-   string 数组。
-   指定要打包进你自己的 package 中的依赖的名字。

This can be useful when preserving your project with tarball files, which, bundled using npm pack, will include files here specified.

## Platform

### engines

-   object
-   指定 library 或者 runtime(e.g. Node.js, NPM or React Native)

This is especially useful when your package depends on modern features (available only in latest Node.js releases) or on other, usually globally-installed libraries and runtimes.

### os

-   array of OS code names
-   如果你的 package 只能在特定的 os 上运行，就可以在 `os` 字段中把它们列出来。
-   `["linux"]` 表示支持 linux 系统，`["!win32"]` (加感叹号)表示不支持 win32 系统。

### cpu

-   跟 `os`，差不多，用来指定你的代码可以运行在哪些 cpu 上。
-   `["x64"]` 表示支持，`["!arm"]` 表示不支持。

## Publishing

### private

-   boolean
-   设为 `true` 则你的代码无论如何都不会发布，即使你不小心做了发布代码的操作。
-   默认为 `false`。

### publishConfig

-   object
-   用来在发布前修改覆盖 [npm config](https://docs.npmjs.com/misc/config#config-settings)
-   常见的修改的配置是：`tag`, `registry`, `access`。

## Custom fields

-   支持 UNPKG, [Babel](https://babeljs.io/docs/en/config-files), [Prettier](https://prettier.io/docs/en/configuration.html) 配置。
-   但是不建议在 `package.json` 中配置其他工具，推荐的做法是使用独立的配置文件(e.g. `.babelrc`)。
