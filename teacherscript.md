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
