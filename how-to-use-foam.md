# foam 使用笔记

_For up-to-date tips, see [Foam Recipes](https://foambubble.github.io/foam/recipes)._

## Discover

### foam 文件格式

- 文件必须包含一级标题 `# Heading`
- 文件名不能有空格
- 扩展名必须是 `.md` 或者 `.markdown`
- 除了常用的 markdown 链接语法 `[]()`，还可以使用 `[[file-name]]` 链接

### 文件链接

在文件中使用 `[[file-name]]` 的语法就会自动创建该文件与 `file-name` 文件的链接。

> 不用包括扩展名。文件名命名规范是 `lower-dash-case`，不接受大写字母、空格、或者其他标点符号。

**文件链接有什么用？**

- 可以用 markdown graph 看到文件关系图。
- `Ctrl + Click` 或者 `选中文件名 + F12` 可以打开对应文件。

### 文件关系可视化

`Ctrl + Shift + P` -> 输入 `show graph` -> 选择 `Markdown Links: Show Graph`

就可以看到所有文件的关系图，这个功能依赖的插件是 Markdown Links。

### backlinking

`Ctrl + Shift + P` -> 输入 `backlinks` -> 选择  `Explorer: Focus on Backlinks view`

就可以在 Backlinks 面板中看到所有与当前笔记相关联的其他笔记。

## Write

### 创建文件

- 在当前文件中输入 `[[new-file-name]]` 然后 `Ctrl + Click` 或者 `选中文件名 + F12` 就会自动创建一个 `new-file-name.md` 文件。
- `Ctrl + Shift + P` -> 输入 `new note` -> 运行 `Markdown Notes: New Note` 也可以创建新文件。

### 每日笔记

- 在控制面板(`Ctrl + Shift + P`)运行 `daily note` 会自动创建一个以日期命名的文件，如果已经存在以当前日期命名的文件，就会自动打开那个文件。
- 也可以使用快捷键 `alt + d`

**config**

`.vscode/setting.json`
```json
{
  "foam.openDailyNote.directory": "journal",
  "foam.openDailyNote.filenameFormat": "'daily-note'-yyyy-mm-dd",
  "foam.openDailyNote.fileExtension": "mdx",
  "foam.openDailyNote.titleFormat": "'Journal Entry, ' dddd, mmmm d",
}
```

**模板**

暂时可以使用 [VS Code Snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets) 来实现这个功能。

**weekly/monthly notes**

[learn more](https://foambubble.github.io/foam/note-macros)

### 其他

- [Image from clipboard](https://foambubble.github.io/foam/images-from-your-clipboard), `Extension: Paste Image`