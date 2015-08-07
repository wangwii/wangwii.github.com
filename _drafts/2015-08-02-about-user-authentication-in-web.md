---
layout: post
title: "About user authentication in web application"
tagline: "用户认证技术发展简史"
comments: false
category: "Note"
tags: [javascript]
---

用户的注册/登录常被称为『非功能性需求』或『影子需求』，几乎是所有软件的必备功能，至少在我从业以来就
写过多个功能类似的用户注册/登录模块，以下简称用户认证模块。

我们无法知道有多少团队和开发人员曾经思考、讨论、甚至加班为了构建自己的用户模块模块，也不知道从哪天开始，
大家终于无法忍受这样『重复制造轮子』的状况，开始有人开源他们的类库或框架，当下成熟并广泛使用的框架有：
* [spring-security](http://projects.spring.io/spring-security/) 针对Java平台，
秉承Spring框架的依赖注入设计理念，与Sping的其它框架无缝集成。
* [apache-shiro](http://shiro.apache.org/introduction.html) Java平台另一个优秀的框架，来自于Apache。
* [Devise](https://github.com/plataformatec/devise) 针对Ruby平台
* [Omniauth](https://github.com/intridea/omniauth) 另一个Ruby平台的解决方案

随着行业的发展，在一个组织构建的多个产品/应用之间共享用户信息(共用同一个注册/登录服务器)的需求开始变得非常重要，
所以单点登录SSO（Single Sign On）应运而生，其中最著名的是
[Yale University CAS（Central Authentication Service）系统]，它既是一个SSO基于Java平台的
[实现](https://github.com/jasig/cas)，也是一个[协议](https://www.apereo.org/projects/cas)，
Ruby平台也有个对应的实现[rubycas](https://rubycas.github.io/)，
另一个流行的Ruby的CAS解决方案[casino](http://casino.rbcas.com/docs/)。

于此同时[Devise](https://github.com/plataformatec/devise)、
[apache-shiro](http://shiro.apache.org/introduction.html)
等认证管理框架也都进一步发展以更好的支持SSO系统。

很明显CAS依赖一个中心服务器来提供用户认证管理，这在互联网极迅速兴起的背景下显得略不协调，于是一个去中性化，
跨组织的SSO方案[OpenID](http://openid.net/)诞生了，其基本思想是把用户的认证功能提供给外部的服务去实现。

而另一方面，[OAuth](http://oauth.net/)协议的发展也异常迅速，特别是在国内的普及和应用非常广泛，例如：
QQ，weibo，wechat对OAuth协议都有很完善的支撑。
OAuth协议旨在解决用户个人数据安全分享的问题，例如分享QQ相册的内容给另一个照片冲印的在线应用。
但非常有趣的是，该协议还有另外一个流行的用法：分享用户的Profile信息给第三方应用——即使用QQ/微博进行登录。
在这个过程中第三方应用通过OAuth协议在取得用户基本信息的同时也认证了用户的身份，看起来和OpenID非常类似——
一个夸组织的SSO解决方案。

事实上，除了认证（authentication）还有授权（authorization），因为授权和特定系统的功能息息相关，
这就是为什么有很多不同层面的协议关于认证，但授权却鲜有标准。
但现在好了，就在最近几年出现了一个新的协议
[SAML(Security Assertion Markup Language)安全断言标记语言](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=security)，
它是一个基于XML的标准，用于在不同的安全域(security domain)之间交换*认证*和*授权*数据。
所谓的安全域是指：身份提供者（identity provider）和服务提供者（service provider）。
更多参考信息：
https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language
http://searchfinancialsecurity.techtarget.com/definition/SAML
https://www.onelogin.com/saml

http://www.ibm.com/developerworks/cn/websphere/library/techarticles/1111_luol_sso/1111_luol_sso.html
http://blog.csdn.net/csethcrm/article/details/20694993
http://blog.csdn.net/peterwanghao/article/details/4271813
http://www.2cto.com/kf/201312/268620.htmlW
