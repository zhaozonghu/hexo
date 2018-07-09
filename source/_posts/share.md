---
title: 移动端H5多平台分享实践
categories: javascript
tags: [web,js]
---
如何按照产品要求实现多平台下一致的分享效果，包括分享文案的动态生成，在没有Native的帮助下是比较困难的。下面总结了一套较为完整的分享方案。
<!--more-->

#### 微信分享

微信下的分享通过调用微信提供的JS-SDK和引导用户点击右上角完成。 使用微信的JS-SDK需要引入如下js文件：

    <script src="//res.wx.qq.com/open/js/jweixin-1.3.1.js"></script>

设置分享内容的代码如下：

    wx.config({
        debug: false,
        appId: '公众号的appid',
        timestamp: '时间戳',
        nonceStr: '随机字符串',
        signature: '签名',
        jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage', 'onMenuShareQQ', 'onMenuShareWeibo', 'onMenuShareQZone', 'showOptionMenu', 'hideAllNonBaseMenuItem', 'showAllNonBaseMenuItem']
    });

    wx.ready(function() {
        const share = {
            title: '分享标题（朋友圈只显示标题）',
            desc: '分享内容',
            imgUrl: '图片URL',
            link: '分享链接（最好是后台JS安全域名）',
            success: function() {
                hideMaskLayer();  // 分享成功，隐藏引导用户分享的浮层
            },
            cancel: function() {
            }
        };
        wx.onMenuShareAppMessage(share);  // 微信好友
        wx.onMenuShareTimeline(share);  // 朋友圈
        wx.onMenuShareQQ(share);  // QQ
        wx.onMenuShareQZone(share);  // QQ空间
        wx.onMenuShareWeibo(share);  // 腾讯微博
    });

其中wx.config中的参数由服务端得到，具体可参见微信的开发文档：https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115 注意在公众号后台设置JS安全域名

#### QQ/TIM的分享

1 通过JS API分享

QQ（以下无特殊说明TIM下同样有效）下也有设置分享内容的API，同样需要先引用JSBridge相关的文件：<tr script src="//open.mobile.qq.com/sdk/qqapi.js"></ script>
设置分享内容的代码如下：

    const share = {
        title: '分享标题，最大45字节',
        desc: '分享内容，最大60字节',
        image_url: '图片URL',
        share_url: '分享链接'
    };
    mqq.data.setShareInfo(share, callback);

需要注意的是：分享链接长度不能超过120字节，并且必须跟页面URL同一个域名，否则设置不生效；分享的图片最小需要200 * 200，否则分享到QQ空间时会被过滤掉。
设置完分享内容后，可通过API调用唤起QQ的分享面板，免去引导的过程。

    mqq.ui.showShareMenu();

还有一种方法，QQ提供了监听点击分享平台的事件，当点击Native分享面板中的分享平台时，会触发此事件，QQ默认的分享行为将不再执行。代码如下：

    mqq.ui.setOnShareHandler(function (platform) {
        mqq.ui.shareMessage({
            title: '分享标题',
            desc: '分享内容',
            share_type: platform,
            share_url: '分享链接',
            image_url: '图片URL',
            sourceName: '掌上理工大',
            back: true
        }, function() {
        });
    });

其中platform是分享平台类型，取值如下：

    编号	    分享平台
    0	      QQ好友
    1	      QQ空间
    2	      微信好友
    3	      微信朋友圈

2 通过meta标签分享
QQ也支持通过设置meta标签定义分享内容。通过定义itemprop可设置分享内容，同时为了更好的兼容其它平台，我们也引入了Open Graph标准。代码如下：

    <meta itemprop="name" property="og:title" content="分享标题">
    <meta property="og:url" content="分享链接">
    <meta itemprop="image" property="og:image" content="图片URL">
    <meta name="description" itemprop="description" property="og:description" content="分享描述">

需要注意的是，meta标签需要是服务端渲染输出，通过js生成或修改无效。

