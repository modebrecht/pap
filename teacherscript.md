// VARIANTE ROT
(() => {
  const CFG = {
    NODE_STROKE:  '#111111',
    NODE_FILL:    '#FF0000',
    NODE_TEXT:    '#FFFFFF',
    NODE_STROKE_WIDTH: 2.0,
    DROP_SHADOW: true
  };
  const STYLE_ID = 'pap-contrast-gentle-nodes';
  const css = `
    svg rect, svg ellipse, svg polygon {
      stroke: ${CFG.NODE_STROKE} !important;
      stroke-width: ${CFG.NODE_STROKE_WIDTH}px !important;
      fill: ${CFG.NODE_FILL} !important;
      ${CFG.DROP_SHADOW ? 'filter: drop-shadow(0 0.5px 0 rgba(0,0,0,.25));' : ''}
      shape-rendering: geometricPrecision;
    }
    svg text {
      fill: ${CFG.NODE_TEXT} !important;
      font-weight: 700 !important;
      paint-order: stroke;
      stroke: transparent;
      stroke-width: 0;
    }
  `;
  function on() {
    if (document.getElementById(STYLE_ID)) return;
    const s = document.createElement('style');
    s.id = STYLE_ID;
    s.textContent = css;
    document.head.appendChild(s);
  }
  function off() {
    const s = document.getElementById(STYLE_ID);
    if (s) s.remove();
  }
  function toggle() { document.getElementById(STYLE_ID) ? off() : on(); }
  window.papContrast = { on, off, toggle, CFG }; // <- kleine API
  window.addEventListener('keydown', (e) => { if (e.key.toLowerCase() === 'h') toggle(); });
  on();
  console.log('PAP: sanfter Node-High-Contrast aktiv. Toggle mit Taste "H".');
})();


// VARIANTE B_W
(() => {
  const CFG = {
    NODE_STROKE:  '#111111',
    NODE_FILL:    '#FFFFFF',
    NODE_TEXT:    '#111111',
    NODE_STROKE_WIDTH: 2.0,
    DROP_SHADOW: true
  };
  const STYLE_ID = 'pap-contrast-gentle-nodes';
  const css = `
    svg rect, svg ellipse, svg polygon {
      stroke: ${CFG.NODE_STROKE} !important;
      stroke-width: ${CFG.NODE_STROKE_WIDTH}px !important;
      fill: ${CFG.NODE_FILL} !important;
      ${CFG.DROP_SHADOW ? 'filter: drop-shadow(0 0.5px 0 rgba(0,0,0,.25));' : ''}
      shape-rendering: geometricPrecision;
    }
    svg text {
      fill: ${CFG.NODE_TEXT} !important;
      font-weight: 700 !important;
      paint-order: stroke;
      stroke: transparent;
      stroke-width: 0;
    }
  `;
  function on() {
    if (document.getElementById(STYLE_ID)) return;
    const s = document.createElement('style');
    s.id = STYLE_ID;
    s.textContent = css;
    document.head.appendChild(s);
  }
  function off() {
    const s = document.getElementById(STYLE_ID);
    if (s) s.remove();
  }
  function toggle() { document.getElementById(STYLE_ID) ? off() : on(); }
  window.papContrast = { on, off, toggle, CFG }; // <- kleine API
  window.addEventListener('keydown', (e) => { if (e.key.toLowerCase() === 'h') toggle(); });
  on();
  console.log('PAP: sanfter Node-High-Contrast aktiv. Toggle mit Taste "H".');
})();

// CONNECTOR Farbe ändern
(() => {
  // ======= KONFIG =======
  const CFG = {
    COLOR: '#111111',   // Linienfarbe der Verbindungen
    WIDTH: 2.2,         // Linienstärke der Verbindungen
    DASH:  null         // z.B. '6,4' für gestrichelt, oder null für durchgezogen
  };
  // ======================
  const STYLE_ID = 'pap-connector-contrast';
  const CSS = `
    /* Nur Verbindungen (line/path), KEINE Marker (Pfeilspitzen) */
    svg :not(marker) > line,
    svg :not(marker) > path {
      stroke: ${CFG.COLOR} !important;
      stroke-width: ${CFG.WIDTH}px !important;
      ${CFG.DASH ? `stroke-dasharray: ${CFG.DASH} !important;` : ''}
    }
    /* Pfeilspitzen (marker) NICHT überschreiben */
    svg marker, svg marker * { /* absichtlich leer */ }
  `;
  function on() {
    if (document.getElementById(STYLE_ID)) return;
    const s = document.createElement('style');
    s.id = STYLE_ID;
    s.textContent = CSS;
    document.head.appendChild(s);
    console.log('Connector-Contrast: AN');
  }
  function off() {
    const s = document.getElementById(STYLE_ID);
    if (s) s.remove();
    console.log('Connector-Contrast: AUS');
  }
  function toggle() {
    document.getElementById(STYLE_ID) ? off() : on();
  }
  // Optional kleine API
  window.papConnectors = { on, off, toggle, CFG };
  // Taste "L" = Toggle
  window.addEventListener('keydown', (e) => {
    if (e.key.toLowerCase() === 'l') toggle();
  });
  on(); // beim Einfügen gleich aktivieren
  'OK';
})();