---
title: Publications

# --- Properties of publication Entries ---

#      title    The title of the publication.
#               Required.

#     autors    A list of all autors.
#               Required.

#        key    A key to use for sorting the papers.
#               Has the patters MM-YYYY-AAA-AAA-... where MM is the month,
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

{% for pType in page.order %}

    {% assign pGrp = site.publications | where_exp: "item", "item.type == pType" | sort: key, title | reverse %}

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
{{ pub.content }}
</li>
    {% endfor %}
</ul>
{% endfor %}
