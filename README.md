之所以创建这个 repository，是因为我希望收集一些比较好的有关于 RESTful API 设计的参考文献。偶尔回顾，偶尔改进，大家一起来吧~

如果你有更好的私藏文章，不凡分享出来，独乐乐不如众乐乐，(⊙o⊙)

RESTful 介绍及设计思路
Principles of good RESTful API Design（译：好 RESTful API 的设计原则 ）简单易懂，条理清晰，推荐
Best Practices for Designing a Pragmatic RESTful API（译：RESTful 最佳实践 译文2）有实际的案例 Enchant
HTTP API Design Guide（译：HTTP API 设计指南）
Some REST best practices
理解 RESTful 架构 - 阮一峰 简单了解什么是 RESTFul
RESTful API 设计指南 - 阮一峰
Restful API 的设计规范 实战经验的总结，具有较强的启发意义
撰写安全合格的REST API 利用好 HTTP 协议所具备的特征
Web 服务编程，REST 与 SOAP REST 与传统的面向服务的接口设计的区别，启发性强
最佳实践：更好的设计你的 REST API 了解 REST 实现缓存的过程
Thoughts on RESTful API Design
REST API Tutorial 全方位介绍 REST
HTTP 接口设计指北
Web API Design 接口就是开发人员提供的“界面”，用户体验在接口设计上同样重要，在线查看 2012 版、2013 版
架构风格与基于网络应用软件的架构设计 原汁原味的博士论文，由李锟翻译，有经验的同学可以挑战一下
Microsoft REST API Guidelines 微软官方的 REST API 设计指南，值得参考
知识碎片
理解 HTTP 幂等性 讲得很清楚，推荐
浅析远程过程调用 RPC 告诉你什么是 RPC
httpstatuses 一眼看完所有常用的 HTTP 状态码，还可以看详细含义
json-api 对 API 应该如何利用好 JSON 的一些建议
介绍 JSON 无论如何都应该读一遍
decision-graph.svg 一张大图展示整个 REST API 的验证过程，及各种状态码出现的时机
书籍
RESTful Web APIs 较新的一本书，对 REST 做了很多系统性的总结，尤其对“超媒体”作了详细的介绍
Jersey-2.x-User-Guide（译：Jersey 2.x 用户指南）译者也提供了入门简易教程 REST 实战以及综合实例 RestDemo（注：读者需要 Java 基础）
REST CookBook 基础介绍构建 RESTful API
例子
Github API v3 被很多人参考和引用，比如对分页的处理方法、接口版本的设计等等
Mailgun Documentation 邮件服务 REST API
Enchant REST API
Coinbase API 设计的挺好的，包括官网提供的接口客户端，都是具有参考意义的
OpenNMS Wiki ReST API
REST API 使用详解 Lean Cloud 中讲解 REST API 的使用，还集成 Swagger UI 在线调试工具，点击查看。
关于例子，实在是太多了，在有时间的时候，多观察别人的设计，有利于写出好的 API。

调试工具
DHC (aka Dev HTTP Client) Chrome 插件，简单易用，可分类管理，界面友好。也很多人推荐 Postman
Fiddler2 抓包，捕捉每一次 REST 请求和响应的详细内容
文档制作
slate 创建的 API 文档很好看，也很实用，三列式，目录、调用说明和代码示例同屏滚动显示。
i5ting_ztree_toc API 把 Markdown 文档生成简单的 HTML API
代码高亮
highlight.js 无需指定代码是什么语言，直接按 TAB 键搞掂，它会自动检测高亮
PrismJS 高亮效果挺好看的
这方面的工具很多，可以自己在网上找找，找一款适合自己的就可以，毕竟只是工具，能达到目的就好。

社区
API Craft Google Group 有梯子才行
RESTful
其他
MarkdownPad2 Windows 下使用 Markdown 语法编写文档。等习惯了它的语法，可以直接使用任何一款文本编辑器直接写了
