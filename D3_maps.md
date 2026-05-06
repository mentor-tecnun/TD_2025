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
  code {
    font-size: 0.75em;
  }
---

# D3.js
## Mapas y Cartografía con SVG en el navegador

Visualización geográfica con **GeoJSON**, **proyecciones** y **path generators**.

---

## ¿Por qué D3 para mapas?

- D3 incluye **d3-geo**: proyecciones, path generators y utilidades geográficas
- Trabaja con el estándar **GeoJSON** y con **TopoJSON** (formato compacto)
- Más de **50 proyecciones** disponibles (Mercator, Robinson, Orthographic…)
- Total control sobre colores, tooltips, animaciones y capas
- Se integra con el resto de D3: escalas, eventos, transiciones

> Para hacer un mapa con D3 solo necesitas tres piezas:
> **datos geográficos**, una **proyección** y un **path generator**.

---

## GeoJSON — el formato estándar

```json
{
  "type": "FeatureCollection",
  "features": [
    {
      "type": "Feature",
      "properties": { "name": "Donostia", "pop": 187000 },
      "geometry": {
        "type": "Polygon",
        "coordinates": [[ [-1.99,43.30], [-1.97,43.30],
                          [-1.97,43.33], [-1.99,43.33],
                          [-1.99,43.30] ]]
      }
    }
  ]
}
```

> ⚠️ Las coordenadas van siempre en orden **[longitud, latitud]** — al revés del GPS.

Tipos de geometría: `Point`, `LineString`, `Polygon`,
`MultiPolygon`, `GeometryCollection`, `FeatureCollection`.

---

## TopoJSON — la versión compacta

TopoJSON codifica la **topología**: los bordes compartidos entre polígonos
se almacenan una sola vez → hasta un **80 % menos de tamaño**.

```js
// Necesitas la librería topojson-client para convertir a GeoJSON
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>

// topojson.feature → convierte un objeto a FeatureCollection GeoJSON
const geojson = topojson.feature(topology, topology.objects.countries);

// topojson.mesh → genera solo los bordes (útil para fronteras)
const fronteras = topojson.mesh(topology, topology.objects.countries,
  (a, b) => a !== b   // solo bordes entre regiones distintas
);
```

| | GeoJSON | TopoJSON |
|---|---|---|
| Tamaño | Mayor | ~80 % menor |
| Bordes compartidos | Duplicados | Una sola vez |
| Uso directo en D3 | ✅ sí | Requiere `topojson.feature()` |

---

## Proyecciones

Una **proyección** convierte coordenadas esféricas `[lng, lat]`
en coordenadas cartesianas `[x, y]` para el SVG.

```js
// Proyección Mercator (la más familiar)
const projection = d3.geoMercator()
  .scale(130)                    // zoom
  .center([0, 20])               // [lng, lat] del centro
  .translate([width/2, height/2]); // píxeles del centro SVG

// .fitSize() calcula scale + translate automáticamente ← úsalo siempre
const projection = d3.geoMercator()
  .fitSize([width, height], geojsonObject);

// Uso: convertir coordenadas geográficas a píxeles
const [x, y] = projection([-1.98, 43.32]); // Donostia → píxeles
```

| Proyección D3 | Uso típico |
|---|---|
| `geoMercator` | Mapas web, navegación |
| `geoNaturalEarth1` | Mapas del mundo "bonitos" |
| `geoOrthographic` | Globo terráqueo 3D |
| `geoTransverseMercator` | Regiones pequeñas, alta precisión |

---

## Path Generator

El **path generator** convierte una geometría GeoJSON
en el atributo `d` de un `<path>` SVG.

