{{#template name="commandLine"}}

<h2 id="command-line">命令行工具</h2>

#### `meteor help`

获取 `meteor` 命令行使用帮助。运行 `meteor help` 会列出`meteor`所有命令。运行` meteor help
<command> `会打印出关于`meteor <command>`的详细帮助。

#### `meteor create <name>`

创建一个名为`<name>`的子目录，并在里面新建一个Meteor应用。

#### `meteor run`

使用Meteor本地开发服务器运行当前应用，地址为：[http://localhost:3000](http://localhost:3000)

#### `meteor debug`

附带着Node Inspector 运行项目，这样你就可以一步一步跟踪服务端代码。更多信息查看[`meteor debug`](#/full/meteordebug)

#### `meteor deploy <site>`

打包你的应用，并发布到`<site>`。如果你发布到`<your app>.meteor.com`，Meteor提供免费的主机，只要`<your app>`的名字还未被其他人使用。

#### `meteor update`

升级Meteor到最新发布版，然后（如果`meteor update`是在一个应用目录中执行的）升级当前项目使用的包到最新兼容版。

#### `meteor add`

添加一个包（或多个）到Meteor项目中。要查询可用的包，使用`meteor search`命令。

#### `meteor remove`

移除之前添加到项目中的包。查看项目使用的包列表，用`meteor list` 命令。

#### `meteor mongo`

打开一个MongoDB shell来查看、操作数据库中的集合。注意：你必须先运行当前应用(在另外的命令行)，`meteor mongo`才能连接到应用的数据库

#### `meteor reset`

重置当前项目为初始状态。移除所有本地数据。

如果你经常使用`meteor reset` ，但是又不想丢失一些初始数据，考虑使用[`Meteor.startup`](#/basic/Meteor-startup) 在服务第一次启动时重建这些数据。

```
if (Meteor.isServer) {
  Meteor.startup(function () {
    if (Rooms.find().count() === 0) {
      Rooms.insert({name: "Initial room"});
    }
  });
}
```

{{/template}}
