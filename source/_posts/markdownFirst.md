---
title: markdownFirst
date: 2017-09-02 15:40:42
tags: markdown
---

MarkDown 语法 解读 (一)
=====================
```
效果如上
最高阶的标题支持两种方式的语法
Setext
A First Level Header
====================
atx
A First Level Header
--------------------
```

* 支持1到6阶的标题显示

> # 标题一
> ## 标题二
> ### 标题三
> #### 标题四
> ###### 标题五
> ####### 标题六

# 标题一
## 标题二
### 标题三
#### 标题四
##### 标题五
###### 标题六

* 修辞和强调

> *斜体*
> **粗体**
> ***加粗斜体***
> ~~删除线~~

这将是一段*斜体*，但是你可能不会发现，
有可能还是**粗体**，你也有可能不会发现，
最后是***加粗斜体***，但你也有可能不会发现，
最后是~~删除线~~，这回可以了啊。

* 超链接
	
	Markdown 支持两种形式的链接语法： 行内式和参考式两种形式，行内式一般使用较多。

>	行内式

>	[]表明了要显示的文本，而()表明链接地址

>	这是一个[链接地址去百度的](http://www.baidu.com)

>	参考式 -------> 多类似标注的形式

I get 10 times more traffic from [Google] [1] than from
[Yahoo] [2] or [MSN] [3].

[1]: http://google.com/        "Google"
[2]: http://search.yahoo.com/  "Yahoo Search"
[3]: http://search.msn.com/    "MSN Search"


>	I get 10 times more traffic from [Google] [1] than from
>	[Yahoo] [2] or [MSN] [3].

[1]: http://google.com/        "Google"
[2]: http://search.yahoo.com/  "Yahoo Search"
[3]: http://search.msn.com/    "MSN Search"

* 自动链接
  
  markdown会将<>包裹的文本自动转化为超链接

>  <http://www.baidu.com>

