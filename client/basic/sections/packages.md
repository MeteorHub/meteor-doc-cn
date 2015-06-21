{{#template name="basicPackages"}}

<h2 id="packages"><span>Packages</span></h2>

Meteor所有的功能都是以模块化的包实现的。除了上面提到的核心包之外，还有很多可以用来增加功能的包。

在命令行，添加和删除包使用`meteor add` 和 `meteor remove`:

```bash
# add the less package
meteor add less

# remove the less package
meteor remove less
```

当添加或是删除包的时候，APP会自动重启。APP的包依赖通过`.meteor/packages`记录跟踪，
当你的同事更新项目源代码后，会自动更新为相同的包，因为你们的`.meteor/packages`文件是相同的。

在APP目录下，运行`meteor list`可以查看APP使用了哪些包。

## Searching for packages

目前查找包最好的方式就是通过官方的Meteor包服务器[Atmosphere](https://atmospherejs.com/)。也可以直接通过命令`meteor search`搜索包。

包名中间带`:`的，例如：`mquandalle:jade`，说明是由社区成员编写和维护的。冒号前边是创建这个包的成员或组织名称。不带冒号的，说明是由Meteor Development Group维护，作为Meteor框架的一部分。

当前在Atmosphere上有超过两千个包。下面列出了一些非常有用的包。

## accounts-ui

Meteor 账户系统的插入式UI。添加这个包之后，用`{{dstache}}> loginButtons}}`插入到模板中。UI自动匹配任何已添加的登录服务，例如`accounts-password`, `accounts-facebook`等等。

[See the docs about accounts-ui above.](#/basic/accounts).

## coffeescript

在APP中使用[CoffeeScript](http://coffeescript.org/)。添加了这个包之后，所有以`.coffee`后缀结尾的文件都会由Meteor的构建系统编译为javascript。

## email

发送邮件。参见[email section of the full API
docs](#/full/email)

<h2 id="jade">mquandalle:jade</h2>

在APP中使用[Jade](http://jade-lang.com/)作为模板语言。添加包之后，任何以`.jade`作为后缀的文件都会编译为Meteor模板。详情参见[page on
Atmosphere](https://atmospherejs.com/mquandalle/jade)

## jquery

JQuery让跨浏览器进行HTML遍历，操作，事件处理，和动画操作变得简单。

JQuery自动添加到每个Meteor APP中，因为框架大量的使用了jQuery。详情参见[JQuery docs](http://jquery.com/) 。

## http

利用这个包可以在客户端和服务端使用相同的API发送HTTP请求。使用方法参见[http docs](#/full/http)。

## less

添加[LESS](http://lesscss.org/) CSS预处理器到APP，`.less`后缀的文件都会编译为常规CSS。如果想使用`@import`引入其他文件，同时又不想让Meteor自动编译它们，使用`.import.less`后缀。

## markdown

在模板中插入[Markdown](http://daringfireball.net/projects/markdown/syntax)代码。使用`{{dstache}}#
markdown}}`Helper很简单：

```html
<div class="my-div">
{{dstache}}#markdown}}
# My heading

Some paragraph text
{{dstache}}/markdown}}
</div>
```

确保你的markdown缩进正确。
## underscore

[Underscore](http://underscorejs.org/) 提供了一系列有用的函数，用来操作数组，对象，和函数。每个Meteor APP都包含`underscore`包，因为框架大量使用了underscore。

## spiderable

这个包可以让APP在服务端进行渲染，允许搜索引擎爬虫抓取页面内容。如果在意SEO的话，那么应该添加这个包。

{{/template}}