3 通过URL Scheme唤起QQ分享
还可以通过URL Scheme唤起QQ进行分享，该方法的好处在于可以在非QQ环境下唤起QQ实现分享，缺点在于不能设置分享图片。代码如下：

    const share = {
        title: '分享标题',
        desc: '分享内容',
        share_url: '分享链接'
    };
    const url_scheme = '//share/to_fri?src_type=web&version=1&file_type=news&share_id=1103437993&title=' + Base64.encode(share.title) + '&thirdAppDisplayName=5o6M5LiK55CG5bel5aSn&url=' + Base64.encode(share.share_url) + '&description=' + Base64.encode(share.desc);
    location.assign('mqqapi:' + url_scheme);
    setTimeout(function() {
        location.assign('timapi:' + url_scheme);
    }, 2000);

其中分享的参数在拼入URL中时需要Base64编码。为了支持TIM下的分享，我们引入了延时函数，如果唤起QQ失败该定时器将会执行唤起TIM，唤起成功离开了此页面将不会执行。QQ和TIM均安装时优先唤起QQ。

4 通过分享组件的URL实现
QQ空间提供了分享组件（可参见：https://connect.qq.com/intro/share），通过分析该组件可得到分享URL的参数。代码如下：

    const share = {
        title: '分享标题',
        desc: '分享内容',
        image_url: ['图片URL'],
        share_url: '分享链接'
    };
    let image_urls = share.image_url.map(function(image) {
        return encodeURIComponent(image);
    });
    location.replace('https://sns.qzone.qq.com/cgi-bin/qzshare/cgi_qzshare_onekey?url=' + encodeURIComponent(share.share_url) + '&site=xxxx&title=' + share.title + '&pics=' + image_urls.join('|') + '&summary=' + share.desc);

其中可支持多图片的分享，图片URL用竖线分隔。该方法优点在于同样支持非QQ环境下的分享。非QQ下用户登录后即可分享，QQ下可免登直接分享。

#### 微博的分享

1 通过分享组件的URL实现
微博同样提供了分享组件，通过分析URL可得到分享参数。代码如下：

    const share = {
        title: '分享标题',
        image_url: '图片URL',
        share_url: '分享链接'
    };
    location.replace('https://service.weibo.com/share/share.php?url=' + encodeURIComponent(share.share_url) + '&title=' + share.title + '&appkey=277159429&&ralateUid=1855112015&pic=' + share.image_url + '&searchPic=true');

其中appKey参数可选，如果设置微博将会显示分享来源为key对应的应用名称（应用可在 https://open.weibo.com/ 注册）；ralateUid参数可选，指定微博用户id，会在微博尾部at此用户；searchPic用于控制是否自动爬取页面的图片，和pic不共存。

2 通过微博API自动发送微博
微博提供了API可通过服务端调用接口直接发送一条微博。

    POST https://api.weibo.com/2/statuses/share.json

参数如下：

    参数            必选            类型及范围         说明编号
    access_token    是               string           采用OAuth授权方式为必填参数，OAuth授权后获得。
    status          是               string           用户分享到微博的文本内容，必须URL Encode，文本中不能包含“#话题词#”，同时文本中必须包含至少一个分享的URL，且该URL的域名需要在后台设置。
    pic             否               binary           用户分享到微博的图片，仅支持JPEG、GIF、PNG图片，上传图片大小限制为5M。上传图片时，POST方式提交请求，需要采用multipart/form-data编码方式。
    rip             否               string           开发者上报的操作用户真实IP，形如：211.156.0.1。

