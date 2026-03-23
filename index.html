<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>System Scan</title>
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap" rel="stylesheet">

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      background: #000;
      color: #ff2222;
      font-family: 'Share Tech Mono', monospace;
      min-height: 100vh;
      overflow: hidden;
      cursor: none;
    }

    #bg-video,
    #yt-bg {
      position: fixed;
      inset: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      opacity: 0.18;
      z-index: 0;
      pointer-events: none;
    }

    #yt-bg {
      top: 50%;
      left: 50%;
      width: 177.78vh;
      height: 100vh;
      min-width: 100vw;
      min-height: 56.25vw;
      transform: translate(-50%, -50%);
      border: none;
    }

    body::after {
      content: "";
      position: fixed;
      inset: 0;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0, 0, 0, 0.18) 2px,
        rgba(0, 0, 0, 0.18) 4px
      );
      pointer-events: none;
      z-index: 50;
    }

    #content {
      position: relative;
      z-index: 10;
      padding: 32px 40px;
      width: 100%;
      max-width: 1200px;
    }

    .line {
      opacity: 0;
      transform: translateY(4px);
      transition: opacity 0.7s ease, transform 0.7s ease;
      font-size: clamp(13px, 1.6vw, 16px);
      line-height: 1.95;
      white-space: nowrap;
    }

    .line.show {
      opacity: 1;
      transform: translateY(0);
    }

    .label {
      color: #550000;
      margin-right: 1ch;
    }

    .value {
      color: #ff2a2a;
    }

    .value.green {
      color: #00ff66;
    }

    .value.amber {
      color: #ffaa00;
    }

    .value.dim {
      color: #555;
    }

    #cursor-follow {
      position: fixed;
      pointer-events: none;
      z-index: 200;
      font-size: 11px;
      color: #ff0000;
      opacity: 0.6;
      white-space: nowrap;
      transform: translate(10px, -50%);
      font-family: 'Share Tech Mono', monospace;
    }
  </style>