```js
// 1. Crear la proyección
const projection = d3.geoNaturalEarth1().fitSize([W, H], geojson);

// 2. Crear el path generator pasando la proyección
const path = d3.geoPath(projection);

// 3. Dibujar cada Feature como un <path>
svg.selectAll("path")
  .data(geojson.features)
  .join("path")
    .attr("d", path)          // path(feature) → string SVG "M x,y L x,y ..."
    .attr("fill", "steelblue")
    .attr("stroke", "white")
    .attr("stroke-width", 0.5);

// Extras útiles:
const [cx, cy] = path.centroid(feature);  // centroide → píxeles
const area     = path.area(feature);      // área en px²
```

> `.call(d3.geoPath())` sin proyección trabaja en coordenadas planas
> (útil con TopoJSON pre-proyectado).

---

## Patrón completo — mapa mínimo

```js
const W = 500, H = 300;

// 1. SVG
const svg = d3.select("#mapa").append("svg")
  .attr("width", W).attr("height", H);

// 2. Cargar TopoJSON
const data = await d3.json(
  "https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json"
);

// 3. Convertir a GeoJSON
const countries = topojson.feature(data, data.objects.countries);

// 4. Proyección — fitSize ajusta automáticamente
const projection = d3.geoNaturalEarth1().fitSize([W, H], countries);

// 5. Path generator
const path = d3.geoPath(projection);

// 6. Dibujar
svg.selectAll("path")
  .data(countries.features)
  .join("path")
    .attr("d", path)
    .attr("fill", "#4a9eca")
    .attr("stroke", "white")
    .attr("stroke-width", 0.4);
```

---

## Ejemplo: Mapa del Mundo

<textarea id="code-world" oninput="runExample('code-world','preview-world')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H)
  .style('background','#a8cce4');
const proj=d3.geoNaturalEarth1().scale(60).translate([W/2,H/2]);
const path=d3.geoPath(proj);
d3.json('https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json').then(world=>{
  const countries=topojson.feature(world,world.objects.countries);
  svg.append('g').selectAll('path').data(countries.features).join('path')
    .attr('d',path).attr('fill','#2d6a4f').attr('stroke','#74c69d').attr('stroke-width',0.4);
  svg.append('path').datum(d3.geoGraticule()())
    .attr('d',path).attr('fill','none')
    .attr('stroke','rgba(255,255,255,0.2)').attr('stroke-width',0.5);
});
</script></textarea>

<div class="preview"><iframe id="preview-world"></iframe></div>

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

## Ejemplo: Globo Terráqueo (`geoOrthographic`)

<textarea id="code-globe" oninput="runExample('code-globe','preview-globe')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const W=380,H=178,R=82;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H);
const proj=d3.geoOrthographic().scale(R).translate([W/2,H/2]).clipAngle(90);
const path=d3.geoPath(proj);
svg.append('circle').attr('cx',W/2).attr('cy',H/2).attr('r',R).attr('fill','#1e3a5f');
d3.json('https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json').then(world=>{
  const countries=topojson.feature(world,world.objects.countries);
  const mesh=topojson.mesh(world,world.objects.countries,(a,b)=>a!==b);
  const g=svg.append('g').selectAll('path').data(countries.features).join('path')
    .attr('d',path).attr('fill','#2d6a4f').attr('stroke','none');
  const border=svg.append('path').datum(mesh)
    .attr('fill','none').attr('stroke','#74c69d').attr('stroke-width',0.5);
  const grat=svg.append('path').datum(d3.geoGraticule()())
    .attr('fill','none').attr('stroke','rgba(255,255,255,0.15)').attr('stroke-width',0.5);
  d3.timer(t=>{
    proj.rotate([t/120,-20]);
    g.attr('d',path); border.attr('d',path); grat.attr('d',path);
  });
});
</script></textarea>

<div class="preview"><iframe id="preview-globe"></iframe></div>

---

## Ejemplo: España — Provincias

