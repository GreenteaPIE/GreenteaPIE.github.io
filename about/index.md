---
layout: portfolio
title: About
description: 백엔드 개발자 GreenteaPIE 소개
---
{%- assign profile = site.data.profile -%}
<section class="prose">
  <h1 style="margin-top:40px">About</h1>
  <p>{{ profile.intro }}</p>
  <p>고등학교 졸업 후 사무직과 현장직을 거쳐 웹 개발 교육으로 개발을 시작했습니다. 교육 과정에서 팀·개인 프로젝트 3개를 만들었고,
  이후 공공기관 행정 프로그램을 유지보수하며 수백 개 테이블을 조인해 수정·통계 요청을 처리했습니다. 그 경험을 바탕으로 SQLD 를 취득했습니다.</p>
  <p>2024년 9월부터 KD운송그룹 전산개발팀에서 Nexacro 로 차량 정비·부품 판매 ERP 를 개발했고, 그룹 자체 ERP 를 MiPlatform 에서 Nexacro 로 옮기며 고도화하고 있습니다.
  그룹웨어, 사내 원격 지원 프로그램, 음주측정 시스템, 숙박·식사 예약 시스템도 만들었습니다.</p>
  <p>개인적으로는 대체공휴일·임시공휴일까지 정확히 반영한 <a href="https://korea-holidays-api.greenteapie2023.workers.dev">대한민국 공휴일 Open API</a> 를 운영하고 있습니다.
  공식 데이터와 매주 자동으로 대조하고, 같은 데이터를 npm·PyPI 패키지와 AI 비서용 MCP 서버로도 제공합니다.</p>

  <h2>걸어온 길</h2>
  {% include timeline.html %}

  <h2>기술</h2>
  <div class="skills">
    {% for group in profile.skills %}
    <div class="card"><h3>{{ group.group }}</h3><div class="chips">{% for s in group.items %}<span class="chip">{{ s }}</span>{% endfor %}</div></div>
    {% endfor %}
  </div>

  <h2>연락처</h2>
  <div class="actions">
    <a class="btn primary" href="mailto:{{ profile.email }}">{{ profile.email }}</a>
    <a class="btn" href="https://github.com/{{ profile.github }}">GitHub</a>
  </div>
</section>
