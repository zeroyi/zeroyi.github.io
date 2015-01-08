---
layout: post
title: "Python 编码小议"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## 背景
在Python 2.x系列，string 有两种类型：**str** and **unicode**， 它们都继承自 **basestring**。
在大多数时候，我们不需要考虑它们的区别，因为这两种实现的方法都基本是相同的。例如，lower(), replace(), strip(), startswith()等。
但是，如果你要混合使用这两种不同的字符串类型的时候，可就得小心为妙了。我们来看几个例子：
## 示例

###  Unicode 在 ASCII 范围
你可以看到下面的str和unicode可以自由混用，效果是一样的

{%highlight python%}
a = 'abc'
b = 'def'
c = u'hij'

print a + b #abcdef
print b + c #defhij
{% endhighlight%}


### 在Python 2.5中混合ascii 和 unicode
你可以看到a+b报错了

{%highlight python%}
a = 'abc'
b = u'\u4e2d\u6587'

print a + b #UnicodeEncodeError: 'ascii' codec can't encode characters in position 3-4: ordinal not in range(128)
{% endhighlight%}


### 在Python 2.7中混合ascii 和 unicode
上面同意的例子，在这里不报错

{%highlight python%}
a = 'abc'
b = u'\u4e2d\u6587'

print a + b #abc中文
{% endhighlight%}


### 千万不要轻易将unicode转型为str
这个是Python新手常常百思不得其解的错误

{%highlight python%}
a = u'\u4e2d\u6587'
b = str(a) #UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
{% endhighlight%}


### 当str的编码同系统默认编码不一样的时候不要将str转换为unicode
{%highlight python%}
a = '中文' # if you have set the file encoding in file header by #coding=utf-8
b = unicode(a) #UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)
c = unicode(a, 'utf-8') #this will be right
{% endhighlight%}


## 总结
事实上，在Python2.x系列，str跟Java中的byte数组差不多。而且，它的默认编码也不是现在大多数编程语言中广为使用的UTF-8，所以，
除非你只与ascii打交道，你可以放心使用str，否则最好用unicode。

在Python3里面，str 已经被弃用了。所有字符串都已经是unicode，这跟Java如出一辙。如果你能够将你的项目升级到Python3，那字符编码对你来说就不是个事。
但由于Python3中众所周知的向后兼容性问题，这个大多数项目来说都是个美丽的梦想。所以下面是我的一些个人经验，供大家参考：

* 尽早判断出字符串的编码
* 如果你不能确定你处理的字符串不是纯ascii，那就尽早把它解码为unicode
* 不要显示的强转字符串类型，例如str(a) or unicode(a)

