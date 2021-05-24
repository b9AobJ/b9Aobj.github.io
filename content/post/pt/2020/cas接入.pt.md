---
title: "Cas接入" # Title of the blog post.
date: 2021-05-11T16:34:30+08:00 # Date of post creation.
description: "Article description." # Description used for search engine.
featured: true # Sets if post is a featured post, making it appear on the sidebar. A featured post won't be listed on the sidebar if it's the current page
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
#featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
#thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - CAS
tags:
  - CAS
  - JavaScript
---

### SSO介绍
#### 背景
在企业发展初期，企业使用的系统很少，通常一个或者两个，每个系统都有自己的登录模块，运营人员每天用自己的账号登录，很方便。
但随着企业的发展，用到的系统随之增多，运营人员在操作不同的系统时，需要多次登录，而且每个系统的账号都不一样，这对于运营人员来说，很不方便。于是，就想到是不是可以在一个系统登录，其他系统就不用登录了呢？这就是单点登录要解决的问题。


#### 定义
`单点登录`英文全称Single Sign On，简称就是SSO。它的解释是：在多个应用系统中，只需要登录一次，就可以访问其他相互信任的应用系统.

![01](/images/2020/cas01.png)
如图所示，图中有4个系统，分别是Application1、Application2、Application3、和SSO。Application1、Application2、Application3没有登录模块,只负责业务模块，而SSO只有登录模块，没有其他的业务模块，当Application1、Application2、Application3需要登录时，将跳到SSO系统，SSO系统完成登录，其他的应用系统也就随之登录了。这完全符合我们对单点登录（SSO）的定义。

同域下的单点登录是运用了Cookie同域共享的特性,`但这不是真正的单点登录`。
因为如果是不同域呢？不同域之间Cookie是不共享的，怎么办？
这里我们就要说一说CAS了，CAS流程才是单点登录`正宗的解决方案`。

#### CAS介绍

`CAS` （ Central Authentication Service ） 是 Yale 大学发起的一个企业级的、开源的项目，旨在为 Web 应用系统提供一种可靠的单点登录解决方法（属于 Web SSO ）。

从结构上看，CAS系统包含两个部分：CAS Server 和CAS Client
`CAS Server` 负责对用户的认证工作；需要独立部署,有官方的详细教程.
`CAS Client` 负责处理对客户端受保护资源的访问请求; 与受保护的客户端应用部署在一起，以Filter方式保护 Web 应用的受保护资源，过滤从客户端过来的每一个 Web请求.

CAS Client 没有官方的Node版本,需要自己实现.
`所以今天我就来谈谈Node与 CAS Client集成的实现`.

#### CAS理论

CAS  协议定义了一组术语，一组票据，一组接口。
##### 术语：
* `Service`: 需要使用单点登录的各个服务。
* `CAS Server`: 中心服务器，也是SSO中负责单点登录的服务器。
* `CAS Client`: 各个service的后台。

##### 接口：
*	/login：登录接口，用于登录到中心服务器。
*	/logout：登出接口，用于从中心服务器登出。
*	/validate：用于验证用户是否登录中心服务器。
*	/serviceValidate：用于让各个 service 验证是否登录中心服务器。

##### 票据：
* TGT：Ticket Grangting Ticket
TGT是CAS为用户签发的登录票据，拥有了TGT，用户就可以正名用户在CAS成功登录过。TGT封装了Cookie值对应的用户信息。当HTTP请求到来时，CAS以此Cookie值（TGC)为key查询缓存中有无TGT，如果有的话，则相信用户已登录过。

* TGC： Ticket Granting Cookie
CAS Server生成TGT放入自己的Session中，而TGC就是这个Session的唯一标识（SessionId），以Cookie形式放到浏览器端，是CAS Server用来明确用户身份的凭证。

* ST： Service Ticket
ST是CAS用户签发的访问某一service的票据。用户访问service时，service发现用户没有ST，则要求用户去CAS获取S。用户想CAS发出获取ST的请求，CAS发现用户有TGT，则签发一个ST，返回给用户。用户拿着ST去访问service，service拿ST去CAS验证，验证通过后，允许用户访问资源。

### CAS流程

#### 官方流程图
![02](/images/2020/cas02.png)
官方文档:https://apereo.github.io/cas/6.0.x/protocol/CAS-Protocol.html

#### 流程图
![03](/images/2020/cas03.png)

#### 流程图解析
1、 用户访问产品a，域名是www.a.cn.
2.	由于用户没有携带在 a 服务器上登录的 a cookie，所以 a 服务器返回 http 重定向，重定向的 url 是 SSO 服务器的地址，同时 url 的 query 中通过参数指明登录成功后，回跳到 a 页面。重定向的url 形如 sso.dxy.cn/login?service=https%3A%2F%2Fwww.a.cn。
3.	由于用户没有携带在 SSO 服务器上登录的 TGC（看上面，票据之一），所以 SSO 服务器判断用户未登录，给用户显示统一登录界面。用户在 SSO 的页面上进行登录操作。
4.	登录成功后，SSO 服务器构建用户在 SSO 登录的 TGT（又一个票据），同时返回一个 http 重定向。这里注意：重定向地址为之前写在 query 里的 a 页面。重定向地址的 query 中包含 sso 服务器派发的 ST。重定向的 http response 中包含写 cookie 的 header。这个 cookie 代表用户在 SSO 中的登录状态，它的值就是 TGC。
5.	浏览器重定向到产品 a。此时重定向的 url 中携带着 SSO 服务器生成的 ST。
6.	根据 ST，a 服务器向 SSO 服务器发送请求，SSO 服务器验证票据的有效性。验证成功后，a 服务器知道用户已经在 sso 登录了，于是 a 服务器构建用户登录 session，记为 a session。并将 cookie 写入浏览器。注意，此处的 cookie 和 session 保存的是用户在 a 服务器的登录状态，和 CAS 无关。
7.	之后用户访问产品 b，域名是 www.b.cn。 
8.	由于用户没有携带在 b 服务器上登录的 b cookie，所以 b 服务器返回 http 重定向，重定向的 url 是 SSO 服务器的地址，去询问用户在 SSO 中的登录状态。
9.	浏览器重定向到 SSO。注意，第 4 步中已经向浏览器写入了携带 TGC 的cookie，所以此时 SSO 服务器可以拿到，根据 TGC 去查找 TGT，如果找到，就判断用户已经在 sso 登录过了。
10.	SSO 服务器返回一个重定向，重定向携带 ST。注意，这里的 ST 和第4步中的 ST 是不一样的，事实上，每次生成的 ST 都是不一样的。
11.	浏览器带 ST 重定向到 b 服务器，和第 5 步一样。
12.	b 服务器根据票据向 SSO 服务器发送请求，票据验证通过后，b 服务器知道用户已经在 sso 登录了，于是生成 b session，向浏览器写入 b cookie。


