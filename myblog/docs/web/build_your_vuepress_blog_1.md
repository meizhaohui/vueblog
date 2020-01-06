# VuePress从零开始搭建自己的博客(1)

[[toc]]


本文接着上一篇文章[VuePress从零开始搭建自己的博客](./build_your_vuepress_blog.md) 对博客系统进行更多的设置。


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

::: tip 重要提示
``lastUpdated``是基于git版本管理的文件最后更新提交的时间，如本博客的源码地址[vueblog](https://github.com/meizhaohui/vueblog.git) ,只有将构建过程中的代码存放在基于git版本管理的GitHub或GitLab网站上面才能正常显示出页面的上次更新时间。
:::

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

## logo和博客标题设置

我们可以给自己的博客设置一个自己喜欢的logo，如可以在 [LogoFree](http://www.logofree.cn/logo.html) 上面制作一款自己喜欢的logo。可以付费导出自己的logo的ico图片，也可以通过截图获取自己喜欢的logo，然后在 [在线生成透明ICO图标](http://ico.duduxuexi.com/) 生成32\*32的favicon.ico文件，将favicon.ico文件保存到 ``docs/.vuepress/public/img``目录下，并修改``config.js``文件的``head``属性值。

同时，我们也可以修改一下博客的标题为"梅朝辉的博客"，修改``config.js``文件的``title``属性值为"梅朝辉的博客"即可。

```javascript
module.exports = {
    title: '梅朝辉的博客',
    description: '种一棵树最好的时间是十年前，其次就是现在。',
    head: [
        ['link', { rel: 'icon', href: '/img/favicon.ico' }],
    ],
    ...省略
```

另外，我们更新一下根目录docs下的README.md文件，将其中的``heroImage: img/gitlab.jfif``中的图片替换成我们自己的logo图片``heroImage: img/hellogitlab_logo.png``:

```
[root@hellogitlab docs]# cat README.md 
---
home: true
heroImage: img/hellogitlab_logo.png
actionText: 查看我的博文 →
actionLink: /python/
features:
- title: Hello Python
  details: 简单易学的Python编程语言
- title: Hello Golang
  details: Go 是互联网时代的C语言
- title: Hello Web
  details: 手把手教你学web页面
footer: MIT Licensed | Copyright © 2019-present Zhaohui Mei
---
```

再次运行程序，显示如下：
![vuepress_set_logo_and_heroImage](/img/vuepress_set_logo_and_heroImage.png)

可以看到标签页上面的小图片已经变成了我们的logo图标了，并且页面中间的图片和说明内容也更新了。

参考: [导航栏 Logo](https://vuepress.vuejs.org/zh/theme/default-theme-config.html#%E5%AF%BC%E8%88%AA%E6%A0%8F-logo)

## 背景音乐设置

之前想着给博客增加背景音乐的，但当自己带着耳机，突然打开一个自动播放背景音乐的网站的时候，把我震惊了(吓一跳)，决定放弃这个功能。


## 使用Valine配置评论功能

首先在 https://leancloud.cn 网站上面注册一个账号，并使用支付宝进行实名认证，并创建一个应用，创建应用后，可以在应用设置中查看到应用的Keys信息。

leancloud中显示如下:

![leancloud_keys](/img/leancloud_keys.png)

然后按照 [在VuePress中使用Valine](https://valine.js.org/vuepress.html) 的配置方法下载安装``vuepress-plugin-comment``插件，并配置config.js文件。

### 安装``vuepress-plugin-comment``插件

```shell
yarn add vuepress-plugin-comment -D
```

::: tip 重要提示
如果你从我的 [vueblog](https://github.com/meizhaohui/vueblog.git) 上面直接克隆下来的代码的话，切换到myblog目录后，使用``yarn``直接就可以安装相关的插件依赖了。
:::

### 将``vuepress-plugin-comment``添加到vuepress项目的插件配置中

```javascript
module.exports = {
  plugins: [
    [
      'vuepress-plugin-comment',
      {
        choosen: 'valine', 
        // options选项中的所有参数，会传给Valine的配置
        options: {
          el: '#valine-vuepress-comment',
          appId: 'Your own appId',
          appKey: 'Your own appKey'
        }
      }
    ]
  ]
}
```

我这边参照上面的方法，并增加了``placeholder``和``lang``属性，配置如下:

```javascript
module.exports = {
    title: '梅朝辉的博客',
    ...省略
    plugins: [
        [
            'vuepress-plugin-comment',
            {
                choosen: 'valine', 
                // options选项中的所有参数，会传给Valine的配置
                options: {
                    el: '#valine-vuepress-comment',
                    appId: 'your leancloud appid',
                    appKey: 'your leancloud appkey',
                    placeholder: '同道中人，文明留言...',  // 评论框占位提示符
                    lang: 'zh-cn', // 支持中文
                }
            }
        ]
    ],
```

重新运行后，打开任意一篇文章，都可以在最下方添加评论信息了。

![vuepress_set_comment](/img/vuepress_set_comment.png)

此时，可以在输入框中添加评论信息了。

如我添加两条评论信息。第一条"写得不错，棒棒哒！"，第二条"与君共勉，加油!👍"。评论后的页面信息如下：

![vuepress_add_comments](/img/vuepress_add_comments.png)

但此时存在一个问题，每个页面都会显示相同的评论信息，后续咨询Valina团队再改进。但过一会再刷新页面，评论又显示是正常的，感觉是API调用延迟的问题。

另外，我们提交了两个评论信息，在leancloud.cn的应用--存储页面也看到提交的Comment评论记录：

![vuepress_leancloud_comments_history](/img/vuepress_leancloud_comments_history.png)

以上说明评论功能配置正常！

## 优化导航栏

在优化导航栏时，我们可以做以下事情：

- 导航栏左侧添加logo图标
- 导航栏右侧通过repo指定GitHub链接，而不是自己手动指定GitHub的链接地址。

我们更新一下config.js文件，配置如下:
```javascript
module.exports = {
    title: '梅朝辉的博客',
    ...省略
    themeConfig: {
        logo: '/img/favicon.ico', // 导航栏左侧的logo,不写就不显示
        lastUpdated: '上次更新',
        repo: 'https://www.github.com/meizhaohui/vueblog',  // 链接的地址
        repoLabel: 'GitHub',  // 链接的名称
        nav: [
            { text:'首页', link: '/'},
            {
                text: '博文',
                items: [
                    { text: 'Python', link: '/python/' },
                    { text: 'Golang', link: '/golang/' },
                    { text: 'Web', link: '/web/' }
                ]
            }, 
            { text:'关于', link: '/about/'},
            // { text: 'Github', link: 'https://www.github.com/meizhaohui' } 此行被移除
        ],
    ...省略
```

重新运行后，可以看到在导航栏的左侧多出了logo图标，并且通过repo属性也生成了GitHub的超链接。效果如下图所示：

![vuepress_set_nar_logo_and_repo](/img/vuepress_set_nar_logo_and_repo.png)

## 添加编辑此页面超链接

我们在优化导航栏的基础上，给``themeConfig``增加``editLinks``、``editLinkText``、``docsDir``参数，参数的说明详见以下代码，具体如下：
```javascript
module.exports = {
    title: '梅朝辉的博客',
    ...省略
    themeConfig: {
        logo: '/img/favicon.ico', // 导航栏左侧的logo,不写就不显示
        lastUpdated: '上次更新',
        repo: 'https://www.github.com/meizhaohui/vueblog',  // 链接的地址
        repoLabel: 'GitHub',  // 链接的名称
        editLinks: true,  // 开启编辑链接功能
        editLinkText: '帮助我们改善此页面',  // 自定义超链接的文本内容
        docsDir: 'myblog/docs',  // docs文件的路径，从根目录开始
        nav: [
            { text:'首页', link: '/'},
            {
                text: '博文',
                items: [
                    { text: 'Python', link: '/python/' },
                    { text: 'Golang', link: '/golang/' },
                    { text: 'Web', link: '/web/' }
                ]
            }, 
            { text:'关于', link: '/about/'},
        ],
    ...省略
```

参考：[Git repository and Edit Links](https://vuepress.vuejs.org/theme/default-theme-config.html#git-repository-and-edit-links)

重新构建后，在页面的底部可以看到多出了"帮助我们改善此页面"超链接，点击超链接可以打开GitHub仓库中文件的编辑页面。效果如下图所示：

![vuepress_set_edit_page_links](/img/vuepress_set_edit_page_links.png)

## 隐藏私密信息


在[使用Valine配置评论功能](./build_your_vuepress_blog_1.html#使用valine配置评论功能) 中，我们在配置文件中使用了``appId: 'your leancloud appid'``和``appKey: 'your leancloud appkey'``,并上传到了GitHub上面，然后在服务上面再进行手动修改成真实的appid或appkey值，每次部署到服务器上面时都需要重新修改一次，显得非常麻烦。

如果我们将自己的leancloud人appid或appkey上传到GitHub中，别人就可以看到你的私密信息，也不够安全。因此我们需要隐藏这些私密信息。

我们在项目根目录docs目录同级创建一个``config``目录，并在其中创建``secureinfo.js``文件。目录结构如下图：

![vueblog_project_structure](/img/vueblog_project_structure.png)

我们同时创建了``secureinfo.js.txt``文件，用于与``secureinfo.js``文件进行对比，``secureinfo.js.txt``文件中增加一些说明。

``secureinfo.js``文件内容如下:

```javascript
module.exports = {
    leancloud_appId: 'your leancloud appid',
    leancloud_appKey: 'your leancloud appkey'
}
```
注：'your leancloud appid'和'your leancloud appkey'改成你自己的leancloud的appid和appkey值。

``secureinfo.js.txt``文件内容如下:

```javascript
// please rename this file to secureinfo.js, and then input your secure leancloud info.
module.exports = {
    leancloud_appId: 'your leancloud appid',
    leancloud_appKey: 'your leancloud appkey'
}
```

最重要的一步，将``secureinfo.js``文件加入到``.gitignore``文件列表中。在``.gitignore``文件最后加以下内容：

```
# secure info
secureinfo.js
```

检查git查看忽略文件：
```shell
$ git check-ignore -v config\secureinfo.js
.gitignore:107:secureinfo.js    "config\\secureinfo.js"

$ git check-ignore -v config\secureinfo.js.txt
```

可以看到config\secureinfo.js文件已经被忽略掉，而secureinfo.js.txt文件不会忽略。所以我们提交时config\secureinfo.js不会被提交。

同时，也可以使用``git status``命令来查看这两个文件是否被提交：

```shell
$ git status config\secureinfo.js
On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean


$ git status config\secureinfo.js.txt
On branch master
Your branch is up-to-date with 'origin/master'.
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        config/secureinfo.js.txt

nothing added to commit but untracked files present (use "git add" to track)
```

可以看到config\secureinfo.js文件未被跟踪，不会被上传到GitHub中。

另外需要更改一下``.vuepress/config.js``文件：

- 在第1行使用``const secureConf = require('../../config/secureinfo.js');``引入安全文件，注意此处使用的相对引用，``..``表示向上一级目录。
- 原来``appId: 'your leancloud appid',``内容替换成``appId: secureConf.leancloud_appId,  // 读取secure_info.js中的配置信息``。
- 原来`` appKey: 'your leancloud appkey',``内容替换成``appKey: secureConf.leancloud_appKey,  // 读取secure_info.js中的配置信息``。

修改后，我们使用正确的``leancloud_appId``和``leancloud_appKey``值时，在本地可以调试时，可以获取到评论信息，显示如下图所示：

![vueblog_set_local_secure_leancloud](/img/vueblog_set_local_secure_leancloud.png)

评论显示正常！

我们修改一下``leancloud_appId``，随机改成其他的值，再运行试一下，显示如下图所示：

![vueblog_set_error_local_secure_leancloud](/img/vueblog_set_error_local_secure_leancloud.png)

此时，可以看到页面显示"Code 401: 未经授权的操作，请检查你的AppId和AppKey."，且在Console界面显示异常。

再将``leancloud_appId``改成正常的值重新运行，又可以正常显示评论信息，说明配置生效。

后续，只用在服务器的项目目录中配置``config/secureinfo.js``文件即可，既可以隐藏私密信息，也方便后续提交部署。

## TODO

- 标签墙功能
- 部署上线，脚本编写，可参考 VuePress从零开始搭建自己专属博客
- 多语言配置
- 不蒜子访问量统计
- 文章置顶功能
- 全文搜索功能
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
- [管鲍切思世所稀的博客](https://blog.usword.cn/)
- [程序员你为什么这么累？](https://xwjie.github.io/)
- [在VuePress中使用Valine](https://valine.js.org/vuepress.html)
- [从今天开始，拿起VuePress打造属于自己的专属博客](https://blog.csdn.net/weixin_34345560/article/details/91457750)
- [导航栏 Logo](https://vuepress.vuejs.org/zh/theme/default-theme-config.html#%E5%AF%BC%E8%88%AA%E6%A0%8F-logo)
- [Git repository and Edit Links](https://vuepress.vuejs.org/theme/default-theme-config.html#git-repository-and-edit-links)
