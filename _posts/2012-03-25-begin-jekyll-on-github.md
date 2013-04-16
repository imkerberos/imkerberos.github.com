---
layout: post
title: 开始在 GitHub 上使用 Jekyll 记录博客
tags: [Jekyll, GitHub]
comments: true
catogory: 感想
---

一直想找一个比较好的博客系统, 最初是在一些博客网站, 比如博客中国啊, 新浪博客等. 但是这些博客网站用起来不是很爽,
相对于一个用惯了 [VIM][] 的码农来说, 这种博客系统真是折磨啊. 除了样式不好看, 空间不稳定
等不爽因素之外, 其编辑器也是非常不爽, 尤其是所谓的所见即所得的 JavaScript 编辑器.  后来发现 GAE 系统也还可以,
于是尝试使用 microblog 在 GAE 上构建博客系统, 同样的问题, 输入不爽. 曾经跟作者提到过最好使用一个 reStructedText
格式编辑器, 后来也不了了之.

今年申请了一个小域名, 无奈没有好的托管空间. 偶尔想起 GitHub 仓库中的 rst 文件可以直接解析成 html, 于是就想能不
能把主页放到 GitHub 里面.

经过 Google 之后, 发现我真的 Out 了! GitHub 上建立个人博客系统已经有了非常成熟的方案. 经过几天的调查, 几个软件
在我的备选之中.

- [Hyde][] 好处是 Python 编写的, 不过是基于我比较讨厌的 Django.
- [Pelican][] 也是基于 Python 的 weblog generator. 不过 GitHub 不支持, 每次发布都需要转换一下, 感觉有点麻烦.
- [Octopress][] 基于 Ruby weblog 生成器. 这个东西跟 [Pelican][] 是一样的, 编写完成以后需要转换以后才能部署到 GitHub
上.
- [Jekyll][] 最终我还试选择了它, 优点是:
    - GitHub 原生支持, 不需要本地安装一堆 Ruby 的软件来搭建生成器的运行环境, 可以直接部署到 GitHub 上.
    - 所有的 weblog 生成器都是使用结构化文本的, 本来我有很多 *rst* 的文本, 现在学些起 *markdown* 实在是轻车熟路,想
        不到的是 *markdown* 比 *rst* 更简单!

虽然决定使用 Jekyll, 但是翻遍了 Jekyll 的所有主题, 都不符合我的审美观点, 相反 [Octopress][] 的 Classic 主题我一眼
就喜欢上了. 既然如此, 就需要把这个主题移植到 GitHub 上来. 由于 [Octopress][] 在 [Jekyll][] 的基础上编写了很多 Plugin,
而 GitHub 为了安全起见, 是不允许在 Server 端执行用户编写的 Ruby 代码的. 所以很悲催, 一些 Octopress 的功能是没有了,
不过还好, 总算是把这个主题在 GitHub 上跑起来了.

如果你也喜欢这个主题, 可以直接从 [我的仓库](http://github.com/imkerberos/imkerberos.github.com) fork 出来使用.

关于在 GitHub 上使用 Jekyll 建立博客我是从 [这篇文章](http://beiyuu.com/github-pages/) 学习到的.希望对你也有帮助.

[VIM]: http://pelican.notmyidea.org/en/2.8/index.html "VIM"
[Hyde]: http://pelican.notmyidea.org/en/2.8/index.html "Hyde"
[Pelican]: http://pelican.notmyidea.org/en/2.8/index.html "Pelican"
[Octopress]: http://otcopress.org "Otcopress" 
[Jekyll]: https://github.com/mojombo/jekyll "Jekyll"
