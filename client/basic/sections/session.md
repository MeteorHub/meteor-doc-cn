{{#template name="basicSession"}}

<h2 id="session"><span>Session</span></h2>

在客户端，`Session`提供了一个全局对象，可以用它来保存任意的键值对。例如：保存列表中当前选中项。

`Session`的特殊之处在于，它是_响应式_的。如果在[template helper](#template_helpers)或[`Tracker.autorun`](#tracker_autorun)里调用了
`Session.get("myKey")`，那么无论何时调用`Session.set("myKey", newValue)` 都会触发相应的模板片段自动重新渲染。

{{> autoApiBox "Session.set"}}
<!-- XXX The Session.set API box is a little wonky -->

{{> autoApiBox "Session.get"}}

例如：

```
<!-- In your template -->
<template name="main">
  <p>We've always been at war with {{dstache}}theEnemy}}.</p>
</template>
```

```
// In your JavaScript
Template.main.helpers({
  theEnemy: function () {
    return Session.get("enemy");
  }
});

Session.set("enemy", "Eastasia");
// Page will say "We've always been at war with Eastasia"

Session.set("enemy", "Eurasia");
// Page will change to say "We've always been at war with Eurasia"
```

`Seesion`带我们初次体会到了_响应式_的魅力，视图会在必要时自动更新，无需手动调用`render`函数。
在下一节，将会学习如何使用Tracker，一个非常轻量的库，使响应式成为可能。

{{/template}}