</head>
<body>

  <!-- Use ONE background option only -->

  <!-- Local video -->
  <!--
  <video id="bg-video" autoplay loop muted playsinline>
    <source src="video.mp4" type="video/mp4">
  </video>
  -->

  <!-- YouTube background -->
  <iframe
    id="yt-bg"
    src="https://www.youtube.com/embed/bImHKGJqzFw?autoplay=1&mute=1&loop=1&playlist=bImHKGJqzFw&controls=0&rel=0&modestbranding=1&iv_load_policy=3&disablekb=1"
    allow="autoplay; encrypted-media"
    allowfullscreen>
  </iframe>

  <!-- Background music -->
  <audio id="music" loop>
    <source src="https://files.catbox.moe/kmugk9.mp3" type="audio/mpeg">
  </audio>

  <div id="cursor-follow"></div>
  <div id="content"></div>

  <script>
    const content = document.getElementById("content");
    const cursor = document.getElementById("cursor-follow");
    const music = document.getElementById("music");

    const PAD = 20;
    const revealDelay = 220;

    const valuesMap = new Map();
    const rowMap = new Map();

    document.addEventListener("mousemove", (e) => {
      cursor.style.left = e.clientX + "px";
      cursor.style.top = e.clientY + "px";
      cursor.textContent = `${e.clientX}, ${e.clientY}`;
    });

    music.volume = 0.45;
    const tryPlayMusic = () => music.play().catch(() => {});
    tryPlayMusic();
    ["click", "keydown", "touchstart"].forEach((ev) => {
      document.addEventListener(ev, tryPlayMusic, { once: true });
    });

    function safe(value, fallback = "UNAVAILABLE") {
      if (value === undefined || value === null || value === "") return fallback;
      return String(value);
    }

    function utcOffsetString() {
      const offset = -new Date().getTimezoneOffset();
      const sign = offset >= 0 ? "+" : "-";
      const abs = Math.abs(offset);
      const hours = String(Math.floor(abs / 60)).padStart(2, "0");
      const mins = String(abs % 60).padStart(2, "0");
      return `UTC${sign}${hours}:${mins}`;
    }

    function detectOS(ua) {
      if (/Windows NT 10|Windows NT 11/.test(ua)) return "Windows 10 / 11";
      if (/Windows NT 6.3/.test(ua)) return "Windows 8.1";
      if (/Windows NT 6.1/.test(ua)) return "Windows 7";
      if (/Mac OS X/.test(ua)) {
        const match = ua.match(/Mac OS X ([0-9_]+)/);
        return "macOS " + (match ? match[1].replace(/_/g, ".") : "");
      }
      if (/Android/.test(ua)) {
        const match = ua.match(/Android ([0-9.]+)/);
        return "Android " + (match ? match[1] : "");
      }
      if (/iPhone/.test(ua)) return "iPhone / iOS";
      if (/iPad/.test(ua)) return "iPad / iPadOS";
      if (/CrOS/.test(ua)) return "Chrome OS";
      if (/Linux/.test(ua)) return "Linux";
      return "UNKNOWN";
    }

    function detectBrowser(ua) {
      if (/Edg\//.test(ua)) return "Microsoft Edge " + ((ua.match(/Edg\/([0-9]+)/) || ["", ""])[1]);
      if (/OPR\//.test(ua)) return "Opera " + ((ua.match(/OPR\/([0-9]+)/) || ["", ""])[1]);
      if (/Firefox\//.test(ua)) return "Mozilla Firefox " + ((ua.match(/Firefox\/([0-9]+)/) || ["", ""])[1]);
      if (/Chrome\//.test(ua)) return "Google Chrome " + ((ua.match(/Chrome\/([0-9]+)/) || ["", ""])[1]);
      if (/Safari\//.test(ua)) return "Safari";
      return "UNKNOWN";
    }

    function getGPU() {
      try {
        const canvas = document.createElement("canvas");
        const gl = canvas.getContext("webgl") || canvas.getContext("experimental-webgl");
        if (!gl) return "UNAVAILABLE";

        const ext = gl.getExtension("WEBGL_debug_renderer_info");
        if (!ext) return "UNAVAILABLE";

        return gl.getParameter(ext.UNMASKED_RENDERER_WEBGL) || "UNAVAILABLE";
      } catch {
        return "UNAVAILABLE";
      }
    }

    function getVendor() {
      try {
        const canvas = document.createElement("canvas");
        const gl = canvas.getContext("webgl") || canvas.getContext("experimental-webgl");
        if (!gl) return "UNAVAILABLE";

        const ext = gl.getExtension("WEBGL_debug_renderer_info");
        if (!ext) return "UNAVAILABLE";

        return gl.getParameter(ext.UNMASKED_VENDOR_WEBGL) || "UNAVAILABLE";
      } catch {
        return "UNAVAILABLE";
      }
    }

    function addRow(key, label, value, cls = "") {
      valuesMap.set(key, { label, value, cls });
    }

    function updateRow(key, newValue, newClass = null) {
      const current = valuesMap.get(key);
      if (!current) return;

      current.value = safe(newValue);
      if (newClass !== null) current.cls = newClass;

      const row = rowMap.get(key);
      if (!row) return;

      const valueEl = row.querySelector(".value");
      valueEl.textContent = current.value;
      valueEl.className = "value" + (current.cls ? ` ${current.cls}` : "");
    }

    function renderRowsSlowly() {
      let index = 0;

      for (const [key, item] of valuesMap.entries()) {
        const line = document.createElement("div");
        line.className = "line";

        const paddedLabel = item.label.padEnd(PAD, "\u00A0");
        line.innerHTML = `
          <span class="label">${paddedLabel}</span>
          <span class="value ${item.cls || ""}">${safe(item.value)}</span>
        `;

        content.appendChild(line);
        rowMap.set(key, line);

        setTimeout(() => {
          line.classList.add("show");
        }, index * revealDelay);

        index++;
      }
    }

    async function fetchGeoData() {
      const endpoints = [
        "https://ipapi.co/json/",
        "https://ipwho.is/"
      ];

      for (const url of endpoints) {
        try {
          const res = await fetch(url);
          const data = await res.json();

          if (url.includes("ipapi.co") && data.ip) {
            return {
              ip: data.ip,
              isp: data.org,
              city: data.city,
              region: data.region,
              country: data.country_name,
              countryCode: data.country,
              coords: `${safe(data.latitude, "?")}, ${safe(data.longitude, "?")}`,
              timezone: data.timezone
            };
          }

          if (url.includes("ipwho.is") && data.success) {
            return {
              ip: data.ip,
              isp: data.connection?.isp || data.connection?.org,
              city: data.city,
              region: data.region,
              country: data.country,
              countryCode: data.country_code,
              coords: `${safe(data.latitude, "?")}, ${safe(data.longitude, "?")}`,
              timezone: data.timezone?.id
            };
          }
        } catch (err) {}
      }

      return null;
    }

    const ua = navigator.userAgent;
    const nav = navigator;
    const scr = screen;
    const conn = navigator.connection || navigator.mozConnection || navigator.webkitConnection;

    addRow("datetime",  "DATE / TIME", new Date().toLocaleString("en-US"), "green");
    addRow("timezone",  "TIME ZONE", `${utcOffsetString()} / ${Intl.DateTimeFormat().resolvedOptions().timeZone}`, "amber");

    addRow("ip",        "IP ADDRESS", "LOADING...");
    addRow("isp",       "ISP", "LOADING...");
    addRow("city",      "CITY", "LOADING...");
    addRow("region",    "REGION", "LOADING...");
    addRow("country",   "COUNTRY", "LOADING...");
    addRow("coords",    "COORDINATES", "LOADING...");

    addRow("os",        "OPERATING SYSTEM", detectOS(ua), "amber");
    addRow("browser",   "BROWSER", detectBrowser(ua));
    addRow("platform",  "PLATFORM", safe(nav.platform));
    addRow("screen",    "SCREEN", `${scr.width} × ${scr.height}`);
    addRow("viewport",  "VIEWPORT", `${window.innerWidth} × ${window.innerHeight}`);
    addRow("ratio",     "PIXEL RATIO", `${window.devicePixelRatio || 1}x`);
    addRow("language",  "LANGUAGE", safe(nav.language));
    addRow("memory",    "RAM", nav.deviceMemory ? `${nav.deviceMemory} GB` : "UNAVAILABLE");
    addRow("cpu",       "CPU CORES", safe(nav.hardwareConcurrency));
    addRow("gpu",       "GPU", getGPU(), getGPU() === "UNAVAILABLE" ? "dim" : "");
    addRow("vendor",    "GPU VENDOR", getVendor(), getVendor() === "UNAVAILABLE" ? "dim" : "");
    addRow("cookies",   "COOKIES", nav.cookieEnabled ? "ENABLED" : "DISABLED");
    addRow("online",    "ONLINE", nav.onLine ? "YES" : "NO");
    addRow("touch",     "TOUCH POINTS", safe(nav.maxTouchPoints, "0"));

    if (conn) {
      addRow(
        "connection",
        "CONNECTION",
        `${safe(conn.effectiveType, "?").toUpperCase()}${conn.downlink ? ` / ${conn.downlink} Mbps` : ""}${conn.rtt ? ` / ${conn.rtt} ms` : ""}`
      );
    } else {
      addRow("connection", "CONNECTION", "UNAVAILABLE", "dim");
    }

    addRow("battery", "BATTERY", "LOADING...");

    renderRowsSlowly();

    setInterval(() => {
      updateRow("datetime", new Date().toLocaleString("en-US"));
    }, 1000);

    if (navigator.getBattery) {
      navigator.getBattery()
        .then((battery) => {
          function refreshBattery() {
            const percent = Math.round(battery.level * 100);
            const status = battery.charging ? "CHARGING" : "DISCHARGING";
            updateRow("battery", `${percent}% / ${status}`, percent <= 20 ? "" : "green");
          }

          refreshBattery();
          battery.addEventListener("chargingchange", refreshBattery);
          battery.addEventListener("levelchange", refreshBattery);
        })
        .catch(() => {
          updateRow("battery", "UNAVAILABLE", "dim");
        });
    } else {
      updateRow("battery", "UNAVAILABLE", "dim");
    }

    fetchGeoData().then((geo) => {
      if (!geo) {
        updateRow("ip", "UNAVAILABLE", "dim");
        updateRow("isp", "UNAVAILABLE", "dim");
        updateRow("city", "UNAVAILABLE", "dim");
        updateRow("region", "UNAVAILABLE", "dim");
        updateRow("country", "UNAVAILABLE", "dim");
        updateRow("coords", "UNAVAILABLE", "dim");
        return;
      }

      updateRow("ip", geo.ip || "UNAVAILABLE");
      updateRow("isp", geo.isp || "UNAVAILABLE");
      updateRow("city", geo.city || "UNAVAILABLE", "amber");
      updateRow("region", geo.region || "UNAVAILABLE");
      updateRow("country", `${geo.country || "UNAVAILABLE"}${geo.countryCode ? ` (${geo.countryCode})` : ""}`);
      updateRow("coords", geo.coords || "UNAVAILABLE");
      updateRow("timezone", geo.timezone ? `${utcOffsetString()} / ${geo.timezone}` : valuesMap.get("timezone").value, "amber");
    });
  </script>
</body>
</html>
