---
title: Qrcode生成二维码
date: 2017-12-11 23:30:35
tags: [插件]
categories: 常用插件
---
 用于根据某字符串生成二维码
<!-- more -->
### 基本使用
``` bash
1、引入jq和jquery.qrcode.min.js即可
2、渲染区域元素<div id="qrcodeDiv"></div>
3、$('#qrcodeDiv').qrcode({
        render :  'canvas',//渲染方式table  canvas，table方式兼容更好些
        width  :  200,
        heigth :  200,
        text   :  'zisefengbao'
   })
4、这样即可扫描二维码，携带信息zisefengbao
```
### 中文字符支持
``` bash
1、默认不支持中文编码，采用charCodeAt（）方式进行编码转换，默认采用UTF-8方式编码。而中文一般采用UTF-16编码实现，会乱码。解决方法是在二维码编码钱转换UTF-8
2、实现代码：
function utf16to8(str) {
    var out, i, len, c;
    out = "";
    len = str.length;
    for (i = 0; i < len; i++) {
        c = str.charCodeAt(i);
        if ((c >= 0x0001) && (c <= 0x007F)) {
            out += str.charAt(i);
        } else if (c > 0x07FF) {
            out += String.fromCharCode(0xE0 | ((c >> 12) & 0x0F));
            out += String.fromCharCode(0x80 | ((c >> 6) & 0x3F));
            out += String.fromCharCode(0x80 | ((c >> 0) & 0x3F));
        } else {
            out += String.fromCharCode(0xC0 | ((c >> 6) & 0x1F));
            out += String.fromCharCode(0x80 | ((c >> 0) & 0x3F));
        }
    }
    return out;
};
3、使用方法:
utf16to8("张三")
```
### 中心区域添加自定义Logo图片   [参考网址](http://blog.csdn.net/gao36951/article/details/48975353)
``` bash
1、html:
<div id='qrcodeDiv'>
    <div id='codeico'></div>
</div>
2、css定位
#codeico{position:fixed;z-index:99999;width:30px;height:30px;background:url(weizhi.jpg) no-repeat 100%;
3、js定位
var margin = [$('#qrcodeDiv').height() - $('#codeico').height()] /2;
$('#codeico').css('margin',margin);
```

![效果.png](http://upload-images.jianshu.io/upload_images/3859151-1c2557ca65d9894a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 使用案例：客户端扫描PC端生成的二维码实现登录功能
``` bash
1、PC端请求接口获得密钥code
2、客户端扫码，传给后台code和userId
3、长轮询请求接口，传给后台code,用于后台判断密钥是否一致，一致的话登录成功，得到userId
```
### 5.http://blog.csdn.net/u011127019/article/details/51226104