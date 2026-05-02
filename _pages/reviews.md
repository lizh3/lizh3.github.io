---
title: "Reviews"
permalink: /reviews/
layout: single
classes: wide
author_profile: true
---

A curated collection with personal ratings and notes. Click column headers to sort.

<!-- ========== 表格目录 ========== -->
<nav class="rv-toc" aria-labelledby="rv-toc-heading">
  <h3 id="rv-toc-heading">目录</h3>
  <ol>
    {%- for category in site.data.reviews -%}
      {%- assign cat_key = category[0] -%}
      {%- assign cat_items = category[1] -%}
    <li>
      <a href="#{{ cat_key | slugify }}">{{ cat_key }}</a>
      <span class="rv-toc-count">{{ cat_items.size }}</span>
    </li>
    {%- endfor -%}
  </ol>
</nav>

<style>
/* ── 目录 ── */
.rv-toc {
  background: #fafafa;
  border: 1px solid #eee;
  border-radius: 4px;
  padding: 1em 1.5em;
  margin-bottom: 2em;
}
.rv-toc h3 {
  margin: 0 0 0.5em;
  font-size: 1.1em;
  color: #494e52;
}
.rv-toc ol {
  margin: 0;
  padding-left: 1.5em;
  list-style: decimal;
}
.rv-toc li {
  margin-bottom: 0.3em;
}
.rv-toc a {
  color: #494e52;
  text-decoration: none;
  font-weight: 500;
}
.rv-toc a:hover {
  text-decoration: underline;
}
.rv-toc-count {
  display: inline-block;
  background: #e8e8e8;
  color: #7a8288;
  font-size: 0.75em;
  padding: 0.1em 0.5em;
  border-radius: 10px;
  margin-left: 0.5em;
  vertical-align: middle;
}

/* ── 分类标题 ── */
.rv-section-heading {
  margin-top: 2.5em;
  margin-bottom: 0.8em;
  padding-bottom: 0.4em;
  border-bottom: 2px solid #bdc1c4;
  scroll-margin-top: 2em;        /* 点击目录锚点时留出顶部空间 */
}

/* ── 表格容器 ── */
.rv-wrap {
  width: 100%;
  overflow-x: auto;
  margin-bottom: 2em;
}
.rv-table {
  width: 100%;
  min-width: 960px;
  border-collapse: collapse;
  font: inherit;
}

/* ── 表头 & 排序指示 ── */
.rv-table th {
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
  position: relative;
  text-align: left;
  padding: 0.5em 0.75em;
  border-bottom: 2px solid #bdc1c4;
  color: #494e52;
}
.rv-table th::after {
  content: "\2195";
  position: absolute;
  right: 0.5em;
  top: 50%;
  transform: translateY(-50%);
  opacity: 0.3;
  color: #7a8288;
  font-size: 0.8em;
}
.rv-table th.sort-up::after   { content: "\2191"; opacity: 1; }
.rv-table th.sort-down::after { content: "\2193"; opacity: 1; }

