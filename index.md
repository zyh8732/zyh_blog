---
layout: default
title: 首页
---
<div class="home-head">
  <h1>{{ site.title }}</h1>
  <div class="theme-switch" role="group" aria-label="明暗主题">
    <button type="button" data-set-theme="auto">跟随系统</button>
    <button type="button" data-set-theme="light">浅色</button>
    <button type="button" data-set-theme="dark">深色</button>
  </div>
</div>

<p>{{ site.description }}</p>

<p>全部：</p>

{%- assign arts = site.pages | where_exp: "p", "p.name contains '.md' and p.path != 'index.md'" | sort: "path" -%}
<ul class="post-list">
  {%- for p in arts %}
  {%- assign segs = p.path | split: "/" -%}
  {%- assign n    = segs.size | minus: 1 -%}
  {%- assign pdir = segs | slice: 0, n | join: "/" -%}
  <li>
    <a href="{{ p.url | relative_url }}">{{ p.name | split: "." | first }}</a>
    {%- if pdir != "" %}<span class="path">{{ pdir | replace: "/", " / " }}</span>{% endif %}
  </li>
  {%- endfor %}
</ul>

<script>
  (function () {
    var KEY = 'zyh-theme';
    var root = document.documentElement;
    var btns = document.querySelectorAll('[data-set-theme]');

    function paint() {
      var current = root.getAttribute('data-theme') || 'auto';
      Array.prototype.forEach.call(btns, function (btn) {
        btn.classList.toggle('is-active', btn.getAttribute('data-set-theme') === current);
      });
    }

    Array.prototype.forEach.call(btns, function (btn) {
      btn.addEventListener('click', function () {
        var next = btn.getAttribute('data-set-theme');
        root.setAttribute('data-theme', next);
        try { localStorage.setItem(KEY, next); } catch (e) {}
        paint();
      });
    });

    paint();
  })();
</script>
