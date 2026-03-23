<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>.</title>
<style>
* { margin:0; padding:0; box-sizing:border-box; }

/* ============================================================
   CONFIGURE AQUI:
   1. Troque o ID do YouTube abaixo (parte após ?v= na URL)
   2. Cole a URL direta do seu MP3 (ex: catbox.moe)
   ============================================================ */

body {
  background: #000;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  cursor: none;
}

/* vídeo YouTube de fundo via iframe */
#yt-bg {
  position: fixed;
  top: 50%; left: 50%;
  width: 177.78vh; /* 16:9 */
  height: 100vh;
  min-width: 100vw;
  min-height: 56.25vw;
  transform: translate(-50%, -50%);
  pointer-events: none;
  opacity: 0.18;
  z-index: 0;
}

/* scanlines */
body::after {
  content:'';
  position:fixed; inset:0;
  background: repeating-linear-gradient(
    0deg, transparent, transparent 2px,
    rgba(0,0,0,0.15) 2px, rgba(0,0,0,0.15) 4px
  );
  pointer-events:none;
  z-index:50;
}

#data {
  position: relative;
  z-index: 10;
  text-align: center;
  color: #ff1111;
  font-family: monospace;
  font-size: clamp(12px, 2vw, 17px);
  line-height: 1.95;
  padding: 2rem;
  max-width: 95vw;
}

#cursor-txt {
  position: fixed;
  pointer-events: none;
  z-index: 200;
  font-family: monospace;
  font-size: 11px;
  color: #ff0000;
  opacity: 0.5;
  transform: translate(8px, -50%);
}
</style>
</head>
<body>

<!-- ===== VÍDEO DE FUNDO ===== -->
<!-- Troque "dQw4w9WgXcQ" pelo ID do seu vídeo do YouTube -->
<iframe id="yt-bg"
  src="https://www.youtube.com/embed/bImHKGJqzFw?autoplay=1&mute=1&loop=1&playlist=bImHKGJqzFw&controls=0&showinfo=0&rel=0&iv_load_policy=3&disablekb=1&modestbranding=1"
  frameborder="0"
  allow="autoplay; encrypted-media"
  allowfullscreen>
</iframe>

<!-- ===== MÚSICA DE FUNDO ===== -->
<!-- Cole a URL direta do seu MP3 (catbox.moe ou qualquer host) -->
<audio id="music" loop>
  <source src="https://files.catbox.moe/kmugk9.mp3" type="audio/mpeg">
</audio>

<div id="cursor-txt"></div>
<div id="data"></div>

<script>
// cursor
document.addEventListener('mousemove', e => {
  const c = document.getElementById('cursor-txt');
  c.style.left = e.clientX + 'px';
  c.style.top  = e.clientY + 'px';
  c.textContent = e.clientX + ' ' + e.clientY;
});

// música
const music = document.getElementById('music');
music.volume = 0.5;
const tryPlay = () => music.play().catch(()=>{});
tryPlay();
['click','keydown','touchstart'].forEach(ev =>
  document.addEventListener(ev, tryPlay, { once: true })
);

// ---- coleta tudo ----
const ua  = navigator.userAgent;
const nav = navigator;
const scr = screen;
const conn = nav.connection || nav.mozConnection || nav.webkitConnection;

function os(ua) {
  if (/Windows NT 10|Windows NT 11/.test(ua)) return 'Windows 10/11';
  if (/Windows NT 6.3/.test(ua)) return 'Windows 8.1';
  if (/Windows NT 6.1/.test(ua)) return 'Windows 7';
  if (/Mac OS X/.test(ua)) return 'macOS ' + (ua.match(/Mac OS X ([0-9_]+)/)||['',''])[1].replace(/_/g,'.');
  if (/Android/.test(ua)) return 'Android ' + (ua.match(/Android ([0-9.]+)/)||['',''])[1];
  if (/iPhone/.test(ua)) return 'iOS — iPhone';
  if (/iPad/.test(ua))   return 'iOS — iPad';
  if (/CrOS/.test(ua))   return 'Chrome OS';
  if (/Linux/.test(ua))  return 'Linux';
  return ua;
}

