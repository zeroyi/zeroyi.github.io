---
layout: post
title: "简单介绍 Python requests lib"
description: ""
category: 
tags: []
---
{% include JB/setup %}
### 引子
最近在用Python写一个小工具的时候，又一次用到了Requests这个神奇的http工具包。上次用到这个工具是很早以前了，当时因为对Python了解不多，对它也没太多印象，今日重温，不得不感叹这一http利器之事半功倍之效。

说起Python的http工具，原生的不是没有，功能也不是不够强大，而是太多太杂不好用，比如urllib,  urllib2。说好听叫不够人性化，说不好听的，这不是给人用的。这正是Requests这一神器推出的口号：HTTP for Humans.  

关于Requests的使用方法，它的官方主页有简明的使用方法和例子，我就举几个简单的例子。
### Get
获取github主页内容
{% highlight python %}
rsp = requests.get('www.github.com')
print rsp.text
{% endhighlight %}

### Post
模拟浏览器登陆某网站：
{% highlight python %}
params = {
	'username': 'hello',
	'password': 'world'
}
rsp = requests.post('www.example.com', params=params)
print rsp.text
{% endhighlight %}

### Proxy
你要用代理，这个就是一两行的事：
{% highlight python %}
proxy = {
	'http':'http://localhost:8080'
}
rsp = requests.get('www.github.com', proxies=proxy)
{% endhighlight %}

### Cookie
在也不用为如何保持cookie发愁了, 下面的代码用来登陆，然后就可以在登陆状态下进行各类操作，cookie已经在session中帮你自动管理了:
{% highlight python %}
s = requests.Session()
params = {
	'username': 'hello',
	'password': 'world'
}
s.post('www.example.com/login', params=params)
rsp = s.get('www.example.com/page')
print rsp.text
{% endhighlight %}

### 结尾
Requests的好处只有你被此类问题折磨过才能体会到，至于其他高级用法，如果有兴趣请其官方站点查看：[Requests](http://docs.python-requests.org)