### 实践演示
发车了......

#### CAS 域名配置

*	cas server 域名  https://cas.ucloudadmin.com
*	cas client 域名   www.clienta.com	www.clientb.com

#### host 配置
```
#CAS
127.0.0.1   clienta.com
127.0.0.1   clientb.com
```

#### nginx 配置
```
server {
listen 80;
server_name clienta.com;

location / {
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_pass http://127.0.0.1:1024;
}
}

server {
listen 80;
server_name clientb.com;

location / {
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_pass http://127.0.0.1:1025;
}
}
```

#### 使用
##### 启动 cas client 服务
```
cd clienta
npm start // or node app.js
cd clientb
npm start // or node app.js
```

##### clienta 代码
```
// app.js
const express = require('express')
const app = express()
const session = require('express-session');
const cas = require('connect-cas');
const URL = require('url');
const querystring = require("querystring");

const casServer = 'cas.ucloudadmin.com'
const casClient = 'clienta.com'

// var _defaults = {
//     protocol: 'https',
//     host: undefined,
//     hostname: undefined, // ex. google
//     port: 443,
//     gateway: false, // set to true only if you wish to do authentication instead of authorization
//     paths: {
//         validate: '/cas/validate',               // not implemented
//         serviceValidate: '/cas/serviceValidate', // CAS 2.0
//         proxyValidate: '/cas/proxyValidate',     // not implemented
//         proxy: '/cas/proxy',                     // not implemented
//         login: '/cas/login',
//         logout: '/cas/logout'
//     }
// }

/*
var myOptions = {
    protocol: 'https',
    host: 'cas.ucloudadmin.com',
    hostname: 'ucloud', // ex. google
    port: 443,
    gateway: false, // set to true only if you wish to do authentication instead of authorization
    paths: {
        validate: '/cas/validate',               // not implemented
        serviceValidate: '/cas/serviceValidate', // CAS 2.0
        proxyValidate: '/cas/proxyValidate',     // not implemented
        proxy: '/cas/proxy',                     // not implemented
        login: '/cas/login',
        logout: '/cas/logout'
    }
}
cas.configure(myOptions);
*/

// Your CAS server's hostname
cas.configure({
    protocol: 'https',
    host: casServer,
});
console.log(cas.configure());

app.use(express.json());
app.use(express.urlencoded({ extended: true }))

app.use(
    session({
        secret: 'clienta',
        name: 'SESSIONID',
        resave: true,
        saveUninitialized: true,
        cookie: { maxAge: 24 * 60 * 60 * 1000, httpOnly: true }, //过期时间 24 小时
    }),
);

// 用户验证
app.use(function (req, res, next) {
    console.log('req.url', req.url);
    let path = req.url.split('?')[0]
    // 跳过登录请求
    if (path === '/login') {
        next()
    } else {
        if (req.session.cas && req.session.cas.user) {
            next()
        } else {
            return res.send(`<h2>this is clientA</h2><p>You are not logged in. <a href=/login?service=http://${casClient}${req.url}>Log in now.</a><p>`);
            // 自动跳转登录
            // return res.redirect(`/login?service=http://${casClient}${req.url}`);
        }
    }
});

app.get('/', function (req, res) {
    console.log('get /', req.url)
    return res.send('<h2>this is clienta</h2><p>You are logged in. Your username is ' + req.session.cas.user + '. <a href="/logout">Log Out</a></p>');
});

app.get('/login', cas.serviceValidate(), cas.authenticate(), function (req, res) {
    console.log('/login', req.url)
    let arg = URL.parse(req.url).query;
    let params = querystring.parse(arg);
    // console.log("params-service " + params.service);
    // 重定向到初始请求地址
    return res.redirect(params.service || '/');
});

app.get('/logout', function (req, res) {
    if (!req.session) {
        return res.redirect('/');
    }
    // Forget our own login session
    if (req.session.destroy) {
        req.session.destroy();
    } else {
        // Cookie-based sessions have no destroy()
        req.session = null;
    }
    // Send the user to the official campus-wide logout URL
    var options = cas.configure();
    options.pathname = options.paths.logout;
    return res.redirect(URL.format(options));
});


// 业务接口路由
app.get('/api', function (req, res) {
    return res.send('<h2>this is clientA API Page</h2><p>You are logged in. Your username is ' + req.session.cas.user);
});
app.get('/api/userInfo', function (req, res) {
    res.json({ retCode: '1', data: req.session.cas });
});

// ......

app.listen(1024, () => {
    console.log('示例程序正在监听 1024 端口！')
});
```

##### 登录 cas client 前台

*	浏览器打开 clientA  clientB

*	跳转 cas server 验证 / 登录
