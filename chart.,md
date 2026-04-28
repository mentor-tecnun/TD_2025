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
    background: #fff;
    overflow: hidden;
    margin-top: 4px;
  }
  .preview iframe {
    width: 100%;
    height: 100%;
    border: none;
  }
---

# Chart.js
## Visualización de datos en el navegador

Librería JavaScript para crear gráficos interactivos
usando solo un elemento `<canvas>`.

> 🌐 Solo CDN — sin npm, sin import, sin bundlers

---

## ¿Qué es Chart.js?

- Librería JavaScript de código abierto
- Renderiza gráficos en un elemento `<canvas>`
- Ligera (~60 KB minificada), sin dependencias
- Compatible con todos los navegadores modernos

> Sitio oficial: [chartjs.org](https://www.chartjs.org)

---

## Instalación via CDN

Una sola línea en el `<head>` de tu HTML:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```

La variable global `Chart` queda disponible en toda la página.
No se necesita `npm install`, ni `import`, ni bundlers.

---

## Estructura básica

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body>
  <canvas id="miGrafico" width="400" height="200"></canvas>
  <script>
    const ctx = document.getElementById('miGrafico').getContext('2d');
    new Chart(ctx, {
      type: 'bar',      // tipo de gráfico
      data: { ... },    // datos
      options: { ... }  // configuración
    });
  </script>
</body>
</html>
```

---

## Tipos de gráficos

| Tipo | `type` |
|------|--------|
| Barras | `bar` |
| Líneas | `line` |
| Circular | `pie` / `doughnut` |
| Radar | `radar` |
| Polar | `polarArea` |
| Dispersión | `scatter` |
| Burbujas | `bubble` |

---

## Ejemplo: Gráfico de barras

<textarea id="code-bar" oninput="runExample('code-bar','preview-bar')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'bar',
  data: {
    labels: ['Enero','Febrero','Marzo','Abril'],
    datasets: [{
      label: 'Ventas 2024',
      data: [120, 190, 80, 150],
      backgroundColor: ['#FF6384','#36A2EB','#FFCE56','#4BC0C0'],
      borderRadius: 4
    }]
  },
  options: { scales: { y: { beginAtZero: true } } }
});
</script></textarea>

<div class="preview"><iframe id="preview-bar"></iframe></div>

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

## Ejemplo: Gráfico de líneas

<textarea id="code-line" oninput="runExample('code-line','preview-line')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'line',
  data: {
    labels: ['Lun','Mar','Mié','Jue','Vie'],
    datasets: [{
      label: 'Temperatura (°C)',
      data: [18, 21, 19, 24, 22],
      borderColor: 'rgb(75,192,192)',
      backgroundColor: 'rgba(75,192,192,0.2)',
      tension: 0.4,
      fill: true
    }]
  }
});
</script></textarea>

<div class="preview"><iframe id="preview-line"></iframe></div>

---

## Ejemplo: Doughnut

<textarea id="code-doughnut" oninput="runExample('code-doughnut','preview-doughnut')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'doughnut',
  data: {
    labels: ['Chrome','Firefox','Safari','Edge'],
    datasets: [{
      data: [65, 15, 12, 8],
      backgroundColor: ['#FF6384','#36A2EB','#FFCE56','#4BC0C0']
    }]
  },
  options: { plugins: { legend: { position: 'right' } } }
});
</script></textarea>

<div class="preview"><iframe id="preview-doughnut"></iframe></div>

---

## Ejemplo: Radar

<textarea id="code-radar" oninput="runExample('code-radar','preview-radar')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'radar',
  data: {
    labels: ['Velocidad','Potencia','Fácil','Bonito','Flexible'],
    datasets: [{
      label: 'Chart.js',
      data: [92, 85, 95, 88, 90],
      borderColor: 'rgba(54,162,235,1)',
      backgroundColor: 'rgba(54,162,235,0.2)'
    }]
  }
});
</script></textarea>

<div class="preview"><iframe id="preview-radar"></iframe></div>

---

## Ejemplo: Opciones avanzadas

