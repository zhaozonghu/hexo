---
title: JS与APP应用交互
categories: javascript
tags: javascript
---

JS打开app应用，如果不存在就去下载应用

	    var APPCommon = {
        iphoneSchema: 'yr-hotblood://',
        iphoneDownUrl: '',
        androidSchema: 'yr-hotblood://',
        androidDownUrl: '',
        openApp: function () {
            var this_ = this;
            //微信
            if (this_.isWeixin()) {
                if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
                    $('.wechat img').attr("src", "images/tips_weixin_ios.png");
                } else {
                    $('.wechat img').attr("src", "images/tips_weixin_android.png");
                }
                $(".wechat").css("height", $(window).height());
                $(".wechat").show();
            } else {//非微信浏览器
                var baseUrl = window.location.href.replace(/http:\/\/|https:\/\//, '');
                if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
                    var loadDateTime = new Date();
                    window.setTimeout(function () {
                        var timeOutDateTime = new Date();
                        if (timeOutDateTime - loadDateTime < 5000) {
                            window.location = this_.iphoneDownUrl;//ios下载地址
                        } else {
                            window.close();
                        }
                    }, 25);
                    $('#js_downSrc').attr('href', this_.iphoneDownUrl);
                    window.location = this.iphoneSchema + baseUrl;
                } else if (navigator.userAgent.match(/android/i)) {
                    try {
                        var appKey=this.androidSchema + baseUrl;
                            openApp(appKey,this.GetQueryString('refresh'));
                    } catch (e) {
                    }
                    $('#js_downSrc').attr('href', this_.androidDownUrl);
                }
            }
        },isWeixin: function () { //判断是否是微信
            var ua = navigator.userAgent.toLowerCase();
            if (ua.match(/MicroMessenger/i) == "micromessenger") {
                return true;
            } else {
                return false;
            }
        },GetQueryString(t){
            var n = new RegExp("(^|&)" + t + "=([^&]*)(&|$)", "i"), e = window.location.search.substr(1).match(n);
            return null != e ? decodeURI(e[2]) : null
        }
    };
    APPCommon.openApp();
    function openApp(appKey,refresh) {
        var isRefresh = refresh; // 获得refresh参数
        if(isRefresh == 1) {return}
        window.location = appKey;
        window.setTimeout(function () {
            window.location.href += '?refresh=1' // 附加一个特殊参数，用来标识这次刷新不要再调用myapp:// 了
        }, 500);
    }
	
注：iphoneSchema、androidSchema 需要开发在app中设置；android 应用不能直接打开，只能通过点击事件触发