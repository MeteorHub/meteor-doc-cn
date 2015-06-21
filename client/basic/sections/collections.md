{{#template name="basicCollections"}}

<h2 id="collections"><span>Collections</span></h2>

Meteor用*集合*保存数据。集合里保存的Javascript对象叫做文档。使用`new Mongo.Collection`声明一个集合。

{{> autoApiBox name="Mongo.Collection" options=""}}

调用`Mongo.Collection`构造函数创建一个集合对象，它的行为就像MongoDB 的集合。如果在创建集合时传入了一个name参数，那么声明的就是一个持久性集合 &mdash; 保存在服务端并可以发布到客户端。

要想使客户端代码和服务端代码都可以通过相同的API访问相同的集合，最好在一个客户端和服务端都能加载到的javascript文件中把集合声明为全局变量。

示例：声明两个命名的，持久性的集合作为全局变量：

```
// In a JS file that's loaded on the client and the server
Posts = new Mongo.Collection("posts");
Comments = new Mongo.Collection("comments");
```

如果传入`null`作为name参数，那么创建出来的就是一个本地集合。本地集合不会在客户端和服务端之间进行同步；它只是javascript对象的临时集合，支持Mongo-style `find`, `insert`, `update`, 和 `remove`操作。

默认情况下，Meteor会自动发布所有集合里的文档到每一个连接上的客户端。要禁用此行为，必须移除`autopublish`包：

```
$ meteor remove autopublish
```

然后，使用[`Meteor.publish`](#meteor_publish) 和
[`Meteor.subscribe`](#meteor_subscribe)来指定集合的哪一部分发送到哪些客户端。

使用`findOne` 和 `find`从集合里检索文档。

{{> autoApiBox name="Mongo.Collection#findOne" options="sort;skip;fields"}}

`findOne`方法可以从集合里查找特定的文档。调用`findOne`时，通常都会传入一个特定文档的`_id`:

```
var post = Posts.findOne(postId);
```

然而，也可以给`findOne`传入一个Mongo 选择器，Mongo选择器是一个对象，指明了目标文档要满足的一系列属性。
例如，下面的选择器

```
var post = Posts.findOne({
  createdBy: "12345",
  title: {$regex: /first/}
});
```

会匹配到下面的文档

```
{
  createdBy: "12345",
  title: "My first post!",
  content: "Today was a good day."
}
```

关于MongoDB query operators 例如`$regex`, `$lt` (小于),
`$text` (文本搜索)的更多信息参见[MongoDB
documentation](http://docs.mongodb.org/manual/reference/operator/query/)。

一个非常有用但是不那么明显的功能就是Mongo 选择器可以匹配数组里的元素。例如：下面的选择器

```
Post.findOne({
  tags: "meteor"
});
```

会匹配到下面的文档

```
{
  title: "I love Meteor",
  createdBy: "242135223",
  tags: ["meteor", "javascript", "fun"]
}
```

`findOne`方法和[`Session.get`](#session_get)一样，也是响应式的，也就是说，如果你在[template helper](#template_helpers)或是 [`Tracker.autorun`](#tracker_autorun)回调函数中使用了`findOne`，如果`findOne`返回的文档发生了变化，那么模板就会自动重新渲染，计算会重新执行。

注意，如果`findOne`没有找到任何文档，则返回`null`，通常发生在文档还没加载或是已经从集合中移除，所以要有处理`null`值的准备。


{{> autoApiBox name="Mongo.Collection#find" options="sort;skip;limit;fields"}}

`find`方法和`findOne`类似，不同的是，它不返回单一文档，而是返回一个MongoDB *游标*。游标是一个特殊的对象，代表一个查询里会被返回的文档列表。可以在模板Helper里返回游标，或是其它可以返回数组的地方：

```
Template.blog.helpers({
  posts: function () {
    // this helper returns a cursor of
    // all of the posts in the collection
    return Posts.find();
  }
});
```

```
<!-- a template that renders multiple posts -->
<template name="blog">
  {{dstache}}#each posts}}
    <h1>{{dstache}}title}}</h1>
    <p>{{dstache}}content}}</p>
  {{dstache}}/each}}
</template>
```

要想从一个游标里检索当前的文档列表时，调用游标的`.fetch()`方法：

```
// get an array of posts
var postsArray = Posts.find().fetch();
```

记住，虽然调用`fetch`的计算会在数据变化时重新运行，但是the resulting array will not be reactive if it is
passed somewhere else.

通过调用`insert`,`update`, 或 `remove`来修改保存在`Mongo.Collection`里的数据。

{{> autoApiBox "Mongo.Collection#insert"}}

下面的例子展示了如何插入文档到集合里：

```
Posts.insert({
  createdBy: Meteor.userId(),
  createdAt: new Date(),
  title: "My first post!",
  content: "Today was a good day."
});
```

每个`Mongo.Collection`里的每个文档都有一个`_id`字段。它必须是唯一的，如果你没有提供，会自动生成。[`collection.findOne`](#findOne)使用`_id`可以用来检索特定的文档。

{{> autoApiBox "Mongo.Collection#update"}}

这里的选择器和你传给`find`方法的是一致的，它可以匹配多个文档。修改器是一个对象，它指明了对匹配到的文档要做的修改。注意：除非你使用了`$set`操作符，否则`update`方法会直接用修改器替换整个匹配到的文档。

下面的例子展示了，设置所有标题包含"first"的文章的内容字段

```
Posts.update({
  title: {$regex: /first/}
}, {
  $set: {content: "Tomorrow will be a great day."}
});
```

关于支持的所有operators参见[MongoDB documentation](http://docs.mongodb.org/manual/reference/operator/update/)。

有一点需要注意：当在客户端调用`update`的时候，只能通过`_id`来查找文档。要使用所有可用的选择器，必须在服务端代码或是
[method](#meteor_methods)中调用`update`。

{{> autoApiBox "Mongo.Collection#remove"}}

`remove`方法使用和`find`、`update`一样的选择器，并且从数据库中移除所有匹配到的文档。请小心使用`remove` &mdash; 删掉的数据没办法恢复。

和`update`一样，客户端代码只能通过`_id`移除文档，而服务端代码和[methods](#meteor_methods)可以使用任何选择器移除文档。

{{> autoApiBox name="Mongo.Collection#allow" options="insert, update, remove"}}

在新创建的APP中，Meteor允许任何客户端和服务端代码调用`insert`, `update`, 和
`remove` 。这是因为用`meteor create`创建的APP默认包含了`insecure`包，目的是简化开发。很显然，如果任何用户都可以修改数据库，这是很不安全的，所以移除`insecure`包，并声明一些权限规则是很重要的：

```
$ meteor remove insecure
```

一旦你移除了`insecure`包，就可以用`allow`和`deny`来控制谁可以执行哪些数据库操作。默认情况下，客户端所有的操作都被禁止，所以，需要添加一些`allow`规则。记住：服务端代码和[methods](#meteor_methods) 不受`allow`和`deny`影响
&mdash; 这些规则只应用于当不可信的客户端代码调用`insert`, `update`, 和 `remove`时。

例如，假设只有当`createBy`字段为当前用户ID时，才允许用户插入新文章，这样用户就不能冒充其他人

```
// In a file loaded on the server (ignored on the client)
Posts.allow({
  insert: function (userId, post) {
    // can only create posts where you are the author
    return post.createdBy === userId;
  },
  remove: function (userId, post) {
    // can only delete your own posts
    return post.createdBy === userId;
  }
  // since there is no update field, all updates
  // are automatically denied
});
```

`allow`方法接受三个回调函数：`insert`,`remove`,`update`。三个回调函数的第一个参数是登录用户的`_id`,其余的参数如下：

1. `insert(userId, document)`

    `document` 是将要插入到数据库的文档。如果允许插入，则返回`true`，否则返回`false`

2. `update(userId, document, fieldNames, modifier)`

    `document` 是将要被修改的文档。`fieldNames`是一个数组，包含了受这次修改影响的一级字段。
    `modifier`是传给`collection.update`的第二个参数[Mongo Modifier](#mongo_modifiers)。
    如果使用这个回调无法实现正确的校验，那么推荐使用[methods](#meteor_methods)。
    如果允许修改，则返回`true`，否则返回`false`

3. `remove(userId, document)`

    `document`是将要从数据库中移除的文档。如果允许移除则返回`true`，否则返回`false`


{{> autoApiBox name="Mongo.Collection#deny" options="insert, update, remove"}}

`deny`方法允许你选择性的重写`allow`规则。只要有一个`allow`回调函数返回`true`，就允许修改，但必须所有`deny`规则都返回false，才允许修改。

例如，我们要重写上面定义的`allow`规则：排除特定标题的文章：

```
// In a file loaded on the server (ignored on the client)
Posts.deny({
  insert: function (userId, post) {
    // Don't allow posts with a certain title
    return post.title === "First!";
  }
});
```

{{/template}}
