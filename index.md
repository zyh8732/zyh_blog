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

{%- assign arts = site.pages | where_exp: "p", "p.name contains '.md'" | where_exp: "p", "p.path != 'index.md'" -%}
<ul class="post-list">
  {%- comment -%} ① 文章 {%- endcomment -%}
  {%- for p in arts %}
  {%- assign segs = p.path | split: "/" -%}
  {%- assign n    = segs.size | minus: 1 -%}
  {%- assign pdir = segs | slice: 0, n | join: "/" -%}
  <li>
    <a href="{{ p.url | relative_url }}">{{ p.name }}</a>
    {%- if pdir != "" %}<span class="path">{{ pdir | replace: "/", " / " }}</span>{% endif %}
  </li>
  {%- endfor %}

  {%- comment -%} ② 文本（.txt，走站内查看页） {%- endcomment -%}
  {%- for f in site.static_files -%}
    {%- assign fp       = f.path | remove_first: "/" -%}
    {%- assign segs     = fp | split: "/" -%}
    {%- assign n        = segs.size | minus: 1 -%}
    {%- assign fdir     = segs | slice: 0, n | join: "/" -%}
    {%- assign firstseg = segs | first -%}
    {%- assign ext      = f.extname | downcase -%}
    {%- if firstseg != "assets" and ext == ".txt" -%}
  <li>
    <a href="{{ '/read/' | relative_url }}?f={{ f.path | relative_url }}" target="_blank" rel="noopener">{{ f.name }}</a>
    {%- if fdir != "" %}<span class="path">{{ fdir | replace: "/", " / " }}</span>{% endif %}
  </li>
    {%- endif -%}
  {%- endfor -%}

  {%- comment -%} ③ 图片（点名字开灯箱） {%- endcomment -%}
  {%- for f in site.static_files -%}
    {%- assign fp       = f.path | remove_first: "/" -%}
    {%- assign segs     = fp | split: "/" -%}
    {%- assign n        = segs.size | minus: 1 -%}
    {%- assign fdir     = segs | slice: 0, n | join: "/" -%}
    {%- assign firstseg = segs | first -%}
    {%- assign ext      = f.extname | downcase -%}
    {%- if firstseg != "assets" -%}
      {%- if ext == ".png" or ext == ".jpg" or ext == ".jpeg" or ext == ".gif" or ext == ".webp" or ext == ".svg" or ext == ".avif" -%}
  <li>
    <button type="button" class="thumb-btn" data-full="{{ f.path | relative_url }}" data-name="{{ f.name }}">{{ f.name }}</button>
    {%- if fdir != "" %}<span class="path">{{ fdir | replace: "/", " / " }}</span>{% endif %}
  </li>
      {%- endif -%}
    {%- endif -%}
  {%- endfor -%}
</ul>

<div id="lightbox" class="lightbox" hidden>
  <button type="button" class="lightbox-close" aria-label="关闭">×</button>
  <img id="lightbox-img" alt="">
</div>

<script>
  // 明暗主题
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

  // 图片灯箱：点名字原地放大，只有关闭键能关
  (function () {
    var box = document.getElementById('lightbox');
    var img = document.getElementById('lightbox-img');
    var closeBtn = box ? box.querySelector('.lightbox-close') : null;
    if (!box || !img || !closeBtn) return;

    function close() {
      box.hidden = true;
      img.removeAttribute('src');
      img.setAttribute('alt', '');
    }

    document.querySelectorAll('.thumb-btn').forEach(function (btn) {
      btn.addEventListener('click', function () {
        img.setAttribute('src', btn.getAttribute('data-full') || '');
        img.setAttribute('alt', btn.getAttribute('data-name') || '');
        box.hidden = false;
      });
    });

    closeBtn.addEventListener('click', close);
  })();
</script>
