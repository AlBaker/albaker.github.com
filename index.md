---
layout: page
title: Linked Java
tagline: Evaluating Data Integration techniques
---
{% include JB/setup %}

Linked Java is a blog on various subjects of interest, centered around exploiting data using different languages, frameworks, and techniques.  The [old blog](http://linkedjava.blogspot.com/) is still accessible for posts earlier than 2014.  Select posts, such as the tutorials will migrate here.


## Posts


<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>




