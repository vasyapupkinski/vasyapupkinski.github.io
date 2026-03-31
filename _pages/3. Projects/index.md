---
layout: page
title: "Projects"
category: "3. Projects"
order: 3
---

<h2 style='border-bottom: 2px solid #F86158; padding-bottom: 10px;'>👥 Team Projects</h2>

<div id='category-list'>
    <ul class='paginated-list'>
    {% assign teams = site.pages | where: 'category', '3. Projects / Team Projects' | sort: 'order' %}
    {% for post in teams %}
        {% unless post.url contains 'index.html' %}
        <li class='paginated-item'>
            <div id='article_content'>
                <div class='box_contents'>
                    <a href='{{ post.url | prepend: site.baseurl }}'><h1 class='title_post'>{{ post.title }}</h1></a>
                    <div class='info-post'>
                        <span class='category' style='color: #F86158;'>Team Project</span>
                    </div>
                </div>
            </div>
        </li>
        {% endunless %}
    {% endfor %}
    </ul>
</div>

<br><br>

<h2 style='border-bottom: 2px solid #2ACB45; padding-bottom: 10px;'>👤 Personal Projects</h2>
<p style='color: #888;'>시작 단계의 개인 프로젝트들이 이곳에 표시됩니다.</p>
