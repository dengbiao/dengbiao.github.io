---
layout: home
title: 猫头的博客
motto: 有一种东西叫做梦想
---

<section id="archive" class="archive_content">

{% for post in site.posts %}
    {% unless post.next %}
        <h3>{{ post.date | date: '%Y年%m月' }}</h3>
        <ul>
    {% else %}
        {% capture month %}{{ post.date | date: '%m' }}{% endcapture %}
        {% capture nmonth %}{{ post.next.date | date: '%m' }}{% endcapture %}
        {% if month != nmonth %}
            </ul>
            <div class="clear" ></div>
            <h3>{{ post.date | date: '%Y年%m月' }}</h3>
            <ul>
        {% endif %}
    {% endunless %}
    <li>
        {% case post.title %}
        {% when nil %}
            {% capture posttitle %} 无题 {% endcapture %}
        {% else %}
            {% capture posttitle %} {{ post.title }} {% endcapture %}
        {% endcase %}
        <a class="post_info" href="{{ post.url }}">
            <span class="post_title">
                {% if post.image %}
                <img src="{{ post.image }}"  />
                {% else %}
                <span class="text">{{ posttitle }}</span>
                {% endif %}
            </span>
            <span class="post_date">
                {{ post.date | date: "%Y-%m-%d" }}   
            </span>
        </a>


    </li>
{% endfor %}
</ul>
<div class="clear" ></div>

</section>

<div class="clear" ></div>
