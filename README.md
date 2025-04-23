<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Class Management Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    .header { display: flex; justify-content: space-between; align-items: center; }
    .header h1 { margin: 0; }
    .search-add { margin: 20px 0; display: flex; gap: 10px; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 10px; text-align: left; }
    .teacher-cell { display: flex; align-items: center; gap: 10px; }
    .teacher-cell img { border-radius: 50%; width: 40px; height: 40px; }
    .notification-box { background: #f0f8ff; border: 1px solid #87ceeb; padding: 10px; margin-top: 20px; }
  </style>
</head>
<body>
  <div class="header">
    <h1>Class Management Dashboard</h1>
    <button onclick="alert('Settings Panel Coming Soon!')">⚙️ Settings</button>
  </div>

  <div class="search-add">
    <input type="text" id="searchInput" placeholder="Search by course or teacher..." oninput="filterClasses()" />
    <button onclick="alert('Add Class Feature Coming Soon!')">➕ Add New Class</button>
  </div>

  <div id="notification" class="notification-box"></div>

  <table id="classTable">
    <thead>
      <tr>
        <th>Category</th>
        <th>Course Name</th>
        <th>Teacher</th>
        <th>Timing</th>
        <th>Room</th>
        <th>Fee</th>
      </tr>
    </thead>
    <tbody id="classBody"></tbody>
  </table>

  <script>
    const classes = [
      {
        category: "Dance",
        courseName: "Bharatnatyam",
        teacherName: "Meenakshi Rao",
        teacherImage: "https://via.placeholder.com/40",
        timing: "Saturday: 8:30 to 10:30am",
        room: "N/A",
        fee: "Rs.2800 (8 classes in a month)"
      },
      {
        category: "Music",
        courseName: "Hindustani Vocal",
        teacherName: "Akansha",
        teacherImage: "https://via.placeholder.com/40",
        timing: "Sat & Sun (Kids): 10:00 to 11:00am",
        room: "N/A",
        fee: "Rs.3000/-, Individual: Rs.1200/hr"
      },
      {
        category: "Fitness",
        courseName: "Dance Fitness",
        teacherName: "Ajay Soni",
        teacherImage: "https://via.placeholder.com/40",
        timing: "Mon, Wed, Fri: 6:15 to 7:15pm",
        room: "N/A",
        fee: "Rs.3500 (8 classes), Rs.4500 (12 classes)"
      }
      // Add more classes as needed
    ];

    const classBody = document.getElementById("classBody");

    function renderTable(filtered = classes) {
      classBody.innerHTML = "";
      filtered.forEach(cls => {
        const row = `<tr>
          <td><input value="${cls.category}" /></td>
          <td><input value="${cls.courseName}" /></td>
          <td class="teacher-cell">
            <img src="${cls.teacherImage}" alt="${cls.teacherName}" />
            <input value="${cls.teacherName}" />
          </td>
          <td><input value="${cls.timing}" /></td>
          <td><input value="${cls.room}" /></td>
          <td><input value="${cls.fee}" /></td>
        </tr>`;
        classBody.innerHTML += row;
      });
    }

    function filterClasses() {
      const search = document.getElementById("searchInput").value.toLowerCase();
      const filtered = classes.filter(c =>
        c.courseName.toLowerCase().includes(search) ||
        c.teacherName.toLowerCase().includes(search)
      );
      renderTable(filtered);
    }

    function showCurrentClasses() {
      const now = new Date();
      const hour = now.getHours();
      const min = now.getMinutes();
      const day = now.toLocaleString('en-US', { weekday: 'long' });

      const activeClasses = classes.filter(cls => {
        const match = cls.timing.match(/(\d{1,2}:\d{2})\s*(?:to|-)\s*(\d{1,2}:\d{2})(am|pm)/gi);
        if (!match) return false;

        return match.some(timeRange => {
          const [startRaw, endRaw] = timeRange.split(/to|-/i);
          const start = convertTo24Hour(startRaw.trim());
          const end = convertTo24Hour(endRaw.trim());

          const current = hour * 60 + min;
          return current >= start && current <= end && cls.timing.toLowerCase().includes(day.toLowerCase());
        });
      });

      const notificationBox = document.getElementById("notification");
      if (activeClasses.length) {
        notificationBox.innerHTML = `<strong>Ongoing Classes:</strong><ul>${activeClasses.map(cls => `<li>${cls.courseName} with ${cls.teacherName} (${cls.room})</li>`).join('')}</ul>`;
      } else {
        notificationBox.innerHTML = "<em>No classes are currently ongoing.</em>";
      }
    }

    function convertTo24Hour(timeStr) {
      const [h, m] = timeStr.match(/\d+/g);
      const isPM = /pm/i.test(timeStr);
      let hours = parseInt(h);
      if (isPM && hours !== 12) hours += 12;
      if (!isPM && hours === 12) hours = 0;
      return hours * 60 + parseInt(m);
    }

    renderTable();
    showCurrentClasses();
    setInterval(showCurrentClasses, 60000);
  </script>
</body>
</html>
