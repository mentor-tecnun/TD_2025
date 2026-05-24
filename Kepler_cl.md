---
marp: true
theme: default
paginate: true
backgroundColor: #fff
style: |
  section {
    font-family: 'Segoe UI', sans-serif;
    font-size: 0.9em;
  }
  textarea {
    width: 100%;
    height: 155px;
    font-family: 'Courier New', monospace;
    font-size: 0.65em;
    background: #1e1e1e;
    color: #d4d4d4;
    border: 1px solid #444;
    border-radius: 6px;
    padding: 8px;
    resize: vertical;
    box-sizing: border-box;
  }
  .preview {
    width: 100%;
    height: 190px;
    border: 1px solid #ddd;
    border-radius: 6px;
    background: #0a0a1a;
    overflow: hidden;
    margin-top: 4px;
  }
  .preview iframe {
    width: 100%;
    height: 100%;
    border: none;
  }
  .law-box {
    background: #f0f4ff;
    border-left: 4px solid #3b82f6;
    padding: 10px 14px;
    border-radius: 4px;
    margin: 8px 0;
  }
---

# Las Leyes de Kepler
## El movimiento planetario

Johannes Kepler (1571–1630) describió matemáticamente
cómo los planetas orbitan al Sol en tres leyes fundamentales.

> 🪐 Basadas en las observaciones de Tycho Brahe

---

## Contexto histórico

- **Tycho Brahe** recopiló décadas de datos astronómicos precisos
- **Kepler** fue su asistente y heredó todos sus datos
- Tardó ~10 años en descubrir sus tres leyes
- Publicadas entre **1609** y **1619**
- Newton las explicaría décadas después con su Ley de Gravitación

> Kepler rompió con el dogma de las órbitas circulares perfectas

---

## Las tres leyes de un vistazo

| Ley | Nombre | Enunciado breve |
|-----|--------|-----------------|
| 1ª | Ley de las órbitas | Las órbitas son **elipses** con el Sol en un foco |
| 2ª | Ley de las áreas | El radio barre **áreas iguales** en tiempos iguales |
| 3ª | Ley de los períodos | T² es proporcional a a³ |

---

## 1ª Ley — Ley de las Órbitas

<div class="law-box">
<strong>Enunciado:</strong> Cada planeta describe una órbita elíptica alrededor del Sol, 
estando el Sol situado en uno de los dos focos de la elipse.
</div>

**Conceptos clave:**
- Una elipse tiene **dos focos** (F₁ y F₂)
- El Sol ocupa **uno** de los focos (no el centro)
- **Perihelio**: punto más cercano al Sol
- **Afelio**: punto más lejano al Sol
- La excentricidad *e* mide cuán "aplastada" es la elipse (0 = círculo)

---

## 1ª Ley — Simulación interactiva

<textarea id="code-ley1" oninput="runExample('code-ley1','preview-ley1')">
<canvas id="c" width="420" height="175" style="background:#0a0a1a"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const cx = 210, cy = 87;
let a = 90, e = 0.6;
let angle = 0;

function draw() {
  ctx.clearRect(0,0,420,175);
  const b = a * Math.sqrt(1 - e*e);
  const f = a * e; // distancia focal

  // Elipse
  ctx.beginPath();
  ctx.ellipse(cx - f, cy, a, b, 0, 0, 2*Math.PI);
  ctx.strokeStyle = '#4488ff';
  ctx.lineWidth = 1.5;
  ctx.stroke();

  // Eje mayor
  ctx.beginPath();
  ctx.moveTo(cx - f - a, cy);
  ctx.lineTo(cx - f + a, cy);
  ctx.strokeStyle = '#334';
  ctx.lineWidth = 0.5;
  ctx.stroke();

  // Focos
  ctx.fillStyle = '#ffdd44';
  ctx.beginPath(); ctx.arc(cx - f - f, cy, 5, 0, 2*Math.PI); ctx.fill(); // Sol
  ctx.fillStyle = '#555';
  ctx.beginPath(); ctx.arc(cx - f, cy, 3, 0, 2*Math.PI); ctx.fill(); // F2

  // Etiquetas focos
  ctx.fillStyle='#ffdd44'; ctx.font='11px sans-serif';
  ctx.fillText('☀ Sol (F₁)', cx - 2*f - 6, cy + 16);
  ctx.fillStyle='#777';
  ctx.fillText('F₂', cx - f - 8, cy + 14);

  // Planeta
  const px = (cx - f - f) + a * Math.cos(angle);
  const py = cy + b * Math.sin(angle);
  ctx.fillStyle = '#4fc3f7';
  ctx.beginPath(); ctx.arc(px, py, 7, 0, 2*Math.PI); ctx.fill();

  // Radio vector
  ctx.beginPath();
  ctx.moveTo(cx - 2*f, cy);
  ctx.lineTo(px, py);
  ctx.strokeStyle = 'rgba(255,100,100,0.7)';
  ctx.lineWidth = 1.5;
  ctx.stroke();

  // Etiquetas
  ctx.fillStyle='#aaa'; ctx.font='10px sans-serif';
  ctx.fillText('Perihelio', cx - 2*f - a + 2, cy - 8);
  ctx.fillText('Afelio', cx - 2*f + a - 32, cy - 8);
  ctx.fillStyle='#4fc3f7';
  ctx.fillText('🌍', px - 6, py - 10);

  // Velocidad kepleriana: más rápido en perihelio
  const r = Math.sqrt((px-(cx-2*f))**2 + (py-cy)**2);
  const speed = 1400 / r;
  angle += speed * 0.001;
}
setInterval(draw, 16);
</script></textarea>