/* ── 单元格 ── */
.rv-table td {
  padding: 0.5em 0.75em;
  vertical-align: top;
  border-bottom: 1px solid #eee;
  overflow-wrap: break-word;
}
.rv-table tr:hover { background: #f8f8f8; }
.rv-score-cell { text-align: center; }
.rv-date-cell  { white-space: nowrap; }
.rv-subtitle {
  display: block;
  font-size: 0.85em;
  color: #7a8288;
  margin-top: 0.2em;
}

/* ── 评论折叠 ── */
.rv-comment-cell { position: relative; }
.rv-comment-wrapper {
  max-height: 4.5em;
  overflow: hidden;
  transition: max-height 0.25s ease;
  position: relative;
  line-height: 1.5em;
}
.rv-comment-wrapper.rv-expanded { max-height: 2000px; }

.rv-comment-wrapper::after {
  content: "";
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 2.5em;
  background: linear-gradient(transparent, #fff);
  pointer-events: none;
}
.rv-comment-wrapper.rv-expanded::after { display: none; }

.rv-comment-toggle {
  color: #7a8288;
  font-size: 0.85em;
  cursor: pointer;
  text-decoration: none;
  border-bottom: 1px dashed #7a8288;
  display: inline-block;
  margin-top: 0.25em;
  margin-right: 0.25em;
}
.rv-comment-toggle:hover { color: #494e52; }
</style>

<!-- ========== 按分类循环生成表格 ========== -->
{%- for category in site.data.reviews -%}
  {%- assign cat_key = category[0] -%}
  {%- assign cat_items = category[1] -%}

<h2 id="{{ cat_key | slugify }}" class="rv-section-heading">{{ cat_key }}</h2>

<div class="rv-wrap">
  <table class="rv-table">
    <thead>
      <tr>
        <th>Title</th>
        <th data-sort-method="number">My Rating</th>
        <th data-sort-method="number">Douban</th>
        <th>Date</th>
        <th>Comment</th>
      </tr>
    </thead>
    <tbody>
      {%- for row in cat_items -%}
        {%- if row.title and row.title != "" -%}
      <tr>
        <td>
          {{ row.title }}
          {%- if row.card_subtitle and row.card_subtitle != "" -%}
            <span class="rv-subtitle">{{ row.card_subtitle }}</span>
          {%- endif -%}
        </td>
        <td class="rv-score-cell">{{ row.my_rating }}</td>
        <td class="rv-score-cell">{{ row.douban_rating }}</td>
        <td class="rv-date-cell">{{ row.create_time }}</td>
        <td class="rv-comment-cell">
          {%- if row.comment and row.comment != "" -%}
            <div class="rv-comment-wrapper">{{ row.comment }}</div>
            <a href="javascript:void(0)" class="rv-comment-toggle" role="button">展开</a>
          {%- else -%}
            —
          {%- endif -%}
        </td>
      </tr>
        {%- endif -%}
      {%- endfor -%}
    </tbody>
  </table>
</div>
{%- endfor -%}

<!-- ========== 脚本 ========== -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/tablesort/5.3.0/tablesort.min.js"></script>
<script>
(function () {
  /* 1) 日期列 → data-sort（所有表格统一处理） */
  document.querySelectorAll('.rv-date-cell').forEach(function (td) {
    var text = td.textContent.trim();
    if (!text) return;
    var parts  = text.split(' ');
    var dp     = parts[0].split('/');
    var tp     = parts[1] ? parts[1].split(':') : ['00', '00'];
    var iso =
      dp[0] + '-' +
      dp[1].padStart(2, '0') + '-' +
      dp[2].padStart(2, '0') + 'T' +
      (tp[0] || '00').padStart(2, '0') + ':' +
      (tp[1] || '00').padStart(2, '0');
    td.setAttribute('data-sort', iso);
  });

  /* 2) 为每张表初始化 Tablesort */
  document.querySelectorAll('.rv-table').forEach(function (table) {
    if (typeof Tablesort !== 'undefined') {
      new Tablesort(table);
    }
  });

  /* 3) 展开 / 收起（事件委托挂在 document，自动覆盖所有表格） */
  document.addEventListener('click', function (e) {
    var toggle = e.target.closest('.rv-comment-toggle');
    if (!toggle) return;
    var wrapper = toggle.previousElementSibling;
    if (!wrapper) return;
    var expanding = !wrapper.classList.contains('rv-expanded');
    wrapper.classList.toggle('rv-expanded', expanding);
    toggle.textContent = expanding ? '收起' : '展开';
  });

  /* 4) 目录锚点平滑滚动 */
  document.querySelectorAll('.rv-toc a[href^="#"]').forEach(function (a) {
    a.addEventListener('click', function (e) {
      e.preventDefault();
      var target = document.querySelector(this.getAttribute('href'));
      if (target) {
        target.scrollIntoView({ behavior: 'smooth', block: 'start' });
      }
    });
  });
})();
</script>
