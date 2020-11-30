# Fixed Problems

### user snippets 在 markdown 文件中不起作用的解决办法

`settings.json`

```json
{
    "[markdown]": {
        "editor.quickSuggestions": true,
        "editor.wordBasedSuggestions": false
    }
}
```

https://stackoverflow.com/questions/43639841/how-to-set-markdown-snippet-trigger-automatically/45910856#45910856
