---
layout: home
---

The RISE Journal Club aims to create a friendly environment to discuss the latest state-of-the-art papers in the areas of medical image analysis, AI and computer vision. The main objective of this bi-weekly reading group is to help you develop/improve your critical and design thinking skills, which are essential skills for researchers and will help you when presenting or writing your own work.

To date, we have held two editions in which participants joined us remotely from different continents in highly engaging and stimulating sessions.

During each session, the moderators briefly introduce the paper and then moderate a discussion where everyone is welcome to provide their thoughts and ask any questions on the paper. The topics of the papers will vary, and we will try to cover different areas of medical data analysis, e.g., registration, segmentation, federated learning, fairness, and reinforcement learning —among others. Similarly, we will review papers from the machine and deep learning communities, providing you with a broader overview of the state-of-the-art method.


<h1>Upcoming Journal Club Sessions</h1>
<table id="journal-club-table">
  <thead>
    <tr>
      <th class="sortable" data-sort="date">Date</th>
      <th class="sortable" data-sort="speaker">Speaker</th>
      <th class="sortable" data-sort="title">Title</th>
      <th>Conference</th>
      <th>Link (Paper)</th>
      <th>Zoom Link</th>
      <th>Calendar</th> <!-- New column for the calendar button -->
    </tr>
  </thead>
  <tbody>
    {% assign today = 'now' | date: '%Y-%m-%d' %}
    {% for session in site.data.schedule %}
      {% if session.speaker != "" %} <!-- Check if speaker exists -->
        {% assign session_date_parts = session.date | split: '/' %}
        {% assign session_date = session_date_parts[2] | append: '-' | append: session_date_parts[1] | append: '-' | append: session_date_parts[0] %}
        {% if session_date >= today %}
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
            <td>
              <span class="save-to-calendar"
                    data-date="{{ session.date }}"
                    data-title="{{ session.paper_title }}"
                    data-speaker="{{ session.speaker }}"
                    data-zoom-link="{{ session.zoom_link }}"
                    style="cursor: pointer; color: #1e90ff; text-decoration: underline;">
                Save Calendar
              </span>
            </td> <!-- New calendar button/link in the new column -->
          </tr>
        {% endif %}
      {% endif %}
    {% endfor %}
  </tbody>
</table>

<script>
  // Sorting functionality for table headers
  document.querySelectorAll('.sortable').forEach(function(header) {
    header.addEventListener('click', function() {
      const table = document.querySelector('#journal-club-table');
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
          aText = new Date(aText);
          bText = new Date(bText);
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


// Handle "Save to Calendar" functionality when clicking on the Date
document.querySelectorAll('.save-to-calendar').forEach(function(span) {
  span.addEventListener('click', function() {
    // Split the date into day, month, and year
    const dateParts = span.dataset.date.split('/');
    const date = new Date(dateParts[2], dateParts[1] - 1, dateParts[0]);  // Convert to YYYY-MM-DD format

    const title = span.dataset.title;
    const author = span.dataset.speaker;
    const zoomLink = span.dataset.zoomLink;

    const eventDate = new Date(date.setHours(16, 0, 0, 0)); // Set time to 16:00 UTC
    // Format the date and time for ICS
    const formattedDate = eventDate.toISOString().replace(/-|:|\.\d+/g, "");
    const startDate = formattedDate;
    const endDate = new Date(eventDate);
    endDate.setHours(eventDate.getHours() + 2);  // Set the end time to 18:00 UTC
    const endDateFormatted = endDate.toISOString().replace(/-|:|\.\d+/g, "");

    // Generate ICS content
    const icsContent = `
BEGIN:VCALENDAR
VERSION:2.0
BEGIN:VEVENT
SUMMARY:${title} by ${author}
DESCRIPTION:Join us for the MICCAI-RISE Journal Club Session on the paper"${title}" presented by ${author}.
DTSTART:${startDate}
DTEND:${endDateFormatted}
LOCATION:Zoom
URL:${zoomLink}
END:VEVENT
END:VCALENDAR`;

    // Create a Blob for the ICS file
    const blob = new Blob([icsContent], { type: 'text/calendar' });
    const url = URL.createObjectURL(blob);

    // Create a link to download the ICS file
    const link = document.createElement('a');
    link.href = url;
    link.download = `${title}-${date.toISOString().split('T')[0]}.ics`;
    link.click();

    // Revoke the object URL after downloading
    URL.revokeObjectURL(url);
  });
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

