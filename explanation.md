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