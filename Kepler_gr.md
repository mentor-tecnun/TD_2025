<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Las Leyes de Kepler</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" rel="stylesheet">
  <style>
    body { font-family: 'Segoe UI', system-ui, sans-serif; }
    .slide { scroll-snap-align: start; }
    .math { font-size: 1.1em; }
  </style>
</head>
<body class="bg-gray-50 text-gray-800">

  <div class="max-w-4xl mx-auto">

    <!-- Título -->
    <div class="bg-blue-800 text-white py-16 text-center">
      <h1 class="text-5xl font-bold mb-2">Las Leyes de Kepler</h1>
      <p class="text-xl opacity-90">El movimiento de los planetas</p>
      <p class="mt-4 text-sm">Astronomía • Física</p>
    </div>

    <!-- Contenido -->
    <div class="p-8 space-y-16">

      <!-- Kepler -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700 border-b-4 border-blue-500 pb-2 mb-6">Johannes Kepler (1571-1630)</h2>
        <ul class="grid grid-cols-1 md:grid-cols-2 gap-4 text-lg">
          <li class="flex items-start gap-3"><i class="fas fa-check text-green-500 mt-1"></i> Matemático y astrónomo alemán</li>
          <li class="flex items-start gap-3"><i class="fas fa-check text-green-500 mt-1"></i> Discípulo de Tycho Brahe</li>
          <li class="flex items-start gap-3"><i class="fas fa-check text-green-500 mt-1"></i> Formuló sus tres leyes entre 1609 y 1619</li>
          <li class="flex items-start gap-3"><i class="fas fa-check text-green-500 mt-1"></i> Basó su trabajo en observaciones precisas</li>
        </ul>
        <blockquote class="mt-6 italic border-l-4 border-blue-500 pl-6 text-gray-600">
          "No invento hipótesis, las deduzco de los datos"
        </blockquote>
      </section>

      <!-- Primera Ley -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700">1ª Ley de Kepler</h2>
        <h3 class="text-2xl mt-2">Ley de las órbitas elípticas</h3>
        <p class="text-xl mt-4 font-medium">"Todos los planetas se mueven en órbitas elípticas con el Sol en uno de los focos."</p>
        
        <div class="mt-8 text-center">
          <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/Kepler_ellipse.svg/800px-Kepler_ellipse.svg.png" 
               alt="Órbita elíptica" class="mx-auto rounded-lg shadow-lg">
        </div>
      </section>

      <!-- Elementos de la elipse -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700">Elementos de una Elipse</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mt-6">
          <div class="bg-white p-6 rounded-xl shadow">
            <strong>Semieje mayor (a)</strong>: Distancia promedio al Sol
          </div>
          <div class="bg-white p-6 rounded-xl shadow">
            <strong>Excentricidad (e)</strong>: 0 = círculo, 0 < e < 1 = elipse
          </div>
          <div class="bg-white p-6 rounded-xl shadow">
            <strong>Perihelio</strong>: Punto más cercano al Sol
          </div>
          <div class="bg-white p-6 rounded-xl shadow">
            <strong>Afelio</strong>: Punto más lejano al Sol
          </div>
        </div>
      </section>

      <!-- Segunda Ley -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700">2ª Ley de Kepler</h2>
        <h3 class="text-2xl mt-2">Ley de las áreas iguales</h3>
        <p class="text-xl mt-4">"El radio vector que une al Sol con el planeta barre áreas iguales en tiempos iguales."</p>
        
        <pre class="bg-gray-900 text-green-400 p-6 rounded-xl mt-8 text-sm overflow-auto"> 
          Área 1                  Área 2
   •••••••••••••••••••     •••••••••••••••••••
  •                     •   •                     •
 •                       • •                       •
