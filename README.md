# Potifly.dj
Una app musica
<!DOCTYPE html>
<html lang="it">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>POTIFLY</title>
<style>
:root{
  --bg:#05060a;
  --panel:#0d1018;
  --card:#141824;
  --line:#242b3d;
  --cyan:#00e5ff;
  --purple:#8b5cff;
  --pink:#ff2fb9;
  --gold:#ffd54a;
  --text:#eef2ff;
  --muted:#93a0b8;
}
*{box-sizing:border-box}
body{
  margin:0;
  padding-bottom:110px;
  color:var(--text);
  font-family:Arial,sans-serif;
  background:
    radial-gradient(circle at 10% 10%, rgba(139,92,255,.25), transparent 35%),
    radial-gradient(circle at 90% 15%, rgba(0,229,255,.18), transparent 35%),
    radial-gradient(circle at 50% 100%, rgba(255,47,185,.18), transparent 40%),
    var(--bg);
}
header{
  position:sticky;top:0;z-index:20;
  display:flex;justify-content:space-between;align-items:center;
  padding:14px 18px;
  background:rgba(8,10,16,.9);
  border-bottom:1px solid var(--line);
  backdrop-filter:blur(12px);
}
.logo{
  font-size:22px;font-weight:bold;letter-spacing:2px;
  background:linear-gradient(90deg,var(--cyan),var(--purple),var(--pink));
  -webkit-background-clip:text;background-clip:text;color:transparent;
}
nav{
  display:flex;gap:8px;padding:12px 18px;overflow-x:auto;
  background:rgba(10,12,18,.8);border-bottom:1px solid var(--line);
  position:sticky;top:59px;z-index:19;
}
nav button{
  white-space:nowrap;border:1px solid var(--line);background:var(--card);
  color:var(--text);padding:10px 15px;border-radius:999px;cursor:pointer;font-weight:bold;
}
nav button.active{
  background:linear-gradient(90deg,var(--cyan),var(--purple));
  color:#04060c;border-color:transparent;
}
main{max-width:1000px;margin:auto;padding:18px}
section{display:none}
section.active{display:block;animation:fade .25s ease}
@keyframes fade{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:none}}
h2{margin:10px 0 16px}
h3{margin:0 0 12px}
.card{
  background:linear-gradient(145deg,rgba(20,24,36,.95),rgba(13,16,24,.95));
  border:1px solid var(--line);border-radius:18px;padding:16px;margin-bottom:16px;
  box-shadow:0 10px 30px rgba(0,0,0,.35);
}
input[type=text]{
  width:100%;padding:12px;border-radius:12px;border:1px solid var(--line);
  background:#0a0d14;color:var(--text);margin-bottom:10px;
}
select{
  width:100%;padding:12px;border-radius:12px;border:1px solid var(--line);
  background:#0a0d14;color:var(--text);margin-bottom:10px;
}
input[type=file]{color:var(--muted);margin-bottom:10px}
button.primary{
  border:none;cursor:pointer;color:#04060c;font-weight:bold;
  padding:12px 16px;border-radius:12px;
  background:linear-gradient(90deg,var(--cyan),var(--purple));
}
button.ghost{
  background:transparent;border:1px solid var(--line);color:var(--text);
  padding:9px 12px;border-radius:10px;cursor:pointer;
}
button.danger{
  background:#ff3b5c;border:none;color:white;padding:9px 12px;border-radius:10px;cursor:pointer;
}
.song{
  display:flex;align-items:center;gap:10px;
  background:var(--card);border:1px solid var(--line);
  border-radius:12px;padding:10px;margin-bottom:8px;
}
.song-info{flex:1;min-width:0}
.song-title{font-weight:bold;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.song-artist{color:var(--muted);font-size:12px}
.play{
  width:38px;height:38px;border:none;border-radius:50%;cursor:pointer;
  background:linear-gradient(135deg,var(--cyan),var(--purple));
  color:#04060c;font-weight:bold;
}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:12px}
.deck{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:14px}
.deck h4{margin:0 0 10px}
input[type=range]{width:100%;accent-color:var(--cyan);margin:6px 0 12px}
.player{
  position:fixed;left:0;right:0;bottom:0;z-index:30;
  background:rgba(8,10,16,.97);border-top:1px solid var(--line);
  padding:10px 14px;display:flex;align-items:center;gap:12px;
}
.player-info{flex:1;min-width:0}
.albo-msg{
  background:#101522;border:1px solid var(--line);border-left:4px solid var(--gold);
  padding:10px 12px;border-radius:10px;margin-bottom:8px;font-size:14px;
}
.empty{color:var(--muted);text-align:center;padding:18px}
.badge{
  display:inline-block;padding:4px 9px;border-radius:999px;font-size:11px;font-weight:bold;
  background:rgba(0,229,255,.15);color:var(--cyan);border:1px solid rgba(0,229,255,.35);
}
</style>
</head>
<body>

