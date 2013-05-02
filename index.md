---
layout: page
title: Welcome buddy!
---
{% include JB/setup %}

###Hey kids, first I wanna tell you a great story:
    
    Once upon a time
    Your dad met a girl
    He bought her a cup of ice cream on sale
    Now she's your mom

###Okay, so what's this website for?
    
    I like coding and technology and I need to prove it.
    See if I can do something like designing?
    Taking notes to remember valuable information.
    Learning more and more to making your mom's like easier.
    
###Here's the latest post list:
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
