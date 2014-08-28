---
layout: post
title: "Don't overide 'backgroundView' as UITableViewCell and it's subclass"
date: 2014-08-28 17:05:46 +0800
comments: true
categories: 
---

`backgroundView` default value is nil in UITableViewCell. So I make a misstake When I wanna to use `backgroundView` as my own customized area view which smaller than cell size. But it dosen't work, at the end I found backgroundView is a standard property of UITableViewCell. I overide it unconsciously.
