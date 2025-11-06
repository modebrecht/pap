//Fallback-Skript (ohne :has()), inkl. H/L-Toggle
(() => {
  // =========================
  // KONFIGURATION (anpassen)
  // =========================
  const CFG_NODES = {
    STROKE:  '#111111',  // 1) Rahmenfarbe der Knoten
    FILL:    '#FFFFFF',  // 2) Hintergrundfarbe der Knoten
    TEXT:    '#111111',  // 3) Schriftfarbe NUR auf Knoten
    STROKE_WIDTH: 2.0,
    DROP_SHADOW: true,
    // Optional: Connector-Text explizit setzen (null = nicht anfassen)
    CONNECTOR_TEXT: null // z.B. '#111111' oder null
  };

  const CFG_CONN = {
    COLOR: '#111111', // Linienfarbe der Verbindungen (line/path)
    WIDTH: 2.2,       // Linienstärke
    DASH:  null       // '6,4' für gestrichelt, sonst null
  };

  // =========================
  // STYLE-IDs (nicht ändern)
  // =========================
  const ID_NODES = 'pap-contrast-nodes-fallback';
  const ID_CONN  = 'pap-contrast-connectors';

  // =========================
  // HILFSFUNKTIONEN
  // =========================
  function addStyle(id, css) {
    let s = document.getElementById(id);
    if (!s) { s = document.createElement('style'); s.id = id; document.head.appendChild(s); }
    s.textContent = css;
  }
  function removeStyle(id) {
    const s = document.getElementById(id);
    if (s) s.remove();
  }
  const isOn = id => !!document.getElementById(id);

  // Alle <g>, die Knoten-Shapes enthalten, als .pap-node markieren
  function markNodeGroups() {
    document.querySelectorAll('svg g').forEach(g => {
      if (!g.classList.contains('pap-node') && g.querySelector('rect, ellipse, polygon')) {
        g.classList.add('pap-node');
      }
    });
  }

  // Bei DOM-Änderungen erneut markieren (falls der Builder live neu zeichnet)
  const svgRoot = document.querySelector('svg');
  if (svgRoot) {
    const mo = new MutationObserver(() => markNodeGroups());
    mo.observe(svgRoot, { childList: true, subtree: true });
  }
  markNodeGroups();

  // =========================
  // NODES (H-TOGGLE)
  // =========================
  function nodesCSS() {
    return `
      /* Knoten-Shapes (rect/ellipse/polygon) */
      svg rect, svg ellipse, svg polygon {
        stroke: ${CFG_NODES.STROKE} !important;
        stroke-width: ${CFG_NODES.STROKE_WIDTH}px !important;
        fill: ${CFG_NODES.FILL} !important;
        ${CFG_NODES.DROP_SHADOW ? 'filter: drop-shadow(0 0.5px 0 rgba(0,0,0,.25));' : ''}
        shape-rendering: geometricPrecision;
      }

      /* NUR Texte in Gruppen, die als .pap-node markiert wurden */
      svg g.pap-node text {
        fill: ${CFG_NODES.TEXT} !important;
        font-weight: 700 !important;
        paint-order: stroke;
        stroke: transparent;
        stroke-width: 0;
      }

      ${CFG_NODES.CONNECTOR_TEXT ? `
        /* Optional: Connector-Texte explizit setzen (Gruppen ohne .pap-node) */
        svg g:not(.pap-node) text {
          fill: ${CFG_NODES.CONNECTOR_TEXT} !important;
        }
      ` : ''}
    `;
  }

  function nodesOn()  { markNodeGroups(); addStyle(ID_NODES, nodesCSS()); console.log('Nodes: AN (H)'); }
  function nodesOff() { removeStyle(ID_NODES);                              console.log('Nodes: AUS (H)'); }
  function nodesToggle() { isOn(ID_NODES) ? nodesOff() : nodesOn(); }

  // =========================
  // CONNECTORS (L-TOGGLE)
  // =========================
  // Greift NUR line/path an – Marker (Pfeilspitzen) bleiben unberührt.
  function connectorsCSS() {
    return `
      svg :not(marker) > line,
      svg :not(marker) > path {
        stroke: ${CFG_CONN.COLOR} !important;
        stroke-width: ${CFG_CONN.WIDTH}px !important;
        ${CFG_CONN.DASH ? `stroke-dasharray: ${CFG_CONN.DASH} !important;` : ''}
      }
      /* Pfeilspitzen (marker) NICHT überschreiben */
      svg marker, svg marker * { /* absichtlich leer */ }
    `;
  }

  function connectorsOn()  { addStyle(ID_CONN, connectorsCSS()); console.log('Connectors: AN (L)'); }
  function connectorsOff() { removeStyle(ID_CONN);                console.log('Connectors: AUS (L)'); }
  function connectorsToggle() { isOn(ID_CONN) ? connectorsOff() : connectorsOn(); }

  // =========================
  // TASTEN
  // =========================
  window.addEventListener('keydown', (e) => {
    const k = e.key.toLowerCase();
    if (k === 'h') nodesToggle();
    if (k === 'l') connectorsToggle();
  });

  // optional direkt aktivieren:
  nodesOn();
  // connectorsOn();

  // Kleine API für die Konsole
  window.papStyleFallback = {
    nodesOn, nodesOff, nodesToggle,
    connectorsOn, connectorsOff, connectorsToggle,
    CFG_NODES, CFG_CONN,
    markNodeGroups
  };

  console.log('Fallback aktiv (ohne :has): H = Nodes, L = Connectors. Konfig oben anpassen. window.papStyleFallback verfügbar.');
})();