<div class="preview"><iframe id="preview-ley1"></iframe></div>

<script>
function runExample(srcId, dstId) {
  document.getElementById(dstId).srcdoc =
    document.getElementById(srcId).value;
}
window.addEventListener('load', function () {
  document.querySelectorAll('textarea[id^="code-"]').forEach(function (ta) {
    runExample(ta.id, ta.id.replace('code-', 'preview-'));
  });
});
</script>

---

## 2ª Ley — Ley de las Áreas

<div class="law-box">
<strong>Enunciado:</strong> El segmento que une el planeta con el Sol (radio vector) 
barre áreas iguales en tiempos iguales.
</div>

**Consecuencias físicas:**
- El planeta se mueve **más rápido** cerca del perihelio
- El planeta se mueve **más lento** cerca del afelio
- Es equivalente a la **conservación del momento angular**
- La velocidad varía, pero el área barrida por unidad de tiempo es constante

> Esta ley implica que el movimiento planetario **no es uniforme**

---

## 2ª Ley — Simulación de áreas

<textarea id="code-ley2" oninput="runExample('code-ley2','preview-ley2')">
<canvas id="c" width="420" height="175" style="background:#0a0a1a"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const cx = 120, cy = 87;
const a = 85, e = 0.6;
const b = a * Math.sqrt(1-e*e);
const f = a * e;
const sx = cx, sy = cy; // foco (Sol)

let angle = 0;
const sectors = [];
let sectorTimer = 0;
const SECTOR_INTERVAL = 60; // frames entre sectores
const SECTOR_DURATION = 180; // frames que dura visible

function getPlanetPos(ang) {
  return {
    x: cx - f + a * Math.cos(ang),
    y: cy + b * Math.sin(ang)
  };
}