<textarea id="code-opts" oninput="runExample('code-opts','preview-opts')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'bar',
  data: {
    labels: ['Q1','Q2','Q3','Q4'],
    datasets: [{
      label: 'Ingresos',
      data: [3200, 4100, 3800, 5200],
      backgroundColor: 'rgba(99,102,241,0.7)',
      borderRadius: 6
    }]
  },
  options: {
    plugins: {
      title: { display: true, text: 'Ingresos anuales' }
    },
    scales: {
      y: { beginAtZero: true, ticks: { callback: v => v + ' €' } }
    }
  }
});
</script></textarea>

<div class="preview"><iframe id="preview-opts"></iframe></div>

---

## Actualizar datos dinámicamente

```js
const chart = new Chart(ctx, { ... });

// Añadir un punto
chart.data.labels.push('Mayo');
chart.data.datasets[0].data.push(175);
chart.update();

// Reemplazar todos los datos
chart.data.datasets[0].data = [10, 20, 30, 40];
chart.update();
```

> ⚠️ Destruir antes de recrear en el mismo canvas:
> ```js
> if (chart) chart.destroy();
> ```

---

## Ejemplo: Datos dinámicos

<textarea id="code-dyn" oninput="runExample('code-dyn','preview-dyn')">
<canvas id="c" width="380" height="140"></canvas>
<button onclick="addData()">+ Añadir</button>
<button onclick="clearData()">Limpiar</button>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
const meses=['Ene','Feb','Mar','Abr','May','Jun','Jul','Ago','Sep','Oct','Nov','Dic'];
let idx = 4;
const chart = new Chart(document.getElementById('c'), {
  type: 'line',
  data: {
    labels: ['Ene','Feb','Mar','Abr'],
    datasets: [{ label: 'Valor', data: [30,50,40,70],
      borderColor:'#f59e0b', backgroundColor:'rgba(245,158,11,0.2)',
      tension:0.4, fill:true }]
  },
  options: { scales: { y: { beginAtZero: true } } }
});
function addData() {
  if (idx >= 12) return;
  chart.data.labels.push(meses[idx]);
  chart.data.datasets[0].data.push(Math.round(20+Math.random()*80));
  chart.update(); idx++;
}
function clearData() {
  chart.data.labels=['Ene']; chart.data.datasets[0].data=[30]; idx=1; chart.update();
}
</script></textarea>

<div class="preview"><iframe id="preview-dyn"></iframe></div>

---

## Ejemplo: Gráfico mixto (bar + line)

<textarea id="code-mixed" oninput="runExample('code-mixed','preview-mixed')">
<canvas id="c" width="400" height="170"></canvas>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
new Chart(document.getElementById('c'), {
  type: 'bar',
  data: {
    labels: ['Ene','Feb','Mar','Abr','May'],
    datasets: [
      { type: 'bar',  label: 'Ventas',
        data: [50,70,60,90,80],
        backgroundColor: 'rgba(99,102,241,0.6)' },
      { type: 'line', label: 'Media',
        data: [55,62,65,75,78],
        borderColor: '#f43f5e', tension: 0.4, fill: false }
    ]
  }
});
</script></textarea>

<div class="preview"><iframe id="preview-mixed"></iframe></div>

---

## Exportar como imagen

```html
<canvas id="miGrafico"></canvas>
<button onclick="descargar()">Descargar PNG</button>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
  const chart = new Chart(
    document.getElementById('miGrafico'), { ... }
  );

  function descargar() {
    const link = document.createElement('a');
    link.href  = chart.toBase64Image();
    link.download = 'grafico.png';
    link.click();
  }
</script>
```

---

## Buenas prácticas

- Usa `responsive: true` para adaptar al contenedor
- Añade `aria-label` al `<canvas>` para accesibilidad
- Destruye el gráfico antes de recrearlo en el mismo canvas
- No uses solo color para distinguir series
- Ofrece una tabla alternativa con los mismos datos

---

## Recursos

| Recurso | URL |
|---------|-----|
| Documentación oficial | chartjs.org/docs |
| Ejemplos interactivos | chartjs.org/samples |
| CDN jsdelivr | jsdelivr.net |
| Plugins comunidad | github.com/chartjs/awesome |

---

## Uso

Edita cualquier `textarea` de la presentación
y el gráfico se actualiza en tiempo real. 🎉