<textarea id="code-spain" oninput="runExample('code-spain','preview-spain')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H);
const color=d3.scaleOrdinal(d3.schemeTableau10);
d3.json('https://unpkg.com/es-atlas@0.5.0/es/provinces.json').then(es=>{
  const prov=topojson.feature(es,es.objects.provinces);
  const proj=d3.geoMercator().fitSize([W,H],prov);
  const path=d3.geoPath(proj);
  svg.append('g').selectAll('path').data(prov.features).join('path')
    .attr('d',path).attr('fill',(d,i)=>color(i%10))
    .attr('stroke','white').attr('stroke-width',0.5).attr('opacity',0.85);
  const ccaa=topojson.mesh(es,es.objects.autonomous_regions,(a,b)=>a!==b);
  svg.append('path').datum(ccaa)
    .attr('d',path).attr('fill','none').attr('stroke','#111').attr('stroke-width',1.2);
});
</script></textarea>

<div class="preview"><iframe id="preview-spain"></iframe></div>

---

## Ejemplo: Comunidades Autónomas — Mapa Coroplético

<textarea id="code-ccaa" oninput="runExample('code-ccaa','preview-ccaa')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
// PIB per cápita relativo (datos de ejemplo, índice España=100)
const pib={'01':88,'02':77,'03':91,'04':87,'05':83,'06':72,'07':105,
  '08':103,'09':132,'10':71,'11':88,'12':119,'13':72,'14':76,
  '15':90,'16':130,'17':92};
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H);
const color=d3.scaleSequential(d3.interpolateYlGnBu).domain([65,140]);
d3.json('https://unpkg.com/es-atlas@0.5.0/es/autonomous_regions.json').then(es=>{
  const ccaa=topojson.feature(es,es.objects.autonomous_regions);
  const proj=d3.geoMercator().fitSize([W,H],ccaa);
  const path=d3.geoPath(proj);
  svg.append('g').selectAll('path').data(ccaa.features).join('path')
    .attr('d',path)
    .attr('fill',d=>color(pib[d.properties.cod_ccaa]||90))
    .attr('stroke','white').attr('stroke-width',0.8);
});
</script></textarea>

<div class="preview"><iframe id="preview-ccaa"></iframe></div>

---

## Ejemplo: País Vasco

<textarea id="code-euskadi" oninput="runExample('code-euskadi','preview-euskadi')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const codigos=['01','20','48']; // Araba, Gipuzkoa, Bizkaia (cod_prov INE)
const colores={'01':'#1b4332','20':'#40916c','48':'#95d5b2'};
const noms={'01':'Araba/Álava','20':'Gipuzkoa','48':'Bizkaia'};
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H)
  .style('background','#e8f4f8');
d3.json('https://unpkg.com/es-atlas@0.5.0/es/provinces.json').then(es=>{
  const all=topojson.feature(es,es.objects.provinces);
  const euskadi={type:'FeatureCollection',
    features:all.features.filter(d=>codigos.includes(d.id))};
  const proj=d3.geoMercator().fitExtent([[15,10],[W-15,H-10]],euskadi);
  const path=d3.geoPath(proj);
  // Fondo: resto de provincias en gris
  svg.append('g').selectAll('path').data(all.features).join('path')
    .attr('d',path).attr('fill','#dde4ea').attr('stroke','white').attr('stroke-width',0.3);
  // Provincias vascas en color
  svg.append('g').selectAll('path').data(euskadi.features).join('path')
    .attr('d',path).attr('fill',d=>colores[d.id])
    .attr('stroke','white').attr('stroke-width',1.5);
  // Etiquetas en el centroide
  svg.selectAll('text').data(euskadi.features).join('text')
    .attr('x',d=>path.centroid(d)[0]).attr('y',d=>path.centroid(d)[1])
    .attr('text-anchor','middle').attr('font-size','11px')
    .attr('fill','white').attr('font-weight','bold').attr('pointer-events','none')
    .text(d=>noms[d.id]);
});
</script></textarea>

<div class="preview"><iframe id="preview-euskadi"></iframe></div>

