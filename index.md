---
title: Dr. Arne Leitert

projects:
  - p2cUnionJoin
  - tbVsStb
  - graphGen
---

# Biography

Since 2022, I am cloud database engineer at [RisingWave Labs](https://www.risingwave-labs.com/) in Singapore.
We are building a cloud native streaming database.
Before that, I was Assistant Professor at the [Department of Computer Science](https://www.cwu.edu/computer-science/), [Central Washington University](https://www.cwu.edu/) in Ellensburg, Washington, USA.

I studied computer science in my home town at the [University of Rostock](https://www.informatik.uni-rostock.de/en/), Germany.
The advisor for my Master's Thesis was [Dr. Andreas Bradstädt](https://users.informatik.uni-rostock.de/~ab/).
Afterwards, I went to [Kent State University](https://www.kent.edu/), Ohio, USA.
There, I received my Ph.D. in 2017 under supervision of [Dr. Feodor F. Dragan](http://www.cs.kent.edu/~dragan/).

Erdős number: 3  
(P. Erdős — D. Kratsch — A. Brandstädt / F.F. Dragan — A. Leitert)


# Selected Projects


<ul id="selProjList">
{% for projName in page.projects %}
    {% assign proj = site.projects | where_exp:"item", "item.slug == projName" | first %}
    <li>
        <strong><a href="{{ proj.url }}">{{ proj.title }}</a></strong>
        {% if proj.partners != nil %}
            &emsp;&mdash;&emsp;with
            {% for pName in proj.partners %}
                {{ pName }}{% unless forloop.last %},{% endunless %}
            {% endfor %}
        {% endif %}
        <br>
        {{ proj.description }}
        {% if proj.links != empty %}
            <br>
            {% if proj.links.git != nil %}[<a href="{{ proj.links.git }}">git</a>]{% endif %}
        {% endif %}
    </li>
{% endfor %}
</ul>
