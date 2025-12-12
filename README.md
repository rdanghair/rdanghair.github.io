<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Barber Booking — Book Now</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--accent:#ffd166;--muted:#9ca3af}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,system-ui,Arial;background:linear-gradient(180deg,#071226 0%, #071a2b 100%);color:#e6eef8;min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px}
    .wrap{width:100%;max-width:980px}
    header{display:flex;align-items:center;gap:16px;margin-bottom:18px}
    .logo{background:var(--accent);color:#081226;padding:10px 12px;border-radius:10px;font-weight:700}
    h1{margin:0;font-size:22px}
    .grid{display:grid;grid-template-columns:1fr 420px;gap:20px}
    .card{background:rgba(255,255,255,0.03);padding:18px;border-radius:12px;box-shadow:0 8px 24px rgba(2,6,23,0.6)}
    label{display:block;font-size:13px;color:var(--muted);margin-bottom:8px}
    input,select,textarea{width:100%;padding:12px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;font-size:15px}
    .row{display:flex;gap:12px}
    .btn{display:inline-block;padding:12px 16px;border-radius:10px;background:var(--accent);color:#071226;border:none;font-weight:700;cursor:pointer}
    .muted{color:var(--muted);font-size:13px}
    .slot{padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);cursor:pointer}
    .slot.selected{outline:2px solid rgba(255,209,102,0.18);background:rgba(255,209,102,0.06)}
    .success{padding:12px;border-radius:8px;background:linear-gradient(90deg,#083344,#0a2633);border:1px solid rgba(255,255,255,0.03)}
    footer{margin-top:14px;color:var(--muted);font-size:13px;text-align:center}
    @media(max-width:880px){.grid{grid-template-columns:1fr;}.logo{display:none}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">BARB</div>
      <div>
        <h1>Book your haircut</h1>
        <div class="muted">Fast, simple booking — confirmations by email & SMS</div>
      </div>
    </header>

    <div class="grid">
      <!-- Form -->
      <div class="card">
        <label for="name">Full name</label>
        <input id="name" placeholder="e.g. Marcus Lee" />

        <label for="phone">Phone number</label>
        <input id="phone" placeholder="+1 416 555 1234" />

        <label for="email">Email</label>
        <input id="email" placeholder="you@example.com" />

        <label for="service">Service</label>
        <select id="service">
          <option value="Haircut">Haircut — $30 (30 min)</option>
          <option value="Fade">Fade — $35 (40 min)</option>
          <option value="Line Up">Line Up — $15 (15 min)</option>
          <option value="Beard">Beard Trim — $20 (20 min)</option>
          <option value="Haircut+Beard">Haircut + Beard — $45 (50 min)</option>
        </select>

        <label for="date">Date</label>
        <input id="date" type="date" />

        <label>Available times</label>
        <div id="slots" style="display:grid;grid-template-columns:repeat(3,1fr);gap:8px"></div>

        <label for="notes">Notes (optional)</label>
        <textarea id="notes" rows="3" placeholder="e.g. prefer clippers, skin fade"></textarea>

        <div style="margin-top:12px;display:flex;gap:8px;align-items:center">
          <button class="btn" id="bookBtn">Book Appointment</button>
          <div id="status" class="muted">We will create a Google Calendar event & send confirmations.</div>
        </div>
      </div>

      <!-- Right column: Preview & saved appointments link -->
      <div>
        <div class="card">
          <h3 style="margin-top:0">Appointment preview</h3>
          <div id="preview" class="muted">No appointment selected yet.</div>
          <div style="height:12px"></div>
          <div id="result" style="display:none" class="success"></div>
        </div>

        <div style="height:12px"></div>
        <div class="card">
          <h3 style="margin-top:0">Saved appointments</h3>
          <p class="muted">This app saves bookings to a Google Sheet. You can view all bookings there (owner only).</p>
          <a id="sheetLink" class="muted" href="#">Open Bookings Sheet (owner)</a>
        </div>
      </div>
    </div>

    <footer>Need customizations? Reply and I’ll tweak times, services, or add payments.</footer>
  </div>

  <script>
    // ---------- CONFIG: set this to your deployed Google Apps Script Web App URL ----------
    // e.g. https://script.google.com/macros/s/AKfycbx.../exec
    const SCRIPT_URL = 'REPLACE_WITH_YOUR_WEB_APP_URL';

    // Utility: format preview text
    function getServiceDuration(service) {
      switch(service){
        case 'Haircut': return 30;
        case 'Fade': return 40;
        case 'Line Up': return 15;
        case 'Beard': return 20;
        case 'Haircut+Beard': return 50;
        default: return 30;
      }
    }

    // Generate time slots for a day (9:00 - 18:00) every 30 minutes
    function generateSlots() {
      const slots = [];
      for(let h=9; h<18; h++){
        slots.push(`${String(h).padStart(2,'0')}:00`);
        slots.push(`${String(h).padStart(2,'0')}:30`);
      }
      return slots;
    }

    const slotsContainer = document.getElementById('slots');
    const allSlots = generateSlots();
    allSlots.forEach(s=>{
      const el = document.createElement('div');
      el.className='slot';
      el.textContent = s;
      el.onclick = ()=>{
        document.querySelectorAll('.slot').forEach(x=>x.classList.remove('selected'));
        el.classList.add('selected');
        updatePreview();
      };
      slotsContainer.appendChild(el);
    });

    document.getElementById('date').addEventListener('change', updatePreview);
    document.getElementById('service').addEventListener('change', updatePreview);
    ['name','phone','email','notes'].forEach(id=>document.getElementById(id).addEventListener('input', updatePreview));

    function updatePreview(){
      const name = document.getElementById('name').value || '[name]';
      const phone = document.getElementById('phone').value || '[phone]';
      const email = document.getElementById('email').value || '[email]';
      const date = document.getElementById('date').value || '[date]';
      const slot = document.querySelector('.slot.selected')?.textContent || '[time]';
      const service = document.getElementById('service').value;
      const dur = getServiceDuration(service);
      document.getElementById('preview').innerHTML = `
        <strong>${service}</strong><br>
        ${date} @ ${slot} — approx ${dur} min<br>
        ${name}<br>
        ${phone} • ${email}
      `;
    }

    // Book button sends data to Apps Script
    document.getElementById('bookBtn').addEventListener('click', async ()=>{
      const name = document.getElementById('name').value.trim();
      const phone = document.getElementById('phone').value.trim();
      const email = document.getElementById('email').value.trim();
      const date = document.getElementById('date').value;
      const slot = document.querySelector('.slot.selected')?.textContent;
      const service = document.getElementById('service').value;
      const notes = document.getElementById('notes').value.trim();

      if(!name || !phone || !email || !date || !slot){
        alert('Please fill name, phone, email, date and pick a time slot.');
        return;
      }

      const payload = {name, phone, email, date, time:slot, service, notes};

      if(!SCRIPT_URL || SCRIPT_URL.includes('REPLACE_WITH')){
        alert('You must set the SCRIPT_URL variable in the HTML (follow the setup steps in the project file).');
        return;
      }

      document.getElementById('bookBtn').disabled=true; document.getElementById('bookBtn').textContent='Booking...';

      try{
        const resp = await fetch(SCRIPT_URL, {method:'POST',headers:{'Content-Type':'application/json'},body:JSON.stringify(payload)});
        const data = await resp.json();
        if(data.status==='OK'){
          document.getElementById('result').style.display='block';
          document.getElementById('result').innerHTML = `Booked! Confirmation sent. Appointment ID: <strong>${data.id}</strong>`;
        } else {
          throw new Error(data.message||'Server error');
        }
      }catch(err){
        alert('Booking failed: '+err.message);
      }finally{
        document.getElementById('bookBtn').disabled=false; document.getElementById('bookBtn').textContent='Book Appointment';
      }
    });
  </script>

  <!--
    ---------------------- BACKEND (Google Apps Script) ----------------------
    Paste the following code into a new Google Apps Script project (script.google.com):

    1) Create a Google Sheet with columns in row 1:
       Timestamp | Name | Phone | Email | Service | Date | Time | Notes | CalendarEventId | BookingID

    2) In Apps Script, add the code below. Fill the constants:
       - SHEET_ID: your Google Sheet ID (from the URL)
       - CALENDAR_ID: your Google Calendar ID (usually primary or a calendar email)
       - TWILIO_SID, TWILIO_TOKEN, TWILIO_PHONE: if you want SMS confirmations (optional)

    3) Deploy -> New deployment -> type 'Web app'
       - Execute as: Me
       - Who has access: Anyone (or Anyone with Google account)
       - Copy the Web App URL and put it in the HTML's SCRIPT_URL constant above.

    4) Authorize the script when prompted.

    5) Optional: set a trigger to run cleanup or reminders.

    ---------------------- APPS SCRIPT CODE ----------------------
  -->

  <!-- Apps Script Server Code (paste into Code.gs in script.google.com) -->
  <script type="text/plain" id="apps-script">
  // ---------- CONFIG ----------
  const SHEET_ID = 'REPLACE_WITH_YOUR_SHEET_ID';
  const SHEET_NAME = 'Bookings';
  const CALENDAR_ID = 'primary'; // or replace with your calendar's email

  // Twilio (optional)
  const TWILIO_SID = 'REPLACE_WITH_TWILIO_SID';
  const TWILIO_TOKEN = 'REPLACE_WITH_TWILIO_TOKEN';
  const TWILIO_PHONE = 'REPLACE_WITH_TWILIO_PHONE'; // e.g. +14165551234

  function doPost(e){
    try{
      const payload = JSON.parse(e.postData.contents);
      const timestamp = new Date();
      const name = payload.name || '';
      const phone = payload.phone || '';
      const email = payload.email || '';
      const service = payload.service || '';
      const date = payload.date || '';
      const time = payload.time || '';
      const notes = payload.notes || '';

      // Booking ID (unique)
      const bookingId = 'BKG-'+Math.random().toString(36).slice(2,9).toUpperCase();

      // Append to sheet
      const ss = SpreadsheetApp.openById(SHEET_ID);
      let sheet = ss.getSheetByName(SHEET_NAME);
      if(!sheet){
        sheet = ss.insertSheet(SHEET_NAME);
        sheet.appendRow(['Timestamp','Name','Phone','Email','Service','Date','Time','Notes','CalendarEventId','BookingID']);
      }
      sheet.appendRow([timestamp,name,phone,email,service,date,time,notes,'',bookingId]);

      // Create Calendar event (combine date + time)
      const eventStart = new Date(date + ' ' + time);
      const durationMinutes = getServiceDuration(service);
      const eventEnd = new Date(eventStart.getTime() + durationMinutes*60000);

      const calendar = CalendarApp.getCalendarById(CALENDAR_ID);
      const event = calendar.createEvent(service + ' — ' + name, eventStart, eventEnd, {description: notes + '
BookingID: '+bookingId});
      const eventId = event.getId();

      // Update sheet with Calendar event id
      // Find the last row (we just appended)
      const lastRow = sheet.getLastRow();
      sheet.getRange(lastRow,9).setValue(eventId);
      sheet.getRange(lastRow,10).setValue(bookingId);

      // Send confirmation email
      const subject = 'Booking confirmation — ' + service + ' on ' + date + ' at ' + time;
      const body = `Hi ${name},

Thanks — your booking is confirmed.

Service: ${service}
Date & Time: ${date} ${time}
Booking ID: ${bookingId}

See you soon!`;
      MailApp.sendEmail(email, subject, body);

      // Send SMS via Twilio (optional)
      if(TWILIO_SID && TWILIO_TOKEN && TWILIO_PHONE && phone){
        try{
          const twUrl = 'https://api.twilio.com/2010-04-01/Accounts/' + TWILIO_SID + '/Messages.json';
          const payloadTw = {
            To: phone,
            From: TWILIO_PHONE,
            Body: `Booking confirmed: ${service} on ${date} at ${time}. ID: ${bookingId}`
          };
          const options = {
            method: 'post',
            payload: payloadTw,
            muteHttpExceptions: true,
            headers: {
              Authorization: 'Basic ' + Utilities.base64Encode(TWILIO_SID + ':' + TWILIO_TOKEN)
            }
          };
          UrlFetchApp.fetch(twUrl, options);
        }catch(twErr){
          // ignore SMS errors but log
          console.error('SMS error', twErr);
        }
      }

      return ContentService.createTextOutput(JSON.stringify({status:'OK',id:bookingId})).setMimeType(ContentService.MimeType.JSON);

    }catch(err){
      return ContentService.createTextOutput(JSON.stringify({status:'ERROR',message:err.message})).setMimeType(ContentService.MimeType.JSON);
    }
  }

  function getServiceDuration(service){
    switch(service){
      case 'Haircut': return 30;
      case 'Fade': return 40;
      case 'Line Up': return 15;
      case 'Beard': return 20;
      case 'Haircut+Beard': return 50;
      default: return 30;
    }
  }
  </script>

</body>
</html>
