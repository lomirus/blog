# 几个知乎和简书的油猴脚本

初始创建于：***2020/12/03***

最后编辑于：***2020/12/17***

## 写脚本的起因

昨天晚上突然想了解一下 Arch，于是打开百度键入关键词回车。在搜索结果页面发现了一个知乎页面，然后就不加思索地点开了。结果没想到，我一突开就突然冒出了个登录框。当时我就心想，算了，登录框就登录框吧，点一下关闭按钮不就行了嘛，毕竟知乎搞弹窗也不是一两天了。

![1](1.png)

因为以前的话我一般是通过点击登录框后面的阴影层来把它关闭的，所以这次我也下意识地点了一下，但是却发现没有反应。我仔细一看，发现登录页面好像是被更新过了，再仔细一看，发现关闭按钮也没了。不知所措的我又抱着死🐎当活🐎医的心态多点了一下各个位置，结果依旧——毫无反应。

> 我向来是不惮以最坏的恶意来推测知乎的。但这回却很有几点出于我的意外。一是管理者竟会这样地凶残，一是产品经理竟至如此之下劣，一是知乎用户临难竟能如是之从容。

虽然以前早就因为知乎各种恶心操作而退乎了，但是有时候查一些资料时还是要用到的。但是，我是无论如何都无法接受这种雷普用户的行为的，所以我最终准备写个脚本，来和知乎斗智斗勇。

## Talk is cheap, show me the code

说实话，其实一开始我也是懒得写脚本的。本就想着用个 Adblock 把他屏蔽掉，但是试过之后发现页面无法滚动，查看元素之后发现是因为`html`元素的`overflow`属性被设置`hidden`了。虽然貌似也能通过再加个 Stylish 插件来解决，但是这样实在太臃肿了，所以最终还是想着自己写个脚本。下面就是屏蔽知乎登录框的代码：

```javascript
// ==UserScript==
// @name         屏蔽知乎登录框
// @namespace    http://tampermonkey.net/
// @version      0.1.0
// @description  屏蔽知乎登录框
// @author       Anno
// @match        https://www.zhihu.com/question/*
// @grant        none
// ==/UserScript==

'use strict';
window.addEventListener('load',function(){
    console.clear()
    const html = document.querySelector('html')
    const divs = document.querySelectorAll('body>div')
    for(let i = 1;i < divs.length;i++) divs[i].remove()
    html.style.overflow = 'auto'
})
```

其实`console.clear()`加不加都无所谓，主要因为我调试脚本时发现知乎页面的控制台有一堆报错和警告，所以就先清理一下（结果最终调试完之后决定还是把它留了下来）。

## 方脱知乎，又入简书

过了一会儿又去查了点有关 Canvas 粒子特效的资料，结果找到了一篇简书的文章。里面有个例子似乎不错，于是就点开了链接。不过打开之后并不是直接跳转到目标页面，而是进入了一个中转页面，提示安全性未知 blablabla. 

![2](2.png)

这一般网站虽然也有差不多的提示，但是也不至于还得让用户自个儿复制粘贴一条龙吧？恁直接给个按钮确认跳转不行？害，您还别说(shùo)，这简书，他还就真(zhèn)不行！

于是懒得复制粘贴的我一不做二不休，索性也给简书写了个脚本，代码如下：

```javascript
// ==UserScript==
// @name         简书外链自动跳转
// @namespace    http://tampermonkey.net/
// @version      0.1.0
// @description  简书外链自动跳转
// @author       Anno
// @match        https://www.jianshu.com/go-wild*
// @grant        none
// ==/UserScript==

'use strict';
(function() {
    document.write('Redirecting...')
    const search = location.search
    const query = search.slice(1, search.length).split("&")
    for (let i in query) {
        let kvp = query[i].split('=')
        if (kvp[0] == 'url'){
            location.href = unescape(kvp[1])
        }
    }
})();
```

## 既离简书，复返知乎

既然都给简书写了个自动跳转的脚本，那也不能让知乎干愣着呀。所以知乎外链自动跳转脚本也赶紧给安排上：

```javascript
// ==UserScript==
// @name         知乎外链自动跳转
// @namespace    http://tampermonkey.net/
// @version      0.1.0
// @description  知乎外链自动跳转
// @author       Anno
// @match        http*://link.zhihu.com/*
// @grant        none
// ==/UserScript==

'use strict';
(function() {
    document.write('Redirecting...')
    const search = location.search
    const url = search.slice(8, search.length)
    location.href = unescape(url)
})();
```

另外有一个要注意的地方就是，`link.zhihu.com`的协议好像是不固定的，有的是`http`有的则是`https`，一开始我的`@match`只匹配了`https`的，没想到知乎居然有的页面还在用`http`，所以当时 debug 了半天硬是没找出来，甚至还以为是 Tampermonkey 出 bug 了(°ー°〃)

## 修订记录

### 2020/12/17

* 修正了一个错别字





