---
layout: portfolio
title: Projects
description: GreenteaPIE 가 만든 프로젝트 — Korea Holidays API, Find My Dog, DiamondBlack, Pet Talk
---
<section>
  <h1 style="margin-top:40px">프로젝트</h1>
  <p class="muted">첫 팀 프로젝트부터 직접 운영하는 서비스까지. 카드의 첫 번째 버튼에서 자세한 개발기를 볼 수 있어요.</p>
  {% for project in site.data.projects %}{% if project.featured %}{% include project-card.html project=project large=true %}{% endif %}{% endfor %}
  <div class="grid" style="margin-top:16px">
    {% for project in site.data.projects %}{% unless project.featured %}{% include project-card.html project=project %}{% endunless %}{% endfor %}
  </div>
</section>
