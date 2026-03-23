<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>.</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #000;
    font-family: 'Share Tech Mono', monospace;
    min-height: 100vh;
    cursor: none;
  }

  #bg-video {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.2;
    z-index: 0;
    pointer-events: none;
  }

  /* scanlines */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg, transparent, transparent 2px,
      rgba(0,0,0,0.2) 2px, rgba(0,0,0,0.2) 4px
    );
    pointer-events: none;
    z-index: 50;
  }

  #content {
    position: relative;
    z-index: 10;
    padding: 2.5rem 3rem;
  }

  .line {
    font-size: clamp(13px, 1.8vw, 15px);
    line-height: 2;
    opacity: 0;
    white-space: nowrap;
  }
  .line.show { opacity: 1; }
  .label { color: #3a0000; margin-right: 1ch; }
  .val { color: #ff2222; }
  .val.green { color: #00ff66; }
  .val.amber { color: #ffaa00; }
  .val.dim   { color: #333; }

  #cursor-follow {
    position: fixed;
    pointer-events: none;
    z-index: 200;
    font-size: 11px;
    color: #ff0000;
    font-family: 'Share Tech Mono', monospace;
    opacity: 0.6;
    white-space: nowrap;
    transform: translate(10px, -50%);
  }
</style>
</head>
<body>

<!--
  ===================================================
  COLOQUE SEU VÍDEO: renomeie para "video.mp4"
  e coloque na mesma pasta que este arquivo.
  ===================================================
-->
<video id="bg-video" autoplay loop muted playsinline>
  <source src="video.mp4" type="video/mp4">
</video>

<!--
  ===================================================
  COLOQUE SUA MÚSICA: renomeie para "musica.mp3"
  e coloque na mesma pasta que este arquivo.
  ===================================================
-->
<audio id="bg-music" loop>
  <source src="musica.mp3" type="audio/mpeg">
</audio>

<div id="cursor-follow"></div>
<div id="content"></div>

<script>
// cursor personalizado
document.addEventListener('mousemove', e => {
  const c = document.getElementById('cursor-follow');
  c.style.left = e.clientX + 'px';
  c.style.top  = e.clientY + 'px';
  c.textContent = e.clientX + ', ' + e.clientY;
});

// música — tenta autoplay, senão aguarda interação
const music = document.getElementById('bg-music');
music.volume = 0.4;
music.play().catch(() => {
  document.addEventListener('click', () => music.play(), { once: true });
  document.addEventListener('keydown', () => music.play(), { once: true });
});

// ---- utilitários ----
function getOS(ua) {
  if (/Windows NT 10|Windows NT 11/.test(ua)) return 'Windows 10 / 11';
  if (/Windows NT 6.3/.test(ua)) return 'Windows 8.1';
  if (/Windows NT 6.1/.test(ua)) return 'Windows 7';
  if (/Mac OS X/.test(ua)) return 'macOS ' + (ua.match(/Mac OS X ([0-9_]+)/)||['',''])[1].replace(/_/g,'.');
  if (/Android/.test(ua)) return 'Android ' + (ua.match(/Android ([0-9.]+)/)||['',''])[1];
  if (/iPhone/.test(ua)) return 'iOS (iPhone)';
  if (/iPad/.test(ua))   return 'iOS (iPad)';
  if (/Linux/.test(ua))  return 'Linux';
  return 'desconhecido';
}

function getBrowser(ua) {
  if (/Edg\//.test(ua))    return 'Microsoft Edge '  + (ua.match(/Edg\/([0-9]+)/)||['',''])[1];
  if (/OPR\//.test(ua))    return 'Opera '            + (ua.match(/OPR\/([0-9]+)/)||['',''])[1];
  if (/Chrome\//.test(ua)) return 'Google Chrome '   + (ua.match(/Chrome\/([0-9]+)/)||['',''])[1];
  if (/Firefox\//.test(ua))return 'Mozilla Firefox ' + (ua.match(/Firefox\/([0-9]+)/)||['',''])[1];
  if (/Safari\//.test(ua)) return 'Safari';
  return 'desconhecido';
}

function getGPU() {
  try {
    const gl  = document.createElement('canvas').getContext('webgl');
    const ext = gl && gl.getExtension('WEBGL_debug_renderer_info');
    return ext ? gl.getParameter(ext.UNMASKED_RENDERER_WEBGL) : null;
  } catch(e) { return null; }
}

const ua   = navigator.userAgent;
const conn = navigator.connection;

// ---- monta a lista de linhas ----
// as de rede ficam com "..." até o fetch responder
const lines = [
  { label: 'endereço ip',     val: '...',  id: 'v-ip' },
  { label: 'provedor (isp)',  val: '...',  id: 'v-isp' },
  { label: 'cidade',          val: '...',  id: 'v-city',    cls: 'amber' },
  { label: 'estado',          val: '...',  id: 'v-region' },
  { label: 'país',            val: '...',  id: 'v-country' },
  { label: 'coordenadas',     val: '...',  id: 'v-coords' },
  { label: 'fuso horário',    val: '...',  id: 'v-tz' },
  { label: 'sistema',         val: getOS(ua),          cls: 'amber' },
  { label: 'navegador',       val: getBrowser(ua) },
  { label: 'resolução',       val: screen.width + ' × ' + screen.height + ' px' },
  { label: 'idioma',          val: navigator.language || '—' },
  { label: 'ram',             val: navigator.deviceMemory ? navigator.deviceMemory + ' GB' : 'oculto' },
  { label: 'núcleos cpu',     val: navigator.hardwareConcurrency || '—' },
  { label: 'gpu',             val: getGPU() || 'oculto', cls: getGPU() ? '' : 'dim' },
  { label: 'conexão',         val: conn ? (conn.effectiveType||'?').toUpperCase() + (conn.downlink ? ' / ' + conn.downlink + ' Mbps' : '') : '—' },
  { label: 'horário',         val: new Date().toLocaleTimeString('pt-BR'), cls: 'green', id: 'v-time' },
  { label: 'data',            val: new Date().toLocaleDateString('pt-BR', {weekday:'long',day:'numeric',month:'long',year:'numeric'}) },
];

// ---- renderiza ----
const content = document.getElementById('content');
const PAD = 18;

lines.forEach((obj, i) => {
  const div = document.createElement('div');
  div.className = 'line';

  const label = obj.label.padEnd(PAD, '\u00a0');
  div.innerHTML =
    `<span class="label">${label}</span>` +
    `<span class="val ${obj.cls||''}" ${obj.id ? 'id="'+obj.id+'"' : ''}>${obj.val}</span>`;

  content.appendChild(div);
  setTimeout(() => div.classList.add('show'), i * 90 + 200);
});

// bateria (async, adiciona no fim)
navigator.getBattery && navigator.getBattery().then(b => {
  const pct    = Math.round(b.level * 100);
  const status = b.charging ? ' (carregando)' : ' (na bateria)';
  const div    = document.createElement('div');
  div.className = 'line';
  div.innerHTML = `<span class="label">${'bateria'.padEnd(PAD,'\u00a0')}</span><span class="val ${pct < 25 ? '' : 'green'}">${pct}%${status}</span>`;
  content.appendChild(div);
  setTimeout(() => div.classList.add('show'), lines.length * 90 + 400);
});

// atualiza horário
setInterval(() => {
  const el = document.getElementById('v-time');
  if (el) el.textContent = new Date().toLocaleTimeString('pt-BR');
}, 1000);

// ---- fetch ip/geo ----
fetch('https://ipapi.co/json/')
  .then(r => r.json())
  .then(d => {
    const up = {
      'v-ip':      d.ip || '—',
      'v-isp':     d.org || '—',
      'v-city':    d.city || '—',
      'v-region':  d.region || '—',
      'v-country': (d.country_name||'—') + ' (' + (d.country||'') + ')',
      'v-coords':  (d.latitude||'?') + ', ' + (d.longitude||'?'),
      'v-tz':      d.timezone || '—',
    };
    for (const [id, val] of Object.entries(up)) {
      const el = document.getElementById(id);
      if (el) el.textContent = val;
    }
  })
  .catch(() => {
    ['v-ip','v-isp','v-city','v-region','v-country','v-coords','v-tz'].forEach(id => {
      const el = document.getElementById(id);
      if (el) { el.textContent = 'erro'; el.className = 'val dim'; }
    });
  });
</script>
</body>
</html>
