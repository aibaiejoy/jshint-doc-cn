# CLI flags

JSHint CLI 程序接受以下标记：

### `--reporter`

允许修改 JSHint 输出的信息，通过始终自定义的实现替换默认输出的方式实现：

    $ jshint --reporter=myreporter.js myfile.js
    
这个标记还支持两个预定义的 reportes：*jshint*，保持输出与  JSHint 一致；*checkstyle*，让输出信息与 ChechStyle XML 一致。

```javascript
$ jshint --reporter=checkstyle myfile.js
<?xml version="1.0" encoding="utf-8"?>
<checkstyle version="4.3">
    <file name="myfile.js">
        <error line="10" column="39" severity="error"
      message="Octal literals are not allowed in strict mode."/>
    </file>
</checkstyle>
```

更多信息请参考：[编写自己的 reporter](write-own-reporter.md)

### `--verbose`

给 JSHint 输出添加信息代码。

### `-show-non-errors`

显示 JSHint 生成的附加信息。

    $ jshint --show-non-errors myfile.js
    myfile.js: line 10, col 39, Octal literals are not allowed in strict mode.
    
    1 error
    
    myfile.js:
        Unused variables:
            foo, bar
            
### `--extra-ext`

允许指定额外的文件扩展名，用于对指定的文件进行检测(默认情况下是 `.js`)。

### `--extract=[auto|always|never]`

告诉 JSHint 在校验之前从 HTML 文件中提取 JavaScript：

    tmp ☭ cat test.html
    <html>
        <head>
        <title>Hello, World!</title>
        <script>
        function hello() {
            return "Hello, World!";
        }
        </script>
        </head>
        <body>
        <h1>Hello, World!</h1>
        <script>
            console.log(hello())
        </script>
        </body>
    </html>

    tmp ☭ jshint --extract=auto test.html
    test.html: line 13, col 27, Missing semicolon.

    1 error
    
如果设置为 `always`，JSHint 将总是试图提取 JavaScript。如果设置为 `auto` 就只会在文件看起来像 HTML 文件的时候试图提取 JavaScript。

### `--exclude`

允许指定指定不希望校验的指令。

### `--exclude-path`

允许提供自定的 `.jshintignore` 文件。例如，可以将 JSHint 指向 `.gitignore` 文件，而不是默认的 `.jshintignore`。

### `--prereq`

允许指定必要的文件，比如包含在项目中使用的全局变量定义的文件。

### `--help`

显示简短的帮助信息。

### `--version`

显示安装的 JSHint 的版本。