function draw() {
  ctx.clearRect(0,0,420,175);

  // Elipse
  ctx.beginPath();
  ctx.ellipse(cx - f, cy, a, b, 0, 0, 2*Math.PI);
  ctx.strokeStyle = '#334';
  ctx.lineWidth = 1;
  ctx.stroke();

  // Sol
  ctx.fillStyle = '#ffdd44';
  ctx.beginPath(); ctx.arc(sx, sy, 6, 0, 2*Math.PI); ctx.fill();
  ctx.fillStyle='#ffdd44'; ctx.font='10px sans-serif';
  ctx.fillText('☀ Sol', sx + 8, sy + 4);

  // Velocidad kepleriana
  const r = Math.sqrt((getPlanetPos(angle).x - sx)**2 + (getPlanetPos(angle).y - sy)**2);
  const speed = 1500 / r;

  // Guardar sector cada N frames
  if (sectorTimer % SECTOR_INTERVAL === 0) {
    sectors.push({
      start: angle,
      end: angle + speed * SECTOR_INTERVAL * 0.001 * 18,
      born: sectorTimer,
      color: `hsl(${(sectorTimer*3)%360},80%,60%)`
    });
    if (sectors.length > 5) sectors.shift();
  }
  sectorTimer++;

  // Dibujar sectores
  sectors.forEach(sec => {
    const alpha = Math.max(0, 1 - (sectorTimer - sec.born) / SECTOR_DURATION);
    ctx.beginPath();
    ctx.moveTo(sx, sy);
    for (let ag = sec.start; ag <= sec.end; ag += 0.01) {
      const p = getPlanetPos(ag);
      ctx.lineTo(p.x, p.y);
    }
    ctx.closePath();
    ctx.fillStyle = sec.color.replace(')',`,${alpha * 0.5})`).replace('hsl','hsla');
    ctx.fill();
    ctx.strokeStyle = sec.color.replace(')',`,${alpha})`).replace('hsl','hsla');
    ctx.lineWidth = 1;
    ctx.stroke();
  });

  // Planeta
  const p = getPlanetPos(angle);
  ctx.fillStyle = '#4fc3f7';
  ctx.beginPath(); ctx.arc(p.x, p.y, 7, 0, 2*Math.PI); ctx.fill();

  // Radio vector
  ctx.beginPath(); ctx.moveTo(sx, sy); ctx.lineTo(p.x, p.y);
  ctx.strokeStyle = 'rgba(255,120,80,0.9)';
  ctx.lineWidth = 1.5; ctx.stroke();

  // Leyenda
  ctx.fillStyle='#aaa'; ctx.font='11px sans-serif';
  ctx.fillText('Áreas iguales = tiempos iguales', 230, 30);
  ctx.fillText(`Velocidad: ${speed.toFixed(1)} u/s`, 230, 48);
  ctx.fillText(r < 70 ? '⚡ Rápido (perihelio)' : '🐢 Lento (afelio)', 230, 66);

  angle += speed * 0.001;
}
setInterval(draw, 16);
</script></textarea>

<div class="preview"><iframe id="preview-ley2"></iframe></div>

---

## 3ª Ley — Ley de los Períodos

<div class="law-box">
<strong>Enunciado:</strong> El cuadrado del período orbital T de un planeta es directamente 
proporcional al cubo del semieje mayor a de su órbita.
</div>

$$T^2 = k \cdot a^3$$

Con el Sol como centro y unidades UA / años:

$$T^2 = a^3 \quad \Rightarrow \quad k = 1$$

**Ejemplo para planetas del Sistema Solar:**

| Planeta | a (UA) | T (años) | a³ | T² |
|---------|--------|----------|-----|-----|
| Mercurio | 0.387 | 0.241 | 0.058 | 0.058 |
| Venus | 0.723 | 0.615 | 0.378 | 0.378 |
| Tierra | 1.000 | 1.000 | 1.000 | 1.000 |
| Marte | 1.524 | 1.881 | 3.540 | 3.538 |

---

## 3ª Ley — Comparación de órbitas

<textarea id="code-ley3" oninput="runExample('code-ley3','preview-ley3')">
<canvas id="c" width="420" height="175" style="background:#0a0a1a"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const sx = 100, sy = 87;

// Planetas: [nombre, semieje_px, excentricidad, color, velBase]
const planets = [
  { name: 'Mercurio', a: 30,  e: 0.21, color: '#aaa',    angle: 0 },
  { name: 'Venus',    a: 52,  e: 0.01, color: '#f5c060',  angle: 1 },
  { name: 'Tierra',   a: 72,  e: 0.02, color: '#4fc3f7',  angle: 2 },
  { name: 'Marte',    a: 95,  e: 0.09, color: '#e05030',  angle: 3 },
];

function speed(a) {
  // Ley de Kepler: T ∝ a^(3/2), velocidad angular ∝ a^(-3/2)
  return 0.022 / Math.pow(a / 30, 1.5);
}

function draw() {
  ctx.clearRect(0,0,420,175);

  planets.forEach(pl => {
    const b = pl.a * Math.sqrt(1 - pl.e*pl.e);
    const f = pl.a * pl.e;

    // Órbita
    ctx.beginPath();
    ctx.ellipse(sx - f, sy, pl.a, b, 0, 0, 2*Math.PI);
    ctx.strokeStyle = pl.color + '55';
    ctx.lineWidth = 1;
    ctx.stroke();

    // Planeta
    const px = (sx - f) - f + pl.a * Math.cos(pl.angle);
    const py = sy + b * Math.sin(pl.angle);
    ctx.fillStyle = pl.color;
    ctx.beginPath(); ctx.arc(px, py, 4, 0, 2*Math.PI); ctx.fill();

    // Nombre
    ctx.fillStyle = pl.color;
    ctx.font = '9px sans-serif';
    ctx.fillText(pl.name, 230, 20 + planets.indexOf(pl) * 32);
    const T = (2 * Math.PI / speed(pl.a)).toFixed(0);
    ctx.fillStyle = '#888';
    ctx.font = '8px sans-serif';
    ctx.fillText(`a=${pl.a}px  T≈${T} frames`, 230, 32 + planets.indexOf(pl) * 32);

    pl.angle += speed(pl.a);
  });

  // Sol
  ctx.fillStyle = '#ffdd44';
  ctx.beginPath(); ctx.arc(sx, sy, 7, 0, 2*Math.PI); ctx.fill();
  ctx.font = '9px sans-serif';
  ctx.fillStyle = '#ffdd44';
  ctx.fillText('☀', sx - 5, sy - 10);

  // Título
  ctx.fillStyle='#aaa'; ctx.font='10px sans-serif';
  ctx.fillText('Más lejos = Período más largo', 230, 150);
}
setInterval(draw, 16);
</script></textarea>

