---
layout: post
link: https://textslashplain.com/2017/01/14/the-line-of-death/
title_cn: 死亡之线
translate_time: 42
comments: true
author: ericlaw
tags:
- design
---

安全设计师在开发一些展示不被信任的内容的应用时会遇到一个严重问题：如果攻击者能取得一小块像素的控制权限，那么攻击者基本可以把这块区域变成任何样子，包括应用本身的UI。然后他就能诱使用户触发一些危险的动作。

在浏览器中，浏览器本身通常会有窗口顶部的完全控制权，然而自顶部以下的部分则由网站来控制。我最近听说这两部分之间的间隔被叫做“死亡之线”。

![](https://textplain.files.wordpress.com/2017/01/image36.png?w=1184&h=480)

如果用户信任这条线以上的部分，由于惯性思维，他们也容易信任下方的部分，然后他们就要悲剧了。但是这个事实对用户来说并不是常识。

比如，由于死线上方的区域太小，导致浏览器需要更多的区域来显示可信任的UI。Chrome通过“穿越”死线，通过一个小气泡来展示一些文字，如下图所示：

![](https://textplain.files.wordpress.com/2017/01/image37.png?w=1184&h=266)

用户会以为不被信任的信息无法穿越死线。但是上图的有半部分可见，Chrome的做法并不统一，因为这段信息并没有“穿越”死线。结果就是，很多用户会被假的气泡信息欺骗，如下图所示：

![](https://textplain.files.wordpress.com/2017/01/image38.png?w=574&h=328)

更严重的问题是，一些危险的数据在死线上方也可能存在。信任死线下方的内容等于严重的安全危机，有一些死线上方的区域也并不安全。下图中表明了三个“死亡区域”。

![](https://textplain.files.wordpress.com/2017/01/image39.png?w=1184&h=482)

在区域1，攻击者可以修改网站的图标和标题。这两个信息可以被网站的建设者任意修改，因此它们可能被改成一些具有欺骗性质的内容。

在区域2，攻击者的域名会显示在地址栏。一些信息安全专家认为这里是一个网址中唯一可信的东西，此例中URL表明了HTTPS，显示这个网站是可信任的，所有的连接已经加密。然而攻击者可以使用一些与真的网站非常相似的域名来欺骗用户：https://paypal-account.com/ vs https://paypal.com

区域3的内容则完全不可信。这个URL：http://account-update.com/paypal.com/ 与真正的Paypal毫无关系。虽然在这里搞事情并没有那么容易骗到人，但是对用户来说把这些内容屏蔽却非常困难，因为它们既不在DNS中，也无法通过[Certificate Transparency](https://www.certificate-transparency.org/)的日志中查询到。

区域4是网站内容区域，这里的任何信息都是可疑的。不过由于“窗口”这个概念在现今操作系统的广泛存在，导致它们非常难以识别。在网站区域，攻击者非常容易通过“画中画”这种方式来进行攻击：包括可信任像素在内的，整个浏览器窗口都可以被造假。

![](https://textplain.files.wordpress.com/2017/01/image40.png?w=1184&h=766)

很多人想出了各种方法来避免这种画中画攻击。比如，如果你的操作系统和浏览器有自定义主题，你就很难被欺骗。

> 2007年IE团队上线了一个扩展验证证书(EV)，微软研究团队发表了[一篇文章](http://www.adambarth.com/papers/2007/jackson-simon-tan-barth.pdf)来质疑这种证书的有效性。一个五百强企业拜访了IE团队，因为他们想要做EV证书产业的调研。他们对EV证书的前景抱有巨大的期望，但是他们注意到画中画问题非常致命。

## 更糟的未来在此...

HTML5添加了全屏API，导致整个死亡区域看起来像这样：

![](https://textplain.files.wordpress.com/2017/01/zodfullscreen.png)

Windows 8上的现代版IE也有同样的问题，因为它的设计哲学是“超越边框的用户体验”，导致没有安全的区域。我曾经建议IE团队在屏幕右下角起码添加一个“可信任”标识之类的东西，但是他们并未接受。