<header>
  <div class="logo">POTIFLY</div>
  <div><span class="badge">Musica reale</span></div>
</header>

<nav>
  <button class="active" onclick="go('search',this)">🔍 Cerca</button>
  <button onclick="go('library',this)">🎵 Libreria</button>
  <button onclick="go('playlists',this)">📚 Playlist</button>
  <button onclick="go('dj',this)">🎛️ DJ Mix</button>
  <button onclick="go('albo',this)">🤖 Albo</button>
</nav>

<main>
  <section id="search" class="active">
    <h2>Cerca cantante e musica</h2>
    <div class="card">
      <input type="text" id="musicSearch" placeholder="Cerca cantante, canzone o album..." onkeydown="if(event.key==='Enter') searchMusic()">
      <button class="primary" onclick="searchMusic()">Cerca</button>
      <p class="song-artist">Trovi artisti, brani, album e copertine reali. Puoi ascoltare l’anteprima ufficiale e aprire la canzone completa.</p>
    </div>
    <div class="card">
      <h3>Risultati</h3>
      <div id="searchResults"><div class="empty">Cerca un artista o una canzone.</div></div>
    </div>
  </section>

  <section id="library">
    <h2>La tua libreria</h2>
    <div class="card">
      <h3>Carica i tuoi MP3</h3>
      <input type="file" id="fileInput" multiple accept="audio/*">
      <button class="primary" onclick="document.getElementById('fileInput').click()">Scegli file musicali</button>
      <p class="song-artist">I tuoi file vengono riprodotti interamente. I brani trovati con la ricerca hanno anteprima ufficiale di 30 secondi.</p>
    </div>
    <div class="card">
      <h3>Brani salvati</h3>
      <div id="libraryBox"></div>
    </div>
  </section>

  <section id="playlists">
    <h2>Playlist</h2>
    <div class="card">
      <input type="text" id="playlistName" placeholder="Nome playlist">
      <button class="primary" onclick="createPlaylist()">Crea playlist</button>
    </div>
    <div id="playlistBox"></div>
  </section>

  <section id="dj">
    <h2>DJ Studio</h2>
    <div class="grid">
      <div class="deck">
        <h4>Deck A</h4>
        <select id="deckASelect"></select>
        <button class="primary" onclick="loadDeck('A')">Carica Deck A</button>
        <button class="ghost" onclick="toggleDeck('A')">Play / Pausa</button>
        <label>Volume A</label>
        <input type="range" min="0" max="100" value="80" oninput="setVolume('A',this.value)">
        <label>Velocità A</label>
        <input type="range" min="70" max="130" value="100" oninput="setSpeed('A',this.value)">
      </div>

      <div class="deck">
        <h4>Deck B</h4>
        <select id="deckBSelect"></select>
        <button class="primary" onclick="loadDeck('B')">Carica Deck B</button>
        <button class="ghost" onclick="toggleDeck('B')">Play / Pausa</button>
        <label>Volume B</label>
        <input type="range" min="0" max="100" value="80" oninput="setVolume('B',this.value)">
        <label>Velocità B</label>
        <input type="range" min="70" max="130" value="100" oninput="setSpeed('B',this.value)">
      </div>
    </div>

    <div class="card">
      <h3>Crossfader</h3>
      <input type="range" min="0" max="100" value="50" oninput="crossfade(this.value)">
      <p class="song-artist">A sinistra senti più Deck A; a destra senti più Deck B.</p>
    </div>

    <div class="card">
      <h3>Stato DJ</h3>
      <button class="primary" onclick="stopAll()">Stop tutto</button>
      <p class="song-artist" id="djStatus">Nessun deck caricato</p>
    </div>
  </section>

  <section id="albo">
    <h2>Albo — Assistente musicale</h2>
    <div class="card">
      <div id="alboChat"></div>
      <input type="text" id="alboInput" placeholder="Chiedi a Albo: consigliami un brano, crea una playlist, come mixo?" onkeydown="if(event.key==='Enter') askAlbo()">
      <button class="primary" onclick="askAlbo()">Invia</button>
    </div>
    <div class="card">
      <h3>Azioni rapide</h3>
      <button class="ghost" onclick="quickAlbo('Consigliami un brano')">Consigliami un brano</button>
      <button class="ghost" onclick="quickAlbo('Crea una playlist')">Crea playlist</button>
      <button class="ghost" onclick="quickAlbo('Come faccio un mix?')">Come mixo?</button>
    </div>
  </section>
