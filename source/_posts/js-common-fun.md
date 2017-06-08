---
title: Javascript常用方法
categories: javascript
tags: [web,js]
---
###### 正则表达式获取地址栏参数

	function GetQueryString(t){
        var n = new RegExp("(^|&)" + t + "=([^&]*)(&|$)", "i"), e = window.location.search.substr(1).match(n);
        return null != e ? decodeURI(e[2]) : null
	}

<!--more-->
	
###### 获取指定数量随机数

	//max数组返回[0-max]
	function randomNum(max){
		var randoms=[];
		while (true)
		{
			var isExists = false;
			var random = parseInt(0 + max * (Math.random()))
			for (var i = 0; i < randoms.length; i++) {
				if (random === randoms[i]) {
					isExists = true;
					break;
				}
			}
			if (!isExists) randoms.push(random);
			//设置返回数组长度（此处默认为6位）
			if (randoms.length === 6)break;
		}
		return randoms;
	}

###### 获取浏览器类型

    function getBrowserType() {
        var t = navigator.userAgent.toLowerCase(), n = "";
        return t.indexOf("msie") > 0 && (n = "IE"), t.indexOf("firefox") > 0 && (n = "firefox"), t.indexOf("chrome") > 0 && t.indexOf("mb2345browser") < 0 && t.indexOf("360 aphone browser") < 0 && (n = "chrome"), (t.indexOf("360 aphone browser") > 0 || t.indexOf("qhbrowser") > 0) && (n = "360"), t.indexOf("ucbrowser") > 0 && (n = "UC"), t.indexOf("micromessenger") > 0 && (n = "WeChat"), (t.indexOf("mqqbrowser") > 0 || t.indexOf("qq") > 0) && t.indexOf("micromessenger") < 0 && (n = "QQ"), t.indexOf("miuibrowser") > 0 && (n = "MIUI"), t.indexOf("mb2345browser") > 0 && (n = "2345"), t.indexOf("sogoumobilebrowser") > 0 && (n = "sogou"), t.indexOf("liebaofast") > 0 && (n = "liebao"), t.indexOf("safari") > 0 && t.indexOf("chrome") < 0 && t.indexOf("ucbrowser") < 0 && t.indexOf("micromessenger") < 0 && t.indexOf("mqqbrowser") < 0 && t.indexOf("miuibrowser") < 0 && t.indexOf("mb2345browser") < 0 && t.indexOf("sogoumobilebrowser") < 0 && t.indexOf("liebaofast") < 0 && t.indexOf("qhbrowser") < 0 && (n = "safari"), n
    }
	
###### 获取设备系统类型及版本号

	function getOsType() {
        var t = navigator.userAgent.toLowerCase(), n = "";
        if (/android/i.test(navigator.userAgent)) {
            var e = t.indexOf("android");
            version = t.substr(e + 8, 3), n = "Android " + version
        }
        if (/(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent)) {
            var e = t.indexOf("os");
            version = t.substr(e + 3, 3), n = "iOS " + version
        }
        return !/Linux/i.test(navigator.userAgent) || /android/i.test(navigator.userAgent) || /(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent) || (n = "Linux"),
        /windows|win32/i.test(navigator.userAgent) && (n = "windows32"), /windows|win32/i.test(navigator.userAgent) && (n = "windows64"), n
    }
	
###### 向页面注入js代码

	//参数说明：
	// t：js内容（字符串）
	// n: 回调函数
	// e: 注入js的位置，eg：document.getElementById('indexHeader')
	function createScript(t, n, e) {
        if (t) {
            var r = document.getElementsByTagName("head")[0], i = document.createElement("script");
            i.setAttribute("type", "text/javascript"), i.innerHTML = t, e ? e.appendChild(i) : r.appendChild(i), n()
        }
    }
	
