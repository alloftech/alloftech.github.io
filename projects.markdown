---
layout: page
title: Projects
permalink: /projects/
---
{% for repo in site.github.public_repositories %}

{% if repo.fork == false and repo.topics.size > 0 %}

{% include project-display.html repo=repo %}

{% endif %}

{% endfor %}