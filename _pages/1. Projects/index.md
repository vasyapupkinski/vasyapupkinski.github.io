---
layout: page
title: "Projects"
category: "1. Projects"
order: 1
---

<h2 style='border-bottom: 2px solid #F86158; padding-bottom: 10px;'>Team Projects</h2>

<div id='category-list'>
    <ul class='paginated-list'>
    {% assign teams = site.pages | where: 'category', '1. Projects / Team Projects' | sort: 'order' %}
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

<h2 style='border-bottom: 2px solid #2ACB45; padding-bottom: 10px;'>Personal Projects</h2>
<p style='color: #888; margin-bottom: 20px;'>그동안 학습한 백엔드 아키텍처, LLM 시스템, 하네스 엔지니어링 기술을 집약하여 구축 예정인 프로젝트들입니다.</p>

<div id='category-list'>
    <ul class='paginated-list'>
    {% assign personals = site.pages | where: 'category', '1. Projects / Personal Projects' | sort: 'order' %}
    {% for post in personals %}
        {% unless post.url contains 'index.html' %}
        <li class='paginated-item'>
            <div id='article_content'>
                <div class='box_contents'>
                    <a href='{{ post.url | prepend: site.baseurl }}'><h1 class='title_post'>{{ post.title }}</h1></a>
                    <div class='info-post'>
                        <span class='category' style='color: #2ACB45;'>Personal Project</span>
                        <span class='category' style='color: #888; margin-left: 10px;'>Status: {{ post.status }}</span>
                    </div>
                </div>
            </div>
        </li>
        {% endunless %}
    {% endfor %}
    </ul>
</div>
