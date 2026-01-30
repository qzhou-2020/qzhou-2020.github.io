---
layout: archive
title: "Research"
permalink: /publications/
author_profile: true
---

{% assign sorted_pubs = site.data.papers | sort: 'date' | reverse %}
{% assign published = sorted_pubs | where: "status", "Published" %}
{% assign working_papers = sorted_pubs | where: "status", "Working" %}
{% assign under_review = sorted_pubs | where: "status", "Under Review" %}
{% assign author_name = site.author.name %}
{% capture bold_name %}<strong>{{ author_name }}</strong>{% endcapture %}


<h2>Publications</h2>

{% assign counter = 0 %}
{% for post in published %}
  {% assign counter = counter | plus: 1 %}
  <p style="margin-top: 1em; margin-bottom: 0em;">
    {{ counter }}. {{ post.citation | replace: author_name, bold_name}}
    {% if post.paperurl %} <a href="{{ post.paperurl }}">[DOI]</a>{% endif %}
  </p>
{% endfor %}


<h2>Selected Working Papers</h2>

{% assign counter = 0 %}
{% for post in under_review %}
  {% assign counter = counter | plus: 1 %}
  <p style="margin-top: 1em; margin-bottom: 0em;">
    {{ counter }}. {{ post.citation | replace: author_name, bold_name}}
    {% if post.paperurl %} <a href="{{ post.paperurl }}">[DOI]</a>{% endif %}
  </p>
{% endfor %}

{% for post in working_papers %}
  {% assign counter = counter | plus: 1 %}
  <p style="margin-top: 1em; margin-bottom: 0em;">{{ counter }}. {{ post.title }}{% if post.paperurl %} <a href="{{ post.paperurl }}">[DOI]</a>{% endif %}</p>
  {% if post.excerpt %}
  <ul style="margin-top: 0em; margin-bottom: 0em;">
  {% for item in post.excerpt %}
  <li style="margin-bottom: 0.1em;">{{ item }}</li>
  {% endfor %}
  </ul>
{% endif %}
{% endfor %}