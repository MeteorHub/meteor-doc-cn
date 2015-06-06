{{#template name="basicMobile"}}

## 开发手机 Apps

当你用Meteor开发完成web app之后，只需几个命令，就可以轻松地构建一个native包装，并发布到Google Play Store或是iOS App Store。
我们用了很大精力，使相同的包和API在桌面端和移动端都能工作，所以你无须担心大量的移动应用开发相关的边界情况。

### 安装手机 SDKs

安装Android 或ios 开发工具：

```bash
meteor install-sdk android     # for Android
meteor install-sdk ios         # for iOS
```

### 添加平台

添加相关平台到你的应用：

```bash
meteor add-platform android    # for Android
meteor add-platform ios        # for iOS
```

### 运行模拟器

```bash
meteor run android             # for Android
meteor run ios                 # for iOS
```

### 在设备上运行

```bash
meteor run android-device      # for Android
meteor run ios-device          # for iOS
```

### 配置APP图标和metadata

可以用特殊的文件[`mobile-config.js`](#/full/mobileconfigjs)配置APP的图标、标题、版本号、启动画面以及其他metadata。

更多关于Meteor对移动端的支持参见[GitHub wiki page](https://github.com/meteor/meteor/wiki/Meteor-Cordova-Phonegap-integration)。
{{/template}}