---
layout: page
permalink: /teaching/
title: teaching
description: List of courses I helped teaching
---

{% for item in site.teaching %}
  <div>
    <em>{{ item.period }}</em>:
    <strong>
        {{ item.course }}
    </strong>

  	{% if item.note %}
	    <p>
	    	{{ item.note }} <br>
            {% for aim in item.aims %}
	    	  - {{ aim }} <br>
            {% endfor %}
	    </p>
  	{% endif %}
  </div>
{% endfor %}
