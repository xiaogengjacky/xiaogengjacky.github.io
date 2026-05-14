---
layout: single
title: "Publications"
permalink: /publications/
author_profile: true
---

{% if site.author.googlescholar %}
<div class="wordwrap">You can also find my articles on <a href="{{site.author.googlescholar}}">my Google Scholar profile</a>.</div>
<div class="wordwrap">Most of my working papers are posted on <a href="{{site.author.ssrn}}">my SSRN page</a>.</div>

{% endif %}

{% if site.publication_category %}
  {% for publication_group in site.publication_category %}
    {% assign category_key = publication_group[0] %}
    {% assign category_meta = publication_group[1] %}
    {% assign category_publications = site.publications | where: "category", category_key | sort: "date" | reverse %}
    {% if category_publications.size > 0 %}
### {{ category_meta.title }}

<ol>
  {% for pub in category_publications %}
    {% assign excerpt_text = pub.excerpt | default: "" | strip %}
    {% assign authors_sentence = excerpt_text %}
    {% assign remainder_text = "" %}
    {% if excerpt_text contains ". " %}
      {% comment %}
        Take the LAST ". "-separated segment as the remainder (vol/page/etc.)
        and everything before it as the author list, so middle initials like
        "William D. Schulze" don't break the split.
      {% endcomment %}
      {% assign parts = excerpt_text | split: ". " %}
      {% assign parts_size = parts | size %}
      {% assign remainder_text = parts | last | strip %}
      {% assign authors_count = parts_size | minus: 1 %}
      {% assign author_parts = parts | slice: 0, authors_count %}
      {% assign authors_sentence = author_parts | join: ". " %}
    {% elsif excerpt_text == "" %}
      {% assign authors_sentence = "" %}
    {% endif %}
    {% assign remainder_formatted = "" %}
    {% if remainder_text != "" %}
      {% assign remainder_first = remainder_text | slice: 0, 1 %}
      {% assign digits = "0123456789" %}
      {% if digits contains remainder_first %}
        {% assign remainder_formatted = ", " | append: remainder_text %}
      {% else %}
        {% assign remainder_formatted = ". " | append: remainder_text %}
      {% endif %}
    {% endif %}
    {% assign authors_markup = "" %}
    {% if authors_sentence != "" %}
      {% assign authors_markup = authors_sentence | append: "." %}
    {% endif %}
  <li><strong>{% if pub.paperurl %}<a href="{{ pub.paperurl }}" target="_blank" rel="noopener">{{ pub.title }}</a>{% elsif pub.link %}<a href="{{ pub.link }}" target="_blank" rel="noopener">{{ pub.title }}</a>{% else %}{{ pub.title }}{% endif %}</strong>. {{ pub.date | default: "1900-01-01" | date: "%Y" }}.{% if authors_markup != "" %} {{ authors_markup }}{% endif %}{% if pub.venue %} <em>{{ pub.venue }}</em>{% endif %}{{ remainder_formatted }}</li>
  {% endfor %}
</ol>

    {% endif %}
  {% endfor %}
{% endif %}
