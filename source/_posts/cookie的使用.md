---
title: cookie的使用
date: 2017-12-11 23:05:28
tags: [插件]
categories: 常用插件
---
cookie的增删改查
<!-- more -->
### 使用场景
- **保存用户登录状态。**例如将用户id存储于一个cookie内，这样当用户下次访问该页面时就不需要重新登录了。 
- **追踪用户行为。**例如天气预报网站，系统能够记住上一次访问的地区。 
- **定制页面。**如果网站提供了换肤功能，当用户下次访问时，仍然可以保存上一次访问的界面风格。 
- **创建购物车。**例如淘宝网就使用cookie记录了用户曾经浏览过的商品，方便随时进行比较。 

### cookie的设置
- **设置cookie：**
``` bash
1、document.cookie = 'userId=121;phone=1311111111';
2、document.cookie="str="+escape("I love ajax");(相当于document.cookie="str=I%20love%20ajax";为了避免空格符号等，使用escape()统一编码)
3、当使用escape()编码后，在取出值以后需要使用unescape()进行解码才能得到原来的cookie值。
```
- **获取cookie值：**
``` bash
1、document.cookie直接获取所有的cookie
2、若想获得单个cookie，需要进行字符串和数组的相关操作。
```
- **删除cookie：**
``` bash
1、删除cookie，可将其过期时间设定为一个过去的值。
```
### 函数
- **添加cookie：addCookie(name,value,time)**
- **调用方法：addCookie('userId','121',5)**
``` bash
function addCookie(name,value,time){
	var str = name+"="+escape(value);
	if(time>0){//time为0时不设定过期事件，浏览器关闭时cookie自动消失	
		var date = new Date();
		var ms = time*24*3600*1000;
		date.setTime(date.getTime()+ms);
		str += ";expires="+date.toGMTString()+";path=/"; 
		document.cookie = str;
	}
}
```
- **获取指定cookie：getCookie(c_name)**
- **调用方法：getCookie('userId')**
``` bash
function getCookie(name) {
	if(document.cookie.length > 0) {
		c_start = document.cookie.indexOf(name + "=")
		if(c_start != -1) {
			c_start = c_start + name.length + 1
			c_end = document.cookie.indexOf(";", c_start)
			if(c_end == -1) c_end = document.cookie.length
			return unescape(document.cookie.substring(c_start, c_end))
		}
	}
	return ""
}
```
- **删除指定cookie：deleteCookie(name)**
- **调用方法：deleteCookie('userId')**
``` bash
function deleteCookie(name) {
	var exp = new Date();
	exp.setTime(exp.getTime() - 1);
	var cval = getCookie(name);
	if(cval != null)
		document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString() + ";path=/";
}
```
- **检测是否有cookie存在：checkCookie(name)**
- **调用方法：checkCookie('userId')**
``` bash
function checkCookie(name) {
	var c = document.cookie.indexOf(name);
	if(c != -1) {
		return true;
	} else {
		return false;
	}
}
```
- **更新cookie：updateCookie(name,value,time)**
- **调用方法：updateCookie('userId','111',2)**
``` bash
function updateCookie(name, value, time) {
	var str = name + "=" + escape(value);
	var date = new Date();
	var ms = time * 24 * 3600 * 1000;
	date.setTime(date.getTime() + ms);
	str += ";expires=" + date.toGMTString() + ";path=/";
	document.cookie = str;
}
```
### 使用案例(记住密码-部分代码)

![html
](http://upload-images.jianshu.io/upload_images/3859151-1a2f613af724376b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](http://upload-images.jianshu.io/upload_images/3859151-75e4dcb3c43a38c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![javascript](http://upload-images.jianshu.io/upload_images/3859151-8e7c257d6242edb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)