</main>

<div class="player">
  <button class="play" onclick="toggleCurrent()">▶</button>
  <div class="player-info">
    <div class="song-title" id="nowTitle">Nessun brano</div>
    <div class="song-artist" id="nowArtist">Potifly</div>
  </div>
  <button class="ghost" onclick="go('dj', document.querySelectorAll('nav button')[3])">🎛️ DJ</button>
</div>

<script>
let songs = [];
let playlists = JSON.parse(localStorage.getItem("potifly_playlists") || "[]");
let currentAudio = null;
let currentSong = null;

const audioContext = new (window.AudioContext || window.webkitAudioContext)();

const decks = {
  A:{audio:null, source:null, gain:null, file:null, playing:false, volume:.8, speed:1},
  B:{audio:null, source:null, gain:null, file:null, playing:false, volume:.8, speed:1}
};

function savePlaylists(){
  localStorage.setItem("potifly_playlists", JSON.stringify(playlists));
}

function go(id, button){
  document.querySelectorAll("section").forEach(s => s.classList.remove("active"));
  document.getElementById(id).classList.add("active");
  document.querySelectorAll("nav button").forEach(b => b.classList.remove("active"));
  button.classList.add("active");
}

async function searchMusic(){
  const query = document.getElementById("musicSearch").value.trim();
  const box = document.getElementById("searchResults");

  if(!query) return alert("Scrivi il nome di un cantante o una canzone.");

  box.innerHTML = "<div class='empty'>Sto cercando...</div>";

  try{
    const url = `https://itunes.apple.com/search?term=${encodeURIComponent(query)}&media=music&limit=25&country=IT`;
    const response = await fetch(url);
    const data = await response.json();

    if(!data.results || data.results.length === 0){
      box.innerHTML = "<div class='empty'>Nessun risultato trovato.</div>";
      return;
    }

    box.innerHTML = "";

    data.results.forEach(item => {
      const isSong = item.kind === "song";
      const title = item.trackName || item.collectionName || item.artistName;
      const artist = item.artistName || "";
      const artwork = (item.artworkUrl100 || "").replace("100x100", "300x300");
      const preview = item.previewUrl || "";
      const link = item.trackViewUrl || item.collectionViewUrl || "";

      const div = document.createElement("div");
      div.className = "song";

      div.innerHTML = `
        <img src="${artwork}" width="55" height="55" style="border-radius:10px" alt="">
        <div class="song-info">
          <div class="song-title">${title}</div>
          <div class="song-artist">${artist}</div>
        </div>
        ${isSong && preview ? `<button class="play" onclick="playPreview('${preview.replace(/'/g,"\\'")}', '${title.replace(/'/g,"\\'")}', '${artist.replace(/'/g,"\\'")}')">▶</button>` : ""}
        ${link ? `<button class="ghost" onclick="window.open('${link}','_blank')">Apri</button>` : ""}
        ${isSong ? `<button class="ghost" onclick='saveSearchSong(${JSON.stringify({title, artist, preview, artwork, link}).replace(/'/g,"&#39;")})'>+</button>` : ""}
      `;

      box.appendChild(div);
    });

  }catch(error){
    box.innerHTML = "<div class='empty'>Errore nella ricerca. Riprova.</div>";
  }
}

function playPreview(url, title, artist){
  if(currentAudio) currentAudio.pause();

  currentAudio = new Audio(url);
  currentAudio.play();

  currentSong = {title, artist, url};
  document.getElementById("nowTitle").textContent = title;
  document.getElementById("nowArtist").textContent = artist;
}

function saveSearchSong(song){
  songs.push({
    id: Date.now() + Math.random(),
    title: song.title,
    artist: song.artist,
    url: song.preview,
    artwork: song.artwork,
    link: song.link,
    isPreview: true
  });

  renderLibrary();
  renderDeckOptions();
  alert("Brano salvato in libreria.");
}

document.getElementById("fileInput").addEventListener("change", event => {
  Array.from(event.target.files).forEach(file => {
    songs.push({
      id: Date.now() + Math.random(),
      title: file.name.replace(/.[^.]+$/, ""),
      artist: "Mia musica",
      url: URL.createObjectURL(file),
      isPreview: false
    });
  });

  renderLibrary();
  renderDeckOptions();
});

function renderLibrary(){
  const box = document.getElementById("libraryBox");
  box.innerHTML = songs.length ? "" : '<div class="empty">Nessun brano in libreria.</div>';

  songs.forEach((song, index) => {
    const div = document.createElement("div");
    div.className = "song";
    div.innerHTML = `
      <div class="song-info">
        <div class="song-title">${song.title}</div>
        <div class="song-artist">${song.artist} ${song.isPreview ? "• anteprima" : "• file completo"}</div>
      </div>
      <button class="play" onclick="playSong(${index})">▶</button>
      <button class="ghost" onclick="addToPlaylist(${index})">+</button>
      <button class="ghost" onclick="loadIntoDeck(${index})">🎛️</button>
    `;
    box.appendChild(div);
  });
}

function playSong(index){
  const song = songs[index];
  if(!song) return;

  if(currentAudio) currentAudio.pause();

  currentAudio = new Audio(song.url);
  currentAudio.play();

  currentSong = song;
  document.getElementById("nowTitle").textContent = song.title;
  document.getElementById("nowArtist").textContent = song.artist;
}

function toggleCurrent(){
  if(!currentAudio) return alert("Scegli prima un brano.");
  currentAudio.paused ? currentAudio.play() : currentAudio.pause();
}

function createPlaylist(){
  const name = document.getElementById("playlistName").value.trim();
  if(!name) return alert("Scrivi il nome della playlist.");

  playlists.push({name, songIds:[]});
  document.getElementById("playlistName").value = "";
  savePlaylists();
  renderPlaylists();
}

function renderPlaylists(){
  const box = document.getElementById("playlistBox");
  box.innerHTML = playlists.length ? "" : '<div class="empty">Nessuna playlist.</div>';

  playlists.forEach((playlist, pIndex) => {
    const playlistSongs = songs.filter(song => playlist.songIds.includes(song.id));

    const div = document.createElement("div");
    div.className = "card";
    div.innerHTML = `
      <h3>${playlist.name}</h3>
      ${playlistSongs.map(song => `
        <div class="song">
          <div class="song-info">
            <div class="song-title">${song.title}</div>
            <div class="song-artist">${song.artist}</div>
          </div>
          <button class="play" onclick="playSongById('${song.id}')">▶</button>
        </div>
      `).join("") || '<div class="empty">Playlist vuota.</div>'}
      <button class="danger" onclick="deletePlaylist(${pIndex})">Elimina</button>
    `;
    box.appendChild(div);
  });
}

function addToPlaylist(songIndex){
  if(playlists.length === 0){
    playlists.push({name:"I miei preferiti", songIds:[]});
  }

  const song = songs[songIndex];
  const name = prompt("In quale playlist aggiungere:
" + song.title + "

" + playlists.map(p => p.name).join("
"));
  const playlist = playlists.find(p => p.name === name);

  if(!playlist) return alert("Playlist non trovata.");

  playlist.songIds.push(song.id);
  savePlaylists();
  renderPlaylists();
}

function deletePlaylist(index){
  if(confirm("Eliminare questa playlist?")){
    playlists.splice(index,1);
    savePlaylists();
    renderPlaylists();
  }
}

function playSongById(id){
  const index = songs.findIndex(song => String(song.id) === String(id));
  if(index >= 0) playSong(index);
}

function renderDeckOptions(){
  ["A","B"].forEach(deck => {
    document.getElementById("deck" + deck + "Select").innerHTML =
      songs.map((song, index) => `<option value="${index}">${song.title}</option>`).join("");
  });
}

async function loadDeck(deck){
  if(songs.length === 0) return alert("Prima salva o carica almeno un brano.");

  const index = Number(document.getElementById("deck" + deck + "Select").value);
  const song = songs[index];
  const d = decks[deck];

  if(d.source){
    try{ d.source.stop(); }catch(e){}
  }

  d.file = song;

  const response = await fetch(song.url);
  const arrayBuffer = await response.arrayBuffer();
  d.audio = await audioContext.decodeAudioData(arrayBuffer);

  d.gain = audioContext.createGain();
  d.gain.connect(audioContext.destination);
  d.gain.gain.value = d.volume;

  startDeck(deck);

  document.getElementById("djStatus").textContent =
    `Deck A: ${decks.A.file ? decks.A.file.title : "vuoto"} | Deck B: ${decks.B.file ? decks.B.file.title : "vuoto"}`;
}

function startDeck(deck){
  const d = decks[deck];
  if(!d.audio) return;

  if(d.source){
    try{ d.source.stop(); }catch(e){}
  }

  d.source = audioContext.createBufferSource();
  d.source.buffer = d.audio;
  d.source.playbackRate.value = d.speed;
  d.source.connect(d.gain);
  d.source.start();
  d.playing = true;
}

function toggleDeck(deck){
  const d = decks[deck];
  if(!d.audio) return alert("Carica prima il Deck " + deck + ".");

  if(d.playing){
    try{ d.source.stop(); }catch(e){}
    d.playing = false;
  } else {
    startDeck(deck);
  }
}

function setVolume(deck, value){
  const d = decks[deck];
  d.volume = Number(value) / 100;
  if(d.gain) d.gain.gain.value = d.volume;
}

function setSpeed(deck, value){
  const d = decks[deck];
  d.speed = Number(value) / 100;
  if(d.source) d.source.playbackRate.value = d.speed;
}

function crossfade(value){
  const v = Number(value) / 100;

  decks.A.volume = 1 - v;
  decks.B.volume = v;

  if(decks.A.gain) decks.A.gain.gain.value = decks.A.volume;
  if(decks.B.gain) decks.B.gain.gain.value = decks.B.volume;
}

function stopAll(){
  ["A","B"].forEach(deck => {
    const d = decks[deck];
    if(d.source){
      try{ d.source.stop(); }catch(e){}
      d.playing = false;
    }
  });
  document.getElementById("djStatus").textContent = "Tutti i deck fermi";
}

function loadIntoDeck(index){
  const deck = prompt("Caricare in quale deck? Scrivi A o B");
  if(deck !== "A" && deck !== "B") return alert("Scrivi A o B.");

  document.getElementById("deck" + deck + "Select").value = index;
  loadDeck(deck);
  go("dj", document.querySelectorAll("nav button")[3]);
}

function askAlbo(){
  const input = document.getElementById("alboInput");
  const text = input.value.trim();
  if(!text) return;

  input.value = "";
  alboReply(text);
}

function quickAlbo(text){
  alboReply(text);
}

function alboReply(text){
  const chat = document.getElementById("alboChat");
  chat.innerHTML += `<div class="albo-msg"><b>Tu:</b> ${text}</div>`;

  let answer = "";
  const lower = text.toLowerCase();

  if(lower.includes("consigli")){
    if(songs.length === 0){
      answer = "Non hai ancora brani in libreria. Cerca un artista e salva una canzone, poi ti consiglio io.";
    } else {
      const song = songs[Math.floor(Math.random() * songs.length)];
      answer = `Ti consiglio <b>${song.title}</b> di ${song.artist}. Puoi metterlo nel Deck A e mixarlo.`;
    }
  } else if(lower.includes("playlist")){
    if(songs.length === 0){
      answer = "Prima salva qualche brano nella libreria.";
    } else {
      playlists.push({
        name:"Playlist Albo " + new Date().toLocaleTimeString(),
        songIds:songs.map(song => song.id)
      });
      savePlaylists();
      renderPlaylists();
      answer = "Ho creato una playlist con tutti i brani della tua libreria.";
    }
  } else if(lower.includes("mix")){
    answer = "Carica un brano nel Deck A e un altro nel Deck B. Poi muovi il crossfader per passare da uno all’altro.";
  } else {
    answer = "Sono Albo. Posso consigliarti brani, creare playlist o spiegarti il DJ Mix.";
  }

  chat.innerHTML += `<div class="albo-msg"><b>Albo:</b> ${answer}</div>`;
}

renderLibrary();
renderPlaylists();
renderDeckOptions();
</script>

</body>
</html>
