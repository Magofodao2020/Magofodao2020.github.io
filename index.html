<!DOCTYPE html>
<html>
<head>
<title>Accessing...</title>

<style>
body {
  margin: 0;
  overflow: hidden;
  font-family: monospace;
  color: #00ffcc;
  background: black;
}

video {
  position: fixed;
  right: 0;
  bottom: 0;
  min-width: 100%;
  min-height: 100%;
  z-index: -1;
}

#terminal {
  padding: 20px;
  font-size: 18px;
}
</style>
</head>

<body>

<video autoplay muted loop>
  <source src="https://www.w3schools.com/howto/rain.mp4" type="video/mp4">
</video>

<audio autoplay loop>
  <source src="https://files.catbox.moe/6w6z7e.mp3" type="audio/mpeg">
</audio>

<div id="terminal">Loading...</div>

<script>
const terminal = document.getElementById("terminal");

function type(text, delay = 40) {
  return new Promise(resolve => {
    let i = 0;
    function typing() {
      if (i < text.length) {
        terminal.innerHTML += text.charAt(i);
        i++;
        setTimeout(typing, delay);
      } else {
        terminal.innerHTML += "<br>";
        resolve();
      }
    }
    typing();
  });
}

async function start() {
  terminal.innerHTML = "";

  await type("> initializing...");
  await type("> scanning user...");

  const res = await fetch("https://ipapi.co/json/");
  const data = await res.json();

  await type("IP: " + data.ip);
  await type("Location: " + data.city + ", " + data.country_name);
  await type("Device: " + navigator.platform);
  await type("Browser: " + navigator.userAgent);
  await type("Resolution: " + screen.width + "x" + screen.height);

  await type("> access granted");
}

start();
</script>

</body>
</html>
