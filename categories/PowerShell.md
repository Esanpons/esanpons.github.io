---
layout: page
title: PowerShell
permalink: /blog/categories/PowerShell/
---

<h5> Categoria : {{ page.title }} </h5>

<div class="card card-categories-list">
    {% assign articles = site.articles | where: "category", "PowerShell" %}
    {% assign sorted_articles = articles %}
    {% assign all_posts = site.categories.PowerShell | concat: sorted_articles | sort: 'date' | reverse%}
    {% for post in all_posts %}
        <li class="category-posts">
            <span>
                {{ post.date | date: "%-d %B del %Y" | capitalize |
                    replace: "january", "Enero" | replace: "february", "Febrero" | replace: "march", "Marzo" |
                    replace: "april", "abril" | replace: "may", "Mayo" | replace: "june", "Junio" | replace: "july",
                    "Julio" | replace: "august", "Agosto" | replace: "september", "Septiembre" | replace: "october", "Cctubre" |
                    replace: "november", "Noviembre" | replace: "december", "Diciembre" }}
            </span> &nbsp; · &nbsp;
            <span class="post-type">{{post.custom_type}}</span> &nbsp; · &nbsp;
            <a href="{{ post.url }}">{{ post.title }}</a>
            
        </li>
    {% endfor %}
</div>