具体可参见：[微博开放平台](http://open.weibo.com/wiki/2/statuses/share)

3 通过JS API分享
微博同样有提供JS-SDK可供调用Native的方法，在使用前需要在微博开放平台申请轻应用，并设置安全域名。
使用微信的JS-SDK需要引入如下js文件：

    <script src="//tjs.sjs.sinajs.cn/open/thirdpart/js/jsapi/mobile.js"></script>
    
同样需要先设置初始化参数。

    WeiboJS.init({
        appkey : '轻应用key',
        debug: false,
        timestamp: '时间戳',
        noncestr: '随机字符串',
        signature: '签名',
        scope: ['openMenu', 'setMenuItems', 'menuItemSelected', 'setSharingContent']
    }, function() {
    });

有3个关于分享的JS API可供使用。

    openMenu

该API可调起Native的分享面板。

    WeiboJS.invoke('openMenu');
    setSharingContent

该API可设置分享的内容。

    WeiboJS.invoke('openMenu', {
        icon: share.title,
        desc: share.desc,
        icon: share.image_url
    });

    invokeMenuItem

该API可以直接触发分享面板中点击相应菜单项。

    WeiboJS.invoke('invokeMenuItem', { 
        code: platform
    });

其中platform是分享平台类型，取值如下：

    编号            分享平台
    1001            微博
    1002            微博好友圈
    1003            微博私信
    1004            微信好友
    1005            微信朋友圈
    1006            微米好友
    1007            微米圈
    1008            来往（点点虫）好友
    1009            来往（点点虫）动态
    1010            QQ好友
    1011            QQ空间
    1101            短信
    2001            系统浏览器
    2002            复制链接

通过使用API 1 + API 2（最佳）或API 2 + API 3即可实现分享。具体可参见微博的开发文档：
[open.weibo.com/wiki/轻应用H5新…](http://open.weibo.com/wiki/%E8%BD%BB%E5%BA%94%E7%94%A8H5%E6%96%B0%E7%89%88JS)

#### 支付宝的分享

支付宝同样提供了JS API，可以很方便的设置分享内容和唤起Native分享面板；不足之处在于不支持分享到QQ、微信等平台。
使用支付宝的JS-SDK需要引入如下js文件：

    <script src="//a.alipayobjects.com/g/h5-lib/alipayjsapi/3.0.6/alipayjsapi.inc.min.js"></script>

代码如下：

    const share = {
        title: '分享标题',
        content: '分享内容',
        image: '图片URL',
        url: '分享链接',
        captureScreen: false,
        showToolBar: false
    };
    ap.share(share, function() {
    });

#### UC浏览器的分享

UC浏览器也提供了JS API调用Native的分享，支持唤起分享面板和分享到具体平台。代码如下：

    const share = {
        title: '分享标题',
        desc: '分享内容',
        image_url: '图片URL',
        share_url: '分享链接'
    };
    const isiOS = /(iPhone|iPad|iPod)/.test(navigator.userAgent);  // 判断应用平台
    if (isiOS) {
        if (ucbrowser.web_shareEX) {
            ucbrowser.web_shareEX(
                JSON.stringify({
                    title: share.title,
                    content: share.desc,
                    sourceUrl: share.share_url,
                    imageUrl: share.image_url,
                    source: '掌上理工大',
                    target: platform
                })
            )
        } else {
            ucbrowser.web_share(share.title, share.desc, share.share_url, platform, '', '掌上理工大', share.image_url);
        }
    }
    else ucweb.startRequest('shell.page_share', [share.title, share.desc, share.share_url, platform, '', '掌上理工大', share.image_url]);

其中platform是分享平台类型，取值如下：

Android编号           iOS编号               分享平台
'WechatFriends'       'kWeixinFriend'      微信好友
'WechatTimeline'      'kWeixin'            微信朋友圈
'QQ'                  'kQQ'                QQ好友
'Qzone'               'kQZone'             QQ空间
'SinaWeibo'           'kSinaWeibo'         微博
undefined             undefined            分享面板

#### QQ浏览器的分享

QQ浏览器也提供了JS API调用Native的分享，同样支持唤起分享面板、生成二维码和分享到具体平台。
首先需要引入如下JS文件：

    <script src="//jsapi.qq.com/get?api=app.share"></script>

代码如下：

    browser.app.share({
        title: share.title,
        description: share.desc,
        url: share.share_url,
        img_url: share.image_url,
        from: '掌上理工大',
        to_app: platform
    });

其中platform是分享平台类型，取值如下：

    编号        分享平台
    1           微信好友
    8           微信朋友圈
    4           QQ好友
    3           QQ空间
    11          微博
    5           更多
    7           生成二维码
    10          复制链接
    undefined   分享面板

作者：Crazy_Urus
链接：https://juejin.im/post/5a61a8b86fb9a01cba42a742
来源：掘金