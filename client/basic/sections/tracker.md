{{#template name="basicTracker"}}

<h2 id="tracker"><span>Tracker</span></h2>

Meteor 包含一个简单地依赖跟踪系统，使用它可以在[`Session`](#session)变量、数据库查询或其它数据源发生变化时自动重新渲染模板或是重新运行某些函数。

和其它的依赖跟踪系统不同，不需要手工声明依赖&mdash; 它就能工作。它的机制简单而高效。一旦你用`Tracker.autorun`初始化了一个计算(computation), `Tracker` 自动记录使用到的数据。当这些数据发生变化时，计算就会自动重新运行。这就是为什么当[helper
functions](#template_helpers)返回新数据时模板知道如何重新渲染。

{{> autoApiBox "Tracker.autorun" }}

`Tracker.autorun`使你可以声明一个依赖响应式数据源的函数，无论何时数据源发生变化，函数都会被重新执行。

例如，可以监测一个`Session`变量，设置另外一个：

```
Tracker.autorun(function () {
  var celsius = Session.get("celsius");
  Session.set("fahrenheit", celsius * 9/5 + 32);
});
```

或者可以等待session变量成为一个特定值，执行一些特定操作。如果想阻止回调函数进一步重新运行，可以调用计算(computation)对象的`stop`，计算对象会作为回调函数的第一个参数传入：

```
// Initialize a session variable called "counter" to 0
Session.set("counter", 0);

// The autorun function runs but does not alert (counter: 0)
Tracker.autorun(function (computation) {
  if (Session.get("counter") === 2) {
    computation.stop();
    alert("counter reached two");
  }
});

// The autorun function runs but does not alert (counter: 1)
Session.set("counter", Session.get("counter") + 1);

// The autorun function runs and alerts "counter reached two"
Session.set("counter", Session.get("counter") + 1);

// The autorun function no longer runs (counter: 3)
Session.set("counter", Session.get("counter") + 1);
```

`Tracker.autorun`第一次被调用的时候，回调函数立即被执行，如果此时`counter === 2`，那么就会alert，然后立即停止。在上面的例子中，当`Tracker.autorun`被调用时，`Session.get("counter") === 0`，所以第一次什么都不会发生，每次`counter`发生变化时，回调函数都会重新运行，直到当`counter` 等于`2`的时候，`computation.stop()`被调用。

如果autorun在第一次执行时抛出了异常，那么计算(computation)会自动停止，以后也不会重新运行。

关于`Tracker`的工作原理和高级用法，参见<a href="http://manual.meteor.com/">Meteor 手册</a>里的<a href="http://manual.meteor.com/#tracker">Tracker</a>一节，里面有更加详细的说明。

{{/template}}
