{{#template name="basicApi"}}

<h1 id="api">The Meteor API</h1>

Javascript代码可以运行在两种环境：客户端(浏览器)，和服务端(服务器上的[Node.js](http://nodejs.org/)容器)。
在API 参考中，我们会指出每个函数是否可以在客户端，或是服务端，或是*Anywhere(客户端和服务端)*被调用。

{{> basicTemplates}}
{{> basicSession}}
{{> basicTracker}}
{{> basicCollections}}
{{> basicAccounts}}
{{> basicMethods}}
{{> basicPubsub}}
{{> basicEnvironment}}

{{/template}}