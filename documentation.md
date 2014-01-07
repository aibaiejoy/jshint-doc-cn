# Documentation

JSHint 是一个用来标记使用 JavaScript 编写的程序中可疑用法的程序。 核心项目包含库本身，以及一个作为 Node 模块发布的 CLI 程序。

更多的文档请参考：[JSHint 选项列表](options.md)，[CLI flags](cli-flags.md)，[编写自己的 reporter](write-own-reporter.md)，[常见问题](faq.md)。

## 基本用法

使用 JSHint 最简单的方式就是作为一个 Node 程序安装后使用。在终端中简单的运行下面的命令就可以做到( `-g` 标记表示在系统中全局安装 JSHint；如果只想针对当前工作目录安装使用 JSHint，可以省略这个标记)：

    npm install jshint -g
    
完成上面的安装任务之后，就可以使用 `jshint` 了。最简单的方式就是校验单个 JavaScript 文件或某个目录中的所有 JavaScript 文件。

    $ jshint myfile.js
    myfile.js: line 10, col 39, Octal literals are not allowed in strict mode.
    
    1 error
    
如果文件路径是一个破折号(`-`)，那么 JSHint 就会从标准输入中读取。

## 配置

JSHint 对于警告信息自带的有默认的设置，但是它本身就被设计为可陪的。主要有三种方式配置我们自己的 JSHin 副本：可以通过 `--config` 标记手动的指定配置文件；或者使用一个特定的 `.jshintrc` 文件；或者将配置信息置入项目的 `package.json` 文件中的 `jshintConfig` 属性下。使用 `.jshintrc` 的情况下，JSHint 会在当前工作目录中查找这个文件，如果没找到，就会查找上一级目录，一直查询到系统根目录。(如果输入是来自标准输入的，JSHint 不会尝试找到一个配置文件)

这个设置就允许我们针对每个项目都有不同的配置。只需要将配置文件放到项目的根目录中，只要是从项目的任何目录中运行 JSHint，都会启用同一个配置文件。

配置文件是一个简单的 JSON文件，在这个文件中指定启用或者禁用 JSHint 选项。例如，在下面的文件中会启用未定义，未使用变量的警告，并通知 JSHint 有一个叫做 `MY_GLOBAL` 的全局变量。

```javascript
{
    "undef": true,
    "unused": true,
    "globals": {
        "MY_GLOBAL": false
    }
}
```

### Inline 配置

此外还可以在 JavaScript 文件中使用特定形式的注释来使用配置文件中用来配置 JSHint 的选项。这个注释使用 `jshint` 或者 `global` 开头，并且跟随这一个逗号分割的值列表。例如，下面的代码片段也启用了与上面配置文件中相同的配置：

    /* jshint undef: true, unused: true */
    /* global MY_GLOBAL */
    
既可以使用多行注释，也可以使用单行注释来配置 JSHint。这些注释也是作用于函数作用域中的，也就是说如果将这些注释放在函数中，配置信息就只对这个函数有效果。

### 指令

下面是 JSHint 所支持的配置指令列表：

**jshint**

设置 JSHint 选项的指令。

    /* jshint strict: true */
    
**jslint**

一个设置 JSHint 兼容 JSLint 选项的指令。

    /* jslint vars: true */
    
**globals**

这个指令用于将定义在其他地方的全局变量告诉给 JSHint。如果设置为 `false` (默认值)，那么 JSHint 就会认为改变量是只读的。通常与的 `undef` 选项一起使用。

    /* global MY_LIB: false */
    
也可以使用黑名单来包含一个全局变量，这用于确保在当前文件中任何地方都不会使用这些全局变量。

    /* global -BAD-LIB */
    
**exported**

这个指令用户告诉 JSHint 在当前文档中定义，但是在其他地方使用的全局变量。通常与 `unused` 选项一起使用。

    /* exports EXPROTED_LIB */
    
**members**

这个选项用于告诉 JSHint 所有你需要使用的属性。**这个属性已经废弃了**

**ignore**

这个只领域用户告诉 JSHint 它要忽略一块代码。

    // Code here will be linted with JSHint.
    /* jshint ignore:start */
    // Code here will be linted with ignored by JSHint.
    /* jshint ignore:end */
    
