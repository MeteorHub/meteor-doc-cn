{{#template name="basicAccounts"}}

<h2 id="accounts"><span>Accounts</span></h2>

要增加账户功能，用`meteor add`添加下面的一个或多个包：

- `accounts-ui`:这个包允许你通过在模板中使用`{{dstache}}> loginButtons}}`，来添加自动生成的登录UI，
  用户可以登录。社区中有其它的替代选择，或者你也可以结合使用[advanced Accounts methods](#accounts)
- `accounts-password`: 这个包允许用户通过密码登录。添加之后，`loginButtons`下拉框会自动增加邮箱和密码文本域。
- `accounts-facebook`, `accounts-google`, `accounts-github`, `accounts-twitter`,
  以及其它由社区贡献的第三方登录包，让你的用户可以通过第三方网站登录。
  它们会自动添加登录按钮到`loginButtons`下拉框中。

<h3 id="loginButtons" class="api-title">
  <a class="name selflink" href="#b-loginButtons">{{dstache}}> loginButtons}}</a>
  <span class="locus">Client</span>
</h3>

在HTML中引入`loginButtions`模板，就可以使用Meteor默认的登录UI。使用前，需要先添加`accounts-ui`包：

```
$ meteor add accounts-ui
```

{{> autoApiBox "Meteor.user"}}

从 [`Meteor.users`](#meteor_users) 集合中获取当前登录用户。等同于`Meteor.users.findOne(Meteor.userId())`。

{{> autoApiBox "Meteor.userId"}}

{{> autoApiBox "Meteor.users"}}

这个集合包含了所有注册用户，每个用户是一个文档。例如：

```
{
  _id: "bbca5d6a-2156-41c4-89da-0329e8c99a4f",  // Meteor.userId()
  username: "cool_kid_13", // unique name
  emails: [
    // each email address can only belong to one user.
    { address: "cool@example.com", verified: true },
    { address: "another@different.com", verified: false }
  ],
  createdAt: Wed Aug 21 2013 15:16:52 GMT-0700 (PDT),
  profile: {
    // The profile is writable by the user by default.
    name: "Joe Schmoe"
  },
  services: {
    facebook: {
      id: "709050", // facebook id
      accessToken: "AAACCgdX7G2...AbV9AZDZD"
    },
    resume: {
      loginTokens: [
        { token: "97e8c205-c7e4-47c9-9bea-8e2ccc0694cd",
          when: 1349761684048 }
      ]
    }
  }
}
```

一个用户文档可以包含任何你想保存的用户相关的数据。不过，Meteor会特殊对待下面的几个字段：

- `username`: 一个唯一的字符串，可以标识用户。
- `emails`: 一个对象的数组。对象包含属性
  `address` 和 `verified` 。一个邮箱地址只能属于一个用户。`verified`是一个布尔值，如果用户已经[验证
  邮箱地址](#accounts_verifyemail)则为true。
- `createdAt`: 用户文档创建时间。
- `profile`: 一个对象，默认情况下用户可以用任何数据新建和更新该字段。
- `services`: 包含第三方登录服务使用的数据的对象。例如，它的`reset`字段包含的token，用于
  [忘记密码](#accounts_forgotpassword)的超链接，它的`resume`字段包含的token，用于维持用户登录状态。

和所有的[Mongo.Collection](#collections)一样，在服务端，你可以获取用户集合
的所有文档，但是在客户端只能获取那些服务端发布的文档。

默认情况下，当前用户的`username`,`emails`,和`profile`会发布到客户端。
可以使用下面的代码发布当前用户的其它字段：

    // server
    Meteor.publish("userData", function () {
      if (this.userId) {
        return Meteor.users.find({_id: this.userId},
                                 {fields: {'other': 1, 'things': 1}});
      } else {
        this.ready();
      }
    });

    // client
    Meteor.subscribe("userData");

如果安装了autopublish包，那么所有用户的信息都会发布到所有客户端。包括`username`,
`profile` ,以及`service`中所有可以公开的字段(例如：`services.facebook.id`,
`services.twitter.screenName`)。另外，使用autopublish时，对于当前登录用户会发布更多的信息，
包括access token。这样就可以直接从客户端发起API调用。


默认情况下，用户可以通过[`Accounts.createUser`](#accounts_createuser)声明自己的`profile`字段，
也可以通过`Meteor.users.update`来修改它。要允许用户修改更多的字段，使用[`Meteor.users.allow`](#allow)
，要禁止用户对自己的文档做任何修改，使用：

    Meteor.users.deny({update: function () { return true; }});


{{> autoApiBox "currentUser"}}

{{/template}}
