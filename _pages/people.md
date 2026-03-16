---
layout: page
title: People
permalink: /people/
description: Members of the GoTerria research group.
nav: true
nav_order: 2
---

## Current Members

<div class="members-grid">
{%- comment -%}
  NOTE: `alumni` must be a boolean (false/true) in member front matter, not a string.
  If members disappear from the list, check that no file uses alumni: "false" (string).
{%- endcomment -%}
{% assign current_members = site.members | where: "alumni", false | sort: "order" %}
{% for member in current_members %}
<div class="member-card">
  {% if member.photo %}
    <img src="{{ '/assets/img/members/' | append: member.photo | relative_url }}" alt="{{ member.name }}" class="member-photo" />
  {% else %}
    <div class="member-photo member-photo-placeholder" aria-label="{{ member.name }}"></div>
  {% endif %}
  <h3>{{ member.name }}</h3>
  <p class="member-role">{{ member.role }}</p>
  <p>{{ member.bio }}</p>
  <div class="member-links">
    {% if member.links.email %}
      <a href="mailto:{{ member.links.email }}" title="Email"><i class="fas fa-envelope"></i></a>
    {% endif %}
    {% if member.links.github %}
      <a href="https://github.com/{{ member.links.github }}" title="GitHub">GitHub</a>
    {% endif %}
    {% if member.links.scholar %}
      <a href="{{ member.links.scholar }}" title="Google Scholar">Scholar</a>
    {% endif %}
    {% if member.links.website %}
      <a href="{{ member.links.website }}" title="Profile" target="_blank" rel="noopener">Profile</a>
    {% endif %}
  </div>
</div>
{% endfor %}
</div>

---

## Alumni

<div class="members-grid">
{%- comment -%}
  NOTE: `alumni` must be a boolean (false/true) in member front matter, not a string.
  If members disappear from the list, check that no file uses alumni: "true" (string).
{%- endcomment -%}
{% assign alumni_members = site.members | where: "alumni", true | sort: "name" %}
{% for member in alumni_members %}
<div class="member-card member-alumni">
  <h3>{{ member.name }}</h3>
  <p class="member-role">{{ member.role }}</p>
</div>
{% endfor %}
</div>
