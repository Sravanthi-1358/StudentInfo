const express = require('express');
const mysql = require('mysql2');
const path = require('path');

const app = express();
const PORT = 3001;

// MySQL connection
const connection = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: '@Sravanthi135',
  database: 'kutty'
});

connection.connect((err) => {
  if (err) {
    console.error('MySQL connection error:', err.message);
  } else {
    console.log('Connected to MySQL database');
  }
});

// Static files (if needed)
app.use(express.static(path.join(__dirname, 'public')));

// Homepage route
app.get('/', (req, res) => {
  res.send(`<h2>Welcome to Kutty Student Portal</h2><p>Visit <a href="/students">/students</a> to view student records.</p>`);
});

// Students route
app.get('/students', (req, res) => {
  connection.query('SELECT * FROM students', (err, results) => {
    if (err) {
      return res.status(500).send('Database query failed');
    }

    let html = `
      <h1>Student Records</h1>
      <table border="1" cellpadding="5" cellspacing="0">
        <tr><th>ID</th><th>Name</th><th>Age</th></tr>
    `;

    results.forEach(student => {
      html += `<tr><td>${student.id}</td><td>${student.name}</td><td>${student.age}</td></tr>`;
    });

    html += `</table>`;
    res.send(html);
  });
});

// Start server
app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}`);
});
