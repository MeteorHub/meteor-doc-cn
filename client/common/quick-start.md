{{#template name="quickStart"}}
## 快速开始!

Meteor 支持 [OS X, Windows, and Linux](https://github.com/meteor/meteor/wiki/Supported-Platforms).

用 Windows?  [下载官方安装包](https://install.meteor.com/windows).

用 OS X 或是 Linux?  通过命令行安装Meteor官方最新版：

```bash
$ curl https://install.meteor.com/ | sh
```

Windows 安装包支持 Windows 7, Windows 8.1, Windows Server
2008, 和 Windows Server 2012。  通过命令行安装，支持 Mac OS X
10.7 (Lion) 及其以上版本,  Linux x86 和 x86_64 架构.

安装完成之后，创建一个项目：

```bash
$ meteor create myapp
```

在本地运行：

```bash
$ cd myapp
$ meteor
# Meteor server running on: http://localhost:3000/
```

然后，打开一个命令行窗口，发布到线上（我们提供的一个免费服务器）

```bash
$ meteor deploy myapp.meteor.com
```
{{/template}}