###### 向页面注入css代码

	//参数说明：
	// t：css内容（字符串）
	// n: 回调函数
	// e: 注入css的位置，eg：document.getElementById('indexHeader')
	function createStyle(t, n, e) {
		if (t) {
            var r = document.getElementsByTagName("head")[0], i = document.createElement("style");
            i.innerHTML = t, e ? e.appendChild(i) : r.appendChild(i), n && n()
        }
    }
	
###### 时间格式化

	//调用方法:getSpecialTimeStr('2017-04-14 15:50:00')
	function getSpecialTimeStr(t) {
        var n = this.strToTime(t);
        if (!n)return !1;
        var e = (new Date).getTime(), r = Number(e - n), i = 864e5, o = 36e5, u = 6e4;
        if (r >= i) {
            var a = r / i;
            return a > 2 ? this.timeToString(n) : a > 1 ? "前天" : "昨天"
        }
        return r >= o ? Math.floor(r / o) + "小时前" : r >= u ? Math.floor(r / u) + "分钟前" : "最新"
    }
	
###### 公用方法

	var GLOBAL = {},
	 GLOBAL.Js = {
		trim: function (t) {
			return t.replace(/^\s+|\s+$/g, "")
		}, isNumber: function (t) {
			return !isNaN(t)
		}, isString: function (t) {
			return "string" == typeof t
		}, isBoolean: function (t) {
			return "boolean" == typeof t
		}, isFunction: function (t) {
			return "function" == typeof t
		}, isNull: function (t) {
			return null === t
		}, isUndefined: function (t) {
			return "undefined" == typeof t
		}, isEmpty: function (t) {
			return /^\s*$/.test(t)
		}, isArray: function (t) {
			return t instanceof Array
		}
	},
	//cookie操作
	GLOBAL.Cookie = {
		set: function (t, n, e) {
			var r = e ? 60 * Number(e) * 60 * 1e3 : 864e5, i = new Date;
			i.setTime(i.getTime() + r);
			var o = e ? "; expires=" + i.toUTCString() : "";
			document.cookie = t + "=" + encodeURI(n) + o;
		}, get: function (t) {
			var arr,reg=new RegExp("(^| )"+t+"=([^;]*)(;|$)");
			if(arr=document.cookie.match(reg)){ return unescape(arr[2]);}else{return null;}
		}, del: function (t) {
			var cval=this.get(t);
			if(cval!=null){
				document.cookie= t + "="+cval+";expires="+(new Date(0)).toGMTString();
			}
		}
	}, 
	//数组比较返回差异的地方
	GLOBAL.Array = {
		difference: function (t, n) {
			try {
				var e = [], r = 0, i = t.length;
				for (r = 0; r < i; r++)n.contains(t[r]) || e.push(t[r]);
				return e
			} catch (o) {
				return console.error(o), t
			}
		}
	}, 
	//终端判断
	GLOBAL.Os = function () {
		for (var t = navigator.userAgent, n = new Array("Android", "iPhone", "SymbianOS", "Windows Phone", "iPad", "iPod"), e = !1, r = 0; r < n.length; r++)if (t.indexOf(n[r]) > -1) {
			e = !0;
			break
		}
		return {
			mobile: e,
			ios: !!t.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
			android: t.indexOf("Android") > -1 || t.indexOf("Linux") > -1,
			iphone: t.indexOf("iPhone") > -1,
			ipad: t.indexOf("iPad") > -1,
			webapp: t.indexOf("Safari") === -1
		}
	}(), 
	//浏览器类型判断
	GLOBAL.Browser = function () {
		var t = navigator.userAgent, n = GLOBAL.Os.mobile;
		return n ? {
			wechat: t.indexOf("MicroMessenger") > -1,
			weibo: t.toLowerCase().indexOf("weibo") > -1,
			qq: t.indexOf("QQ/") > -1,
			qqbrowser: t.indexOf("MQQBrowser") > -1,
			qybrowser: t.indexOf("QianYing") > -1
		} : {}
	}();
	
	