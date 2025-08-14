<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Ticket Verification - Kenya vs Zambia</title>
  <script src="https://unpkg.com/html5-qrcode" type="text/javascript"></script>
  <style>
    body { font-family: Arial, sans-serif; background: #f8fafc; margin: 0; color: #0f172a; }
    header, footer { background: white; padding: 1rem; border-bottom: 1px solid #cbd5e1; }
    main { max-width: 900px; margin: auto; padding: 1rem; }
    .card { background: white; border: 1px solid #cbd5e1; border-radius: 10px; padding: 1rem; margin-bottom: 1rem; }
    .status { display: flex; align-items: center; gap: .5rem; font-weight: bold; color: green; }
    .event-details, .ticket-details { display: flex; flex-direction: column; gap: .3rem; margin-top: .5rem; }
    .scan-history { font-size: .9rem; color: #475569; }
    #reader { width: 100%; max-width: 400px; margin: auto; }
  </style>
</head>
<body>
  <header>
    <h1>Ticket Verification</h1>
  </header>
  <main>
    <div class="card">
      <div class="status" id="ticketStatus">✅ Ticket Verified</div>
      <p>Hi, <strong id="ticketName">James Baraza</strong></p>
      <h2>You're all set for the match!</h2>
      <div class="event-details">
        <div>🏆 Event: Kenya vs Zambia</div>
        <div>📅 Date: Sunday, August 17, 2025</div>
        <div>🕒 Time: 3:00 PM</div>
        <div>📍 Venue: Moi International Sports Centre, Nairobi</div>
      </div>
      <div class="ticket-details">
        <div>🎟 Ticket Code: <strong id="ticketCode">KZ2025JB</strong></div>
        <div>Status: <span id="ticketValidity" style="color:green; font-weight:bold;">Valid</span></div>
      </div>
    </div>

    <div class="card">
      <h3>QR Code Scanner</h3>
      <div id="reader"></div>
      <p id="scanResult"></p>
    </div>

    <div class="card">
      <h3>Scan History</h3>
      <div class="scan-history" id="scanHistory">No scans yet.</div>
    </div>
  </main>
  <footer>
    <p>© 2025 Ticket Verify. All rights reserved.</p>
  </footer>

  <script>
    const mockDatabase = {
      'KZ2025JB': { name: 'James Baraza', event: 'Kenya vs Zambia', used: false }
    };

    function onScanSuccess(decodedText) {
      const ticket = mockDatabase[decodedText];
      const statusEl = document.getElementById('ticketStatus');
      const nameEl = document.getElementById('ticketName');
      const codeEl = document.getElementById('ticketCode');
      const validityEl = document.getElementById('ticketValidity');
      const scanHistory = document.getElementById('scanHistory');
      const scanResult = document.getElementById('scanResult');

      if (!ticket) {
        scanResult.textContent = '❌ Ticket not found';
        scanResult.style.color = 'red';
        return;
      }

      nameEl.textContent = ticket.name;
      codeEl.textContent = decodedText;

      if (ticket.used) {
        statusEl.textContent = '⚠️ Ticket already used';
        statusEl.style.color = 'orange';
        validityEl.textContent = 'Used';
        validityEl.style.color = 'orange';
      } else {
        statusEl.textContent = '✅ Ticket Verified';
        statusEl.style.color = 'green';
        validityEl.textContent = 'Valid';
        validityEl.style.color = 'green';
        ticket.used = true;
        scanHistory.innerHTML += `<div>${new Date().toLocaleString()} - ${ticket.name} - Verified via QR</div>`;
      }
    }

    const html5QrCode = new Html5Qrcode("reader");
    Html5Qrcode.getCameras().then(devices => {
      if (devices && devices.length) {
        html5QrCode.start(
          { facingMode: "environment" },
          { fps: 10, qrbox: 250 },
          onScanSuccess
        );
      }
    }).catch(err => console.error(err));
  </script>
</body>
</html>
