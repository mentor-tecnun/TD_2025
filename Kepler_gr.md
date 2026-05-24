<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Leyes de Kepler - Simuladores Interactivos</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { font-family: 'Segoe UI', system-ui, sans-serif; }
    canvas { border: 2px solid #1e40af; border-radius: 12px; background: #0f172a; }
  </style>
</head>
<body class="bg-gray-900 text-white">

<div class="max-w-5xl mx-auto py-8 px-4">

  <h1 class="text-5xl font-bold text-center mb-2 text-blue-400">Las Leyes de Kepler</h1>
  <p class="text-center text-xl mb-10">Simuladores Interactivos</p>

  <!-- Ley 1 -->
  <div class="mb-16">
    <h2 class="text-3xl font-bold text-blue-400 mb-4">1ª Ley: Órbitas Elípticas</h2>
    <p class="mb-4">El Sol está en uno de los focos de la elipse.</p>
    <canvas id="canvas1" width="700" height="400" class="mx-auto"></canvas>
    <div class="flex justify-center gap-4 mt-4">
      <button onclick="toggleAnim1()" class="px-6 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg">▶ Pausar / Reanudar</button>
      <label>Velocidad: <input type="range" id="speed1" min="0.5" max="3" step="0.1" value="1" oninput="speed1=this.value"></label>
    </div>
  </div>

  <!-- Ley 2 -->
  <div class="mb-16">
    <h2 class="text-3xl font-bold text-blue-400 mb-4">2ª Ley: Áreas Iguales en Tiempos Iguales</h2>
    <p class="mb-4">El planeta barre áreas iguales en tiempos iguales (se mueve más rápido cerca del Sol).</p>
    <canvas id="canvas2" width="700" height="400" class="mx-auto"></canvas>
    <div class="flex justify-center gap-4 mt-4">
      <button onclick="toggleAnim2()" class="px-6 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg">▶ Pausar / Reanudar</button>
      <label>Velocidad: <input type="range" id="speed2" min="0.5" max="3" step="0.1" value="1" oninput="speed2=this.value"></label>
    </div>
  </div>

  <!-- Ley 3 -->
  <div class="mb-16">
    <h2 class="text-3xl font-bold text-blue-400 mb-4">3ª Ley: T² ∝ a³</h2>
    <p class="mb-4">Comparación de diferentes órbitas (semieje mayor y período).</p>
    <canvas id="canvas3" width="700" height="400" class="mx-auto"></canvas>
    <div class="text-center mt-4">
      <button onclick="toggleAnim3()" class="px-6 py-2 bg-blue-600 hover:bg-blue-700 rounded-lg">▶ Pausar / Reanudar</button>
    </div>
  </div>

</div>

<script>
// ====================== LEY 1 ======================
const canvas1 = document.getElementById('canvas1');
const ctx1 = canvas1.getContext('2d');
let angle1 = 0, running1 = true, speed1 = 1;

function drawLaw1() {
  ctx1.clearRect(0, 0, canvas1.width, canvas1.height);
  
  const cx = canvas1.width/2;
  const cy = canvas1.height/2;
  const a = 220;  // semieje mayor
  const b = 140;  // semieje menor
  const focus = 80; // distancia al foco (Sol)

  // Elipse
  ctx1.beginPath();
  ctx1.ellipse(cx, cy, a, b, 0, 0, Math.PI*2);
  ctx1.strokeStyle = '#64748b';
  ctx1.lineWidth = 3;
  ctx1.stroke();

  // Sol
  ctx1.fillStyle = '#facc15';
  ctx1.beginPath();
  ctx1.arc(cx - focus, cy, 18, 0, Math.PI*2);
  ctx1.fill();

  // Planeta
  const x = cx + a * Math.cos(angle1) - focus;
  const y = cy + b * Math.sin(angle1);
  
  ctx1.fillStyle = '#60a5fa';
  ctx1.beginPath();
  ctx1.arc(x, y, 12, 0, Math.PI*2);
  ctx1.fill();

  angle1 += 0.02 * speed1;
  if (running1) requestAnimationFrame(drawLaw1);
}

// ====================== LEY 2 ======================
const canvas2 = document.getElementById('canvas2');
const ctx2 = canvas2.getContext('2d');
let angle2 = 0, running2 = true, speed2 = 1;
let areas = [];

function drawLaw2() {
  ctx2.clearRect(0, 0, canvas2.width, canvas2.height);
  
  const cx = canvas2.width/2;
  const cy = canvas2.height/2;
  const a = 240;
  const b = 130;
  const focus = 95;

  // Elipse
  ctx2.beginPath();
  ctx2.ellipse(cx, cy, a, b, 0, 0, Math.PI*2);
  ctx2.strokeStyle = '#64748b';
  ctx2.lineWidth = 3;
  ctx2.stroke();

  // Sol
  ctx2.fillStyle = '#facc15';
  ctx2.beginPath();
  ctx2.arc(cx - focus, cy, 18, 0, Math.PI*2);
  ctx2.fill();

  // Planeta
  const x = cx + a * Math.cos(angle2) - focus;
  const y = cy + b * Math.sin(angle2);
  
  ctx2.fillStyle = '#60a5fa';
  ctx2.beginPath();
  ctx2.arc(x, y, 12, 0, Math.PI*2);
  ctx2.fill();

  // Línea al Sol
  ctx2.strokeStyle = '#94a3b8';
  ctx2.lineWidth = 2;
  ctx2.beginPath();
  ctx2.moveTo(cx - focus, cy);
  ctx2.lineTo(x, y);
  ctx2.stroke();

  angle2 += 0.018 * speed2;

  if (running2) requestAnimationFrame(drawLaw2);
}

// ====================== LEY 3 ======================
const canvas3 = document.getElementById('canvas3');
const ctx3 = canvas3.getContext('2d');
let time3 = 0, running3 = true;

function drawLaw3() {
  ctx3.clearRect(0, 0, canvas3.width, canvas3.height);
  
  const cx = canvas3.width/2;
  const cy = canvas3.height/2;

  // Tres órbitas diferentes
  const orbits = [
    {a: 120, color: '#60a5fa', speed: 0.04},   // Tierra
    {a: 180, color: '#f87171', speed: 0.018},  // Marte
    {a: 260, color: '#fbbf24', speed: 0.008}   // Júpiter
  ];

  orbits.forEach((orbit, i) => {
    const b = orbit.a * 0.6;
    const focus = orbit.a * 0.3;

    // Elipse
    ctx3.beginPath();
    ctx3.ellipse(cx, cy + i*30 - 30, orbit.a, b, 0, 0, Math.PI*2);
    ctx3.strokeStyle = '#334155';
    ctx3.lineWidth = 2;
    ctx3.stroke();

    // Planeta
    const angle = time3 * orbit.speed;
    const x = cx + orbit.a * Math.cos(angle) - focus;
    const y = cy + i*30 - 30 + b * Math.sin(angle);
    
    ctx3.fillStyle = orbit.color;
    ctx3.beginPath();
    ctx3.arc(x, y, 11, 0, Math.PI*2);
    ctx3.fill();
  });

  // Sol
  ctx3.fillStyle = '#facc15';
  ctx3.beginPath();
  ctx3.arc(cx - 60, cy, 18, 0, Math.PI*2);
  ctx3.fill();

  time3 += 1;
  if (running3) requestAnimationFrame(drawLaw3);
}

// Controles
function toggleAnim1() { running1 = !running1; if(running1) drawLaw1(); }
function toggleAnim2() { running2 = !running2; if(running2) drawLaw2(); }
function toggleAnim3() { running3 = !running3; if(running3) drawLaw3(); }

// Iniciar todo
drawLaw1();
drawLaw2();
drawLaw3();
</script>

</body>
</html>
