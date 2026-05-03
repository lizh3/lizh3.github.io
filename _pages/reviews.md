---
title: "评录"
permalink: /reviews/
layout: single
classes: wide
author_profile: true
---

A curated collection with personal ratings and notes. Click column headers to sort.

<!-- ========== 右侧悬浮目录 ========== -->
<nav class="rv-toc" id="rv-toc-box" aria-labelledby="rv-toc-heading">
  <div style="display:flex; justify-content:space-between; align-items:center; margin-bottom: 0.5em;">
    <h3 id="rv-toc-heading" style="margin:0; font-size:1em; color:#494e52;">目录</h3>
    <button id="rv-toc-close-btn" style="background:none;border:none;cursor:pointer;font-size:1.1em;color:#7a8288;line-height:1;padding:0 2px;" title="收起目录">✕</button>
  </div>
  <ol>
    {%- for category in site.data.reviews -%}
      {%- assign cat_key = category[0] -%}
      {%- assign cat_data = category[1] -%}
      {%- if cat_data.headers -%}
        {%- assign cat_items = cat_data.items -%}
      {%- else -%}
        {%- assign cat_items = cat_data -%}
      {%- endif -%}
    <li>
      <a href="#{{ cat_key | slugify }}">{{ cat_key }}</a>
      <span class="rv-toc-count">{{ cat_items.size }}</span>
    </li>
    {%- endfor -%}
  </ol>
</nav>

<button id="rv-toc-open-btn" class="rv-toc-open" title="展开目录">目录</button>