---

## Ejemplo: Mapa interactivo con tooltip

<textarea id="code-tooltip" oninput="runExample('code-tooltip','preview-tooltip')">
<style>
#tip{position:fixed;background:#0f172a;color:#e2e8f0;padding:5px 10px;
  border-radius:5px;font:12px sans-serif;display:none;pointer-events:none;
  border:1px solid #38bdf8}
</style>
<div id="tip"></div><div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H);
const tip=d3.select('#tip');
const colorScale=d3.scaleOrdinal(d3.schemePastel1);
d3.json('https://unpkg.com/es-atlas@0.5.0/es/provinces.json').then(es=>{
  const prov=topojson.feature(es,es.objects.provinces);
  const proj=d3.geoMercator().fitSize([W,H],prov);
  const path=d3.geoPath(proj);
  svg.append('g').selectAll('path').data(prov.features).join('path')
    .attr('d',path).attr('fill',(d,i)=>colorScale(i%9))
    .attr('stroke','#333').attr('stroke-width',0.5)
    .on('mouseover',function(e,d){
      d3.select(this).attr('stroke','#38bdf8').attr('stroke-width',2).raise();
      tip.style('display','block')
        .style('left',e.clientX+12+'px').style('top',e.clientY-30+'px')
        .text(d.properties.name||'Provincia');
    })
    .on('mousemove',e=>tip.style('left',e.clientX+12+'px').style('top',e.clientY-30+'px'))
    .on('mouseout',function(){
      d3.select(this).attr('stroke','#333').attr('stroke-width',0.5);
      tip.style('display','none');
    });
});
</script></textarea>

<div class="preview"><iframe id="preview-tooltip"></iframe></div>

---

## Ejemplo: Ciudades sobre el mapa

<textarea id="code-puntos" oninput="runExample('code-puntos','preview-puntos')">
<div id="g"></div>
<script src="https://cdn.jsdelivr.net/npm/d3@7"></script>
<script src="https://cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js"></script>
<script>
const ciudades=[
  {n:'Bilbao',lng:-2.935,lat:43.263,pop:346},
  {n:'Donostia',lng:-1.981,lat:43.318,pop:187},
  {n:'Gasteiz',lng:-2.673,lat:42.849,pop:249}
];
const codigos=['01','20','48'];
const W=380,H=178;
const svg=d3.select('#g').append('svg').attr('width',W).attr('height',H)
  .style('background','#eaf4f8');
d3.json('https://unpkg.com/es-atlas@0.5.0/es/provinces.json').then(es=>{
  const all=topojson.feature(es,es.objects.provinces);
  const region={type:'FeatureCollection',
    features:all.features.filter(d=>codigos.includes(d.id))};
  const proj=d3.geoMercator().fitExtent([[10,10],[W-10,H-10]],region);
  const path=d3.geoPath(proj);
  svg.append('g').selectAll('path').data(all.features).join('path')
    .attr('d',path).attr('fill','#d8e4ea').attr('stroke','white').attr('stroke-width',0.3);
  svg.append('g').selectAll('path').data(region.features).join('path')
    .attr('d',path).attr('fill','#b7d7e8').attr('stroke','white').attr('stroke-width',0.8);
  // Burbujas proporcionales a la población
  svg.selectAll('circle').data(ciudades).join('circle')
    .attr('cx',d=>proj([d.lng,d.lat])[0]).attr('cy',d=>proj([d.lng,d.lat])[1])
    .attr('r',d=>Math.sqrt(d.pop)*0.55)
    .attr('fill','#e63946').attr('stroke','white').attr('stroke-width',1.2).attr('opacity',0.85);
  svg.selectAll('text').data(ciudades).join('text')
    .attr('x',d=>proj([d.lng,d.lat])[0]+8).attr('y',d=>proj([d.lng,d.lat])[1]+4)
    .attr('font-size','9px').attr('fill','#1a1a2e').attr('font-weight','bold')
    .text(d=>d.n);
});
</script></textarea>

