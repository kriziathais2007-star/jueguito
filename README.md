<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Aventura del Amor 💕</title>
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Inter:wght@400;500&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --pink: #D4537E;
    --pink-light: #FBEAF0;
    --pink-dark: #993556;
    --blue: #378ADD;
    --blue-light: #E6F1FB;
    --green: #1D9E75;
    --bg: #0d0d1a;
    --bg2: #161628;
    --bg3: #1e1e35;
    --border: rgba(212, 83, 126, 0.3);
    --text: #f0e8ff;
    --text2: #a89bcc;
    --pixel: 'Press Start 2P', monospace;
    --body: 'Inter', sans-serif;
  }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: var(--body);
    min-height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 1rem;
    overflow-x: hidden;
  }

  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background:
      radial-gradient(ellipse 60% 40% at 20% 20%, rgba(212,83,126,0.08) 0%, transparent 60%),
      radial-gradient(ellipse 50% 50% at 80% 80%, rgba(55,138,221,0.07) 0%, transparent 60%);
    pointer-events: none;
    z-index: 0;
  }

  /* scanlines */
  body::after {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      to bottom,
      transparent 0px,
      transparent 3px,
      rgba(0,0,0,0.08) 3px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1;
  }

  #game {
    width: 100%;
    max-width: 560px;
    position: relative;
    z-index: 2;
  }

  header {
    text-align: center;
    margin-bottom: 1.5rem;
  }

  .game-title {
    font-family: var(--pixel);
    font-size: clamp(10px, 3vw, 14px);
    color: var(--pink);
    letter-spacing: 0.05em;
    line-height: 1.8;
    text-shadow: 0 0 20px rgba(212,83,126,0.5);
  }

  .game-subtitle {
    font-family: var(--pixel);
    font-size: clamp(7px, 2vw, 9px);
    color: var(--text2);
    margin-top: 6px;
    letter-spacing: 0.1em;
  }

  /* HP Bar */
  .status-bar {
    display: flex;
    align-items: center;
    gap: 10px;
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 14px;
    margin-bottom: 12px;
  }

  .hp-icon {
    font-size: 18px;
    animation: pulse 1.5s ease-in-out infinite;
  }

  @keyframes pulse {
    0%, 100% { transform: scale(1); }
    50% { transform: scale(1.2); }
  }

  .hp-label {
    font-family: var(--pixel);
    font-size: 8px;
    color: var(--pink);
    white-space: nowrap;
  }

  .hp-track {
    flex: 1;
    height: 10px;
    background: rgba(255,255,255,0.08);
    border-radius: 99px;
    overflow: hidden;
    border: 1px solid rgba(212,83,126,0.2);
  }

  .hp-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--pink-dark), var(--pink));
    border-radius: 99px;
    transition: width 0.5s cubic-bezier(.4,0,.2,1);
    position: relative;
  }

  .hp-fill::after {
    content: '';
    position: absolute;
    top: 2px;
    left: 4px;
    right: 4px;
    height: 3px;
    background: rgba(255,255,255,0.3);
    border-radius: 99px;
  }

  .score-display {
    font-family: var(--pixel);
    font-size: 8px;
    color: var(--text2);
    white-space: nowrap;
  }

  /* Screen */
  .screen {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.25rem 1.5rem;
    margin-bottom: 12px;
    position: relative;
    overflow: hidden;
    min-height: 160px;
  }

  .screen::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 3px;
    background: linear-gradient(90deg, transparent, var(--pink), transparent);
  }

  .tags {
    display: flex;
    gap: 6px;
    margin-bottom: 10px;
  }

  .tag {
    font-family: var(--pixel);
    font-size: 7px;
    padding: 3px 8px;
    border-radius: 99px;
    letter-spacing: 0.05em;
  }

  .tag-pink { background: rgba(212,83,126,0.15); color: var(--pink); border: 1px solid rgba(212,83,126,0.3); }
  .tag-blue { background: rgba(55,138,221,0.12); color: #7ab8f5; border: 1px solid rgba(55,138,221,0.3); }
  .tag-gold { background: rgba(239,159,39,0.12); color: #f5c542; border: 1px solid rgba(239,159,39,0.3); }

  .scene-title {
    font-family: var(--pixel);
    font-size: clamp(9px, 2.5vw, 11px);
    color: var(--text);
    margin-bottom: 12px;
    line-height: 1.8;
  }

  .scene-text {
    font-size: 14px;
    color: var(--text2);
    line-height: 1.8;
  }

  /* Result message */
  .result-msg {
    font-size: 13px;
    color: var(--pink);
    font-style: italic;
    min-height: 22px;
    margin-bottom: 10px;
    padding: 0 2px;
    transition: opacity 0.3s;
  }

  /* Choices */
  .choices {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .choice-btn {
    background: var(--bg3);
    border: 1px solid rgba(255,255,255,0.08);
    border-radius: 8px;
    padding: 12px 16px;
    font-size: 13px;
    font-family: var(--body);
    color: var(--text2);
    cursor: pointer;
    text-align: left;
    line-height: 1.5;
    transition: all 0.15s;
    position: relative;
    overflow: hidden;
  }

  .choice-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, rgba(212,83,126,0.08), transparent);
    opacity: 0;
    transition: opacity 0.2s;
  }

  .choice-btn:hover {
    border-color: rgba(212,83,126,0.4);
    color: var(--text);
    transform: translateX(3px);
  }

  .choice-btn:hover::before { opacity: 1; }
  .choice-btn:active { transform: scale(0.98) translateX(3px); }
  .choice-btn:disabled { opacity: 0.5; cursor: not-allowed; transform: none; }

  /* Restart */
  .restart-btn {
    display: none;
    margin-top: 14px;
    width: 100%;
    padding: 12px;
    background: transparent;
    border: 1px solid var(--pink);
    border-radius: 8px;
    font-family: var(--pixel);
    font-size: 9px;
    color: var(--pink);
    cursor: pointer;
    letter-spacing: 0.08em;
    transition: all 0.2s;
  }

  .restart-btn:hover {
    background: rgba(212,83,126,0.1);
    box-shadow: 0 0 20px rgba(212,83,126,0.2);
  }

  /* Pixel hearts deco */
  .deco {
    text-align: center;
    font-family: var(--pixel);
    font-size: 8px;
    color: rgba(212,83,126,0.3);
    margin-top: 1rem;
    letter-spacing: 0.2em;
  }
</style>
</head>
<body>

<div id="game">
  <header>
    <div class="game-title">❤ AVENTURA DEL AMOR ❤</div>
    <div class="game-subtitle">una misión épica de 8 bits</div>
  </header>

  <div class="status-bar">
    <span class="hp-icon">💗</span>
    <span class="hp-label" id="hp-label">AMOR 100%</span>
    <div class="hp-track"><div class="hp-fill" id="hp-fill" style="width:100%"></div></div>
    <span class="score-display" id="score-label">PTS: 0</span>
  </div>

  <div class="screen">
    <div class="tags">
      <span class="tag tag-pink" id="zone-tag">ZONA 1</span>
      <span class="tag tag-blue" id="diff-tag">NORMAL</span>
    </div>
    <div class="scene-title" id="scene-title"></div>
    <div class="scene-text" id="scene-text"></div>
  </div>

  <div class="result-msg" id="result-msg"></div>
  <div class="choices" id="choices"></div>
  <button class="restart-btn" id="restart-btn" onclick="restart()">↺ JUGAR DE NUEVO</button>

  <div class="deco">· · · ♥ · · ·</div>
</div>

<script>
const scenes = [
  {
    id: 0, zone: "ZONA 1", diff: "NORMAL", diffClass: "tag-blue",
    title: "El inicio de la misión",
    text: "Eres un pixel heroico en un mundo 8-bit. Tu misión: llegar hasta Amor, atrapada en la Torre del Final. El guardián del portal bloquea el camino y pregunta: '¿Cuál es el verdadero poder del amor?'",
    choices: [
      { text: "A) Un abrazo en el momento correcto", pts: 20, hp: 0, next: 1, msg: "¡Respuesta legendaria! El guardián llora y abre el portal." },
      { text: "B) El amor es un bug emocional sin solución", pts: 5, hp: -15, next: 1, msg: "El guardián se ofende un poco, pero te deja pasar igual." },
      { text: "C) Gritas: 'NO SÉ, DÉJAME PASAR'", pts: -20, hp: -100, next: 1, msg: "El guardián, confundido, se aparta." }
    ]
  },
  {
    id: 1, zone: "ZONA 2", diff: "DIFÍCIL", diffClass: "tag-blue",
    title: "El laberinto de las notificaciones",
    text: "Estás en el Laberinto de los Mensajes No Leídos. Hay 47 notificaciones sin responder. Una voz pregunta: '¿Cómo sobrevives a esto?'",
    choices: [
      { text: "A) Responder todas con un solo 'jajaja exacto'", pts: 15, hp: 5, next: 2, msg: "¡Inesperadamente efectivo! +5 puntos de sanidad mental." },
      { text: "B) Fingir que no tienes wi-fi y desaparecer 3 días", pts: 5, hp: -25, next: 2, msg: "Riesgoso. Amor te manda una captura del 'en línea'." },
      { text: "C) Fácil: respondiéndole primero a mi novia", pts: 25, hp: 0, next: 2, msg: "¡Perfecto! Las demás notificaciones lloran de envidia." }
    ]
  },
  {
    id: 2, zone: "ZONA 3", diff: "ÉPICO", diffClass: "tag-blue",
    title: "El jefe de la película de cita",
    text: "¡Aparece el Jefe Final: '¿Qué película ponen esta noche?' Un dilema terrible. Ambos tienen gustos distintos. El reloj corre.",
    choices: [
      { text: "A) 'La que tú quieras, mi amor'... y te quedas dormido a los 10 min", pts: 5, hp: -5, next: 3, msg: "Clásico. Amor te tapa con una cobija de todas formas y se acuesta contigo" },
      { text: "B) Negociar: acción ahora, romántica la siguiente", pts: 10, hp: 10, next: 3, msg: "¡Diplomacia nivel máximo! Ambos ganan." },
      { text: "C) Poner las dos a la vez en pantalla dividida", pts: 30, hp: 0, next: 3, msg: "Caos creativo. Nadie entiende nada pero es un recuerdo épico." }
    ]
  },
  {
    id: 3, zone: "ZONA FINAL", diff: "LEGENDARIO", diffClass: "tag-gold",
    title: "La torre del final",
    text: "¡Llegaste! En lo alto de la Torre del Final, Amor te espera. Pero el Dragón del Olvido Aniversario bloquea el paso. Ruge: '¿Cuándo es nuestro aniversario?'",
    choices: [
      { text: "A) Dar la fecha exacta y obtener el logro: ‘Memoria Nivel Dios’", pts: 50, hp: 20, next: 4, msg: "EL DRAGÓN EXPLOTA EN CONFETI. Leyenda absoluta." },
      { text: "B) Decir una fecha incorrecta con mucha confianza", pts: 5, hp: -30, next: 4, msg: "El dragón ríe malvado. Amor frunce el ceño desde arriba." },
      { text: "C) 'No sé la fecha exacta pero me acuerdo de cómo me sentí'", pts: 35, hp: 10, next: 4, msg: "El dragón llora. Nadie esperaba eso. Muy bien jugado." }
    ]
  },
  { id: 4, ending: true }
];

let hp = 100, score = 0, current = 0;

function render() {
  const scene = scenes[current];
  if (scene.ending) { showEnding(); return; }

  document.getElementById('zone-tag').textContent = scene.zone;
  const diffEl = document.getElementById('diff-tag');
  diffEl.textContent = scene.diff;
  diffEl.className = 'tag ' + scene.diffClass;
  document.getElementById('scene-title').textContent = scene.title;
  document.getElementById('scene-text').textContent = scene.text;
  document.getElementById('result-msg').textContent = '';

  const choicesEl = document.getElementById('choices');
  choicesEl.innerHTML = '';
  scene.choices.forEach((c, i) => {
    const btn = document.createElement('button');
    btn.className = 'choice-btn';
    btn.textContent = c.text;
    btn.onclick = () => choose(i);
    choicesEl.appendChild(btn);
  });
}

function choose(idx) {
  const c = scenes[current].choices[idx];
  score += c.pts;
  hp = Math.min(100, Math.max(0, hp + c.hp));
  document.getElementById('hp-fill').style.width = hp + '%';
  document.getElementById('hp-label').textContent = 'AMOR ' + hp + '%';
  document.getElementById('score-label').textContent = 'PTS: ' + score;
  document.getElementById('result-msg').textContent = c.msg;
  document.querySelectorAll('.choice-btn').forEach(b => b.disabled = true);
  setTimeout(() => {
    if (hp <= 0) { showGameOver(); return; }
    current = c.next;
    render();
  }, 1900);
}

function showEnding() {
  document.getElementById('zone-tag').textContent = 'FIN';
  document.getElementById('diff-tag').textContent = '';
  let title, msg;
  if (score >= 130) {
    title = "Final S+: Amor desbloqueado al 100%";
    msg = "¡Increíble! Venciste todos los jefes con maestría. Amor corre hacia ti, lanza un corazón de 8-bit gigante y juntos se convierten en el equipo más legendario del universo pixelado. Créditos finales: su historia de amor, narrada en chiptune. 💕";
  } else if (score >= 80) {
    title = "Final A: Misión completada con amor";
    msg = "Lo lograste. Con algún que otro traspié gracioso, llegaste hasta Amor. Se abrazan fuerte entre píxeles y confeti. No es el final perfecto, pero es el más auténtico. 🎉";
  } else {
    title = "Final B: Lo intentaste, eso cuenta";
    msg = "Hubo caídas, respuestas dudosas y un dragón que casi te derrota. Pero llegaste. Amor te mira y dice: 'siguiente vez coordinamos mejor, ¿sí?' y sonríe.";
  }
  document.getElementById('scene-title').textContent = title;
  document.getElementById('scene-text').textContent = msg + '\n\nPuntuación final: ' + score + ' pts  |  Amor: ' + hp + '%';
  document.getElementById('choices').innerHTML = '';
  document.getElementById('result-msg').textContent = '';
  document.getElementById('restart-btn').style.display = 'block';
}

function showGameOver() {
  document.getElementById('zone-tag').textContent = 'GAME OVER';
  document.getElementById('diff-tag').textContent = '';
  document.getElementById('scene-title').textContent = "Se acabaron los corazones";
  document.getElementById('scene-text').textContent = "El amor necesita mantenimiento, como cualquier partida guardada. ¡Vuelve a intentarlo con más cariño! 💔";
  document.getElementById('choices').innerHTML = '';
  document.getElementById('result-msg').textContent = '';
  document.getElementById('restart-btn').style.display = 'block';
}

function restart() {
  hp = 100; score = 0; current = 0;
  document.getElementById('hp-fill').style.width = '100%';
  document.getElementById('hp-label').textContent = 'AMOR 100%';
  document.getElementById('score-label').textContent = 'PTS: 0';
  document.getElementById('restart-btn').style.display = 'none';
  render();
}

render();
</script>
</body>
</html>
