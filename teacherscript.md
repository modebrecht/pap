(() => {
  // =========================
  // KONFIGURATION
  // =========================
  const CFG_NODES = {
    STROKE:  '#111111',  // 1) Rahmenfarbe der Knoten
    FILL:    '#FF0000',  // 2) Hintergrundfarbe der Knoten
    TEXT:    '#FFFFFF',  // 3) Schriftfarbe NUR auf Knoten
    STROKE_WIDTH: 2.0,
    DROP_SHADOW: true,
    // Optional: Connector-Text explizit setzen (null = nicht anfassen)
    CONNECTOR_TEXT: null // z.B. '#111111' oder null
  };

  const CFG_CONN = {
    COLOR: '#111111', // Linienfarbe der Verbindungen (line/path)
    WIDTH: 2.2,       // Linienstärke
    DASH:  null       // z.B. '6,4' für gestrichelt, sonst null
  };

  // =========================
  // STYLE-IDs (nicht ändern)
  // =========================
  const ID_NODES = 'pap-contrast-nodes';
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

  // =========================
  // NODES (H-TOGGLE)
  // =========================
  // ACHTUNG: Die folgenden Selektoren nutzen :has(), um NUR Texte in Knoten-Gruppen zu färben.
  // ► Wenn dein Browser kein :has() kann, lies den Fallback unten und ersetze diese Zeilen.
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

      /* NUR Texte in Gruppen, die Knoten-Shapes enthalten  ← ← ←  HIER wird :has() verwendet */
      svg g:has(rect) text,
      svg g:has(ellipse) text,
      svg g:has(polygon) text {
        fill: ${CFG_NODES.TEXT} !important;
        font-weight: 700 !important;
        paint-order: stroke;
        stroke: transparent;
        stroke-width: 0;
      }

      ${CFG_NODES.CONNECTOR_TEXT ? `
        /* Optional: Connector-Texte (Gruppen OHNE Node-Shapes) explizit setzen */
        svg g:not(:has(rect)):not(:has(ellipse)):not(:has(polygon)) text {
          fill: ${CFG_NODES.CONNECTOR_TEXT} !important;
        }
      ` : ''}
    `;
  }

  function nodesOn()  { addStyle(ID_NODES, nodesCSS()); console.log('Nodes: AN (H)'); }
  function nodesOff() { removeStyle(ID_NODES);          console.log('Nodes: AUS (H)'); }
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
  function connectorsOff() { removeStyle(ID_CONN);               console.log('Connectors: AUS (L)'); }
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
  window.papStyle = {
    nodesOn, nodesOff, nodesToggle,
    connectorsOn, connectorsOff, connectorsToggle,
    CFG_NODES, CFG_CONN
  };

  console.log('PAP Style: H = Nodes, L = Connectors. Konfig oben anpassen. window.papStyle verfügbar.');
})();
