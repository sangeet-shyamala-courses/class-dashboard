<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Class Management Dashboard</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 10px; text-align: left; }
    #searchBox { padding: 10px; width: 300px; margin-bottom: 20px; }
    button { padding: 10px 20px; margin-left: 10px; }
  </style>
</head>
<body>
  <input type="text" id="searchBox" placeholder="Search for a course..." onkeyup="searchCourse()" />
  <button onclick="addNewRow()">Add New Row</button>
  <button onclick="exportToExcel()">Export to Excel</button>
  <button onclick="exportToPDF()">Export to PDF</button>

  <table id="courseTable">
    <thead>
      <tr>
        <th>Course</th>
        <th>Instructor</th>
        <th>Days</th>
        <th>Time</th>
        <th>Fee</th>
        <th>Room No.</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <!-- Existing rows remain unchanged -->
      <!-- ... -->
    </tbody>
  </table>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.0/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script>
    function searchCourse() {
      let input = document.getElementById("searchBox").value.toLowerCase();
      let rows = document.querySelectorAll("#courseTable tbody tr");
      for (let i = 0; i < rows.length; i++) {
        let rowText = rows[i].innerText.toLowerCase();
        rows[i].style.display = rowText.includes(input) ? "" : "none";
      }
    }

    function addNewRow() {
      const table = document.querySelector("#courseTable tbody");
      const newRow = document.createElement("tr");
      newRow.innerHTML = `
        <td contenteditable="true">New Course</td>
        <td contenteditable="true">Instructor</td>
        <td contenteditable="true">Days</td>
        <td contenteditable="true">Time</td>
        <td contenteditable="true">Fee</td>
        <td contenteditable="true">Room No.</td>
        <td><button onclick="alert('Changes saved!')">Save</button></td>
      `;
      table.appendChild(newRow);
    }

    function exportToExcel() {
      let table = document.getElementById("courseTable");
      let workbook = XLSX.utils.table_to_book(table, {sheet: "Courses"});
      XLSX.writeFile(workbook, "ClassSchedule.xlsx");
    }

    async function exportToPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      const table = document.getElementById("courseTable");
      let rows = Array.from(table.rows).map(row =>
        Array.from(row.cells).map(cell => cell.innerText.trim())
      );

      let startY = 10;
      rows.forEach((row, i) => {
        row.forEach((cell, j) => {
          doc.text(cell, 10 + j * 30, startY);
        });
        startY += 10;
      });

      doc.save("ClassSchedule.pdf");
    }
  </script>
</body>
</html>
