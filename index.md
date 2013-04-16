---
layout: page
title: 码农农场
tagline:
---
{% include JB/setup %}

<div class="row-fluid">
    {% for post in site.posts limit 4 %}
    <div class="span12" display="none"></div>
    <div class="span12 row">
    <h2><a class="title" href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>
    <div class="post_at_index">
    <p>
    {{ post.content | split:'<!--more-->' | first }}
    </p>
    <a href="{{ BASE_PATH }}{{ post.url }}" rel="nofollow">Read more ...</a><div class="date">{{ post.date | date_to_long_string }}</div>
    </div>
    <br/>
    <div>
        {% unless post.tags == empty %}
        <ul class="tag_box inline">
        <li><i class="icon-tags"></i></li>
        {% assign tags_list = post.tags %}
        {% include JB/tags_list %}
        </ul>
        {% endunless %}
    </div>
    </div>
    <div style="clear: both;"></div>
    <hr/>
    {% endfor %}
</div>

<div class="pagination">
      <ul>{% if paginator.previous_page %}
        <li class="next"><a href='{% if paginator.previous_page > 1 %}/page{{ paginator.previous_page}}{% else %}/{% endif %}'>&larr; 上一页</a></li>{% endif %}
        <li><a href="{{ BASE_PATH }}{{ site.JB.archive_path }}">归档</a></li>{% if paginator.next_page %}
        <li class="prev"><a href='/page{{ paginator.next_page }}'>下一页 &rarr;</a></li>{% endif %}
      </ul>
</div>
