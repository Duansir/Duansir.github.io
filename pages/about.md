---
layout: page
title: About
description: 做一个快乐的嵌入式码农
keywords: Duansir, duansir
comments: true
menu: 关于
permalink: /about/
---

我是段先生，一个不断追求理想的嵌入式码农。

坚信开源改变世界，努力改变人生。

## 联系

{% for website in site.data.social %}
* {{ website.sitename }}：[@{{ website.name }}]({{ website.url }})
{% endfor %}

## Skill Keywords

{% for category in site.data.skills %}
### {{ category.name }}
<div class="btn-inline">
{% for keyword in category.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
