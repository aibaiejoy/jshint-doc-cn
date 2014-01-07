# JSHint Options

这是 JSHint 所接受的所有配置选项的完整列表。如果发现遗漏了某些信息，你可以给作者提[issues](https://github.com/jshint/jshint/issues/new)或者发[邮件](anton@kovalyov.net)。

## Enforcing options

设置为 `true` 时，这些选项就会让 JSHint 针对你的代码生成更多的警告信息。

<table>
    <thead>
        <tr>
            <th>选项</th>
            <th>描述</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>bitwise</code></td>
            <td>这个选项用于禁止使用位运算符，如<code>^</code>(XOR)，<code>|</code>(OR)以及其他位运算符。在日常 JavaScript 编程中位运算符的使用极其罕见，通常都只会存在输入 <code>&&</code>时输入错误而输入 <code>&</code>。</td>
        </tr>
        <tr>
            <td><code>camelcase</code></td>
            <td>这个选项允许你强制要求所有的变量使用驼峰式峰哥或者是带下划线的 UPPER_CASE 风格。</td>
        </tr>
        <tr>
            <td><code>curly</code></td>
            <td>这个选项要求在对循环代码块和条件语句中始终使用大括号。在 JavaScript 代码中允许如果循环块或者条件块只有一个语句时省略大括号，比如：<br/><pre><code>while(day)<br>   shuffle();</code></pre>然而，在某些情况下，他可能引起 bug(比如下面的代码，你可能会认为 <code>sleep()</code>是 while 循环的一部分，但实际上不是的)。<br><pre><code>while(day)<br>   shuffle()<br>   sleep()</code></pre></td>
        </tr>
        <tr>
            <td><code>eqeqeq</code></td>
            <td>这个选项用于禁止使用 <code>==</code> 和 <code>!=</code>，支持使用 <coe>===</code> 和 <code> !== </code>。前者在比较值之前会尝试执行强制转换，强制转换操作可能引起意想不到的结果。后者不会执行任何强制转换操作，通常更安全。如果项了解更多关于 JavaScript 类型转换的信息，推荐阅读 Angus Croll 的文章<a href="http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/">Truth, Equality and JavaScript</a></td>
        </tr>
        <tr>
            <td><code>es3</code></td>
            <td>这个选项用于告诉 JSHint 你的代码需要遵循 ECMAScript 3 规范。使用这个选项，那么你的代码就需要能够在较低版本的浏览器中运行 — 比如 IE6/7/8 — 以及其他传统的 JavaScript 环境中。</td>            
        </tr>
        <tr>
            <td><code>forin</code></td>
            <td>这个选项要求所有的<code>for in</code>循环都要过滤对象的属性。因为 for in 语句允许对对象的所有属性进行循环，这包括继承自原型链中的属性。这种行为可能导致遍历对象的时候产生意向不到的结果，因此使用过滤继承属性通常是安全的：<pre><code>for(key in obj) { <br>   if(obj.hasOwnProperty(key)) { <br>       // We are sure that obj[key] belongs to the object and was not inherited.<br>   }<br>}</code></pre>更多的深入理解 <code>for in</code> 的信息请阅读 Angus Croll 的<a href="http://javascriptweblog.wordpress.com/2011/01/04/exploring-javascript-for-in-loops/">Exploring JavaScript for-in loops</a>。</td>
        </tr>
        <tr>
            <td><code>freeze</code></td>
            <td>这个选项用于禁止重写原生对象诸如<code>Array</code>, <code>Date</code>的原型等。<pre><code>/* jshint freeze:true */<br>Array.prototype.count = function (value) {<br>    return 4;<br>}<br>// -> Warning: Extending prototype of native object: 'Array'.</code></pre></td>
        </tr>
        <tr>
            <td><code>immed</code></td>
            <td>这个选项用于禁止使用立即调用函数的时候不将他们包裹在一对括号中。将代码包裹在括号中利于读者理解，表达的是函数执行的结果，而不是函数自身。</td>
        </tr>
        <tr>
            <td><code>indent</code></td>
            <td>这个选项用于强制指定代码中 tab 缩进的宽度。例如下面的代码第4行会触发警告信息：<pre><code>/* jshint indent:4 */<br>if (cond) {<br>  doSomething(); // We used only two spaces for indentation here<br>}</code></pre></td>
        </tr>
        <tr>
            <td><code>latedef</code></td>
            <td>这个选项用于禁止用户在变量定义之前使用。JavaScript 中除了全局作用域就只有函数作用域，此外，函数中的所有变量都会移动或者说提升到函数的顶部。先使用再声明的行为就容易带来一些问题，因而总是先声明后使用是安全的。<br> 设置这个选项位 'nofunc' 就会忽略函数申明。<br>深入理解关于作用域和JavaScript hoisting 的信息请阅读 Ben Cherry 的<a href="http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting">JavaScript Scoping and Hoisting</a></td>
        </tr>
        <tr>
            <td><code>newcap</code></td>
            <td>
                这个选项要求收早函数首字母大写。函数首字母大写的目的是遵循使用 <code>new</code> 运算符的惯例，从视觉上帮助程序员分辨构造函数和其他类型的函数，有利于在使用 <code>this</code> 时候准确的捕获错误信息。<br><br>不这么做实际上也不会破坏代码，无论是在浏览器环境还是在其他环境，但是在是否需要使用 <code>new</code> 运算符的问题上就很难搞清楚了。这是非常重要的，因为使用 <code>new</code> 结合函数使用的时候 <code>this</code> 会正确的指向新建的对象实例，否则它就指向全局对象了。
            </td>
        </tr>
        <tr>
            <td colspan="2"><span style="color:red">下面的文档粗译啦</span></td>
        </td>
        <tr>
            <td><code>noarg</code></td>
            <td>
                这个选项用于禁止使用 <code>arguments.caller</code> 和 <code>arguments.callee</code>。这两个属性不建议使用了。实际上，在 ES5 严格模式中禁止使用 <code>argument.callee</code>了。
            </td>
        </tr>
        <tr>
            <td><code>noempty</code></td>
            <td>
                这个选项在代码中存在空代码块的时候发出警告。JSHint 原本就会对空代码块发出警告，这里知识做成可选的了。虽然还没明确的研究报告说明空代码块会破坏 JavaScript 代码。
            </td>
        </tr>
        <tr>
            <td><code>nonew</code></td>
            <td>
                这个选项用于禁用使用构造函数的副作用。比如不指定返回结果给变量：<br>
                <pre><code>new MyConstructor()</code></pre><br>
                这种方式知识使用 <code>new</code> 运算符调用 <code>MyConstructor</code> 创建了一个新对象，并没有使用。一般情况下应该避免这样使用构造函数。                
            </td>
        </tr>
        <tr>
            <td><code>plusplus</code></td>
            <td>
                这个选项用于禁止使用一元运算符 <code>++</code>和<code>--</code>。有些人认为这回破坏代码风格。比如 Python 中就没有这写操作符。
            </td>
        </tr>
        <tr>
            <td><code>quotmark</code></td>
            <td>
                这个选项用于强制要求在代码中使用一致的引号。它接受三个值：<code>true</code>，如果不希望强制使用特定的风格，但是还是希望遵循习惯；<code>"single"</code> 表示仅使用单引号；<code>"double"</code> 表示仅使用双引号。
            </td>
        </tr>
        <tr>
            <td><code>undef</code></td>
            <td>
            这个选项用于禁止使用未显示声明的变量。对于遗漏声明和输入错误非常有用。<br>
            <pre><code>/* jshint undef:true */<br>function test() {<br>    var myVar = 'hello world';<br>    console.log(myvar); // Oops, typoed here. JSHint with undef will complain <br>}</code></pre><br>
            如果变量定义在其他文件中，可以使用<code>/* global ... */ 指令来告诉 JSHint 在当前文件中不检查指定的变量。
            </td>
        </tr>
        <tr>
            <td><code>unused</code></td>
            <td>
                这个选项会在发现定义了而从未使用的变量时发出警告信息。对于保持整洁的代码非常有用。<br>
                <pre><code>/* jshint unused: true */<br>function test(a,b){<br>   var c, d = 2;<br><br>   return a + d;<br>}<br>test(1, 2)<br><br>especially when used in addition to undef.<br>// Line 4: 'c' was defined but never used.</code></pre><br>
                此外，这个选项还会在警告信息中告诉你应该使用 <code>/* global ... */</code>指令声明未使用的全局变量。<br>
                还可以设置为 <code>vars</code>，让 JSHint 只检查变量而不检查参数，或者设置为 <code>strict</code> 让 JSHint 严格检查所有变量和参数。默认(<code>true</code>)情况下允许在使用的参数后面跟随这未使用的参数。                                
            </td>
        </tr>
        <tr>
            <td><code>strict</code></td>
            <td>
                这个选项要求所有的函数都运行在 ES5 的严格模式下。关于严格模式的信息自行查询文档。严格模式消除了一些 JavaScript 的陷阱，从而某种程度上优化了代码的执行。<br>
                注意：这个选项仅仅在函数中启用严格模式。禁止在全局使用严格模式，因为可能会影响页面中第三方代码。如果真的要在全局使用严格模式，查看 <code>globalstrict</code> 选项。
            </td>
        </tr>
        <tr>
            <td><code>trailing</code></td>
            <td>
                这个选项会在代码中存在错误的尾部空格的情况下报错。比如使用多行字符串的时候尾部不小心输入了空格。
            </td>
        </tr>
        <tr>
            <td><code>maxparams</code></td>
            <td>这个选项方便设置最大形式参数数量，如果超过设置的值就会发出警告。</td>
        </tr>
        <tr>
            <td><code>maxdepth</code></td>
            <td>这个选项用来嵌套层级。超过指定的层级则会发出警告。</td>
        </tr>
        <tr>
            <td><code>maxstatements</code></td>
            <td>
            这个选项用来设置每个函数中最多允许有多少数量的语句。超过则发出警告。
            </td>
        </tr>
        <tr>
            <td><code>maxcomplexity</code></td>
            <td>
            这个选项用来控制代码复杂度。这里的复杂度数据是使用程序测量出来的。更多信息请查看：http://en.wikipedia.org/wiki/Cyclomatic_complexity
            </td>
        </tr>
        <tr>
            <td><code>maxlen</code></td>
            <td>这个选项用来每行代码的最大长度</td>
        </tr>
    </tbody>
</table>

## Relaxing options

设置为 true 是，以下选项会让 JSHint 处理你的代码时生成更少的警告。

<table>
    <tr>
        <td><code>asi</code></td>
        <td>
            这个选项用于抑制缺少分号的警告。JavaScript 的分号机制并不是特别严格。<br>参考阅读：
            http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding<br>http://inimino.org/~inimino/blog/javascript_semicolons            
        </td>
    </tr>
    <tr>
        <td><code>boss</code></td>
        <td>
        这个选项抑制某些可能正确的警告信息。
        </td>
    </tr>
    <tr>
        <td><code>debug</code></td>
        <td>这个选项用于抑制使用 <code>debugger</code> 的警告信息。</td>
    </tr>
    <tr>
        <td><code>eqnull</code></td>
        <td>这个选项用于抑制 <code>==null</code> 比较的警告信息。比如通常检测某个变量是不是 <code>null</code> 或者 <code>undefined</code>。</td>
    </tr>
    <tr>
        <td><code>esnext</code></td>
        <td>这个选项用于告诉 JSHint，在你的代码中使用了 ES 6 中的语法。但是 ES6 还没发布，并不是所有浏览器都实现了。<br> 参考阅读：</br>http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts</td>
    </tr>
    <tr>
        <td><code>evil</code></td>
        <td>这个选项用于抑制使用 <code>eval</code> 时的警告信息。eval 要谨慎使用。</td>
    </tr>
    <tr>
        <td><code>expr</code></td>
        <td>这个选项用于抑制表达式相关的一些错误信息。</td>
    </tr>
    <tr>
        <td><code>funcscope</code></td>
        <td>这个选项用于抑制在控制结构中声明变量在外部访问这些变量的警告信息。</td>
    </tr>
    <tr>
        <td><code>gcl</code></td>
        <td>让 JSHint 与 Goggle Closure Compiler 兼容的选项</td>
    </tr>
    <tr>
        <td><code>globalstrict</code></td>
        <td>这个选项抑制全局使用严格模式的警告信息</td>
    </tr>
    <tr>
        <td><code>iderator</code></td>
        <td>这个选项用于抑制使用诸如 <code>__iterator__</code> 属性的警告信息。因为这类属性并非所有浏览器支持，应该谨慎使用<td>        
    </tr>
    <tr>
        <td><code>lastsemic</code></td>
        <td>这个选项用于抑制分号缺省的警告，单仅仅是在语句最后的分号缺省的时候不发出警告信息。</td>
    </tr>
    <tr>
        <td><code>laxbreak</code></td>
        <td>这个选项抑制大多数代码中可能存在不安全的代码的警告。</td>
    </tr>
    <tr>
        <td><code>laxcomma</code></td>
        <td>这个选项用于抑制逗号前置的编码风格相关的警告信息。</td>
    </tr>
    <tr>
        <td><code>loopfunc</code></td>
        <td>这个选项用于抑制循环中有函数的警告信息。在循环内定义函数可能导致 bug</td>
    </tr>
    <tr>
        <td><code>maxerr</code></td>
        <td>这个选项允许设置警告信息的最大数量。默认为 50</td>        
    </tr>
    <tr>
        <td><code>moz</code></td>
        <td>告诉 JSHint 代码使用了 Mozilla JavaScript 扩展。https://developer.mozilla.org/en-US/docs/JavaScript/New_in_JavaScript/1.7</td>        
    </tr>
    <tr>
        <td><code>multistr</code></td>
        <td>这个选项用于抑制使用多行字符串时的警告信息。</td>
    </tr>
    <tr>
        <td><code>notypeof</code></td>
        <td>抑制使用 typeof 运算符的警告信息。</td>
    </tr>
    <tr>
        <td><code>proto</code></td>
        <td>抑制使用 <code>__proto__</code> 属性的警告信息。</td>
    </tr>
    <tr>
        <td><code>scripturl</code></td>
        <td>抑制使用诸如<code>javascript:..</code>的错误信息。</td>
    </tr>
    <tr>
        <td><code>smarttabs</code></td>
        <td>抑制混合使用 tabs 和空格的警告信息。</td>
    </tr>
    <tr>
        <td><code>shadow</code></td>
        <td>抑制变量阴影的警告，比如声明已经在外部声明的变量。</td>
    </tr>
    <tr>
        <td><code>sub</code></td>
        <td>抑制可以使用点表示法而使用[]表示法的错误信息。</td>
    </tr>
    <tr>
        <td><code>supernew</code></td>
        <td>抑制怪异的使用构造器，诸如 <code>new function() {...}</code>产生的警告信息</td>
    </tr>
    <tr>
        <td><code>validthis</code></td>
        <td>抑制在严格模式中或者在构造器中使用<code>this<code>时可能产生的警告信息。</td>
    </tr>
</table>