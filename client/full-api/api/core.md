{{#template name="apiCore"}}

<h2 id="core"><span>Meteor 核心</span></h2>

{{> autoApiBox "Meteor.isClient"}}
{{> autoApiBox "Meteor.isServer"}}

{{#note}}
`Meteor.isServer` 可以用来限制代码运行的位置，但是不能阻止代码被发送到客户端。
任何你不希望提供给客户端的敏感代码，例如：包括密码、认证机制的代码，都应该保存在 `server` 目录中。
{{/note}}

{{> autoApiBox "Meteor.isCordova"}}

{{> autoApiBox "Meteor.startup"}}

在服务端，当服务进程启动完毕，就会立即执行上面的函数。
在客户端，当 DOM 准备好之后，就会立即执行上面的函数。

`startup` 回调函数被调用的顺序与调用 `Meteor.startup` 的顺序一致。

在客户端，packages 里的 `startup` 回调会首先被调用，随后是 `.html` 文件里的 `<body>` 模版，接下来是应用代码。

    // On server startup, if the database is empty, create some initial data.
    if (Meteor.isServer) {
      Meteor.startup(function () {
        if (Rooms.find().count() === 0) {
          Rooms.insert({name: "Initial room"});
        }
      });
    }

{{> autoApiBox "Meteor.wrapAsync"}}

{{> autoApiBox "Meteor.absoluteUrl"}}

{{> autoApiBox "Meteor.settings"}}

{{> autoApiBox "Meteor.release"}}

{{/template}}