<div class="preview"><iframe id="preview-ley3"></iframe></div>

---

## 3ª Ley — Calculadora

<textarea id="code-calc" oninput="runExample('code-calc','preview-calc')">
<style>
  body { background: #0a0a1a; color: #ccc; font-family: sans-serif;
         padding: 10px; font-size: 13px; }
  input { background: #1e1e2e; color: #7dc; border: 1px solid #335;
          padding: 4px 8px; border-radius: 4px; width: 80px; }
  button { background: #3b5; color: #fff; border: none; padding: 5px 14px;
           border-radius: 4px; cursor: pointer; margin-left: 8px; }
  .result { margin-top: 10px; background: #111a2a; padding: 10px;
            border-radius: 6px; border-left: 3px solid #3b82f6; }
  .big { font-size: 20px; color: #4fc3f7; font-weight: bold; }
  label { color: #aaa; }
</style>
<label>Semieje mayor (UA): </label>
<input id="a" type="number" value="1.524" step="0.01">
<button onclick="calc()">Calcular T</button>
<div class="result" id="res">Introduce un semieje mayor y pulsa Calcular.</div>
<script>
function calc() {
  const a = parseFloat(document.getElementById('a').value);
  if (isNaN(a) || a <= 0) { document.getElementById('res').innerHTML='⚠ Valor inválido'; return; }
  const T = Math.pow(a, 1.5);
  const a3 = (a**3).toFixed(4);
  const T2 = (T**2).toFixed(4);
  document.getElementById('res').innerHTML = `
    <div class="big">T = ${T.toFixed(4)} años</div>
    <br>
    📐 a³ = ${a3} &nbsp;&nbsp; T² = ${T2}<br>
    ✅ a³ ≈ T² → ${Math.abs(parseFloat(a3)-parseFloat(T2)) < 0.001 ? 'Verificado ✓' : 'Aprox: ' + a3 + ' ≈ ' + T2}<br><br>
    <small style="color:#888">
      Mercurio: 0.387 UA | Venus: 0.723 UA | Tierra: 1 UA | Marte: 1.524 UA<br>
      Júpiter: 5.203 UA | Saturno: 9.537 UA | Urano: 19.19 UA | Neptuno: 30.07 UA
    </small>
  `;
}
calc();
</script></textarea>

<div class="preview"><iframe id="preview-calc"></iframe></div>

---

## Relación con la Gravitación Universal

Newton demostró en 1687 que las Leyes de Kepler se derivan de la **Ley de Gravitación Universal**:

$$F = G \frac{m_1 m_2}{r^2}$$

De aquí se obtiene la versión general de la 3ª ley:

$$T^2 = \frac{4\pi^2}{G(M+m)} \cdot a^3$$

| Si... | Entonces... |
|-------|-------------|
| M >> m (Sol >> planeta) | T² ≈ (4π²/GM) · a³ |
| k = 4π²/GM | es constante para todos los planetas del mismo sistema |

> Kepler encontró la ley empíricamente; Newton la explicó teóricamente.

---

## Parámetros de la elipse

```
Semieje mayor: a  →  la "mitad larga" de la elipse
Semieje menor: b  →  la "mitad corta"
Distancia focal: f = a·e
Excentricidad: e = f/a  (0 = círculo, 1 = parábola)

Relación: b² = a²(1 - e²)

Perihelio (más cercano):  r_min = a(1 - e)
Afelio   (más lejano):    r_max = a(1 + e)
```

**Excentricidades reales:**

| Planeta | e |
|---------|---|
| Venus | 0.007 (casi círculo) |
| Tierra | 0.017 |
| Marte | 0.093 |
| Mercurio | 0.206 |
| Plutón | 0.248 |

---

## Síntesis visual

<textarea id="code-resumen" oninput="runExample('code-resumen','preview-resumen')">
<canvas id="c" width="420" height="175" style="background:#0a0a1a"></canvas>
<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const sx = 130, sy = 87;
const a = 100, e = 0.55;
const b = a * Math.sqrt(1-e*e);
const f = a * e;

let angle = 0;
const trail = [];

function draw() {
  ctx.fillStyle = 'rgba(10,10,26,0.25)';
  ctx.fillRect(0,0,420,175);

  // Estela
  const px = (sx - f) + a * Math.cos(angle);
  const py = sy + b * Math.sin(angle);
  trail.push({x: px, y: py});
  if (trail.length > 120) trail.shift();

  trail.forEach((pt, i) => {
    const alpha = i / trail.length * 0.6;
    ctx.fillStyle = `rgba(100,180,255,${alpha})`;
    ctx.beginPath(); ctx.arc(pt.x, pt.y, 2, 0, 2*Math.PI); ctx.fill();
  });

  // Elipse guía
  ctx.beginPath();
  ctx.ellipse(sx - f, sy, a, b, 0, 0, 2*Math.PI);
  ctx.strokeStyle = 'rgba(60,80,160,0.5)';
  ctx.lineWidth = 1; ctx.stroke();

  // Sol
  ctx.fillStyle = '#ffcc00';
  ctx.beginPath(); ctx.arc(sx, sy, 7, 0, 2*Math.PI); ctx.fill();
  ctx.shadowColor = '#ff8800'; ctx.shadowBlur = 15;
  ctx.beginPath(); ctx.arc(sx, sy, 7, 0, 2*Math.PI); ctx.fill();
  ctx.shadowBlur = 0;

  // Radio vector
  ctx.beginPath(); ctx.moveTo(sx, sy); ctx.lineTo(px, py);
  ctx.strokeStyle = 'rgba(255,120,60,0.7)';
  ctx.lineWidth = 1.5; ctx.stroke();

  // Planeta
  ctx.fillStyle = '#4fc3f7';
  ctx.shadowColor = '#4fc3f7'; ctx.shadowBlur = 10;
  ctx.beginPath(); ctx.arc(px, py, 6, 0, 2*Math.PI); ctx.fill();
  ctx.shadowBlur = 0;

  // Velocidad
  const r = Math.sqrt((px-sx)**2 + (py-sy)**2);
  const speed = 1600 / r;

  // Texto
  ctx.fillStyle = '#ccc'; ctx.font = '11px sans-serif';
  ctx.fillText('1ª Ley: órbita elíptica', 250, 50);
  ctx.fillStyle = 'rgba(255,120,60,0.9)';
  ctx.fillText('2ª Ley: radio barre áreas iguales', 250, 70);
  ctx.fillStyle = '#4fc3f7';
  ctx.fillText(`v = ${speed.toFixed(1)}  (e = ${e})`, 250, 90);
  ctx.fillStyle = '#aaa'; ctx.font = '10px sans-serif';
  ctx.fillText('3ª Ley: T² ∝ a³', 250, 110);

  angle += speed * 0.001;
}
setInterval(draw, 16);
</script></textarea>

<div class="preview"><iframe id="preview-resumen"></iframe></div>

---

## Importancia y legado

- Las leyes de Kepler fueron las **primeras leyes cuantitativas** de la astronomía
- Permitieron predecir con exactitud la posición de los planetas
- Inspiraron a Newton para formular la gravitación universal
- Se aplican hoy en la planificación de **misiones espaciales**
- Válidas para cualquier sistema con dos cuerpos bajo gravedad

> "La astronomía es la hermana mayor de la física." — Kepler

---

## Recursos

| Recurso | Enlace |
|---------|--------|
| NASA — Kepler's Laws | nasa.gov/audience/foreducators/kepler |
| Simulación Solar System | eyes.nasa.gov |
| Khan Academy | khanacademy.org/science/physics |
| Wikipedia (ES) | es.wikipedia.org/wiki/Leyes_de_Kepler |

---

## Uso

Edita cualquier `textarea` de la presentación
y la simulación se actualiza en tiempo real. 🪐

Usa **Marp** para exportar esta presentación a PDF o HTML.
