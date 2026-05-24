---
title: "Las Leyes de Kepler"
author: "Apuntes de Astronomía"
theme: default
paginate: true
---

# Las Leyes de Kepler

## Introducción

A comienzos del siglo XVII, el astrónomo alemán **Johannes Kepler (1571–1630)** formuló tres leyes fundamentales que describen cómo se mueven los planetas alrededor del Sol.

Kepler desarrolló estas leyes utilizando las observaciones extremadamente precisas realizadas por **Tycho Brahe**.

Estas leyes permitieron abandonar la idea clásica de órbitas perfectamente circulares y prepararon el camino para la teoría gravitatoria de Newton.

---

# Contexto histórico

Antes de Kepler:

- **Ptolomeo** → modelo geocéntrico
- **Copérnico** → modelo heliocéntrico
- **Tycho Brahe** → medidas astronómicas de gran precisión
- **Kepler** → formulación matemática del movimiento planetario

Cronología:

| Año | Evento |
|------|---------|
| 1543 | Copérnico publica el modelo heliocéntrico |
| 1571 | Nace Johannes Kepler |
| 1600 | Kepler trabaja con Tycho Brahe |
| 1609 | Primera y Segunda Ley |
| 1619 | Tercera Ley |
| 1687 | Newton explica físicamente las leyes |

---

# Primera Ley de Kepler

## Ley de las órbitas

> Los planetas se mueven alrededor del Sol siguiendo órbitas elípticas.

El Sol no está en el centro exacto de la órbita.

Se sitúa en uno de los **focos** de la elipse.

```
          Planeta
             ●
         .-'''''''-.
      .-'           '-.
     /                 \
    |      ● Sol        |
     \                 /
      '-.           .-'
         '-._____.-'
```

### Consecuencias

- Las órbitas no son círculos perfectos.
- La distancia Sol–planeta cambia durante el recorrido.
- Existen dos posiciones importantes:

| Concepto | Definición |
|-----------|------------|
| Perihelio | Punto más cercano al Sol |
| Afelio | Punto más lejano al Sol |

---

# Segunda Ley de Kepler

## Ley de las áreas

> La línea que une un planeta con el Sol barre áreas iguales en tiempos iguales.

Esto significa que:

- El planeta se mueve **más rápido** cuando está cerca del Sol.
- El planeta se mueve **más lento** cuando está lejos.

Representación simplificada:

```
          Afelio
             ●
           /   \
         /       \
       /           \
      ●-----☉-------●
       \           /
         \       /
           \   /
             ●
         Perihelio
```

Si un planeta tarda 30 días en recorrer una región cercana al Sol y 30 días en una región lejana:

Área 1 = Área 2

aunque las distancias recorridas sean diferentes.

---

# Segunda Ley: intuición física

La gravedad del Sol acelera más a los planetas cuando están próximos.

Por ello:

- Mercurio cambia mucho su velocidad orbital.
- La Tierra cambia poco.
- Los cometas presentan variaciones enormes.

---

# Tercera Ley de Kepler

## Ley armónica

> El cuadrado del período orbital es proporcional al cubo de la distancia media al Sol.

Matemáticamente:

\[
T^2 \propto a^3
\]

donde:

| Símbolo | Significado |
|----------|-------------|
| \(T\) | Período orbital |
| \(a\) | Semieje mayor de la órbita |

También puede escribirse:

\[
\frac{T^2}{a^3}=k
\]

siendo \(k\) una constante para todos los planetas del mismo sistema.

---

# Ejemplo

Comparación entre la Tierra y Marte:

| Planeta | Distancia media (UA) | Periodo orbital (años) |
|----------|---------------------|------------------------|
| Tierra | 1.00 | 1.00 |
| Marte | 1.52 | 1.88 |

Comprobación:

Tierra:

\[
1^2 = 1^3
\]

Marte:

\[
1.88^2 \approx 3.53
\]

\[
1.52^3 \approx 3.51
\]

Los valores son prácticamente iguales.

---

# Relación con Newton

Isaac Newton explicó posteriormente las leyes de Kepler mediante:

## Ley de gravitación universal

\[
F =
G
\frac{m_1m_2}{r^2}
\]

Kepler describió **cómo** se movían los planetas.

Newton explicó **por qué**.

---

# Resumen

| Ley | Idea principal |
|------|----------------|
| Primera | Órbitas elípticas |
| Segunda | Áreas iguales en tiempos iguales |
| Tercera | Relación entre distancia y periodo orbital |

---

# Impacto científico

Las leyes de Kepler fueron esenciales para:

- La mecánica celeste
- La teoría gravitatoria
- El cálculo de órbitas espaciales
- Los satélites artificiales
- Las misiones interplanetarias modernas

---

# Idea clave

