# 自定义 JSHint reporter

JSHint Reporter 就是一个 JavaScript 文件，接受来自 JSHint 的未经处理的数据然后输出一些信息到控制台或者令一个文件中，或者发个email神马的。

下面有个简单的例子，无论 JSHint 执行成功与否都打印一个消息。

```javascript
module.exports = {
    reporter: function(errors){
        console.log(errors.length ? 'fail' : 'ok');
    }
};
```

每个 reporter 文件都必须暴露一个函数，`reporter`，接受 errors 数组。该数组的每个条目的结构如下所示：

```javascript
{
  file:        [string, filename]
  error: {
    id:        [string, usually '(error)'],
    code:      [string, error/warning code],
    reason:    [string, error/warning message],
    evidence:  [string, a piece of code that generated this error]
    line:      [number]
    character: [number]
    scope:     [string, message scope;
                usually '(main)' unless the code was eval'ed]
    [+ a few other legacy fields that you don't need to worry about.]
  }
}
```
下面是一个真实的例子：

```javascript
[
  {
    file: 'demo.js',
    error:  {
      id: '(error)',
      code: 'W117',
      reason: '\'module\' is not defined.'
      evidence: 'module.exports = {',
      line: 3,
      character: 1,
      scope: '(main)',
      // [...]
    }
  },
  // [...]
]
```

如果还有疑惑，看看 JSHint repo 中的[例子](https://github.com/jshint/jshint/blob/master/examples/reporter.js)吧。