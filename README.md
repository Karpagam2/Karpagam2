const express = require('express');
const app = express();

// Middleware to parse JSON bodies
app.use(express.json());

// Route to handle requests for timestamp conversion
app.get('/api/timestamp/:date_string?', (req, res) => {
  let dateString = req.params.date_string;
  
  // If date string is empty, use current date
  if (!dateString) {
    dateString = new Date().toISOString();
  }

  let date;
  // Check if the date string is a valid date
  if (/^\d{4}-\d{2}-\d{2}$/.test(dateString)) {
    date = new Date(dateString);
  } else {
    date = new Date(parseInt(dateString));
  }

  // Check if the date is valid
  if (isNaN(date.getTime())) {
    return res.status(400).json({ error: 'Invalid date' });
  }

  // Return JSON response
  res.json({ unix: date.getTime(), utc: date.toUTCString() });
});

// Start the server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
