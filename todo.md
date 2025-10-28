[x] #t1 Fix stale `state.pendingLinkFrom` when a source node gets deleted while link mode is active. Deleting the node should cancel link mode to avoid orphan connectors and misleading toasts.
[x] #t2 Clear the JSON import file input after processing so the same file can be re-imported consecutively.
[x] #t3 Revoke the object URL used during JSON export after triggering the download to prevent leaks.
[x] #t4 Ensure global keyboard shortcuts ignore events originating from text inputs (apply `isTypingTarget` guard before handling Delete / Ctrl+D).
[] #t5 Keep fallback autosave safe when starting a new project (preserve `pap.autosave.fallback` or move current snapshot to `pap.autosave.previous` before clearing the tab key).
[] #t6 Touch/pen dragging is broken for nodes (mouse-only listeners) - switch node drag to pointerdown/pointermove/pointerup with capture so tablets can drag.
[] #f1: CTRL+Z / undo
---
t=task
f=feature
