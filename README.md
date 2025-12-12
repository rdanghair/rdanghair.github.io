<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Barber Appointment Booking</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #0f0f0f;
      color: white;
    }
    header {
      background: #111;
      padding: 20px;
      text-align: center;
      font-size: 28px;
      font-weight: bold;
      letter-spacing: 1px;
    }
    .container {
      max-width: 600px;
      margin: 30px auto;
      background: #1a1a1a;
      padding: 25px;
      border-radius: 15px;
      box-shadow: 0px 0px 15px rgba(255,255,255,0.08);
    }
    label {
      font-size: 16px;
    }
    input, select {
      width: 100%;
      padding: 12px;
      margin-top: 6px;
      margin-bottom: 18px;
      border-radius: 8px;
      border: none;
      background: #333;
      color: white;
      font-size: 15px;
    }
    button {
      width: 100%;
      padding: 15px;
      background: #d10000;
      color: white;
      border: none;
      border-radius: 10px;
      font-size: 18px;
      cursor: pointer;
      margin-top: 10px;
    }
    button:hover {
      background: #ff0000;
    }
    .social {
      text-align: center;
      margin-top: 20px;
    }
    .social a {
      color: #ff0055;
      font-size: 18px;
      text-decoration: none;
    }
  </style>
</head>
<body>

<header>Book Your Cut ✂️</header>

<div class="container">
  <form id="bookingForm" action="https://script.google.com/macros/s/YOUR-SCRIPT-ID/exec" method="POST">

    <label>Full Name</label>
    <input type="text" name="name" required />

    <label>Phone Number</label>
    <input type="text" name="phone" required />

    <label>Date</label>
    <input type="date" name="date" required />

    <label>Time</label>
    <input type="time" name="time" required />

    <label>Choose Your Style</label>
    <select name="style" required>
      <option value="Fade">Fade</option>
      <option value="Taper">Taper</option>
      <option value="Buzz Cut">Buzz Cut</option>
      <option value="Line Up">Line Up</option>
      <option value="Waves Maintenance">Waves Maintenance</option>
      <option value="Beard Trim">Beard Trim</option>
      <option value="Fade + Beard">Fade + Beard</option>
      <option value="Custom Style">Custom Style</option>
    </select>

    <button type="submit">Confirm Booking</button>
  </form>

  <div class="social">
    <p>Follow me on Instagram:</p>
    <a href="https://instagram.com/YOUR_INSTAGRAM" target="_blank">@YOUR_INSTAGRAM</a>
  </div>

</div>

<script>
  document.getElementById("bookingForm").addEventListener("submit", function() {
    alert("Your appointment is booked! You will receive a confirmation message.");
  });
</script>

</body>
</html>
