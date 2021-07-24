---
title: Publications

# --- Properties of publication Entries ---

#      title    The title of the publication.
#               Required.

#     autors    A list of all autors.
#               Required.

#        key    A key to use for sorting the papers.
#               Has the patters YYYY-MM-AAA-AAA-... where MM is the month,
#               YYYY the year, and AAA the first the letters of the autors
#               family name.

#       type    The type of the publication.
#               J - journal paper
#               C - conference paper
#               R - under review or technical report
#               T - Thesis
#               Required.

# conference    The conference where the paper is published at.
#               Required if type = C.

# journal       The journal where the paper is published at.
#               Required if type = J.

order:
  - R
  - J
  - C
  - T

headers:
  RR: Under Review
  RT: Technical Reports
  J: Journal Papers
  C: Conference Papers
  T: Thesis
---

<script type="text/javascript">
    function toggleAbstract(paperID)
    {
        var abstractDiv = document.getElementById(paperID);

        if (abstractDiv.style.display == 'none' || abstractDiv.style.display == '')
        {
            abstractDiv.style.display = 'block';
        }
        else
        {
            abstractDiv.style.display = 'none';
        }
    }
</script>


{% for pType in page.order %}

    {% assign pGrp = site.publications | where_exp: "item", "item.type == pType" | sort: 'key' | reverse %}

    {% if pGrp == empty %}
        {% continue %}
    {% endif %}

    {% if pType != "R" %}
# {{ page.headers[pType] }}
    {% else %}
        {% assign rList = pGrp | where_exp: "item", "item.submitted != nil" %}
        {% assign tList = pGrp | where_exp: "item", "item.submitted == nil" %}

# {% if rList != empty %}{{ page.headers.RR }}{% endif %}{% if rList != empty and tList != empty %} and {% endif %}{% if tList != empty %}{{ page.headers.RT }}{% endif %}
    {% endif %}
<ul>
    {% for pub in pGrp %}
<li>
<strong>{{ pub.title }}</strong>
<br>
        {% if pub.type != 'T' %}
            {% for auth in pub.authors %}
                <em{% if auth contains 'Leitert' %} style="font-weight: bold;"{% endif %}>{{ auth }}</em>{% unless forloop.last %}, {% endunless %}
            {% endfor %}
            <br>
       {% endif %}


        {% assign infoLine = "" | split: "/" %}

        {% if pub.type == 'C' or pub.type == 'J' %}

            {% if pub.conference != nil %}{% assign infoLine = infoLine | push: pub.conference %}{% endif %}

            {% if pub.journal != nil %}
                {% if pub.volume != nil %}
                    {% assign jStr = pub.journal | append: " " | append: pub.volume %}
                {% else %}
                    {% assign jStr = "Accepted for " | append: pub.journal %}
                {% endif %}
                {% assign infoLine = infoLine | push: jStr %}
            {% endif %}

            {% if pub.pages != nil %}{% assign infoLine = infoLine | push: pub.pages %}{% endif %}
            {% if pub.year != nil %}{% assign infoLine = infoLine | push: pub.year %}{% endif %}

        {% elsif pub.type == 'T' %}

            {% assign infoLine = infoLine | push: pub.tType %}

            {% assign uniName = "<em>" | append: pub.uni | append: "</em>" %}
            {% assign infoLine = infoLine | push: uniName %}

            {% assign finishDate = pub.month | append: " " | append: pub.year %}
            {% assign infoLine = infoLine | push: finishDate %}

            {% if pub.comment != nil %}{% assign infoLine = infoLine | push: pub.comment %}{% endif %}

        {% endif %}

        {% if infoLine != empty %}
        {% for info in infoLine %}
            {{ info }}{% if forloop.last %}.{% else %},{% endif %}
        {% endfor %}
        <br>
        {% endif %}


        [<a href="javascript:toggleAbstract('{{ pType }}_{{ pub.key }}')">abstract</a>]
        {% if pub.arXiv != nil %}[<a href="https://arxiv.org/abs/{{ pub.arXiv }}">arXiv</a>]{% endif %}
        {% if pub.doi != nil %}[<a href="https://doi.org/{{ pub.doi }}">doi</a>]{% endif %}
        {% if pub.pdf != nil %}[<a href="/{{ pub.pdf }}">pdf</a>]{% endif %}
        {% if pub.slides != nil %}[<a href="/{{ pub.slides }}">slides</a>]{% endif %}
        {% if pub.git != nil %}[<a href="{{ pub.git }}">git</a>]{% endif %}

<blockquote id="{{ pType }}_{{ pub.key }}" class="abstract">
    {{ pub.content }}
</blockquote>
</li>
    {% endfor %}
</ul>
{% endfor %}
