<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Birthday Celebration 🎉</title>

<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>

<style>
:root {
  --theme: #ff2f6d;
}

* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  height: 100vh;
  overflow: hidden;
}

/* VIDEO BACKGROUND */
video {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  min-height: 100%;
  object-fit: cover;
  z-index: -2;
}

.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.55);
  z-index: -1;
}

/* CARD */
.card {
  background: rgba(255,255,255,0.95);
  width: 380px;
  margin: auto;
  margin-top: 40px;
  padding: 25px;
  border-radius: 25px;
  text-align: center;
  box-shadow: 0 20px 40px rgba(0,0,0,0.4);
}

h1 { color: var(--theme); }

input, select, textarea {
  width: 100%;
  padding: 10px;
  margin: 6px 0;
  border-radius: 10px;
  border: 1px solid #ccc;
}

button {
  width: 100%;
  padding: 12px;
  margin-top: 8px;
  border: none;
  border-radius: 12px;
  background: var(--theme);
  color: white;
  font-size: 16px;
  cursor: pointer;
}

button:hover { opacity: 0.9; }

/* CAKE */
.cake {
  font-size: 60px;
  cursor: pointer;
  margin: 10px 0;
}

.message {
  display: none;
  margin-top: 10px;
  font-size: 18px;
}
</style>
</head>

<body>

<!-- VIDEO BACKGROUND -->
<video autoplay muted loop>
  <source src="https://cdn.coverr.co/videos/coverr-birthday-candles-2179/1080p.mp4" type="video/mp4">
</video>
<div class="overlay"></div>

<div class="card">
  <h1>🎂 Birthday Celebration</h1>

  <input id="name" placeholder="Name">
  <input id="age" type="number" placeholder="Age">

  <!-- THEME PICKER -->
  🎨 Theme Color:
  <input type="color" onchange="setTheme(this.value)">

  <!-- LANGUAGE -->
  🌍 Language:
  <select id="lang">
    <option value="en">English</option>
    <option value="hi">Hindi</option>
    <option value="es">Spanish</option>
  </select>

  <!-- CUSTOM MESSAGE -->
  <textarea id="custom" rows="2" placeholder="Custom message 💌"></textarea>

  <!-- CANDLE -->
  <div class="cake" onclick="blowCandle()">🕯️🎂🕯️</div>
  <small>Tap candles to blow them!</small>

  <button onclick="showWish()">🎁 Celebrate</button>

  <div class="message" id="output"></div>
</div>

<audio id="music" loop>
  <source src="https://www.bensound.com/bensound-music/bensound-buddy.mp3">
</audio>

<script>
function setTheme(color) {
  document.documentElement.style.setProperty('--theme', color);
}

/* MULTI LANGUAGE */
const text = {
  en: name => `🎉 Happy Birthday ${name}! May your day be filled with joy and love.`,
  hi: name => `🎉 जन्मदिन मुबारक हो ${name}! आपका दिन खुशियों से भरा रहे।`,
  es: name => `🎉 ¡Feliz cumpleaños ${name}! Que tu día esté lleno de alegría.`
};

/* CANDLE */
function blowCandle() {
  document.querySelector(".cake").textContent = "🎂💨";
  confetti({ particleCount: 150, spread: 90 });
}

/* AGE ANIMATION */
function animateAge(age, el) {
  let i = 0;
  const t = setInterval(() => {
    el.textContent = `🎂 Age: ${i}`;
    if (i >= age) clearInterval(t);
    i++;
  }, 40);
}

function showWish() {
  const name = document.getElementById("name").value.trim();
  const age = document.getElementById("age").value;
  const lang = document.getElementById("lang").value;
  const custom = document.getElementById("custom").value;
  const out = document.getElementById("output");

  if (!name) return alert("Please enter a name 😊");

  out.style.display = "block";
  out.innerHTML = `
    ${text[lang](name)}<br>
    ${custom ? "💌 " + custom + "<br>" : ""}
    ${age ? `<div id="ageBox"></div>` : ""}
  `;

  if (age) animateAge(age, document.getElementById("ageBox"));

  confetti({ particleCount: 200, spread: 120 });
  document.getElementById("music").play();

  const link = `${location.origin}${location.pathname}?name=${name}&age=${age}&lang=${lang}`;
  history.replaceState(null,"",link);
}

/* AUTO LOAD */
const q = new URLSearchParams(location.search);
if (q.get("name")) {
  name.value = q.get("name");
  age.value = q.get("age");
  lang.value = q.get("lang") || "en";
  showWish();
}
</script>

</body>
</html>
