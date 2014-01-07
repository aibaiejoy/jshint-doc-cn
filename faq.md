# FAQ

### 如何关闭*tabs和空格混合*的警告

如果使用的是 [smart tabs](http://www.emacswiki.org/SmartTabs)，JSHint 提供了一个 `smarttabs` 选项。否则，解决方案就是指定自定义的 repoter 来运行 JSHint，在自定义的 reporter 中忽略不喜欢的警告信息。

- [示例](https://gist.github.com/3885619) 这个 reporter 所有关于 tabs 和空格混合的警告信息。

### 让 JSHint 跳过一些未使用的变量

假设有如下代码：

```javascript
function test(a, b, c) {
    return c;
}
```

接下来如果将 `unused` 选项设置为 `true`， JSHint 就不会对未使用的变量 `a` 和 `b` 发出警告。它会认为在使用的变量之后的未使用的变量是有意识这么做的，而不会认为它是一个错误。如果希望对所有出现过但未使用的变量发出警告信息，设置 `unused` 选项为 `strict` 即可：

```javascript
/*jshint unused:strict */
function test(a, b, c) {
  return c;
}
// Warning: unused variable 'a'
// Warning: unused variable 'b'
```
更多信息请查看完整[选项](options.md)列表中的 `unused` 选项。