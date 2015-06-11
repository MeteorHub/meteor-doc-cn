{{#template name="basicTemplates"}}

<h2 id="templates"><span>模板</span></h2>

在Meteor中，视图定义在 _模板_ 。模板就是包含动态数据的HTML代码段。你可以通过javascript给模板插入数据，或是监听模板事件。

<h3 class="api-title" id="defining-templates">用HTML定义模板</h3>

模板定义在`.html`文件中，可以放在项目任何位置，除了`server`,`public`,`private`文件夹

每个`.html`文件可以包含任意数量的顶级元素：`<head>`,`<body>`或是`<template>`。`<head>`,`<body>`标签里的代码会附加到HTML页面中对应的标签，
`<tempalte>`标签里的代码可以用`{{dstache}}> templateName}}`引入，如下面的例子所示。模板可以引入多次&mdash;模板的主要目的之一就是避免重复手写相同的HTML。

```
<!-- add code to the <head> of the page -->
<head>
  <title>My website!</title>
</head>

<!-- add code to the <body> of the page -->
<body>
  <h1>Hello!</h1>
  {{dstache}}> welcomePage}}
</body>

<!-- define a template called welcomePage -->
<template name="welcomePage">
  <p>Welcome to my website!</p>
</template>
```

`{{dstache}} ... }}`是Spacebars语法，Meteor使用Spacebars给HTML增加功能。如上所示，利用它你可以引入模板。
使用Spacebars，你可以显示从 _helpers_ 中获取的数据。Helpers 用javascript来写，既可以是简单值，也可以是函数。

{{> autoApiBox "Template#helpers"}}

给`nametag`模板定义一个叫做`name`的helper(在javascript里)：

```
Template.nametag.helpers({
  name: "Ben Bitdiddle"
});
```

`nametag` 模板本身 (在HTML里):

```
<!-- In an HTML file, display the value of the helper -->
<template name="nametag">
  <p>My name is {{dstache}}name}}.</p>
</template>
```

Spacebars还有几个方便使用的控制结构，可以使视图更加动态：

- `{{dstache}}#each data}} ... {{dstache}}/each}}` - 循环`data`里的每一项，每一项都会显示一次`#each`块里的HTML。
- `{{dstache}}#if data}} ... {{dstache}}else}} ... {{dstache}}/if}}` - 如果`data`为`true`，显示第一个块，反之，显示第二个块。
- `{{dstache}}#with data}} ... {{dstache}}/with}}` - 设置内部HTML的数据上下文，然后显示。

每一个嵌套的`#each`或`#with`块都有自己的 _数据上下文_ ,数据上下文是一个对象，它的属性在块内部可以用作helper。例如：`#with`块，它的数据上下文就是跟在它后面，`}}`之前的变量值。`#each`块，循环到的元素会作为当前数据上下文。

例如，`people` helper 的值为：

```
Template.welcomePage.helpers({
  people: [{name: "Bob"}, {name: "Frank"}, {name: "Alice"}]
});
```

然后你可以用一个`<p>`标签列表显示每一个人的姓名：

```html
{{dstache}}#each people}}
  <p>{{dstache}}name}}</p>
{{dstache}}/each}}
```

或是用上面的"nametag"模板代替 `<p>`标签：

```html
{{dstache}}#each people}}
  {{dstache}}> nametag}}
{{dstache}}/each}}
```

记住：helper可以是简答值，也可以是函数。例如，要显示登录用户的用户名，你可以定义一个叫做`username`的helper：


```
// in your JS file
Template.profilePage.helpers({
  username: function () {
    return Meteor.user() && Meteor.user().username;
  }
});
```

现在，每次使用`username` helper的时候，都会调用上面的helper函数来确定用户名：

```
<!-- in your HTML -->
<template name="profilePage">
  <p>Profile page for {{dstache}}username}}</p>
</template>
```

Helper可以接收参数。例如，

```js
Template.post.helpers({
  commentCount: function (numComments) {
    if (numComments === 1) {
      return "1 comment";
    } else {
      return numComments + " comments";
    }
  }
});
```

参数放在大括号里，helper名之后：

```html
<p>There are {{dstache}}commentCount 3}}.</p>
```

上面定义的helper都关联到特定的模板，可以用[`Template.registerHelper`](#template_registerhelper)定义所有模板都能用的helper。

关于Spacebars更详细的文档参见[README on GitHub](https://github.com/meteor/meteor/blob/devel/packages/spacebars/README.md)。后面的`Session`, `Tracker`,`Collections`, 和 `Accounts`小节会有如何给模板添加动态数据的讨论。


{{> autoApiBox "Template#events"}}

传给`Template.myTemplate.events`的事件map,用事件描述符作为key,事件处理函数作为value。事件处理函数接收两个参数：事件对象和模板实例。事件处理函数中通过`this`获取数据上下文。

假设有下面的模板：

```
<template name="example">
  {{dstache}}#with myHelper}}
    <button class="my-button">My button</button>
    <form>
      <input type="text" name="myInput" />
      <input type="submit" value="Submit Form" />
    </form>
  {{dstache}}/with}}
</template>
```

调用 `Template.example.events` 给模板增加事件处理器：

```
Template.example.events({
  "click .my-button": function (event, template) {
    alert("My button was clicked!");
  },
  "submit form": function (event, template) {
    var inputValue = event.target.myInput.value;
    var helperValue = this;
    alert(inputValue, helperValue);
  }
});
```

key的前半部分是要捕获的事件名称。几乎支持所有的DOM事件，常见的有：`click`, `mousedown`, `mouseup`, `mouseenter`, `mouseleave`, `keydown`, `keyup`, `keypress`, `focus`, `blur`, 和 `change`。

key的后半部分是CSS选择器，用来指明要监听的元素。几乎支持所有[JQuery支持的选择器](http://api.jquery.com/category/selectors/).

无论何时，选定元素上触发了监听的事件时，对应的事件处理函数就会被调用，参数为：DOM 事件对象和模板实例。详细信息参见[Event Maps section](#eventmaps)
<!-- TODO Update the link to full docs for Event Maps -->

{{> autoApiBox "Template#onRendered"}}

The functions added with this method are called once for every instance of
*Template.myTemplate* when it is inserted into the page for the first time.

These callbacks can be used to integrate external libraries that
aren't familiar with Meteor's automatic view rendering, and need to be
initialized every time HTML is inserted into the page.
You can perform initialization or clean-up on any objects in
[`onCreated`](#template_oncreated) and [`onDestroyed`](#template_ondestroyed)
callbacks.

For example, to use the HighlightJS library to apply code highlighting to
all `<pre>` elements inside the `codeSample` template, you might pass
the following function to `Template.codeSample.onRendered`:

```
Template.codeSample.onRendered(function () {
  hljs.highlightBlock(this.findAll('pre'));
});
```

In the callback function, `this` is bound to a [template
instance](#template_inst) object that is unique to this inclusion of the
template and remains across re-renderings. You can use methods like
[`this.find`](#template_find) and
[`this.findAll`](#template_findAll) to access DOM nodes in the template's
rendered HTML.

<h2 id="template_inst"><span>Template instances</span></h2>

A template instance object represents a single inclusion of a template in the
document.  It can be used to access the HTML elements inside the template and it
can be assigned properties that persist as the template is reactively updated.

Template instance objects can be found in several places:

1. The value of `this` in the `created`, `rendered`,
   and `destroyed` template callbacks
2. The second argument to event handlers
3. As [`Template.instance()`](#template_instance) inside helpers

You can assign additional properties of your choice to the template instance to
keep track of any state relevant to the template. For example, when using the
Google Maps API you could attach the `map` object to the current template
instance to be able to refer to it in helpers and event handlers. Use the
[`onCreated`](#template_onCreated) and [`onDestroyed`](#template_onDestroyed)
callbacks to perform initialization or clean-up.

{{> autoApiBox "Blaze.TemplateInstance#findAll"}}

`template.findAll` returns an array of DOM elements matching `selector`. You can
also use `template.$`, which works exactly like the JQuery `$` function but only
returns elements within `template`.

{{> autoApiBox "Blaze.TemplateInstance#find"}}

<!-- XXX Why is this not findOne? -->

`find` is just like `findAll` but only returns the first element found. Like
`findAll`, `find` only returns elements from inside the template.

{{/template}}
