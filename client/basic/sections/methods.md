{{#template name="basicMethods"}}
<h2 id="methods"><span>Methods</span></h2>

Methods 是可以从客户端调用的服务端函数。当你想做一些比`insert`, `update` 或是 `remove`复杂的事情，或是进行数据验证，而`allow` 和 `deny`又无法满足需求时，使用Methods非常合适。

Methods 可以返回值或是抛出错误。

{{> autoApiBox "Meteor.methods"}}

在服务端调用`Meteor.methods`定义的函数可以在客户端远程调用。下面是一个method的例子，检查参数，抛出错误：

```
// On the server
Meteor.methods({
  commentOnPost: function (comment, postId) {
    // Check argument types
    check(comment, String);
    check(postId, String);

    if (! this.userId) {
      throw new Meteor.Error("not-logged-in",
        "Must be logged in to post a comment.");
    }

    // ... do stuff ...

    return "something";
  },

  otherMethod: function () {
    // ... do other stuff ...
  }
});
```

[`check`](#check)函数用于确保method的参数是期望的[类型和结构](#matchpatterns)。

在method定义中，`this`绑定到一个method调用对象，method调用对象有几个非常有用的属性，包括：用于标识当前登录用户的`this.userId`。

你不用把所有的method定义都放到一个`Meteor.methods`里面；可以多次调用`Meteor.methods`，只要每个method的名字是唯一的。

### Latency Compensation

调用服务端的一个method会产生在网络上一个来回的延迟。如果用户由于这个延迟，需要等待一秒才能看到自己的评论显示出来，会让人非常不爽。这就是为什么Meteor有一个叫做 _method stubs_ 的功能。如果你在客户端定义了一个和服务端同名的method，Meteor会运行它，尝试预测服务端运行的结果。当服务端代码执行完毕后，客户端生成的预测结果会被实际结果替代。

[`insert`](#insert), [`update`](#update), 和[`remove`](#remove)的客户端版本，都是以method方式实现，这样在客户端与数据库进行交互的结果就会立即呈现。

{{> autoApiBox "Meteor.call"}}

用上面的方法来调用method。

### On the client

在客户端调用的Method是异步执行，为了能够获取执行结果，你需要传入一个回调函数。回调函数会收到两个参数`error` 和 `result`。除非有异常抛出，否则`error`参数为`null`。当有异常抛出时，`error`参数是`Meteor.Error`的一个实例，同时`result`参数是undefined。

示例：调用`commentOnPost`method ，传入两个参数`comment` 和 `postId`：

```
// Asynchronous call with a callback on the client
Meteor.call('commentOnPost', comment, postId, function (error, result) {
  if (error) {
    // handle error
  } else {
    // examine result
  }
});
```

作为方法调用的一部分，Meteor会跟踪数据库的更新，直到所有的更新都发送到客户端之后才调用客户端回调函数。

### On the server

在服务端，不用传入回调函数 &mdash; 方法调用会阻塞直到执行完毕，返回结果或是抛出异常，就好像直接调用函数一样：

```js
// Synchronous call on the server with no callback
var result = Meteor.call('commentOnPost', comment, postId);
```

{{> autoApiBox "Meteor.Error"}}

如果想在method中返回一个错误，使用抛出异常。Method可以抛出任意类型的异常，但是只有`Meteor.Error`类型的错误会发送到客户端。如果method抛出一个其它类型的异常，客户端会收到`Meteor.Error(500, 'Internal server error')`。

{{/template}}
