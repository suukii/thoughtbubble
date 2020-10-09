# The Cat Command

- [The Cat Command](#the-cat-command)
  - [语法](#语法)
  - [用法 1：读取文件](#用法-1读取文件)
    - [标准输出](#标准输出)
    - [标准输入](#标准输入)
  - [用法 2：拼接](#用法-2拼接)
  - [用法 3：创建文件](#用法-3创建文件)
  - [每日一题](#每日一题httpsgithubcomazl397985856fe-interviewissues151)
    - [题目](#题目)
    - [回答](#回答)

## 语法

```shell
cat [options] [filenames] [-] [filenames]
```

## 用法 1：读取文件

用于读取文件内容。

```shell
cat file1
```

-   读取 file1 文件的内容，输出到屏幕上。

### 标准输出

默认情况下，`cat` 的标准输出就是屏幕。也可以把读取的文件内容写入另一个文件，或者作为另一个命令的输入。

e.g.

```shell
cat file1 > file2
```

-   读取 file1 的内容，写入 file2。
-   file1 的内容就不会在终端屏幕上显示了。

e.g.

如果内容过长，直接打印在屏幕上不方便阅读，可以通过 `|` 把内容传给 filter `less`。

```shell
cat file1 | less
```

-   在 less 中，按空格键一次滚动一个屏幕的内容，按 `b` 向上滚动，按 `q` 退出。

### 标准输入

`cat` 的默认标准输入是键盘。

e.g.

```shell
cat > file1
```

-   输入以上命令，按下回车后，继续输入的内容会被写入到 file1 中。
-   先按回车，再按 `Ctrl + d` 结束输入。
-   如果没有指定文件的话，输入的内容会输出到屏幕上。

## 用法 2：拼接

这个用法也是 `cat` 命令名称的来源(Concatenation)。

e.g.

```shell
cat file1 file2 file3
```

-   把这三个文件的内容拼接起来，输出在终端屏幕上。
-   拼接的是拷贝的内容，所以不会影响原文件。

e.g.

```shell
cat file1 file2 file3 > file4
```

-   把拼接内容写入 file4

e.g.

```shell
cat file1 file2 file3 | sort > file4
```

-   先把拼接的文本通过 sort filter 排序(按行排序)，再写入 file4。

## 用法 3：创建文件

用于创建文件，看[这里](#标准输入)。

-   如果文件已经存在，原文件会被新文件覆盖。
-   不希望覆盖的话，可以使用 `cat >> file1` 拼接内容。

e.g.

```shell
cat file1 > file2
```

-   新建一个文件 file2，将已存在的 file1 文件的内容复制到 file2 中。

e.g.

```shell
cat - file1 > file2
```

-   `-` 表示读取键盘输入。
-   将键盘输入的内容和 file1 的内容写入 file2 中。

e.g.

```shell
cat file1 - > file2
```

-   将 file1 的内容和键盘输入的内容写入 file2 中，内容写入顺序和上一个例子不同。

键盘输入看[这里](#标准输入)。

这个用法的实际用途：e.g. 根据模板文件新建文件时，需要自定义开头或者结尾的内容的话，这个命令就用得上了。

## [每日一题](https://github.com/azl397985856/fe-interview/issues/151)

### 题目

```shell
mkfifo pipe1
mkfifo pipe2
echo -n run | cat - pipe1 > pipe2 &
cat < pipe2 > pipe1
```

### 回答

```shell
mkfifo pipe1
```

-   创建了一个 FIFO(named pipe) pipe1(可以把它看成一个文件)

```shell
mkfifo pipe2
```

-   创建了一个 FIFO(named pipe) pipe2

```shell
echo -n run | cat - pipe1 > pipe2 &
```

-   `echo run` 输入字符串 `run`，`-n` 参数去除 trailing line
-   `|` pipe to
-   `cat - pipe1 > pipe2` 将输入(`run`) 和 pipe1(暂时没有内容) 的内容输入到 pipe2
-   `&` run in background

```shell
cat < pipe2 > pipe1
```

-   将 pipe2 的内容作为 `cat` 命令的输入，然后再输出到 pipe1 中。

一顿操作之后，pipe1 和 pipe2 中的内容都是 `run`。
