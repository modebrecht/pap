const raw = localStorage.getItem('pap.autosave.previous');
if(raw){
  const snapshot = JSON.parse(raw);
  window.loadFromData(snapshot);
  toast.textContent = 'Vorheriges Projekt geladen';
}else{
  console.warn('Nichts zu restaurieren');
}