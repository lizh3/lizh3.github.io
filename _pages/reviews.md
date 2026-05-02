---
title: "Reviews"
permalink: /reviews/
layout: single
classes: wide
author_profile: true
---

Still testing...

<style>
/* 让表格撑满宽页容器（配合 classes: wide） */
.rv-wrap {
  width: 100%;
  overflow-x: auto;
}
.rv-table {
  width: 100%;
  min-width: 960px;          /* 防止窄屏下字段挤在一起；可根据实际情况微调 */
  border-collapse: collapse;
  font: inherit;
}

/* 表头与排序指示 */
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
.rv-table th.sort-up::after { content: "\2191"; opacity: 1; }
.rv-table th.sort-down::after { content: "\2193"; opacity: 1; }

/* 单元格通用 */
.rv-table td {
  padding: 0.5em 0.75em;
  vertical-align: top;
  border-bottom: 1px solid #eee;
  overflow-wrap: break-word;
}
.rv-table tr:hover { background: #f8f8f8; }

/* 列样式 */
.rv-score-cell { text-align: center; }
.rv-date-cell { white-space: nowrap; }
.rv-subtitle {
  display: block;
  font-size: 0.85em;
  color: #7a8288;
  margin-top: 0.2em;
}

/* 评论折叠（展开/收起） */
.rv-comment-cell { position: relative; }
.rv-comment-wrapper {
  max-height: 4.5em;         /* 默认只显示大约 3 行 */
  overflow: hidden;
  transition: max-height 0.25s ease;
  position: relative;
  line-height: 1.5em;
}
.rv-comment-wrapper.rv-expanded { max-height: 2000px; }   /* 足够大，视内容长度可微调 */

/* 底部渐隐遮罩 */
.rv-comment-wrapper::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2.5em;
  background: linear-gradient(transparent, #fff);
  pointer-events: none;
}
.rv-comment-wrapper.rv-expanded::after { display: none; }

/* “展开/收起”按钮 */
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

<div class="rv-wrap">
  <table id="rv-reviews-table" class="rv-table">
    <thead>
      <tr>
        <th data-sort-method="string" class="no-sort">Title</th>
        <th data-sort-method="number">My Rating</th>
        <th data-sort-method="number">Douban</th>
        <th>Date</th>
        <th data-sort-method="string">Comment</th>
      </tr>
    </thead>
    <tbody>
      {%- assign reviews = site.data.reviews -%}
      {%- for row in reviews -%}
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

<script src="https://cdnjs.cloudflare.com/ajax/libs/tablesort/5.3.0/tablesort.min.js"></script>
<script>
(function () {
  // 为日期列增加 data-sort 属性，确保排序稳定（把 2022/7/17 22:22 转为 2022-07-17T22:22）
  var dateCells = document.querySelectorAll('.rv-date-cell');
  dateCells.forEach(function (td) {
    var text = td.textContent.trim();
    var parts = text.split(' ');
    var dateParts = parts[0].split('/');
    var timeParts = parts[1] ? parts[1].split(':') : ['00', '00'];

    var iso =
      dateParts[0] + '-' +
      dateParts[1].padStart(2, '0') + '-' +
      dateParts[2].padStart(2, '0') + 'T' +
      (timeParts[0] || '00').padStart(2, '0') + ':' +
      (timeParts[1] || '00').padStart(2, '0');

    td.setAttribute('data-sort', iso);
  });

  // 初始化排序
  var table = document.getElementById('rv-reviews-table');
  if (table && typeof Tablesort !== 'undefined') {
    new Tablesort(table);
  }

  // 展开/收起（事件委托）
  var tbody = table ? table.querySelector('tbody') : null;
  if (tbody) {
    tbody.addEventListener('click', function (e) {
      var toggle = e.target.closest('.rv-comment-toggle');
      if (!toggle) return;
      var wrapper = toggle.previousElementSibling;
      if (!wrapper) return;

      var expanding = !wrapper.classList.contains('rv-expanded');
      wrapper.classList.toggle('rv-expanded', expanding);
      toggle.textContent = expanding ? '收起' : '展开';
    });
  }
})();
</script>