所有在 `ignore:start` 和 `ignore:end` 之间的代码就不会出传递给 JSHint，因此你可以使用人言扩展，比如 [Facebook React](http://facebook.github.io/react/)。此外，还可以使用下面的形式以行忽略单行代码：

    ignoreThis(); // jsint ingore:line
    
### options

大多数情况下，我们通常都只需要根据自己的需要来调整 JSHint，因此只需要根据自己的需要使用合适的选项即可。如果觉得 JSHint 的选项复杂且混乱(实际上我们在努力解决这个问题)，那么请仔细阅读下面的内容。

JSHint 有两种类型的选项：强制类型和宽松类型。前者用于使 JSHint 更加严格，后者用于抑制一些警告。下面有一个例子：

```javascript
function main(a, b) {
    return a == null;
}
```

当使用默认的 JSHint 选项来校验上面这段代码时将会生成下面的警告信息：

    line 2, col 14, Use '===' to compare with 'null'.
    
比如说你知道你自己在做什么，你希望禁用生成警告信息的选项，与此同时，你还对哪些变量定义了但并没有使用比较好奇。那么该怎么做呢，在这种情况下，只需要启用两个选项即可：一个宽松的选项，用来抑制 `===null` 的警告信息，一个是强类型的选项，用于启用检测未使用变量的功能。针对前面的代码而言，这两个选项就是 `unused` 和 `eqnull`。

```javascript
/* jshint unused: true, eqnull: true */
function main(a, b) {
    return a == null;
}
```
在配置好选项之后，再使用 JSHint 对代码进行校验，将会生成下面的警告信息(对于上面这段代码而言)：

    demo.js: line 2, col 14, 'main' is defined but never used.
    demo.js: line 2, col 19, 'b' is defined but never used.

有时候，JSHint 可能并没有合适的选项来禁用某些错误信息的输出。在这种情况下，可以使用 `jshint` 指令来禁用这些代码产生的警告信息。比如说，有某个文件是通过合并多个不同文件到一个文件中而得到的：

```javascript
"use strict";
/* ... */
// From another file
function b() {
    "use strict";
    /* ... */
}
```

上面的这个代码将会触发一个在函数 `b` 中存在不必要的指令的警告信息。JSHint 会检测到在全局已经有一个 "use strict" 指令了，那么它就会告诉你，所有其他相同的指令就是多余的了。但是对于这种自动生成的文件，你可能并不想去掉这些指令。这种情况下的解决方案就是在运行 JSHint 的时候启用 `--verbose` 标志，同时需要注意警告代码(在这个例子中是 W034)：

    $jshint  --verbose myfile.js
    myfile.js: line 6, col 3, Unnecessary directive "use strict". (W034)
    
然后，只需要在文件中添加以下片段即可隐藏这个警告信息：

    /* jshint -W304 */
    
还有两件事情需要注意：

1. 上面的语法形式仅适用于警告信息(以 `W` 开头的代码)，并不适用于错误处理(错误处理的代码以 `E` 开头)。
2. 上面的语法形式会禁用对应的前面的代码中的所有警告信息。有些警告信息比其他的更通用，因而请紧身使用。

可以使用下面的形式启用上面的代码片段中已经禁用的警告信息：

    /* jshint +W304 */
    
当你的代码触发了警告信息，但是你知道它是执行在安全环境中的，那么着就变得非常有用了。这种情况下就可以在代码的顶部禁用警告信息，然后在后面重新启用警告信息：

```javascript
var y = Object.create(null);
// ...
/* jshint -W089 */
for(var prop in y) {
    // ...
}
/* jshint +W089 */
```

在单独的 [options 文档中](options.md) 包含了一个 JSHint 所支持的所有选项的列表。

**Switch 语句**

默认情况下在 swicth 语句中省略 `break` 或者 `return` 语句时 JSHint 会发出警告信息：

```javascript
switch(cond) {
case 'one':
    domeSomething(); // JSHint 会提示这里缺少 'break' 的警告信息
case 'two':
    domeSometingElese();
}
```

如果你确实知道你自己为什么这么做，你可以告诉 JSHint 落空 case 块是你预期的，只需要在注释中添加 `/* falls through */` 即可：

```javascript
switch (cond) {
case "one":
    doSomething();
    /* falls through */
case "two":
    doSomethingElse();
}
```