---
layout: page
title: Past Sessions
permalink: /past-sessions/
---
{% assign years = "2024,2023" | split: "," %}

{% for year in years %}
  <h2>{{ year }}</h2>
  <table class="past-journal-club-table">
    <thead>
      <tr>
        <th class="sortable" data-sort="date">Date</th>
        <th class="sortable" data-sort="speaker">Speaker</th>
        <th class="sortable" data-sort="title">Title</th>
        <th>Conference</th>
        <th>Link (Paper)</th>
        <th>Zoom Link</th>
      </tr>
    </thead>
    <tbody>
      {% assign today = 'now' | date: '%Y-%m-%d' %}
      {% for session in site.data[year] %}
        {% assign session_date_parts = session.date | split: '/' %}
        {% assign session_date = session_date_parts[2] | append: '-' | append: session_date_parts[0] | append: '-' | append: session_date_parts[1] %}
        {% if session_date < today %}
          <tr>
            <td>
              <span class="save-to-calendar"
                    data-date="{{ session.date }}"
                    data-title="{{ session.paper_title }}"
                    data-speaker="{{ session.speaker }}"
                    data-zoom-link="{{ session.zoom_link }}">
                {{ session.date }}
              </span>
            </td>
            <td>{{ session.speaker }}</td>
            <td>{{ session.paper_title }}</td>
            <td>{{ session.conference }}</td>
            <td><a href="{{ session.paper_link }}" target="_blank">Link</a></td>
            <td><a href="{{ session.zoom_link }}" target="_blank">Join Zoom</a></td>
          </tr>
        {% endif %}
      {% endfor %}
    </tbody>
  </table>
{% endfor %}

<script>
// Sorting functionality for table headers
function sortTable(table) {
  document.querySelectorAll('.sortable').forEach(function(header) {
    header.addEventListener('click', function() {
      const rows = Array.from(table.querySelectorAll('tbody tr'));
      const columnIndex = Array.from(header.parentNode.children).indexOf(header);
      const isAscending = header.classList.contains('ascending');

      // Toggle sorting direction
      rows.sort(function(a, b) {
        const aCell = a.children[columnIndex];
        const bCell = b.children[columnIndex];

        let aText = aCell.textContent.trim();
        let bText = bCell.textContent.trim();

        // Convert date column to date objects for proper sorting
        if (header.dataset.sort === 'date') {
          aText = new Date(aText.split('/').reverse().join('-'));
          bText = new Date(bText.split('/').reverse().join('-'));
        }

        if (aText < bText) return isAscending ? 1 : -1;
        if (aText > bText) return isAscending ? -1 : 1;
        return 0;
      });

      // Reorder rows
      rows.forEach(function(row) {
        table.querySelector('tbody').appendChild(row);
      });

      // Toggle sort direction indicator
      if (isAscending) {
        header.classList.remove('ascending');
      } else {
        header.classList.add('ascending');
      }
    });
  });
}

// Apply sorting to all tables
document.querySelectorAll('.past-journal-club-table').forEach(function(table) {
  sortTable(table);
});
</script>

<style>
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    padding: 10px;
    text-align: left;
    border-bottom: 1px solid #ddd;
  }
  th.sortable {
    cursor: pointer;
  }
  th.sortable::after {
    content: " 🔽";
    margin-left: 10px;
  }
  th.sortable.ascending::after {
    content: " 🔼";
  }
  .save-to-calendar {
    color: #1e90ff;
    cursor: pointer;
    text-decoration: underline;
  }
  .save-to-calendar:hover {
    color: #0066cc;
  }
</style>

