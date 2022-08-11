> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [fishyer.notion.site](https://fishyer.notion.site/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#370803ddbaef45e1a5683edefafb2461)

> A new tool that blends your everyday work apps into one. It's the all-in-one workspace for you and yo......

> 其实就是自动将 md 文件渲染为 html 文件，本质上和 hugo、hexo、Jekyll 等博客渲染工具一样的。但是本方法支持全文搜索、双链和关系图谱。

文章目录：[1 - 自动提交到 github 仓库](/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#3d1951e5b5b0488992aa6aa9a93f1563)[2 - 配置 github actions 自动生成 html](/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#1a5eb052b94740f6a4af3e4c694d70ab)[3-vercel 自动发布为网站](/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#6e7d0bd2d3e44952b9bc20117f882c6d)[4 - 通过 CloudFlare 绑定自定义域名 [可选]](/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#7ece4d669d84410ba030b1200714a825)[极简部署的模板](/obsidian-logseq-ab5ad3d994324cea9f5c909a70653e05#370803ddbaef45e1a5683edefafb2461)效果可见： [logseq.fishyer.com](http://logseq.fishyer.com/) 源文件 Github 地址：[fishyer/MyLogseq: 我的 Logseq 卡片笔记仓库](https://github.com/fishyer/MyLogseq)适用场景：

1 - 分享双链大纲，因为 WorkFlowy 在国内访问太慢了，而且不支持双链。超适合分享一些卡片笔记，一个卡片笔记的案例：[Logseq](https://logseq.fishyer.com/#/page/logseq)，可以存放一些主题的相关资源、书签、笔记，一定程序上可以代替书签管理工具，并分享书签集锦。2 - 批量分享 md 文件，比如一些付费内容，通过 MarkDownLoad 插件下载为本地 md 文件后，可以通过这种方式分享

*   1 - 分享双链大纲，因为 WorkFlowy 在国内访问太慢了，而且不支持双链。超适合分享一些卡片笔记，一个卡片笔记的案例：[Logseq](https://logseq.fishyer.com/#/page/logseq)，可以存放一些主题的相关资源、书签、笔记，一定程序上可以代替书签管理工具，并分享书签集锦。

只想快速部署，不关心具体细节的，可以直接移步最后：[极简部署的模板](https://fishyer.notion.site/ab5ad3d994324cea9f5c909a70653e05#370803ddbaef45e1a5683edefafb2461)

1 - 自动提交到 github 仓库
===================

*   1-1 - 将 logseq 库作为 obsidian 库的子文件夹, 将要公开的 md 文件移动到 logseq 库的 pages 里面

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4ef2525b-f9ff-4f63-9574-0619fbba35c5%2FUntitled.png?table=block&id=75194190-4033-4cb2-b595-788d28204f48&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   1-2 - 配置 logseq 为全部公开

*   虽然 logseq 能在指定 md 文件里添加 public:: true 属性来公开此页面，但是批量管理很麻烦，建议直接将所有可公开的 md 存放到一个库里面，或放在一个单独的文件夹通过 git 忽略配置来隐藏私有文件夹 (不提交到 gihub 就不会参与后续的编译了)

*   logseq 库的 config.edn 路径

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9a99ed95-a219-438e-8854-05ff4f5e96cc%2FUntitled.png?table=block&id=ad387a19-3f5b-4607-baae-54f737f165a0&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   修改 logseq 的 config.edn，我的配置文件已存放到 github-gist: [logseq-config.edn](https://gist.github.com/fishyer/ec7520ae794c5456470a0c62dc30f2c0) ，直接复制粘贴即可

```
:publishing/all-pages-public? true
:default-home {:page "README" :sidebar ["contents"]}
Dart
```

*   如上，可以配置网站的起始页面为 README.md

*   1-3 - 配置 logseq 的 git 自动提交

*   本来 Obsidian 库也有 git 插件自动提交，但是好像没法指定特定文件夹，只能全库提交，这里只提交公开的 logseq 库

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe10e907b-753d-4ed3-914d-a68a088cfb24%2FUntitled.png?table=block&id=249d9060-ab04-4235-9f96-04ff6326c8c8&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   1-4 - 配置 git hooks，commit 时自动 push 到 github 仓库

*   主要是因为 logseq 的 git，只能自动 commit，不能自动 push，故需添加 git hooks

*   路径如下

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcebf89a4-5dbd-4736-827b-fb7537c7b6fd%2FUntitled.png?table=block&id=9f6e1f6b-f1c5-4a13-8e3d-13a1e40a50a6&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   shell 脚本文件如下

```
#!/bin/sh  git push origin main
Shell
```

2 - 配置 github actions 自动生成 html
===============================

*   在 logseq 库下新建. github 文件夹添加 main.yml

*   路径如下

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fddd8549c-18af-4ff7-948d-0a78e0fd9dcf%2FUntitled.png?table=block&id=492f4078-d3c0-4405-aa03-01b9542a08be&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   配置文件如下：

```
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  push:
    branches: [main]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      # - name: change config.edn and custom.css
      #   run: cp logseq/config-public.edn logseq/config.edn && cp logseq/custom-public.css logseq/custom.css
      - name: Logseq Publish 🚩
        uses: pengx17/logseq-publish@main
      # - name: Inject Script
        # run: sed -i "s@@$( cat logseq/inject.html | tr '\n' ' ' | sed 's@&@\\&@g' )@"  www/index.html
      - name: add a nojekyll file
        run: touch $GITHUB_WORKSPACE/www/.nojekyll
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4
        with:
          repository-name: fishyer/MyLogseq-Publish
          branch: master # The branch the action should deploy to.
          folder: www # The folder the action should deploy.
          clean: true
          single-commit: true
          token: ${{ secrets.ACCESS_TOKEN }}
YAML
```

*   还可以添加评论模块，具体可以查看：[配置 Logseq 自动发布相关流程](https://logseq.fishyer.com/#/page/%E9%85%8D%E7%BD%AELogseq%E8%87%AA%E5%8A%A8%E5%8F%91%E5%B8%83%E7%9B%B8%E5%85%B3%E6%B5%81%E7%A8%8B)，不过我现在已经关闭了，感觉会增加编译耗时，而且也没啥用，没人评论

*   上面的 repository-name: fishyer/MyLogseq-Publish 和 token: ${{secrets.ACCESS_TOKEN}} 其实是可选的

*   如果自己生成的 html 存放到本仓库的指定分支，而不是存放到其它仓库，可以不配置。

*   配置的顺序貌似不可修改，容易导致 action 执行出错。

*   推荐保存到其它仓库，因为强迫症无法忍受每次打开 github 仓库时都提示两个分支有差异，提示合并。命名规范推荐为 (加一个 publish 的后缀而已)：

*   我的 md 仓库为：[![](https://www.notion.so/image/https%3A%2F%2Fwww.notion.so%2Fimages%2Fexternal_integrations%2Fgithub-icon.png?width=200&userId=&cache=v2)MyLogseq![](https://www.notion.so/image/https%3A%2F%2Favatars.githubusercontent.com%2Fu%2F16300425%3Fv%3D4?width=200&userId=&cache=v2)](https://github.com/fishyer/MyLogseq) 我

*   对应 的 html 仓库为：[![](https://www.notion.so/image/https%3A%2F%2Fwww.notion.so%2Fimages%2Fexternal_integrations%2Fgithub-icon.png?width=200&userId=&cache=v2)MyLogseq-Publish![](https://www.notion.so/image/https%3A%2F%2Favatars.githubusercontent.com%2Fu%2F16300425%3Fv%3D4?width=200&userId=&cache=v2)](https://github.com/fishyer/MyLogseq-publish)

*   secrets.ACCESS_TOKEN 的生成，点击 [![](https://www.notion.so/image/https%3A%2F%2Fwww.notion.so%2Fimages%2Fexternal_integrations%2Fgithub-icon.png?width=200&userId=&cache=v2)tokens](https://github.com/settings/tokens)，创建新 token，然后勾选所有的权限

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F866eed02-ddc4-4bcc-9120-c0025b5ef8d2%2FUntitled.png?table=block&id=3e62855d-fcc5-4f9f-9083-2ae82588065a&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   secrets.ACCESS_TOKEN 的配置

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F93458a25-6c53-4e57-a98a-681c815b444c%2FUntitled.png?table=block&id=62b7bf4b-7a04-4d23-a14e-381eff49a0bd&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   可以查看 gitub action 的自动编译的历史，可以看到，一般每次编译耗时不足 3m

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa7a3662b-af43-4574-9bb6-bc02c9c620a8%2FUntitled.png?table=block&id=21ea32fb-51ad-466a-b924-854b7609eece&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2) [github action 的数量限制]([object Object])

*   生成的 html 文件效果，其实并没有将每个 md 都生成一个 html，而是都聚合在 index.html 里面。好处是在 [https://logseq.fishyer.com](https://logseq.fishyer.com/#/page/README) 这里查看时，可以快捷键 Ctrl+K 进行全文搜索，可以折叠展开，支持双链和关系图谱。感觉比 hugo 这种博客引擎更方便。

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F3da516e9-5c24-4046-afe2-fe04a7388fc2%2FUntitled.png?table=block&id=bb9c4091-b4b3-49ad-ba78-05174b5cf5bc&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=480&userId=&cache=v2)

3-vercel 自动发布为网站
================

*   其实可以直接借助 github pages 来发布，无须 vercel，但是 github page 实在是访问太慢了，不如 vercel 自带 cdn，国内也有节点，访问速度大大加快

*   访问 [https://vercel.com/](https://vercel.com/)，使用 github 授权登录

*   新建 vercel 项目，导入 github 仓库

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4f07443f-9eb3-4b42-be38-fa5a1896b2c5%2FUntitled.png?table=block&id=71f794da-e1cd-4da5-a106-c0ccb224a76d&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=580&userId=&cache=v2)

*   啥都不用配置，直接 deploy 即可

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc16c2b7a-1f9c-464c-ab7d-0183fe9ab214%2FUntitled.png?table=block&id=90fb4e0c-7264-4aa7-b62c-6609d12a95ea&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=580&userId=&cache=v2)

*   可以查看自动发布的历史，可以看到，一般每次发布耗时不足 10s

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6e02fedb-2bec-4b23-8c3f-0d77c7aabccd%2FUntitled.png?table=block&id=9cc78dd3-3610-448a-8e4f-8b80321e952b&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=580&userId=&cache=v2)

4 - 通过 CloudFlare 绑定自定义域名 [可选]
==============================

*   不是每个人都有自己的域名的，其实直接用 vercel 自动生成的. vercel.app 域名也挺好的，唯一的缺点就是在微信里面打开时会提示风险

*   4-1 - 先在 GoDaddy 购买一个域名：[GoDaddy | 购买域名](https://www.godaddy.com/zh-sg/offers/godaddy?isc=gotnsgl01&countryview=1&currencyType=SGD&cdtl=c_836275849.g_46463500767.k_kwd-296914754384.a_502457539370.d_c.ctv_g&bnb=b&gclid=CjwKCAjwp7eUBhBeEiwAZbHwkSl7k4E2LSJhmR5ANDZGyk6fL8WUZHtubjtoxq_ZL0Cwswvti8xA1xoCGNsQAvD_BwE)，费用看你的名称而定，我的域名：[fishyer.com](https://fishyer.com/) 费用为平均 81 人民币 / 年。

*   建议优先选择. com .cn .net 之类的域名，不然小心在微信里面打不开, 比如. me .top .app 等小众域名

*   推荐选择国外域名的原因，是因为可以免备案，直接在国内的阿里云买域名也可以，就是必须备案了，感觉备案挺麻烦的，要上传很多资料，还只能线下办理，懒得去折腾

*   4-2 - 在 GoDaddy 的管理后台，设置 DNS 域名服务器为 CloudFlare

*   配置路径为：

*   示例：[https://dcc.godaddy.com/control/fishyer.com/dns?plid=1](https://dcc.godaddy.com/control/fishyer.com/dns?plid=1)

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc79fe504-03e9-4d6f-8519-727a7f83b249%2FUntitled.png?table=block&id=b396e34c-e1b6-4817-9119-bd28baa7f98d&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=580&userId=&cache=v2)![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6d6b932d-6433-4e5f-8a25-b29a43bb113f%2FUntitled.png?table=block&id=9496362d-7826-403d-adbf-5c6e89b3b192&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=480&userId=&cache=v2)

*   CloudFlare 服务器

```
ned.ns.cloudflare.com
gabriella.ns.cloudflare.com
Shell
```

*   4-3- 在自己的 [CloudFlare 管理后台](https://dash.cloudflare.com/)，添加一个 DNS 解析记录，解析到 Vercel-DNS

*   类型为 CNAME，名称为 logseq（实际上就是 logseq.fishyer.com，可自定义，不过建议命名为这个，方便区分和记忆，因为还可能会有 notion.fishyer.com mweb.fishyer.com hugo.fishyer.com 等等），内容为 cname.vercel-dns.com ，代理状态选仅限 DNS（不需要 CloudFlare 的 cdn，因为 Vercel 自带 cdn，比 CloudFlare 的 cdn 好像还快一点）

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9696f157-6aff-48c8-83ea-918a89273b71%2FUntitled.png?table=block&id=072aadef-8a6f-48be-b93e-82f7d6f69c30&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=770&userId=&cache=v2)

*   4-4 - 在 Vercel 的项目配置里面，输入自定义的域名，比如：logseq.fishyer.com ，然后 Add

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Ff31e1a51-e59c-41f0-8128-64cfc81855e0%2FUntitled.png?table=block&id=5ae61b86-8606-4afd-8e32-42d35e0c19af&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=960&userId=&cache=v2)

极简部署的模板
=======

*   1 - 直接下载我的 [![](https://www.notion.so/image/https%3A%2F%2Fwww.notion.so%2Fimages%2Fexternal_integrations%2Fgithub-icon.png?width=200&userId=&cache=v2)MyLogseq-Template![](https://www.notion.so/image/https%3A%2F%2Favatars.githubusercontent.com%2Fu%2F16300425%3Fv%3D4?width=200&userId=&cache=v2)](https://github.com/fishyer/MyLogseq-Template)，解压

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8a46b5c2-8a87-4f81-a655-850b80408603%2FUntitled.png?table=block&id=286b91ab-8c83-4591-a92a-63000d7bdc3f&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=770&userId=&cache=v2)

*   2 - 拖动到自己的 ob 库下面，在 logseq 文件夹的路径下，配置好 git 仓库 - origin 和分支名称 - main

*   将第 3 行的仓库路径 git@github.com:fishyer/MyLogseq-Template.git 修改为自己的仓库路径，然后依次执行即可

```
git init
cp assets/post-commit .git/hooks/post-commit
git remote add origin git@github.com:fishyer/MyLogseq-Template.git
git checkout -b main
git add .
git commit -m "init"
git push -f origin main
Shell
```

*   3 - 等待 github action 执行完毕后，导入到 vercel 中，等待 Vercel 发布完成，就大功告成了 🎉 

*   效果可见：[https://my-logseq-template.vercel.app](https://my-logseq-template.vercel.app/#/page/README)

*   需要注意的点就是需要修改一下 vercel 中对于的 git 分支为 gh-pages

![](https://fishyer.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F72c1b53a-84b6-4257-8b94-5d8c90c55069%2FUntitled.png?table=block&id=3e764719-7906-40f5-90b6-1f801027b1f7&spaceId=009b464c-24e9-41af-abc1-e159c1f06108&width=2000&userId=&cache=v2)

*   极简版本的注意事项：

*   1 - 注意仓库的名称和分支的名称要和 git hooks 里面的 post-commit 脚本对应，不然会导致自动 push 失效

*   为了让自动 commit 和自动 push 生效，logseq 软件需要开着，并打开了这个仓库

*   2 - 本模板已经添加好了 github-actions 脚本，但为简单起见，是发布到原始仓库的 gh-pages 分支下面，不同于正文中是发布到另一个 MyLogseq-Publish 仓库下面

*   3 - 需要修改 vercel 中对于的 git 分支为 gh-pages

如果你在部署过程中，遇到什么问题，欢迎和我交流讨论，微信：fishyer2850，或者加入我们的群聊来讨论：[https://blog.fishyer.com/about](https://blog.fishyer.com/about)