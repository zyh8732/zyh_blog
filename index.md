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

<p>全部文章：</p>

<ul class="post-list">
  {%- for post in site.posts %}
  <li>
    <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <time>{{ post.date | date: "%Y-%m-%d" }}</time>
    {%- if post.categories.size > 0 %}<span class="path">{{ post.categories | join: " / " }}</span>{% endif %}
  </li>
  {%- endfor %}
</ul>

<p style="margin-top:1.5rem"><a href="{{ '/blog/' | relative_url }}">→ 查看全部文章树</a></p>

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
