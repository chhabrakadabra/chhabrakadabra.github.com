---
layout: page
title: Posts
header: Posts
group: navigation
---
{% include JB/setup %}
{% for post in site.posts %}
<div class="well">
  <div class="row-fluid">
    <div class="span10">
      <a href="{{ post.url }}"><h2>{{ post.title }}</h2></a>
      {% unless post.tags == empty %}
      <ul class="inline">
        {% assign tags_list = post.tags %}
        {% include custom/inline_tags %}
      </ul>
      {% endunless %}  
    </div>
    <div class="span2">
      <!-- social sharing -->
      {% assign social_layout = "vertical" %}
      {% include custom/sharing_icons %}
    </div>
  </div>
  <strong>{{ post.date | date_to_string }}</strong>
  <hr>
  {% unless post.tagline == null %}
  {{ post.tagline }}
  (<a href="{{ post.url }}">more...</a>)
  {% endunless %}
</div>
{% endfor %}