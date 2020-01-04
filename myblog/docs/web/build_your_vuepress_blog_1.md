# VuePress从零开始搭建自己的博客(1)

[[toc]]


本文接着上一篇文章[VuePress从零开始搭建自己的博客](http://hellogitlab.com/web/build_your_vuepress_blog.html) 对博客系统进行更多的设置。


## 增加文档目录(table of contents)

在Markdown文件中第一行的一级标题"# VuePress从零开始搭建自己的博客"的下面增加``[[toc]]``：

```
# VuePress从零开始搭建自己的博客
[[toc]]
```


使用``yarn docs:dev``重新构建，则会生成目录结构。如 [VuePress从零开始搭建自己的博客](http://hellogitlab.com/web/build_your_vuepress_blog.html) 增加目录结构显示如下：

![vuepress_set_table_of_centent](/img/vuepress_set_table_of_centent.png)

参考： [VuePress目录设置](http://caibaojian.com/vuepress/guide/markdown.html#%E7%9B%AE%E5%BD%95-table-of-contents)


## Emoji支持

- Emoji表情非常有趣，我们在写文章的时候，可以在Markdown文件加入一些Emoji表情，让我们的文档更生动。
- 如``:tada:``代表:tada:，``:100:``代表:100:，更多的Emoji表情可参考[markdown-it-emoji](https://github.com/markdown-it/markdown-it-emoji/blob/master/lib/data/full.json)。

下面列出部分表情与英文的对应关系：

```markdown
{
  "100": "💯",
  "1234": "🔢",
  "grinning": "😀",
  "smiley": "😃",
  "smile": "😄",
  "grin": "😁",
  "laughing": "😆",
  "satisfied": "😆",
  "sweat_smile": "😅",
  "joy": "😂",
  "rofl": "🤣",
  "relaxed": "☺️",
  "blush": "😊",
  "innocent": "😇",
  "slightly_smiling_face": "🙂",
  "upside_down_face": "🙃",
  "wink": "😉",
  "relieved": "😌",
  "heart_eyes": "😍",
  "kissing_heart": "😘",
  "kissing": "😗"
}
```

## 自定义容器(custom containers)

下面在MarkDown文件中使用一些容器，对用户进行一些提示或者警告。

- 可以使用``tip``、``warning``、``danger``关键字来指定提示的类型和颜色。
- 在``tip``、``warning``、``danger``关键字后面增加其他字符则指定了提示的标题。

::: tip
这是一个提示，未设置提示标题
:::

::: tip 提示
这是一个提示，设置了提示标题
:::

::: warning
这是一个警告，未设置警告标题
:::

::: warning 注意
这是一个警告，设置了警告标题
:::

::: danger
这是一个危险警告，未设置危险警告标题
:::

::: danger 危险
这是一个危险警告，设置了危险警告标题
:::


在Markdown文件中第一行的一级标题"# VuePress从零开始搭建自己的博客"的下面增加以下内容，展示Emoji表情和提示的效果：

```
# VuePress从零开始搭建自己的博客                                                                                                            
:heavy_check_mark: :heart: :basketball: :snowman:

::: tip 提示
这是一个提示，设置了提示标题
:::

::: warning 注意
这是一个警告，设置了警告标题
:::

::: danger 危险
这是一个危险警告，设置了危险警告标题
:::
```


使用``yarn docs:dev``重新构建，则会生成目录结构。如 [VuePress从零开始搭建自己的博客](http://hellogitlab.com/web/build_your_vuepress_blog.html) Emoji表情和提示警告显示如下：

![vuepress_set_emoji_and_tip_warning_danger](/img/vuepress_set_emoji_and_tip_warning_danger.png)

参考： [自定义容器](https://vuepress.vuejs.org/zh/guide/markdown.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AE%B9%E5%99%A8)


## 显示上次更新时间

在config.js中``themeConfig``中配置``lastUpdated``参数即可，则会在每篇文章最后显示上次更新的时间。
```javascript
module.exports = {
    ...省略
    themeConfig: {
            lastUpdated: '上次更新',
            ...省略
)
```

使用``yarn docs:dev``重新构建，则会生成目录结构。如 [VuePress从零开始搭建自己的博客](http://hellogitlab.com/web/build_your_vuepress_blog.html) 上次更新时间显示如下：

![vuepress_set_lastUpdated_time](/img/vuepress_set_lastUpdated_time.png)

参考： [多语言支持](https://vuepress.vuejs.org/zh/guide/i18n.html#%E5%A4%9A%E8%AF%AD%E8%A8%80%E6%94%AF%E6%8C%81)

此时显示的时间不符合国人的使用习惯，设置一下语言本地化。

## 语言本地化设置

在config.js中增加``locales``参数关键字，并配置``lang: 'zh-CN'``，即指定使用中文。
```javascript
module.exports = {
    title: '个人主页',
    description: '梅朝辉的博客',
    locales: {
         '/': {
             lang: 'zh-CN',
         }
    },
    ...省略
}
```

再次运行程序，显示如下：
![vuepress_set_lastUpdated_time_with_locales](/img/vuepress_set_lastUpdated_time_with_locales.png)

参考： [多语言支持](https://vuepress.vuejs.org/zh/guide/i18n.html#%E5%A4%9A%E8%AF%AD%E8%A8%80%E6%94%AF%E6%8C%81)


## TODO

- 评论功能，可参考 基于vuepress的个人博客搭建完全教程
- 评论功能，可参考  Vuepress使用Valine搭建带有评论系统的博客
- 标签墙功能
- 背景音乐播放
- 部署上线，脚本编写，可参考 VuePress从零开始搭建自己专属博客
- 多语言配置
- 不蒜子访问量统计
- 文章置顶功能
- 全文搜索功能
- logo修改
- 增加文章
- 返回top插件
- vuepress自动生成侧边栏的插件
- 网站文章字数统计 Site words total count
- https配置


参考：

- [基于vuepress的个人博客搭建完全教程](https://www.jianshu.com/p/2220dbacfde1)
- [VuePress从零开始搭建自己专属博客](https://segmentfault.com/a/1190000015237352?utm_source=tag-newest)
- [markdown-it-emoji](https://github.com/markdown-it/markdown-it-emoji/blob/master/lib/data/full.json)
- [Vuepress使用Valine搭建带有评论系统的博客](https://segmentfault.com/a/1190000016144076)
- [Finen's Blog](https://www.finen.top/)
- [VuePress指南](https://vuepress.vuejs.org/zh/)
- [試毅-思伟_技术笔记](https://zhousiwei.gitee.io/ibooks/)
- [飞跃高山与大洋的鱼 视频教程 一步步搭建 VuePress 及优化](https://www.bilibili.com/video/av43316513?p=5)
- [一步步搭建 VuePress 及优化](https://juejin.im/post/5c9efe596fb9a05e122c73f1)
- [Vuepress Plugin Auto Sidebar](https://www.npmjs.com/package/vuepress-plugin-auto-sidebar)
- [飞跃高山与大洋的鱼的博客](https://docs.shanyuhai.top/)
- [vuepress自动生成侧边栏](https://fangzheng.xyz/Other/VuePress/1.vuepress-auto-sidebar.html)
- [前端进阶积累](http://obkoro1.com/web_accumulate/)
- [Zhu Zhaohua的博客搭建过程](https://zhuzhaohua.com/technology/vue/20190915_myblog.html#vuepress%E5%AE%89%E8%A3%85)