<div class="preview"><iframe id="preview-puntos"></iframe></div>

---

## Escalas de color para coropletas

```js
// Secuencial (1 variable numérica: bajo → alto)
const color = d3.scaleSequential(d3.interpolateYlGnBu).domain([0, 100]);

// Divergente (valores positivos y negativos desde un centro)
const color = d3.scaleDiverging(d3.interpolateRdBu).domain([-50, 0, 50]);

// Ordinal (categorías discretas)
const color = d3.scaleOrdinal(d3.schemeTableau10);

// Uso en data join
svg.selectAll('path')
  .data(geojson.features)
  .join('path')
    .attr('fill', d => {
      const val = datos.get(d.properties.cod_prov); // Map() con tus datos
      return val !== undefined ? color(val) : '#ccc';  // fallback si no hay dato
    });

// Escalas secuenciales disponibles:
// interpolateBlues · Reds · Greens · Oranges · Purples
// interpolateYlGnBu · YlOrRd · PuBuGn · RdPu
// interpolatePlasma · Viridis · Inferno · Magma
```

---

## Actualizar datos — datos dinámicos en mapas

El mismo patrón `enter / update / exit` funciona para mapas:

```js
function actualizar(nuevosDatos) {
  const colorScale = d3.scaleSequential(d3.interpolateReds)
    .domain(d3.extent(nuevosDatos, d => d.valor));

  // La proyección y path no cambian → solo actualizamos el fill
  svg.selectAll('path')
    .data(geojson.features, d => d.properties.cod_ccaa)  // key function
    .join('path')
      .transition().duration(600)
      .attr('fill', d => {
        const dato = nuevosDatos.find(x => x.id === d.properties.cod_ccaa);
        return dato ? colorScale(dato.valor) : '#eee';
      });
}

// Llamar con datos nuevos cada vez que cambien
actualizar(datos2023);
```

> `key function` en `.data(arr, d => d.id)` es **esencial**
> para que las transiciones sean correctas al actualizar.

---

## CDNs recomendados

| Dataset | URL |
|---|---|
| Mundo (110m) | `cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json` |
| Mundo (50m) | `cdn.jsdelivr.net/npm/world-atlas@2/countries-50m.json` |
| España — provincias | `unpkg.com/es-atlas@0.5.0/es/provinces.json` |
| España — CCAA | `unpkg.com/es-atlas@0.5.0/es/autonomous_regions.json` |
| topojson-client | `cdn.jsdelivr.net/npm/topojson-client@3/dist/topojson-client.min.js` |
| d3 v7 | `cdn.jsdelivr.net/npm/d3@7` |

Para el **País Vasco a nivel municipal** existe el portal
[opendata.euskadi.eus](https://opendata.euskadi.eus) con datos en WFS/GeoJSON.
En clase bastará con filtrar `cod_prov` del es-atlas como en los ejemplos.

---

## Buenas prácticas

- Usa siempre **`.fitSize()` o `.fitExtent()`** — calculan `scale` y `translate` solos
- Prefiere **TopoJSON** sobre GeoJSON para archivos grandes (80 % menos peso)
- Guarda los bordes con **`topojson.mesh()`** para dibujarlos en una sola `<path>` (más eficiente)
- Filtra las features **antes** de proyectar para mapas regionales
- Une datos externos con las features mediante una **key function**: `d.properties.cod_prov`, `d.id`…
- Usa **`path.centroid(d)`** para colocar etiquetas en el centro geográfico
- Añade manejadores de eventos directamente en el **data join** (igual que en barras)

---

## Uso

Edita cualquier `textarea` de la presentación
y el mapa SVG se actualiza en tiempo real.

> Recuerda: D3 no te da mapas — te da **herramientas**
> para construir exactamente la visualización geográfica que imaginas.
