<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Gestor de Personajes One Piece</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Pirata+One&display=swap');

body {
  font-family: 'Pirata One', cursive, Arial, sans-serif;
  background: linear-gradient(to right, #1b2735, #090a0f);
  color: #fff;
  margin: 0;
  padding: 20px;
  text-align: center;
}

h1 { color: #ffd700; text-shadow: 2px 2px #000; }

#abrirFormulario, button { padding: 10px; border: none; border-radius: 5px; background: #ffd700; color: #000; cursor: pointer; font-weight: bold; }

#buscarPersonaje { padding:8px 12px; border-radius:5px; border:none; margin:10px 0; width:300px; max-width:90%; display:block; margin-left:auto; margin-right:auto; font-size:16px; }

.personajes { display: flex; flex-wrap: wrap; gap: 20px; justify-content: center; margin-top: 10px; }
.personaje-wrapper { position: relative; }
.personaje-wrapper.leyenda::before {
  content: ''; position: absolute; bottom: -15px; left: 50%; transform: translateX(-50%);
  width: 20px; height: 20px; background: #ffd700; border-radius: 50%;
  box-shadow: 0 0 15px 5px #ffd700, 0 0 30px 10px rgba(255,215,0,0.5); z-index:5;
}

.personaje { width: 100px; height: 100px; border-radius: 50%; overflow: hidden; cursor:pointer; transition: transform 0.2s, box-shadow 0.3s; position: relative; }
.personaje.bueno { box-shadow: 0 0 15px 4px #00aaff inset, 0 0 25px 8px #00cfff; }
.personaje.malo { box-shadow: 0 0 15px 4px #ff0000 inset, 0 0 25px 8px #ff5555; }
.personaje.neutral { box-shadow: 0 0 15px 4px #ffffff inset, 0 0 25px 8px #ddddff; }

@keyframes brilloLeyenda { 0%,100%{box-shadow:0 0 40px 15px #ffd700;} 50%{box-shadow:0 0 60px 25px #ffd700;} }
.personaje.leyenda::after { content:''; position:absolute; top:-12px; left:-12px; right:-12px; bottom:-12px; border:5px solid #ffd700; border-radius:50%; pointer-events:none; box-shadow:0 0 50px 20px #ffd700; z-index:10; animation: brilloLeyenda 2s infinite ease-in-out; }

.personaje img { width:100%; height:100%; object-fit:cover; display:block; }
.personaje:hover { transform: scale(1.1); }

form { background: #2c2c2c; padding: 20px; border-radius: 15px; max-width: 500px; margin: auto; text-align: left; display: none; }
form input, form select, form textarea { width: 100%; margin-bottom:10px; padding:8px; border-radius:5px; border:none; }
form label { margin-top:10px; display:block; font-weight:bold; }

.modal { display: none; position: fixed; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.9); justify-content: center; align-items: center; }
.modal-content { background:#2c2c2c; padding:20px; border-radius:15px; max-width:400px; width:90%; color:#fff; max-height:90%; overflow-y:auto; text-align:left; }

.estadistica { cursor:pointer; margin:5px 0; padding:5px; background:#444; border-radius:5px; }
.descripcion { display:none; margin-left:10px; color:#ffd700; }
.eliminar { background:red; border:none; border-radius:50%; color:#fff; cursor:pointer; padding:2px 6px; position:absolute; top:5px; right:5px; }
.reliquia-img { width:60px; height:60px; object-fit: cover; border-radius:10px; border:2px solid #ffd700; display:block; margin:auto; }
</style>
</head>
<body>

<h1>Gestor de Personajes One Piece</h1>

<button id="abrirFormulario">Añadir Personaje</button>

<!-- Búsqueda y filtros -->
<input type="text" id="buscarPersonaje" placeholder="Buscar personaje...">
<select id="filtroAfiliacion"><option value="todos">Todos</option></select>
<select id="filtroRol"><option value="todos">Todos</option></select>
<select id="filtroRareza"><option value="todos">Todos</option>
  <option value="normal">Normal</option>
  <option value="epico">Épico</option>
  <option value="leyenda">Leyenda</option>
</select>

<div class="personajes" id="personajesContainer"></div>

<form id="formPersonaje">
  <h2 id="formTitle">Nuevo Personaje</h2>

  <label>Nombre:</label>
  <input type="text" id="nombre" required>

  <label>Foto de Perfil:</label>
  <input type="file" id="fotoPerfilFile" accept="image/*">
  <label>Foto Cuerpo Completo:</label>
  <input type="file" id="fotoCuerpoFile" accept="image/*">
  <label>Reliquia (imagen opcional):</label>
  <input type="file" id="reliquiaFile" accept="image/*">

  <div id="estadisticasContainer">
    <label>Estadística:</label>
    <input type="text" class="estadisticaNombre" placeholder="Nombre" required>
    <textarea class="estadisticaDesc" placeholder="Descripción" required></textarea>
  </div>
  <button type="button" id="agregarEstadistica">+ Añadir otra estadística</button>

  <label>Afiliación (halo bueno/malo/neutral):</label>
  <select id="afiliacionHalo">
    <option value="bueno">Bueno</option>
    <option value="neutral">Neutral</option>
    <option value="malo">Malo</option>
  </select>

  <label>Afiliación (nombre libre):</label>
  <input type="text" id="afiliacion" placeholder="Escribe la afiliación del personaje" required>

  <label>Rol:</label>
  <select id="rol">
    <option value="atacante">Atacante</option>
    <option value="apoyo">Apoyo</option>
    <option value="tanque">Tanque</option>
    <option value="sanador">Sanador</option>
  </select>

  <label>Rareza:</label>
  <select id="rareza">
    <option value="normal">Normal</option>
    <option value="epico">Épico</option>
    <option value="leyenda">Leyenda</option>
  </select>

  <button type="submit" id="submitBtn">Añadir Personaje</button>
  <button type="button" id="cancelEdit" style="display:none;">Cancelar</button>
</form>

<div class="modal" id="modal"><div class="modal-content" id="modalContent"></div></div>

<script>
const personajesContainer = document.getElementById('personajesContainer');
const form = document.getElementById('formPersonaje');
const abrirFormularioBtn = document.getElementById('abrirFormulario');
const estadisticasContainer = document.getElementById('estadisticasContainer');
const agregarEstadisticaBtn = document.getElementById('agregarEstadistica');
const submitBtn = document.getElementById('submitBtn');
const cancelEditBtn = document.getElementById('cancelEdit');
const formTitle = document.getElementById('formTitle');
const buscarInput = document.getElementById('buscarPersonaje');

let personajes = JSON.parse(localStorage.getItem('personajes')) || [];
let editIndex = null;

abrirFormularioBtn.addEventListener('click', ()=>{ form.style.display='block'; abrirFormularioBtn.style.display='none'; });
agregarEstadisticaBtn.addEventListener('click', ()=>{
  const label = document.createElement('label'); label.textContent='Estadística:';
  const nombre = document.createElement('input'); nombre.type='text'; nombre.classList.add('estadisticaNombre'); nombre.placeholder='Nombre';
  const desc = document.createElement('textarea'); desc.classList.add('estadisticaDesc'); desc.placeholder='Descripción';
  estadisticasContainer.appendChild(label); estadisticasContainer.appendChild(nombre); estadisticasContainer.appendChild(desc);
});

function convertirArchivoABase64(file){ return new Promise((resolve,reject)=>{ const reader=new FileReader(); reader.onload=()=>resolve(reader.result); reader.onerror=err=>reject(err); reader.readAsDataURL(file); }); }
async function obtenerImagen(fileInput, defaultData){ if(fileInput.files[0]) return await convertirArchivoABase64(fileInput.files[0]); return defaultData; }

function guardarPersonajes(){ localStorage.setItem('personajes', JSON.stringify(personajes)); }
function crearPersonajeDiv(data,index){
  const wrapper=document.createElement('div'); wrapper.classList.add('personaje-wrapper');
  if(data.rareza==='leyenda') wrapper.classList.add('leyenda');
  const div=document.createElement('div'); div.classList.add('personaje'); div.classList.add(data.halo);
  div.innerHTML=`<img src="${data.fotoPerfil}" alt="${data.nombre}"><p style="margin-top:5px; font-weight:bold;">${data.afiliacion}</p>`;
  div.addEventListener('click', ()=>{
    const modal=document.getElementById('modal'); const modalContent=document.getElementById('modalContent');
    modalContent.innerHTML=`
      <h2>${data.nombre}</h2>
      <img src="${data.fotoCuerpo}" alt="${data.nombre}" style="width:150px;height:150px;border-radius:15px;border:2px solid #ffd700;">
      ${data.reliquia?`<img src="${data.reliquia}" class="reliquia-img" alt="Reliquia">`:''}
      <p><strong>Afiliación:</strong> ${data.afiliacion}</p>
      <p><strong>Rol:</strong> ${data.rol}</p>
      <p><strong>Rareza:</strong> <span style="color:${data.rareza==='normal'?'#00ff00':data.rareza==='epico'?'#8000ff':'#ffd700'}; font-weight:bold">${data.rareza.toUpperCase()}</span></p>
      <div>${data.estadisticas.map(e=>`<div class="estadistica">${e.nombre}<div class="descripcion">${e.desc}</div></div>`).join('')}</div>
      <button id="editarPersonaje">Editar Personaje</button>
    `;
    modal.style.display='flex';
    modalContent.querySelectorAll('.estadistica').forEach(a=>{ a.addEventListener('click',()=>{ const desc=a.querySelector('.descripcion'); desc.style.display=desc.style.display==='none'?'block':'none'; }); });
    modalContent.querySelector('#editarPersonaje').addEventListener('click',()=>{ abrirEdicion(index); modal.style.display='none'; });
  });
  const btnEliminar=document.createElement('button'); btnEliminar.textContent='x'; btnEliminar.classList.add('eliminar');
  btnEliminar.addEventListener('click', e=>{ e.stopPropagation(); personajes.splice(index,1); guardarPersonajes(); renderizarPersonajes(); actualizarFiltrosDinamicos(); });
  wrapper.appendChild(div); wrapper.appendChild(btnEliminar); personajesContainer.appendChild(wrapper);
}

function renderizarPersonajes(){ personajesContainer.innerHTML=''; personajes.forEach((p,i)=>crearPersonajeDiv(p,i)); }

form.addEventListener('submit', async e=>{
  e.preventDefault();
  const estadisticas=[]; 
  document.querySelectorAll('.estadisticaNombre').forEach((n,i)=>{ if(n.value && document.querySelectorAll('.estadisticaDesc')[i].value) estadisticas.push({nombre:n.value,desc:document.querySelectorAll('.estadisticaDesc')[i].value}); });
  const fotoPerfil = await obtenerImagen(document.getElementById('fotoPerfilFile'), editIndex!==null ? personajes[editIndex].fotoPerfil : '');
  const fotoCuerpo = await obtenerImagen(document.getElementById('fotoCuerpoFile'), editIndex!==null ? personajes[editIndex].fotoCuerpo : '');
  const reliquia = await obtenerImagen(document.getElementById('reliquiaFile'), editIndex!==null ? personajes[editIndex].reliquia : '');
  
  const personajeData = {
    nombre: document.getElementById('nombre').value,
    fotoPerfil,
    fotoCuerpo,
    reliquia,
    afiliacion: document.getElementById('afiliacion').value.trim(),
    halo: document.getElementById('afiliacionHalo').value,
    rol: document.getElementById('rol').value,
    rareza: document.getElementById('rareza').value,
    estadisticas
  };

  if(editIndex!==null){ personajes[editIndex]=personajeData; editIndex=null; submitBtn.textContent='Añadir Personaje'; cancelEditBtn.style.display='none'; formTitle.textContent='Nuevo Personaje'; }
  else personajes.push(personajeData);

  guardarPersonajes(); renderizarPersonajes(); form.reset(); estadisticasContainer.innerHTML=`<label>Estadística:</label><input type="text" class="estadisticaNombre" placeholder="Nombre" required><textarea class="estadisticaDesc" placeholder="Descripción" required></textarea>`;
  form.style.display='none'; abrirFormularioBtn.style.display='block';
  actualizarFiltrosDinamicos();
});

function abrirEdicion(index){
  editIndex=index;
  const p=personajes[index];
  document.getElementById('nombre').value=p.nombre;
  document.getElementById('afiliacion').value=p.afiliacion;
  document.getElementById('afiliacionHalo').value=p.halo;
  document.getElementById('rol').value=p.rol;
  document.getElementById('rareza').value=p.rareza;
  estadisticasContainer.innerHTML='';
  p.estadisticas.forEach(a=>{ const label=document.createElement('label'); label.textContent='Estadística:'; const nombre=document.createElement('input'); nombre.type='text'; nombre.classList.add('estadisticaNombre'); nombre.value=a.nombre; const desc=document.createElement('textarea'); desc.classList.add('estadisticaDesc'); desc.value=a.desc; estadisticasContainer.appendChild(label); estadisticasContainer.appendChild(nombre); estadisticasContainer.appendChild(desc); });
  submitBtn.textContent='Guardar Cambios'; cancelEditBtn.style.display='inline-block'; formTitle.textContent='Editar Personaje';
  form.style.display='block'; abrirFormularioBtn.style.display='none';
}

cancelEditBtn.addEventListener('click', ()=>{
  editIndex=null; form.reset(); estadisticasContainer.innerHTML=`<label>Estadística:</label><input type="text" class="estadisticaNombre" placeholder="Nombre" required><textarea class="estadisticaDesc" placeholder="Descripción" required></textarea>`;
  submitBtn.textContent='Añadir Personaje'; cancelEditBtn.style.display='none'; formTitle.textContent='Nuevo Personaje'; form.style.display='none'; abrirFormularioBtn.style.display='block';
});

document.getElementById('modal').addEventListener('click', e=>{ if(e.target.id==='modal') e.target.style.display='none'; });

function actualizarFiltrosDinamicos(){
  const filtroAfiliacion=document.getElementById('filtroAfiliacion');
  const filtroRol=document.getElementById('filtroRol');
  const afiliaciones=[...new Set(personajes.map(p=>p.afiliacion))];
  filtroAfiliacion.innerHTML='<option value="todos">Todos</option>'; afiliaciones.forEach(a=>{ const o=document.createElement('option'); o.value=a; o.textContent=a; filtroAfiliacion.appendChild(o); });
  const roles=[...new Set(personajes.map(p=>p.rol))];
  filtroRol.innerHTML='<option value="todos">Todos</option>'; roles.forEach(r=>{ const o=document.createElement('option'); o.value=r; o.textContent=r; filtroRol.appendChild(o); });
}

function filtrarPersonajes(){
  const filtroNombre=buscarInput.value.toLowerCase();
  const filtroAfiliacion=document.getElementById('filtroAfiliacion').value;
  const filtroRol=document.getElementById('filtroRol').value;
  const filtroRareza=document.getElementById('filtroRareza').value;
  personajesContainer.innerHTML='';
  personajes.forEach((p,i)=>{ if(p.nombre.toLowerCase().includes(filtroNombre) && (filtroAfiliacion==='todos'||p.afiliacion===filtroAfiliacion) && (filtroRol==='todos'||p.rol===filtroRol) && (filtroRareza==='todos'||p.rareza===filtroRareza)) crearPersonajeDiv(p,i); });
}

buscarInput.addEventListener('input', filtrarPersonajes);
document.getElementById('filtroAfiliacion').addEventListener('change', filtrarPersonajes);
document.getElementById('filtroRol').addEventListener('change', filtrarPersonajes);
document.getElementById('filtroRareza').addEventListener('change', filtrarPersonajes);

renderizarPersonajes(); actualizarFiltrosDinamicos();
</script>

</body>
</html>
