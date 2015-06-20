{{#template name="basicEnvironment"}}

<h2 id="environment"><span>Environment</span></h2>

{{> autoApiBox "Meteor.isClient"}}
{{> autoApiBox "Meteor.isServer"}}

{{#note}}
`Meteor.isServer`可以用来限制代码的运行位置，但是它不会阻止代码发送到客户端。任何你不想发送到客户端的敏感代码，例如包含密码或是认证机制的代码，都应该放到`server`文件夹。
{{/note}}

{{> autoApiBox "Meteor.startup"}}

在服务端，只要服务进程启动完成，回调函数就会执行。在客户端，只要页面ready，回调函数就会执行。

最佳实践是：把模板事件，模板Helper，`Meteor.methods`, `Meteor.publish`, 或是
`Meteor.subscribe` 之外的代码包裹进 `Meteor.startup`，这样APP的代码就不会在环境准备好之前运行。

例如：当服务端启动时，如果数据库为空则创建一些初始数据，可以用下面的方式：

```
if (Meteor.isServer) {
  Meteor.startup(function () {
    if (Rooms.find().count() === 0) {
      Rooms.insert({name: "Initial room"});
    }
  });
}
```

如果在服务端进程启动完成之后，或是客户端，页面ready之后调用`Meteor.startup`，回调函数会立即执行。<!-- XXX It should still fire asynchronously, though -->

{{/template}}
