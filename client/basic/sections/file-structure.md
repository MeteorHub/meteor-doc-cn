{{#template name="basicFileStructure"}}

## 文件结构

对于该如何组织应用的文件结构，Meteor是非常灵活的。它会自动加载所有文件，所以不需要再用`<script>` 或 `<link>`标签来引入javascript和CSS。

### 文件的默认加载

如果某个文件在下面提到的特殊文件夹之外，Meteor会做如下处理：

1. HTML模板编译完成后发送到客户端。详细信息参见[the templates section](#/basic/templates)。
2. CSS文件发送到客户端。在生产模式下，会自动合并、压缩。
3. Javascript被加载到客户端和服务端。可以使用`Meteor.isClient` 和 `Meteor.isServer` 来控制特定代码的运行位置。

如果你想更好的控制哪些javascript代码加载到客户端或是服务端，可以使用下面列出的特殊文件夹。

### 特殊文件夹

#### `/client`

`/client`文件夹中所有文件都只发送到客户端。用来放置HTML，CSS和UI相关的javascript代码。

#### `/server`

`/server`文件夹中所有文件都只提供给服务端使用，不会发送到客户端。用来放置不应该被客户端看到的敏感逻辑和数据。

#### `/public`

`/public`文件夹中的文件会原样发送到客户端。用来放置资源，例如：图片。假设有张图片`/public/background.png`，
在HTML中用 `<img src='/background.png'/> `引用或是在CSS中用 `background-image: url(/background.png)` 引用。
注意图片URL中不包含`/public`。

#### `/private`

`/private`文件夹中的文件只能由服务端代码通过[`Assets`](#assets) API 来获取，客户端无法获取。

更多关于文件加载顺序和特殊文件夹的说明参见[Structuring Your App section](#/full/structuringyourapp)

{{/template}}