---
title: 在ios桌面添加web站点图标及增加启动画面
categories: web
tags: [javascript]
---

设置全屏：

    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content=" black " />
    <meta name="apple-mobile-web-app-title" content="应用名称" />
    <meta name="full-screen" content="yes">
    <meta name="x5-fullscreen" content="true">
    <meta name="applicable-device" content="mobile">

添加图标到主屏幕：

    <link rel="apple-touch-icon" href="images/app/144x144.png" />
    <link rel="apple-touch-icon-precomposed" ref="images/app/144x144.png" />
    <link rel="apple-touch-startup-image" ref="images/app/144x144.png" />

添加启动界面：

    <link rel="apple-touch-startup-image" href="images/app/750x1294.png" media="(device-width: 375px) and (-webkit-device-pixel-ratio: 2)">
    <link rel="apple-touch-startup-image" href="images/app/1242x2148.png" media="(device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3)">
    <link rel="apple-touch-startup-image" href="images/app/2048x1496.png" sizes="2048x1496" media="screen and (min-device-width:481px) and (max-device-width:1024px) and (orientation:landscape) and (-webkit-min-device-pixel-ratio: 2)">  
    <link rel="apple-touch-startup-image" href="images/app/1536x2008.png" sizes="1536x2008" media="screen and (min-device-width:481px) and (max-device-width:1024px) and (orientation:portrait) and (-webkit-min-device-pixel-ratio: 2)">  

阻止iOS Web APP中点击链接跳转到Safari 浏览器新标签页：

    <script>
        if(('standalone' in window.navigator)&&window.navigator.standalone){
            var noddy,remotes=false;
            document.addEventListener('click',function(event){
                    noddy=event.target;
                    while(noddy.nodeName!=='A'&&noddy.nodeName!=='HTML') noddy=noddy.parentNode;
                    if('href' in noddy&&noddy.href.indexOf('http')!==-1&&(noddy.href.indexOf(document.location.host)!==-1||remotes)){
                            event.preventDefault();
                            document.location.href=noddy.href;
                    }
            },false);
        }
    </script>