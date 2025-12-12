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
      background: linear-gradient(135deg, #0a0a0a, #1a1a1a);
      color: white;
      overflow-x: hidden;
    }
    header {
      background: rgba(20,20,20,0.9);
      padding: 25px;
      text-align: center;
      font-size: 32px;
      font-weight: bold;
      letter-spacing: 1px;
      backdrop-filter: blur(10px);
      animation: fadeDown 1s ease;
    }
    @keyframes fadeDown {
      from {opacity: 0; transform: translateY(-20px);} to {opacity: 1; transform: translateY(0);} }

    .container {
      max-width: 650px;
      margin: 40px auto;
      background: rgba(255,255,255,0.05);
      padding: 30px;
      border-radius: 20px;
      box-shadow: 0px 0px 20px rgba(255,255,255,0.1);
      backdrop-filter: blur(12px);
      animation: fadeUp 1s ease;
    }
    @keyframes fadeUp {
      from {opacity: 0; transform: translateY(20px);} to {opacity: 1; transform: translateY(0);} }

    label {
      font-size: 17px;
    }
    input, select {
      width: 100%;
      padding: 14px;
      margin-top: 6px;
      margin-bottom: 20px;
      border-radius: 10px;
      border: none;
      background: rgba(255,255,255,0.1);
      color: white;
      font-size: 16px;
      backdrop-filter: blur(4px);
    }
    button {
      width: 100%;
      padding: 16px;
      background: #d10000;
      color: white;
      border: none;
      border-radius: 12px;
      font-size: 20px;
      cursor: pointer;
      margin-top: 10px;
      transition: 0.2s;
    }
    button:hover {
      background: #ff0000;
      transform: scale(1.03);
    }

    .social {
      text-align: center;
      margin-top: 25px;
      animation: fadeUp 1.2s ease;
    }
    .social a {
      color: #ff0066;
      font-size: 20px;
      text-decoration: none;
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }

    .ig-preview {
      margin-top: 15px;
      width: 100%;
      height: 400px;
      border-radius: 15px;
      overflow: hidden;
      border: 2px solid rgba(255,255,255,0.15);
      animation: fadeUp 1.4s ease;
    }

    iframe {
      width: 100%;
      height: 100%;
      border: none;
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
    <a href="https://instagram.com/YOUR_INSTAGRAM" target="_blank">
      <span>📸</span>@YOUR_INSTAGRAM
    </a>
  </div>

  <div class="ig-preview">
    <iframe src="https://www.instagram.com/YOUR_INSTAGRAM/embed" allowtransparency="true"></iframe>
  </div>
</div>

<script>
  document.getElementById("bookingForm").addEventListener("submit", function() {
    alert("Your appointment is booked! You will receive a confirmation message.");
  });
</script>

</body>
</html>