function browser(ua) {
  if (/Edg\//.test(ua))     return 'Microsoft Edge '  + (ua.match(/Edg\/([0-9]+)/)||['',''])[1];
  if (/OPR\//.test(ua))     return 'Opera '            + (ua.match(/OPR\/([0-9]+)/)||['',''])[1];
  if (/Brave/.test(ua))     return 'Brave';
  if (/YaBrowser/.test(ua)) return 'Yandex Browser';
  if (/Chrome\//.test(ua))  return 'Google Chrome '   + (ua.match(/Chrome\/([0-9]+)/)||['',''])[1];
  if (/Firefox\//.test(ua)) return 'Mozilla Firefox '  + (ua.match(/Firefox\/([0-9]+)/)||['',''])[1];
  if (/Safari\//.test(ua))  return 'Safari';
  return 'desconhecido';
}

function gpu() {
  try {
    const gl  = document.createElement('canvas').getContext('webgl') ||
                document.createElement('canvas').getContext('experimental-webgl');
    const ext = gl && gl.getExtension('WEBGL_debug_renderer_info');
    return ext ? gl.getParameter(ext.UNMASKED_RENDERER_WEBGL) : '—';
  } catch(e) { return '—'; }
}

function vendor() {
  try {
    const gl  = document.createElement('canvas').getContext('webgl');
    const ext = gl && gl.getExtension('WEBGL_debug_renderer_info');
    return ext ? gl.getParameter(ext.UNMASKED_VENDOR_WEBGL) : '—';
  } catch(e) { return '—'; }
}

function tzOffset() {
  const off = -new Date().getTimezoneOffset();
  const h = Math.floor(Math.abs(off)/60).toString().padStart(2,'0');
  const m = (Math.abs(off)%60).toString().padStart(2,'0');
  return 'UTC' + (off >= 0 ? '+' : '-') + h + ':' + m;
}

function colorDepth(d) {
  const map = {24:'16 milhões de cores (24-bit)',30:'1 bilhão de cores (30-bit)',32:'32-bit',16:'65 mil cores (16-bit)'};
  return map[d] || d + '-bit';
}

const data = document.getElementById('data');
const lines = [];

function add(v) { if (v !== undefined && v !== null && v !== '') lines.push(String(v)); }

// ---- dados locais imediatos ----
add(new Date().toLocaleString('pt-BR', {weekday:'long',day:'numeric',month:'long',year:'numeric',hour:'2-digit',minute:'2-digit',second:'2-digit'}));
add(tzOffset() + ' — ' + Intl.DateTimeFormat().resolvedOptions().timeZone);
add(os(ua));
add(browser(ua));
add(nav.platform || '—');
add(scr.width + ' × ' + scr.height + ' px  /  viewport ' + window.innerWidth + ' × ' + window.innerHeight + ' px');
add('pixel ratio  ' + (window.devicePixelRatio || 1) + 'x');
add(colorDepth(scr.colorDepth));
add(nav.language + (nav.languages && nav.languages.length > 1 ? '  (' + Array.from(nav.languages).join(', ') + ')' : ''));
add('ram  ' + (nav.deviceMemory ? nav.deviceMemory + ' GB' : 'não exposto'));
add((nav.hardwareConcurrency || '—') + ' núcleos de cpu lógicos');
add('gpu  ' + gpu());
add('fabricante gpu  ' + vendor());
add('touch points  ' + nav.maxTouchPoints);
add('cookies  ' + (nav.cookieEnabled ? 'habilitados' : 'desabilitados'));
add('do not track  ' + (nav.doNotTrack || 'não configurado'));
add('java  ' + (nav.javaEnabled ? nav.javaEnabled() ? 'sim' : 'não' : 'não'));
add('plugins  ' + (nav.plugins ? nav.plugins.length : '0'));

if (conn) {
  add((conn.effectiveType||'?').toUpperCase()
    + (conn.type ? ' / ' + conn.type : '')
    + (conn.downlink != null ? ' / ' + conn.downlink + ' Mbps downlink' : '')
    + (conn.rtt != null ? ' / rtt ' + conn.rtt + 'ms' : '')
    + (conn.saveData ? ' / modo economia de dados ativo' : ''));
}

add('online  ' + nav.onLine);

// renderiza o que temos agora
render();

// bateria (async)
if (nav.getBattery) {
  nav.getBattery().then(b => {
    const pct = Math.round(b.level * 100);
    const status = b.charging
      ? (b.chargingTime < Infinity ? 'completa em ' + Math.ceil(b.chargingTime/60) + 'min' : 'carregando')
      : (b.dischargingTime < Infinity ? 'acaba em ' + Math.ceil(b.dischargingTime/60) + 'min' : 'na bateria');
    lines.splice(lines.length - 3, 0, 'bateria  ' + pct + '%  ' + status);
    render();
  }).catch(()=>{});
}

// geolocalização por IP — tenta duas APIs
function fetchGeo() {
  return fetch('https://ipapi.co/json/')
    .then(r => { if (!r.ok) throw new Error(); return r.json(); })
    .then(d => {
      if (!d.ip) throw new Error();
      return d;
    })
    .catch(() =>
      fetch('https://ip-api.com/json/?fields=status,message,country,countryCode,regionName,city,zip,lat,lon,timezone,isp,org,as,query')
        .then(r => r.json())
        .then(d => ({
          ip: d.query,
          city: d.city,
          region: d.regionName,
          country_name: d.country,
          country: d.countryCode,
          postal: d.zip,
          latitude: d.lat,
          longitude: d.lon,
          timezone: d.timezone,
          org: d.isp,
          asn: d.as,
        }))
    );
}

fetchGeo().then(d => {
  const geo = [
    d.ip,
    d.org || '—',
    d.asn || '',
    (d.city || '—') + (d.postal ? ',  CEP ' + d.postal : ''),
    d.region || '—',
    (d.country_name || '—') + ' (' + (d.country || '') + ')',
    (d.latitude || '?') + ', ' + (d.longitude || '?'),
    d.timezone || '—',
  ].filter(Boolean);

  // insere no começo, depois da data/hora
  lines.splice(1, 0, ...geo);
  render();
}).catch(() => {
  lines.splice(1, 0, 'ip  —  falha na resolução');
  render();
});

// atualiza hora a cada segundo
setInterval(() => {
  lines[0] = new Date().toLocaleString('pt-BR', {weekday:'long',day:'numeric',month:'long',year:'numeric',hour:'2-digit',minute:'2-digit',second:'2-digit'});
  render();
}, 1000);

function render() {
  data.innerHTML = lines.map(l => '<div>' + l + '</div>').join('');
}
</script>
</body>
</html>