<style>
/* ── 右侧悬浮目录 ── */
.rv-toc {
  position: fixed;
  top: 6rem;
  right: 1.5rem;
  width: 170px;
  max-height: 70vh;
  overflow-y: auto;
  background: rgba(255, 255, 255, 0.92);
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
  border: 1px solid #e8e8e8;
  border-radius: 6px;
  padding: 1em;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  z-index: 10;
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.rv-toc h3 { font-size: 1em; }
.rv-toc ol { margin: 0; padding-left: 1.2em; list-style: decimal; }
.rv-toc li { margin-bottom: 0.4em; font-size: 0.9em; }
.rv-toc a { color: #494e52; text-decoration: none; font-weight: 500; }
.rv-toc a:hover { color: #00a4bd; text-decoration: underline; }
.rv-toc-count {
  display: inline-block;
  background: #e8e8e8;
  color: #7a8288;
  font-size: 0.75em;
  padding: 0.05em 0.4em;
  border-radius: 8px;
  margin-left: 0.3em;
}
.rv-toc.rv-toc-hidden {
  opacity: 0;
  pointer-events: none;
  transform: translateX(20px);
}
.rv-toc-open {
  position: fixed;
  bottom: 2rem;
  right: 1.5rem;
  z-index: 10;
  background: #494e52;
  color: #fff;
  border: none;
  border-radius: 20px;
  padding: 0.5em 1em;
  cursor: pointer;
  font-size: 0.85em;
  box-shadow: 0 2px 8px rgba(0,0,0,0.2);
  transition: background 0.2s;
  display: none;
}
.rv-toc-open:hover { background: #00a4bd; }

/* 移动端：目录改为底部弹出 + 默认收起 */
@media (max-width: 1024px) {
  .rv-toc {
    top: auto;
    bottom: 4.5rem;
    right: 0.75rem;
    left: 0.75rem;
    width: auto;
    max-height: 50vh;
  }
  .rv-toc.rv-toc-hidden { transform: translateY(20px); }
  .rv-toc-open { bottom: 1rem; right: 1rem; }
}

/* ── 分类标题 ── */
.rv-section-heading {
  margin-top: 2.5em;
  margin-bottom: 0.8em;
  padding-bottom: 0.4em;
  border-bottom: 2px solid #bdc1c4;
  scroll-margin-top: 2em;
}

/* ── 表格容器 ── */
.rv-wrap {
  width: 100%;
  overflow-x: auto !important;
  margin-bottom: 2em;
  -webkit-overflow-scrolling: touch;
}
p .rv-wrap { width: 100%; }

/* ── 表格（自适应布局，兼容任意列数） ── */
.rv-table {
  width: 100%;
  min-width: 700px;
  border-collapse: collapse;
  font: inherit;
}

/* ── 表头 & 排序箭头 ── */
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
  right: 0.3em;
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
.rv-subtitle { display: block; font-size: 0.85em; color: #7a8288; margin-top: 0.2em; }

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
{%- assign cat_data = category[1] -%}
{%- comment -%} 兼容逻辑：有 headers 用自定义，没有则降级默认5列 {%- endcomment -%}
{%- if cat_data.headers -%}
{%- assign headers = cat_data.headers -%}
{%- assign cat_items = cat_data.items -%}
{%- else -%}
{%- assign headers = "Title,My Rating,Douban,Date,Comment" | split: "," -%}
{%- assign cat_items = cat_data -%}
{%- endif -%}
<h2 id="{{ cat_key | slugify }}" class="rv-section-heading">{{ cat_key }}</h2>
<div class="rv-wrap">
<table class="rv-table">
<thead>
<tr>
<th>{{ headers[0] }}</th>
{%- if headers.size > 1 -%}<th>{{ headers[1] }}</th>{%- endif -%}
{%- if headers.size > 2 -%}<th>{{ headers[2] }}</th>{%- endif -%}
{%- if headers.size > 3 -%}<th>{{ headers[3] }}</th>{%- endif -%}
{%- if headers.size > 4 -%}<th data-sort-method="number">{{ headers[4] }}</th>{%- endif -%}
{%- if headers.size > 5 -%}<th>{{ headers[5] }}</th>{%- endif -%}
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
{%- if headers.size > 1 -%}<td>{{ row.author }}</td>{%- endif -%}
{%- if headers.size > 2 -%}<td class="rv-date-cell">{{ row.create_time }}</td>{%- endif -%}
{%- if headers.size > 3 -%}<td class="rv-date-cell">{{ row.finish_tiem }}</td>{%- endif -%}
{%- if headers.size > 4 -%}<td>{{ row.my_rating }}</td>{%- endif -%}
{%- if headers.size > 5 -%}
<td class="rv-comment-cell">
{%- if row.comment and row.comment != "" -%}
<div class="rv-comment-wrapper">{{ row.comment }}</div>
<a href="javascript:void(0)" class="rv-comment-toggle" role="button">展开</a>
{%- else -%}
—
{%- endif -%}
</td>
{%- endif -%}
</tr>
{%- endif -%}
{%- endfor -%}
</tbody>
</table>
</div>
{%- endfor -%}

<script>
(function () {
  /* ── 1. 日期列标准化（兼容 2023/01/15 21:00 和 1999.05 格式） ── */
  document.querySelectorAll('.rv-date-cell').forEach(function (td) {
    var t = td.textContent.trim();
    if (!t) return;
    var iso = '';
    
    if (t.includes('/')) {
      var p = t.split(' ');
      var d = p[0].split('/');
      var tm = p[1] ? p[1].split(':') : ['00', '00'];
      iso = d[0] + '-' + d[1].padStart(2, '0') + '-' + (d[2] || '01').padStart(2, '0') + 'T' +
            (tm[0] || '00').padStart(2, '0') + ':' + (tm[1] || '00').padStart(2, '0');
    } 
    else if (t.includes('.')) {
      var dp = t.split('.');
      iso = dp[0] + '-' + (dp[1] || '01').padStart(2, '0') + '-' + (dp[2] || '01').padStart(2, '0') + 'T00:00:00';
    } 
    else {
      iso = t;
    }
    
    td.setAttribute('data-sort', iso);
  });

  /* ── 2. 纯 JS 排序 ── */
  document.querySelectorAll('.rv-table').forEach(function (table) {
    var ths = table.querySelectorAll('th');
    var tbody = table.querySelector('tbody');
    var activeCol = -1;
    var activeAsc = true;

    ths.forEach(function (th, idx) {
      th.addEventListener('click', function () {
        var method = th.getAttribute('data-sort-method') || 'string';

        if (activeCol === idx) {
          activeAsc = !activeAsc;
        } else {
          activeCol = idx;
          activeAsc = true;
        }

        ths.forEach(function (h) { h.classList.remove('sort-up', 'sort-down'); });
        th.classList.add(activeAsc ? 'sort-up' : 'sort-down');

        var rows = Array.from(tbody.children);
        rows.sort(function (a, b) {
          var aCell = a.children[activeCol];
          var bCell = b.children[activeCol];
          var av = aCell.getAttribute('data-sort') || aCell.textContent.trim();
          var bv = bCell.getAttribute('data-sort') || bCell.textContent.trim();

          if (method === 'number') {
            var an = parseFloat(av) || 0;
            var bn = parseFloat(bv) || 0;
            return activeAsc ? an - bn : bn - an;
          }
          var cmp = String(av).localeCompare(String(bv), undefined, { numeric: true, sensitivity: 'base' });
          return activeAsc ? cmp : -cmp;
        });

        rows.forEach(function (r) { tbody.appendChild(r); });
      });
    });
  });

  /* ── 3. 展开 / 收起 ── */
  document.addEventListener('click', function (e) {
    var btn = e.target.closest('.rv-comment-toggle');
    if (!btn) return;
    var w = btn.previousElementSibling;
    if (!w) return;
    var expanding = !w.classList.contains('rv-expanded');
    w.classList.toggle('rv-expanded', expanding);
    btn.textContent = expanding ? '收起' : '展开';
  });

  /* ── 4. 目录锚点平滑滚动 ── */
  document.querySelectorAll('.rv-toc a[href^="#"]').forEach(function (a) {
    a.addEventListener('click', function (e) {
      e.preventDefault();
      var el = document.querySelector(this.getAttribute('href'));
      if (el) el.scrollIntoView({ behavior: 'smooth', block: 'start' });
      if (window.innerWidth <= 1024) {
        var box = document.getElementById('rv-toc-box');
        var ob = document.getElementById('rv-toc-open-btn');
        if (box) box.classList.add('rv-toc-hidden');
        if (ob) ob.style.display = 'block';
      }
    });
  });

  /* ── 5. 目录 打开 / 关闭 ── */
  var box = document.getElementById('rv-toc-box');
  var cBtn = document.getElementById('rv-toc-close-btn');
  var oBtn = document.getElementById('rv-toc-open-btn');

  if (cBtn && box && oBtn) {
    cBtn.addEventListener('click', function () {
      box.classList.add('rv-toc-hidden');
      oBtn.style.display = 'block';
    });
    oBtn.addEventListener('click', function () {
      box.classList.remove('rv-toc-hidden');
      oBtn.style.display = 'none';
    });
    if (window.innerWidth <= 1024) {
      box.classList.add('rv-toc-hidden');
      oBtn.style.display = 'block';
    }
  }
})();
</script>
