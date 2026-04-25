---
layout: page
title: Bookmarks
permalink: /bookmarks/
---

{% for bookmark in site.data.bookmarks %}
### [{{ bookmark.title }}]({{ bookmark.url }}){:target="_blank" rel="noopener"}

<span class="bookmark-domain">{{ bookmark.domain }}</span>

{{ bookmark.description }}

{% endfor %}
