# meteor-doc-cn
Meteor中文文档

Meteor中文社区[MeteorHub](http://www.meteorhub.org/)，新社区，建设中。感兴趣的童鞋可以一起参与进来。

**如果想贡献翻译或者校对，请加QQ群：327885034，感谢**

## 翻译进度

## 贡献力量

如果想做出贡献的话，你可以：

- 帮忙校对，挑错别字、病句等等
- 提出修改建议
- 提出术语翻译建议

## 贡献流程

### 如果是挑错别字，病句

直接在仓库中找到对应的文件，在线编辑修改提交即可。

### 如果是提修改建议

直接提issue就OK

### 如果是贡献翻译

这里有一个简单的流程，供大家参考：

1. 首先fork MeteorHub 的meteor-doc-cn仓库
2. 把fork过去的项目也就是你的项目clone到你的本地
3. 在命令行运行 `git branch develop` 来创建一个新分支
4. 运行 `git checkout develop` 来切换到新分支
5. 运行 `git remote add upstream git@github.com:MeteorHub/meteor-doc-cn.git` 把MeteorHub的库添加为远端库
6. 运行 `git remote update`更新
7. 运行 `git fetch upstream master` 拉取meteor-doc-cn的更新到本地
8. 运行 `git rebase -i upstream/master` 将meteor-doc-cn的更新合并到你的分支

这是一个初始化流程，只需要做一遍就行，之后请一直在develop分支进行修改。

如果修改过程中meteor-doc-cn有了更新，请重复6、7、8步。

修改之后，首先push到你的库，然后登录GitHub，在你的库的首页可以看到一个 `pull request` 按钮，点击它，填写一些说明信息，然后提交即可。