•           Sol          ••           Sol          •
 •                       • •                       •
  •                     •   •                     •
   •••••••••••••••••••     •••••••••••••••••••
        </pre>
        <p class="text-center mt-4 font-medium">El planeta se mueve más rápido cerca del Sol</p>
      </section>

      <!-- Tercera Ley -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700">3ª Ley de Kepler</h2>
        <h3 class="text-2xl mt-2">Ley armónica</h3>
        <p class="text-xl mt-6">"El cuadrado del período orbital es proporcional al cubo del semieje mayor."</p>
        <div class="bg-white p-8 rounded-2xl shadow mt-8 text-center">
          <p class="text-3xl font-mono text-blue-600">T² ∝ a³</p>
          <p class="mt-4 text-gray-600">Para el Sistema Solar: T² / a³ = constante</p>
        </div>
      </section>

      <!-- Tabla -->
      <section class="slide">
        <h2 class="text-3xl font-bold text-blue-700">Comparación de Planetas</h2>
        <table class="w-full mt-6 border-collapse">
          <thead>
            <tr class="bg-blue-700 text-white">
              <th class="p-4 text-left">Planeta</th>
              <th class="p-4">Semieje mayor (UA)</th>
              <th class="p-4">Período (años)</th>
              <th class="p-4">T² / a³</th>
            </tr>
          </thead>
          <tbody class="divide-y">
            <tr><td class="p-4">Mercurio</td><td class="p-4 text-center">0.387</td><td class="p-4 text-center">0.241</td><td class="p-4 text-center">1.00</td></tr>
            <tr><td class="p-4">Venus</td><td class="p-4 text-center">0.723</td><td class="p-4 text-center">0.615</td><td class="p-4 text-center">1.00</td></tr>
            <tr><td class="p-4">Tierra</td><td class="p-4 text-center">1.000</td><td class="p-4 text-center">1.000</td><td class="p-4 text-center">1.00</td></tr>
            <tr><td class="p-4">Marte</td><td class="p-4 text-center">1.524</td><td class="p-4 text-center">1.881</td><td class="p-4 text-center">1.00</td></tr>
            <tr><td class="p-4">Júpiter</td><td class="p-4 text-center">5.203</td><td class="p-4 text-center">11.86</td><td class="p-4 text-center">1.00</td></tr>
            <tr><td class="p-4">Saturno</td><td class="p-4 text-center">9.537</td><td class="p-4 text-center">29.46</td><td class="p-4 text-center">1.00</td></tr>
          </tbody>
        </table>
      </section>

      <!-- Importancia -->
      <section class="slide bg-gradient-to-r from-blue-50 to-indigo-50 p-10 rounded-3xl">
        <h2 class="text-3xl font-bold text-blue-700">Importancia</h2>
        <ul class="mt-8 space-y-4 text-lg">
          <li>🔹 Rompió 2000 años de creencia en órbitas circulares</li>
          <li>🔹 Base para la Ley de la Gravitación de Newton</li>
          <li>🔹 Demostró que las leyes físicas son universales</li>
          <li>🔹 Fundamento de la mecánica celeste moderna</li>
        </ul>
      </section>

      <!-- Resumen -->
      <section class="slide text-center py-12">
        <h2 class="text-4xl font-bold mb-8">Resumen</h2>
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <div class="bg-white p-8 rounded-2xl shadow">
            <span class="text-2xl">1</span>
            <p class="font-semibold mt-2">Órbitas elípticas</p>
          </div>
          <div class="bg-white p-8 rounded-2xl shadow">
            <span class="text-2xl">2</span>
            <p class="font-semibold mt-2">Áreas iguales en tiempos iguales</p>
          </div>
          <div class="bg-white p-8 rounded-2xl shadow">
            <span class="text-2xl">3</span>
            <p class="font-semibold mt-2">T² ∝ a³</p>
          </div>
        </div>
      </section>

    </div>

    <footer class="text-center py-12 text-gray-500 text-sm">
      Las Leyes de Kepler • 1609 - 1619<br>
      Fuente: Astronomia Nova y Harmonices Mundi
    </footer>

  </div>

</body>
</html>