> Kepler transformó la astronomía desde una descripción geométrica hacia una ciencia matemática capaz de predecir el movimiento de los cuerpos celestes.

```
"Los planetas no giran en círculos perfectos;
siguen las reglas matemáticas de la naturaleza."
```

---
Fin.


# Simuladores Visuales de las Leyes de Kepler

## Primera Ley — Órbitas Elípticas

> Los planetas describen órbitas elípticas con el Sol en uno de los focos.

<div align="center">

<svg id="kepler1" width="500" height="300" viewBox="0 0 500 300">

<ellipse
cx="250"
cy="150"
rx="180"
ry="100"
fill="none"
stroke="#4A90E2"
stroke-width="3"/>

<circle
cx="180"
cy="150"
r="10"
fill="orange"/>

<circle
id="planet1"
cx="430"
cy="150"
r="7"
fill="steelblue"/>

<text x="160" y="135">☉ Sol</text>

</svg>

</div>

<script>

(function(){

const p=document.getElementById("planet1");

let t=0;

function animate(){

t+=0.01;

const a=180;
const b=100;

const x=250+a*Math.cos(t);
const y=150+b*Math.sin(t);

p.setAttribute("cx",x);
p.setAttribute("cy",y);

requestAnimationFrame(animate);

}

animate();

})();

</script>

---

## Segunda Ley — Áreas Iguales en Tiempos Iguales

> El radio Sol-planeta barre áreas iguales en tiempos iguales.

<div align="center">

<svg id="kepler2" width="500" height="320">

<ellipse
cx="250"
cy="160"
rx="180"
ry="100"
fill="none"
stroke="#4A90E2"
stroke-width="3"/>

<circle
cx="180"
cy="160"
r="10"
fill="orange"/>

<polygon
id="sector"
fill="rgba(100,150,255,0.4)"
stroke="none"/>

<line
id="radius"
stroke="gray"
stroke-width="2"/>

<circle
id="planet2"
r="7"
fill="steelblue"/>

</svg>

</div>

<script>

(function(){

const p=document.getElementById("planet2");
const line=document.getElementById("radius");
const sector=document.getElementById("sector");

let theta=0;

function orbitSpeed(t){

return 0.012*(1+0.55*Math.cos(t));

}

function frame(){

theta+=orbitSpeed(theta);

const a=180;
const b=100;

const px=250+a*Math.cos(theta);
const py=160+b*Math.sin(theta);

p.setAttribute("cx",px);
p.setAttribute("cy",py);

line.setAttribute(
"x1",180);

line.setAttribute(
"y1",160);

line.setAttribute(
"x2",px);

line.setAttribute(
"y2",py);

const prev=theta-0.45;

const x0=250+a*Math.cos(prev);
const y0=160+b*Math.sin(prev);

sector.setAttribute(
"points",
`
180,160
${x0},${y0}
${px},${py}
`
);

requestAnimationFrame(frame);

}

frame();

})();

</script>

---

## Tercera Ley — Distancia y Período Orbital

> Cuanto más lejos está un planeta del Sol, más tiempo tarda en completar una órbita.

<div align="center">

<svg width="600" height="350">

<circle
cx="300"
cy="175"
r="12"
fill="orange"/>

<circle
cx="300"
cy="175"
r="90"
fill="none"
stroke="#66AA66"
stroke-width="2"/>

<circle
cx="300"
cy="175"
r="150"
fill="none"
stroke="#4A90E2"
stroke-width="2"/>

<circle
id="inner"
r="7"
fill="green"/>

<circle
id="outer"
r="7"
fill="steelblue"/>

<text x="380" y="90">
Planeta cercano
</text>

<text x="450" y="175">
Planeta lejano
</text>

</svg>

</div>

<script>

(function(){

const inner=
document.getElementById("inner");

const outer=
document.getElementById("outer");

let t1=0;
let t2=0;

function animate(){

t1+=0.03;

t2+=0.015;

inner.setAttribute(
"cx",
300+90*Math.cos(t1)
);

inner.setAttribute(
"cy",
175+90*Math.sin(t1)
);

outer.setAttribute(
"cx",
300+150*Math.cos(t2)
);

outer.setAttribute(
"cy",
175+150*Math.sin(t2)
);

requestAnimationFrame(
animate
);

}

animate();

})();

</script>

---

## Qué observar

### Primera Ley
- La órbita es una elipse.
- El Sol no está en el centro.

### Segunda Ley
- Cerca del Sol el planeta acelera.
- Lejos del Sol se mueve más despacio.
- El área barrida permanece equivalente.

### Tercera Ley
- El planeta interior completa más órbitas.
- El exterior tarda más.
- Se visualiza la relación:

\\[
T^2 \propto a^3
\\]
