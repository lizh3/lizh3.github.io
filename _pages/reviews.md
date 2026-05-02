---
title: "Reviews"
permalink: /reviews/
layout: single
author_profile: true
---

A curated collection of books I've read, with personal ratings and notes. Click column headers to sort.

{% raw %}<style>
.rv-table {
  width: 100%;
  border-collapse: collapse;
  font: inherit;
}
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
.rv-table th.sort-up::after {
  content: "\2191";
  opacity: 1;
}
.rv-table th.sort-down::after {
  content: "\2193";
  opacity: 1;
}
.rv-table th.no-sort {
  cursor: default;
  pointer-events: none;
}
.rv-table th.no-sort::after {
  content: "";
}
.rv-table td {
  padding: 0.5em 0.75em;
  vertical-align: top;
  border-bottom: 1px solid #eee;
  overflow-wrap: break-word;
}
.rv-table tr:hover {
  background: #f8f8f8;
}
.rv-score-cell {
  text-align: center;
}
.rv-date-cell {
  white-space: nowrap;
}
.rv-subtitle {
  display: block;
  font-size: 0.85em;
  color: #7a8288;
  margin-top: 0.2em;
}
.rv-comment-cell {
  position: relative;
}
.rv-comment-wrapper {
  max-height: 4.5em;
  overflow: hidden;
  transition: max-height 0.3s ease;
  position: relative;
  line-height: 1.5em;
}
.rv-comment-wrapper.rv-expanded {
  max-height: 1000px;
}
.rv-comment-wrapper::after {
  content: "";
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 2.5em;
  background: linear-gradient(transparent, white);
  pointer-events: none;
}
.rv-comment-wrapper.rv-expanded::after {
  display: none;
}
.rv-comment-toggle {
  color: #7a8288;
  font-size: 0.85em;
  cursor: pointer;
  text-decoration: none;
  border-bottom: 1px dashed #7a8288;
  display: inline-block;
  margin-top: 0.25em;
}
.rv-comment-toggle:hover {
  color: #494e52;
}
</style>{% endraw %}

<script src="https://cdnjs.cloudflare.com/ajax/libs/tablesort/5.3.0/tablesort.min.js"></script>

<table class="responsive rv-table" id="rv-reviews-table">
  <thead>
    <tr>
      <th>Title</th>
      <th>My Rating</th>
      <th>Douban</th>
      <th>Date</th>
      <th class="no-sort">Comment</th>
    </tr>
  </thead>
  <tbody>
    {%- for row in site.data.reviews -%}
      {%- if row.title and row.title != "" -%}
      <tr>
        <td data-label="Title" data-sort="{{ row.title }}">
          {{ row.title }}
          <span class="rv-subtitle">{{ row.card_subtitle }}</span>
        </td>
        <td data-label="My Rating" data-sort="{{ row.my_rating }}" class="rv-score-cell">
          {{ row.my_rating }}
        </td>
        <td data-label="Douban" data-sort="{{ row.douban_rating }}" class="rv-score-cell">
          {{ row.douban_rating }}
        </td>
        <td data-label="Date" class="rv-date-cell">
          {{ row.create_time }}
        </td>
        <td data-label="Comment" class="rv-comment-cell">
          {%- if row.comment and row.comment != "" and row.comment.size > 80 -%}
          <div class="rv-comment-wrapper">{{ row.comment }}</div><span class="rv-comment-toggle">展开</span>
          {%- elsif row.comment and row.comment != "" -%}
          {{ row.comment }}
          {%- else -%}
          —
          {%- endif -%}
        </td>
      </tr>
      {%- endif -%}
    {%- endfor -%}
  </tbody>
</table>

{% raw %}<script>
(function() {
  // Normalize dates for sorting
  var dateCells = document.querySelectorAll('.rv-date-cell');
  dateCells.forEach(function(td) {
    var text = td.textContent.trim();
    var parts = text.split(' ');
    var dateParts = parts[0].split('/');
    var timeParts = parts[1] ? parts[1].split(':') : ['00', '00'];
    var iso = dateParts[0] + '-' +
              dateParts[1].padStart(2, '0') + '-' +
              dateParts[2].padStart(2, '0') + 'T' +
              (timeParts[0] || '00').padStart(2, '0') + ':' +
              (timeParts[1] || '00').padStart(2, '0');
    td.setAttribute('data-sort', iso);
  });

  // Initialize Tablesort
  var table = document.getElementById('rv-reviews-table');
  if (table && typeof Tablesort !== 'undefined') {
    new Tablesort(table);
  }

  // Expand/collapse with event delegation
  var tbody = table ? table.querySelector('tbody') : null;
  if (tbody) {
    tbody.addEventListener('click', function(e) {
      var toggle = e.target.closest('.rv-comment-toggle');
      if (!toggle) return;
      var wrapper = toggle.previousElementSibling;
      if (!wrapper || !wrapper.classList.contains('rv-comment-wrapper')) return;
      var isExpanded = wrapper.classList.toggle('rv-expanded');
      toggle.textContent = isExpanded ? '\u6536\u8D77' : '\u5C55\u5F00';
      e.stopPropagation();
    });
  }

  // Reset expanded comments on sort
  if (table) {
    table.addEventListener('beforeSort', function() {
      var expanded = table.querySelectorAll('.rv-comment-wrapper.rv-expanded');
      expanded.forEach(function(w) {
        w.classList.remove('rv-expanded');
        var t = w.nextElementSibling;
        if (t && t.classList.contains('rv-comment-toggle')) t.textContent = '\u5C55\u5F00';
      });
    });
  }
})();
</script>{% endraw %}
