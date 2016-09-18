```
  var property = function(key) {
    return function(obj) {
      return obj == null ? void 0 : obj[key];
    };
  };
```
这个函数是干嘛的呢？
看一下这段调用`var getLength = property('length');`大概就能理解了，其实就是一个简单的调用obj的原生属性的方法，至于为什么要用这种闭包的形式，我猜可能就是为了定义了这个getLength之后调用比较方便？
>Tips: 调用对象的属性两种方法.length和['length']昨天给我纠结了好久。

***
这里在介绍一下Array-like object这个概念。
字面上理解就是长得像数组的Object。我们常用的`document.getElementsByTagName()`所得到的就是一个Array-like object。同样特殊的参数变量`aruguments`也是。
Array-like object一般我们用到的有两个东西，一个是它体内的每个个体的索引index，
```
arugments[0]
```
另一个就是它的长度length
```
aruguments.length
```
但是呢，不要就把它当成一个数组了，它并没有数组的方法`push`, `forEach`, `indexOf`这些方法。
推荐一下Dr. Axel大神的[文章](http://www.2ality.com/2013/05/quirk-array-like-objects.htm),下面的评论也很精彩。