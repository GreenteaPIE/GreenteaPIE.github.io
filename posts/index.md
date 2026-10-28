---
layout: portfolio
title: Posts
description: GreenteaPIE 의 개발 기록 — 프로젝트 개발기, Java·SQL 공부, 자격증
---
{%- comment -%} 글의 태그로 분류한다. data-tags 는 소문자로 비교한다. {%- endcomment -%}
<section>
  <h1 style="margin-top:40px">글 <span class="muted" style="font-size:0.5em">{{ site.posts.size }}편</span></h1>
  <div class="filters" role="group" aria-label="분류">
    <button type="button" data-filter="" aria-pressed="true">전체</button>
    <button type="button" data-filter="project">프로젝트</button>
    <button type="button" data-filter="java">Java·Spring</button>
    <button type="button" data-filter="sql">SQL</button>
    <button type="button" data-filter="study">자격증</button>
    <button type="button" data-filter="baekjoon">코딩 테스트</button>
  </div>
  {% assign years = site.posts | group_by_exp: 'post', "post.date | date: '%Y'" %}
  {% for year in years %}
  <h3 class="year" data-year>{{ year.name }}</h3>
  <ul class="post-list">
    {% for post in year.items %}
    {%- capture tags -%}{{ post.tags | join: ' ' | downcase }} {{ post.url | downcase }}{%- endcapture -%}
    <li data-tags="{{ tags | escape }}"><a href="{{ post.url | relative_url }}">{{ post.title }}<span class="excerpt">{{ post.excerpt | strip_html | strip_newlines | truncate: 90 }}</span></a><time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: '%m.%d' }}</time></li>
    {% endfor %}
  </ul>
  {% endfor %}
</section>
<script>
  const buttons = document.querySelectorAll('.filters button')
  const match = { project: ['project'], java: ['java', 'spring'], sql: ['sql'], study: ['sqld', 'ieip', 'study'], baekjoon: ['baekjoon', 'beakjoon'] }
  buttons.forEach((b) => b.addEventListener('click', () => {
    buttons.forEach((x) => x.setAttribute('aria-pressed', String(x === b)))
    const keys = match[b.dataset.filter] || []
    document.querySelectorAll('.post-list li').forEach((li) => {
      li.hidden = keys.length > 0 && !keys.some((k) => li.dataset.tags.includes(k))
    })
    document.querySelectorAll('[data-year]').forEach((h) => {
      const list = h.nextElementSibling
      h.hidden = list.hidden = ![...list.children].some((li) => !li.hidden)
    })
  }))
</script>
