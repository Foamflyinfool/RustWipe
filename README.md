# RustWipe
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Rust Force Wipe Tracker</title>
  <style>
    /* Global Aesthetic Reset mapping to rustforcewipe.com */
    :root {
      --bg-color: #0d0e11;
      --card-bg: #14161d;
      --border-color: #242836;
      --accent-color: #ce422b; /* Rust Orange */
      --text-main: #f3f4f6;
      --text-muted: #6b7280;
      --glow-color: rgba(206, 66, 43, 0.15);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      background-color: var(--bg-color);
      color: var(--text-main);
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      padding: 20px;
    }

    .container {
      width: 100%;
      max-width: 650px;
      text-align: center;
    }

    /* Top Branding Header */
    .header {
      margin-bottom: 2rem;
    }

    .header h1 {
      font-size: 1.25rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--text-muted);
      font-weight: 600;
    }

    .header span {
      color: var(--accent-color);
    }

    /* Main Centerpiece Dashboard Card */
    .wipe-card {
      background-color: var(--card-bg);
      border: 1px solid var(--border-color);
      border-radius: 12px;
      padding: 2.5rem 2rem;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5), 0 0 40px var(--glow-color);
      margin-bottom: 1.5rem;
    }

    .card-label {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--accent-color);
      font-weight: 700;
      margin-bottom: 0.5rem;
    }

    .card-title {
      font-size: 1.75rem;
      font-weight: 700;
      margin-bottom: 2rem;
      letter-spacing: -0.02em;
    }

    /* The Ticking Countdown Clock Grid */
    .countdown-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1rem;
      margin-bottom: 2.5rem;
    }

    .time-block {
      background: rgba(0, 0, 0, 0.25);
      border: 1px solid var(--border-color);
      border-radius: 8px;
      padding: 1rem 0.5rem;
    }

    .time-value {
      font-family: "Courier New", Courier, monospace;
      font-size: 2.5rem;
      font-weight: bold;
      color: var(--text-main);
      line-height: 1;
      margin-bottom: 0.25rem;
    }

    .time-label {
      font-size: 0.7rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      color: var(--text-muted);
    }

    /* Bottom Information Section */
    .info-divider {
      height: 1px;
      background: var(--border-color);
      margin-bottom: 1.5rem;
    }

    .local-info-box {
      text-align: center;
    }

    .local-info-title {
      font-size: 0.75rem;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--text-muted);
      margin-bottom: 0.5rem;
    }

    .local-time-display {
      font-size: 1.1rem;
      font-weight: 600;
      color: var(--text-main);
    }

    .timezone-tag {
      display: inline-block;
      margin-top: 0.5rem;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid var(--border-color);
      padding: 0.25rem 0.75rem;
      border-radius: 20px;
      font-size: 0.75rem;
      color: var(--text-muted);
    }

    /* Footer Credits */
    .footer {
      font-size: 0.75rem;
      color: var(--text-muted);
      line-height: 1.5;
    }

    .footer p {
      margin-top: 0.5rem;
    }

    @media (max-width: 480px) {
      .time-value { font-size: 1.8rem; }
      .card-title { font-size: 1.4rem; }
    }
  </style>
</head>
<body>

  <div class="container">
    <!-- Branding Header -->
    <div class="header">
      <h1>Global Force <span>Wipe</span></h1>
    </div>

    <!-- Main Widget Module -->
    <div class="wipe-card">
      <div class="card-label">Monthly Update Schedule</div>
      <div class="card-title">Next Global Force Wipe</div>

      <!-- Ticking Digital Segment Grid -->
      <div class="countdown-grid">
        <div class="time-block">
          <div class="time-value" id="days">00</div>
          <div class="time-label">Days</div>
        </div>
        <div class="time-block">
          <div class="time-value" id="hours">00</div>
          <div class="time-label">Hours</div>
        </div>
        <div class="time-block">
          <div class="time-value" id="minutes">00</div>
          <div class="time-label">Minutes</div>
        </div>
        <div class="time-block">
          <div class="time-value" id="seconds">00</div>
          <div class="time-label">Seconds</div>
        </div>
      </div>

      <div class="info-divider"></div>

      <!-- Localized Time Engine Output Container -->
      <div class="local-info-box">
        <div class="local-info-title">Wipe In Your Timezone</div>
        <div class="local-time-display" id="local-wipe-time">Calculating...</div>
        <div class="timezone-tag" id="user-timezone">Detecting system zone...</div>
      </div>
    </div>

    <!-- Supplementary Informative Footnotes -->
    <div class="footer">
      <p>All servers reset globally on the first Thursday of each month.</p>
      <p>Target time calculation relies completely on immutable UTC offsets and parses dynamically into your local hardware clock season rules (DST compatible).</p>
    </div>
  </div>

  <script>
    /**
     * Algorithmic Engine targeting Facepunch's standard update window.
     * Calculates the first Thursday of a given month safely in strict UTC.
     */
    function getNextForceWipe(now = new Date()) {
      function calculateFirstThursday(year, month) {
        // Facepunch standard force patches go live at 19:00 UTC globally
        // 19:00 UTC is exactly 2:00 PM Eastern / 1:00 PM Central / 11:00 AM Pacific
        let date = new Date(Date.UTC(year, month, 1, 19, 0, 0));
        let dayOfWeek = date.getUTCDay(); 
        let offsetToThursday = (4 - dayOfWeek + 7) % 7;
        date.setUTCDate(date.getUTCDate() + offsetToThursday);
        return date;
      }

      let year = now.getUTCFullYear();
      let month = now.getUTCMonth();

      let targetWipe = calculateFirstThursday(year, month);

      // If the patch target has passed for the month, calculate the next consecutive month
      if (now.getTime() >= targetWipe.getTime()) {
        month += 1;
        if (month > 11) {
          month = 0;
          year += 1;
        }
        targetWipe = calculateFirstThursday(year, month);
      }

      return targetWipe;
    }

    // Capture static target based on launch time
    const targetWipeDate = getNextForceWipe();

    // Populate localized layout labels using native Intl parameters (accounts for local DST naming)
    try {
      const userZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
      document.getElementById('user-timezone').textContent = userZone;
      
      const formatOptions = { 
        weekday: 'short', 
        day: 'numeric', 
        month: 'short', 
        hour: 'numeric', 
        minute: '2-digit',
        timeZoneName: 'short'
      };
      document.getElementById('local-wipe-time').textContent = targetWipeDate.toLocaleString(undefined, formatOptions);
    } catch (e) {
      document.getElementById('local-wipe-time').textContent = targetWipeDate.toUTCString();
    }

    // Standard pad logic for unified string formatting
    function padValue(num) {
      return num.toString().padStart(2, '0');
    }

    // Ticking engine runner
    function updateCountdown() {
      const currentTime = new Date().getTime();
      const distance = targetWipeDate.getTime() - currentTime;

      if (distance < 0) {
        document.getElementById('days').textContent = "00";
        document.getElementById('hours').textContent = "00";
        document.getElementById('minutes').textContent = "00";
        document.getElementById('seconds').textContent = "00";
        return;
      }

      const days = Math.floor(distance / (1000 * 60 * 60 * 24));
      const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
      const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
      const seconds = Math.floor((distance % (1000 * 60)) / 1000);

      document.getElementById('days').textContent = padValue(days);
      document.getElementById('hours').textContent = padValue(hours);
      document.getElementById('minutes').textContent = padValue(minutes);
      document.getElementById('seconds').textContent = padValue(seconds);
    }

    // Start running loop immediately
    setInterval(updateCountdown, 1000);
    updateCountdown();
  </script>
</body>
</html>
