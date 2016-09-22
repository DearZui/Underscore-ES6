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

***
`iteratee`的实现比较复杂，我们延后再谈。

继续看`_.each`方法。

其中有个`var keys = _.keys(obj);`,是在确实我们的obj不是一个Array-like object之后的定义。`_.keys(object)`方法用来检索**object**拥有的所有可枚举属性的名称。
```
_keys({{one: 1, two: 2, three: 3}})  //["one", "two", "three"]
```
首先判断传入obj是不是个对象，这里的`_.isObject()`是判断是否为对象的一个方法，很简单。IE9以上的浏览器有个原生方法[Object.keys(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys),和我们要实现的方法所达到的功能是一样。那么IE9以下呢，我们用一个循环，把它里面的所有key都放进keys数组。接下来又是一句难懂的，`hasEnumBug`这又是啥玩意? 我咋无论在哪个浏览器里面测`!{toString: null}.propertyIsEnumerable('toString');`返回都是false呢，那到底它什么时候会true呢?想到了红宝书里面似乎有讲这个**数据属性**，4个特性不具体说了，其中的Enumerable设置为false后就不能用for-in循环来返回属性了。`collectNonEnumProps`这个函数就来解决这个问题。

***
***
其实each和map是很类似的，它们之间有啥区别呢，我们通过代码来瞅瞅

两者实现的方式很类似，最大的区别为返回值

map返回的值`results`为一个新定义的数组，而each返回的是参数中的`obj`;

所以两者的区别就在`each(obj, iteratee)`中的iteratee是会改变obj的，而map返回的是一个新的iteratee过的数组。