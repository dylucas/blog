<%*
// https://www.todaybing.com/api/today/
const fruits = [];
fruits.push('cn', 'de', 'fr', 'in', 'jp', 'gb');
var randomKey = fruits[parseInt(Math.random()*(6-0)+0)];

// https://cn.bing.com/HPImageArchive.aspx?format=js&n=10
// https://api.oneneko.com/v1/bing_today
// https://www.willkwok.cn/article/35.html
var httpRequest = new XMLHttpRequest();
httpRequest.open('GET', 'https://api.oneneko.com/v1/bing_today', false);
httpRequest.send();
var imgUrl = httpRequest.responseURL;
-%>
---
title: "<% tp.file.folder() %>"
description: 
date: <% tp.date.now() %>
image: "<% imgUrl %>"
draft: true
tags: []
categories: []
---