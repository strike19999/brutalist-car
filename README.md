# brutalist-car
Generador interactivo de coches brutalistas en 2D con edición por arrastre de vértices. Permite ajustar parámetros como largo, altura, techo, ruedas y chaflanes. Incluye exportación a SVG/PNG y guardado/carga de formas en JSON. Funciona 100% en el navegador.
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Generador de Coche Brutalista — Procedural + Drag</title>
<style>
  :root { --bg:#0f1115; --panel:#151821; --ink:#e7ebf4; --muted:#9aa3b2; --accent:#7bd7ff; }
  * { box-sizing: border-box; }
  html,body { margin:0; height:100%; background:var(--bg); color:var(--ink); font:14px/1.35 system-ui, -apple-system, Segoe UI, Roboto, sans-serif; }
  .wrap { display:grid; grid-template-columns: 320px 1fr; gap:16px; height:100%; padding:16px; }
  .panel { background:var(--panel); border:1px solid #202431; border-radius:12px; padding:14px; overflow:auto; }
  h1 { font-size:16px; margin:0 0 8px; }
  h2 { font-size:13px; margin:14px 0 6px; color:var(--muted); text-transform:uppercase; letter-spacing:.06em; }
  label { display:flex; gap:8px; align-items:center; margin:8px 0; }
  input[type="range"] { width:100%; }
  input[type="text"] { width:100%; padding:8px; background:#0e1016; border:1px solid #242a36; border-radius:8px; color:var(--ink); }
  button { cursor:pointer; padding:8px 10px; background:#0e1016; border:1px solid #2a3242; border-radius:8px; color:var(--ink); }
  button.primary { background:#0c1b26; border-color:#33526b; }
  button:active { transform:translateY(1px); }
  .row { display:flex; gap:8px; align-items:center; }
  .row > * { flex:1; }
  .help { color:var(--muted); font-size:12px; margin-top:6px; }
  .canvas-wrap { position:relative; background:#0a0c11; border:1px solid #1b2030; border-radius:12px; }
  svg { width:100%; height:100%; display:block; background:linear-gradient(#0a0c11,#0b0e14); }
  .grid line { opacity:0.15; }
  .handle { cursor:grab; }
  .handle:active { cursor:grabbing; }
  .tag { position:absolute; top:10px; left:10px; background:#0d121a; border:1px solid #1d2736; padding:6px 8px; border-radius:8px; color:var(--muted); }
  .kbd { font:600 11px ui-monospace, SFMono-Regular, Menlo, Consolas, monospace; background:#10131a; border:1px solid #1f2735; padding:1px 5px; border-radius:6px; }
  textarea { width:100%; min-height:90px; background:#0e1016; border:1px solid #242a36; border-radius:8px; color:var(--ink); padding:8px; }
  .small { font-size:12px; color:var(--muted); }
</style>
</head>
<body>
<div class="wrap">
  <div class="panel">
    <h1>Generador Brutalista</h1>
    <div class="row">
      <button id="btnNew" class="primary">Nuevo aleatorio</button>
      <button id="btnReset">Revertir a seed</button>
    </div>
    <label>Seed
      <input id="seed" type="text" placeholder="ej: 12345 ó texto" />
    </label>

    <h2>Parámetros</h2>
    <label>Largo
      <input id="length" type="range" min="420" max="900" step="1" />
      <span id="lengthv"></span>
    </label>
    <label>Alto
      <input id="height" type="range" min="160" max="340" step="1" />
      <span id="heightv"></span>
    </label>
    <label>Blockiness (chaflanes)
      <input id="chamfer" type="range" min="0" max="120" step="1" />
      <span id="chamferv"></span>
    </label>
    <label>Techo (altura)
      <input id="roofH" type="range" min="0.4" max="0.98" step="0.01" />
      <span id="roofHv"></span>
    </label>
    <label>Techo (largo)
     