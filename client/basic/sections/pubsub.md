{{#template name="basicPubsub"}}

<h2 id="pubsub"><span>发布和订阅</span></h2>

Meteor 服务端可以通过`Meteor.publish`发布文档集，同时客户端可以通过`Meteor.subscribe`订阅这些发布。
任何客户端订阅的文档都可以通过`find`方法进行查询使用。

默认情况下，每个新创建的 Meteor 应用包含有 autopublish 包，它会自动为每个客户端发布所有可用的文档。
为了可以更细化的控制不同客户端所接收的数据文档,首先应该在终端移除 autopublish：

```
$ meteor remove autopublish
```

现在，你可以使用`Meteor.publish` 和 `Meteor.subscribe` 来控制不同的文档从服务端推送到不同的客户端了。

{{> autoApiBox "Meteor.publish"}}

将数据发布到客户端，通过在服务端调用 Meteor.publish，需要传入两个参数：该记录集的名称，以及一个会在每一次客户端订阅该记录集的时候被调用的发布功能函数。

发布功能通过返回在一些`collection`调用`collection.find(query)`的结果。通过`query`来限制发布的文档集：

```
// 发布已登录用户的文章集合
Meteor.publish("posts", function () {
  return Posts.find({ createdBy: this.userId });
});
```

你可以发布来自多个collection的文档，通过返回一个`collection.find` 结果集合:

```
// 发布一个单独的文章和对应的评论
Meteor.publish("postAndComments", function (postId) {
  // 检查参数
  check(postId, String);

  return [
    Posts.find({ _id: postId }),
    Comments.find({ postId: roomId })
  ];
});
```

在发布功能里，`this.userId`是当前登录用户的`_id`, 在某些文档仅对特定用户可见时，可以用其来过滤集合。
如果客户端登录的用户发生改变，发布功能也将自动使用新的`userId`,所以新用户无法访问任何只限于之前登录用户的文档。

{{> autoApiBox "Meteor.subscribe"}}

客户端调用 `Meteor.subscribe`来订阅服务端发布的文档集合。
客户端可以进一步过滤文档集合通过调用[`collection.find(query)`](#find).
当任何通过发布功能发布的可访问的数据在服务端发生改变时，发布功能会自动返回和更新发布的文档集合推送给客户端。

`onReady`回调函数在服务端已经发送了订阅的所有初始化数据时被调用，无需传入任何参数。
`onStop`订阅不管以任何原因被终止时调用 ;如果订阅失败是由于服务端错误，它将收到一个[`Meteor.Error`](#meteor_error)。

`Meteor.subscribe` 返回一个订阅句柄，是一个包含以下方法的对象：

<dl class="callbacks">
{{#dtdd "stop()"}}
取消订阅。这通常会导致服务器引导客户端从客户端缓存中删除订阅的数据。
{{/dtdd}}

{{#dtdd "ready()"}}
如果服务端已经[设置状态为ready](#publish_ready)，将会返回 True。一个反应式数据源。
{{/dtdd}}
</dl>

如果你在[`Tracker.autorun`](#tracker_autorun)里调用`Meteor.subscribe`, 当运算返回时，订阅将被自动取消(所以如果合适的话，一个新的订阅会被创建),
意味着你不能够在来自`Tracker.autorun`里的订阅中调用`stop`。

{{/template}}
