<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Time Remaining</title>
  <style>
    :root{
      --bg:#0b0f14;
      --panel:#121926;
      --text:#e8eef6;
      --muted:#a9b6c7;
      --accent:#5dd6ff;
      --danger:#ff5d7a;
      --ok:#7CFF9B;
      --border:#223047;
    }
    *{box-sizing:border-box}
    body{
      margin:0;
      background:radial-gradient(1200px 700px at 30% 10%, #101a2b 0%, var(--bg) 55%);
      color:var(--text);
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Helvetica, Arial, "Apple Color Emoji","Segoe UI Emoji";
      line-height:1.4;
    }
    .wrap{
      max-width:860px;
      margin:0 auto;
      padding:28px 18px 48px;
    }
    header{
      padding:14px 0 22px;
    }
    h1{
      margin:0 0 8px;
      font-size:28px;
      letter-spacing:0.2px;
    }
    p.sub{
      margin:0;
      color:var(--muted);
      font-size:14px;
      max-width:70ch;
    }
    .grid{
      display:grid;
      grid-template-columns: 1fr;
      gap:14px;
    }
    @media (min-width: 820px){
      .grid{
        grid-template-columns: 1fr 1fr;
      }
    }
    .card{
      background:color-mix(in srgb, var(--panel) 92%, #000 8%);
      border:1px solid var(--border);
      border-radius:16px;
      padding:16px;
      box-shadow: 0 10px 30px rgba(0,0,0,.25);
    }
    .card h2{
      margin:0 0 10px;
      font-size:16px;
      color:var(--muted);
      font-weight:600;
    }
    label{
      display:block;
      margin:10px 0 6px;
      color:var(--muted);
      font-size:13px;
    }
    input{
      width:100%;
      padding:12px 12px;
      border-radius:12px;
      border:1px solid var(--border);
      background:#0c121c;
      color:var(--text);
      font-size:15px;
      outline:none;
    }
    input:focus{
      border-color: color-mix(in srgb, var(--accent) 65%, var(--border));
      box-shadow: 0 0 0 3px rgba(93,214,255,.12);
    }
    .row{
      display:flex;
      gap:10px;
      flex-wrap:wrap;
      margin-top:12px;
    }
    button{
      appearance:none;
      border:none;
      background:linear-gradient(180deg, color-mix(in srgb, var(--accent) 80%, #fff 0%) 0%, color-mix(in srgb, var(--accent) 55%, #000 45%) 100%);
      color:#021018;
      padding:12px 14px;
      border-radius:12px;
      font-weight:700;
      cursor:pointer;
      letter-spacing:.2px;
    }
    button.secondary{
      background:transparent;
      border:1px solid var(--border);
      color:var(--text);
      font-weight:600;
    }
    .note{
      margin-top:12px;
      color:var(--muted);
      font-size:12px;
    }
    .result{
      display:grid;
      grid-template-columns: repeat(2, 1fr);
      gap:10px;
      margin-top:12px;
    }
    @media (min-width: 520px){
      .result{
        grid-template-columns: repeat(4, 1fr);
      }
    }
    .tile{
      border:1px solid var(--border);
      border-radius:14px;
      padding:14px 12px;
      background:rgba(0,0,0,.18);
      min-height:84px;
    }
    .num{
      font-size:22px;
      font-weight:800;
      letter-spacing:.4px;
      margin:0 0 4px;
    }
    .lbl{
      margin:0;
      color:var(--muted);
      font-size:12px;
    }
    .meta{
      margin-top:14px;
      padding-top:12px;
      border-top:1px dashed var(--border);
      color:var(--muted);
      font-size:13px;
    }
    .status{
      margin-top:10px;
      padding:10px 12px;
      border-radius:12px;
      border:1px solid var(--border);
      background:rgba(0,0,0,.14);
      color:var(--muted);
      font-size:13px;
    }
    .status.ok{ border-color: color-mix(in srgb, var(--ok) 40%, var(--border)); color: color-mix(in srgb, var(--ok) 70%, var(--text)); }
    .status.bad{ border-color: color-mix(in srgb, var(--danger) 40%, var(--border)); color: color-mix(in srgb, var(--danger) 75%, var(--text)); }
    footer{
      margin-top:18px;
      color:var(--muted);
      font-size:12px;
    }
    code.inline{
      background:rgba(255,255,255,.06);
      border:1px solid rgba(255,255,255,.08);
      padding:2px 6px;
      border-radius:8px;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Countdown to an Estimated Death Date</h1>
      <p class="sub">
        This uses a simple assumption: <code class="inline">deathDate = birthDate + expectedAgeYears</code>.
        It is not a prediction, just arithmetic.
      </p>
    </header>

    <div class="grid">
      <section class="card" aria-labelledby="inputsTitle">
        <h2 id="inputsTitle">Inputs</h2>

        <label for="birth">Birthday</label>
        <input id="birth" type="date" />

        <label for="age">Expected age to die (years)</label>
        <input id="age" type="number" inputmode="decimal" min="0" step="0.1" placeholder="e.g., 80" />

        <div class="row">
          <button id="startBtn" type="button">Start</button>
          <button id="pauseBtn" class="secondary" type="button">Pause</button>
          <button id="resetBtn" class="secondary" type="button">Reset</button>
        </div>

        <div class="note">
          Assumption for fractional years: this adds years + fractional days using <code class="inline">365.2425</code> days/year (average solar year).
        </div>

        <div id="status" class="status" role="status" aria-live="polite">
          Enter inputs, then Start.
        </div>
      </section>

      <section class="card" aria-labelledby="outputTitle">
        <h2 id="outputTitle">Time Remaining</h2>

        <div class="result" aria-live="polite">
          <div class="tile">
            <p class="num" id="days">—</p>
            <p class="lbl">Days</p>
          </div>
          <div class="tile">
            <p class="num" id="hours">—</p>
            <p class="lbl">Hours</p>
          </div>
          <div class="tile">
            <p class="num" id="minutes">—</p>
            <p class="lbl">Minutes</p>
          </div>
          <div class="tile">
            <p class="num" id="seconds">—</p>
            <p class="lbl">Seconds</p>
          </div>
        </div>

        <div class="meta">
          <div>Estimated death date/time (local): <span id="deathLocal">—</span></div>
          <div>Now (local): <span id="nowLocal">—</span></div>
        </div>
      </section>
    </div>

    <footer>
      Implementation notes: Date input is interpreted in your browser’s local timezone. For a “midnight” birthday, this uses local midnight.
    </footer>
  </div>

  <script>
    (function(){
      const el = (id) => document.getElementById(id);

      const birthEl = el("birth");
      const ageEl   = el("age");

      const daysEl  = el("days");
      const hrsEl   = el("hours");
      const minEl   = el("minutes");
      const secEl   = el("seconds");

      const deathLocalEl = el("deathLocal");
      const nowLocalEl   = el("nowLocal");
      const statusEl     = el("status");

      const startBtn = el("startBtn");
      const pauseBtn = el("pauseBtn");
      const resetBtn = el("resetBtn");

      let timer = null;
      let deathDate = null;

      // Average length of a year (days) used for fractional years.
      const AVG_YEAR_DAYS = 365.2425;
      const MS = 1000;
      const MIN = 60 * MS;
      const HOUR = 60 * MIN;
      const DAY = 24 * HOUR;

      function setStatus(kind, text){
        statusEl.classList.remove("ok","bad");
        if(kind) statusEl.classList.add(kind);
        statusEl.textContent = text;
      }

      function parseBirthDate(){
        // HTML date input returns "YYYY-MM-DD" (no time zone info).
        const v = birthEl.value;
        if(!v) return null;

        const [y, m, d] = v.split("-").map(Number);
        if(!y || !m || !d) return null;

        // Local midnight
        const dt = new Date(y, m - 1, d, 0, 0, 0, 0);
        // Reject invalid dates (e.g., NaN)
        if(Number.isNaN(dt.getTime())) return null;
        return dt;
      }

      function computeDeathDate(birth, expectedAgeYears){
        if(expectedAgeYears < 0) return null;

        const wholeYears = Math.trunc(expectedAgeYears);
        const fracYears = expectedAgeYears - wholeYears;

        // First: add whole calendar years preserving month/day as much as JS allows.
        const d = new Date(birth.getTime());
        d.setFullYear(d.getFullYear() + wholeYears);

        // Then: add fractional years as average days.
        if(fracYears > 0){
          const addMs = fracYears * AVG_YEAR_DAYS * DAY;
          d.setTime(d.getTime() + addMs);
        }

        return d;
      }

      function formatLocal(dt){
        // Local readable format
        return dt.toLocaleString(undefined, {
          year: "numeric",
          month: "short",
          day: "2-digit",
          hour: "2-digit",
          minute: "2-digit",
          second: "2-digit"
        });
      }

      function pad2(n){
        return String(n).padStart(2, "0");
      }

      function render(){
        if(!deathDate){
          daysEl.textContent = hrsEl.textContent = minEl.textContent = secEl.textContent = "—";
          deathLocalEl.textContent = "—";
          nowLocalEl.textContent = "—";
          return;
        }

        const now = new Date();
        nowLocalEl.textContent = formatLocal(now);
        deathLocalEl.textContent = formatLocal(deathDate);

        const diff = deathDate.getTime() - now.getTime();

        if(diff <= 0){
          daysEl.textContent = "0";
          hrsEl.textContent  = "00";
          minEl.textContent  = "00";
          secEl.textContent  = "00";
          setStatus("bad", "Estimated death date is in the past (based on your inputs).");
          stopTimer();
          return;
        }

        const days = Math.floor(diff / DAY);
        const hours = Math.floor((diff % DAY) / HOUR);
        const minutes = Math.floor((diff % HOUR) / MIN);
        const seconds = Math.floor((diff % MIN) / MS);

        daysEl.textContent = String(days);
        hrsEl.textContent  = pad2(hours);
        minEl.textContent  = pad2(minutes);
        secEl.textContent  = pad2(seconds);

        setStatus("ok", "Running.");
      }

      function startTimer(){
        const birth = parseBirthDate();
        const age = Number(ageEl.value);

        if(!birth){
          setStatus("bad", "Invalid birthday.");
          return;
        }
        if(!Number.isFinite(age)){
          setStatus("bad", "Invalid expected age.");
          return;
        }
        if(age <= 0){
          setStatus("bad", "Expected age must be greater than 0.");
          return;
        }

        deathDate = computeDeathDate(birth, age);
        if(!deathDate || Number.isNaN(deathDate.getTime())){
          setStatus("bad", "Could not compute death date.");
          deathDate = null;
          render();
          return;
        }

        stopTimer();
        render();
        timer = setInterval(render, 250); // smoother seconds display
      }

      function stopTimer(){
        if(timer){
          clearInterval(timer);
          timer = null;
        }
      }

      function reset(){
        stopTimer();
        deathDate = null;
        birthEl.value = "";
        ageEl.value = "";
        setStatus(null, "Enter inputs, then Start.");
        render();
      }

      startBtn.addEventListener("click", startTimer);
      pauseBtn.addEventListener("click", () => {
        if(timer){
          stopTimer();
          setStatus(null, "Paused.");
        }else{
          if(deathDate){
            timer = setInterval(render, 250);
            setStatus("ok", "Running.");
          }
        }
      });
      resetBtn.addEventListener("click", reset);

      // Optional: update live if user edits inputs while running
      birthEl.addEventListener("change", () => { if(timer) startTimer(); });
      ageEl.addEventListener("input",  () => { if(timer) startTimer(); });

      render();
    })();
  </script>
</body>
</html>