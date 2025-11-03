<html lang="es">
<head>
  <meta charset="UTF‑8">
  <meta name="viewport" content="width=device‑width, initial‑scale=1.0">
  <title>Monumentos de Valladolid</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css">
  <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      font‑family: "Roboto", sans‑serif;
      background: #f0f2f5;
      color: #333;
    }
    header {
      background: linear-gradient(90deg, #00695c, #26a69a);
      color: #fff;
      padding: 1.5rem;
      text‑align: center;
      font‑size: 2rem;
      font‑weight: 700;
      box‑shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
    #map {
      height: 90vh;
      width: 100%;
    }
    .tooltip‑card {
      background: #ffffffcc;
      border‑radius: 12px;
      padding: 8px;
      text‑align: center;
      box‑shadow: 0 2px 8px rgba(0,0,0,0.2);
      width: 160px;
    }
    .tooltip‑card img {
      width: 140px;
      height: 90px;
      object‑fit: cover;
      border‑radius: 8px;
      margin‑bottom: 6px;
    }
    #info‑panel {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.95);
      color: #fff;
      display: none;
      flex‑direction: column;
      align‑items: center;
      justify‑content: center;
      text‑align: center;
      z‑index: 2000;
      padding: 2rem;
      overflow‑y: auto;
      animation: fadeIn 0.4s ease;
    }
    #info‑panel img {
      width: 70%;
      max‑width: 700px;
      border‑radius: 15px;
      margin‑bottom: 2rem;
      box‑shadow: 0 0 25px rgba(255, 255, 255, 0.3);
      transition: transform 0.3s;
    }
    #info‑panel img:hover {
      transform: scale(1.03);
    }
    #info‑panel h2 {
      font‑size: 2.4rem;
      margin‑bottom: 1rem;
      color: #ffca28;
    }
    #info‑panel p {
      font‑size: 1.15rem;
      line‑height: 1.6;
      max‑width: 850px;
    }
    #close‑btn {
      position: absolute;
      top: 25px;
      right: 35px;
      background: #ffca28;
      color: #212121;
      border: none;
      font‑size: 1.1rem;
      padding: 12px 25px;
      border‑radius: 10px;
      cursor: pointer;
      transition: background 0.3s;
    }
    #close‑btn:hover {
      background: #ffd54f;
    }
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
  </style>
</head>
<body>
  <header>🗺️ Monumentos emblemáticos de Valladolid</header>
  <div id="map"></div>

  <div id="info‑panel">
    <button id="close‑btn">Cerrar</button>
    <img id="info‑imagen" src="" alt="">
    <h2 id="info‑titulo"></h2>
    <p id="info‑texto"></p>
  </div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    const map = L.map('map').setView([41.6540, -4.7240], 15);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);

    const monumentos = [
      {
        nombre: "Iglesia de San Pablo",
        coords: [41.657000, -4.724500],  
        imagen: "https://upload.wikimedia.org/wikipedia/commons/2/2a/Iglesia_de_San_Pablo_de_Valladolid_2019.jpg",
        breve: "Gótico isabelino con fachada impresionante.",
        info: "Construida a finales del siglo XV, la Iglesia de San Pablo representa un ejemplo destacado del gótico isabelino en Castilla y León."  
      },
      {
        nombre: "Museo Nacional de Escultura",
        coords: [41.656900, -4.723610],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/7/79/Colegio_de_San_Gregorio%2C_Valladolid%2C_Espa%C3%B1a%2C_2015-12-30%2C_DD_49.JPG",
        breve: "Antiguo Colegio de San Gregorio, joya del arte plateresco.",
        info: "El Colegio de San Gregorio alberga el Museo Nacional de Escultura, con una fachada plateresca y patios muy representativos del Renacimiento."  
      },
      {
        nombre: "Catedral de Valladolid",
        coords: [41.652678, -4.723415],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/0/0d/Catedral_de_Valladolid_2018.jpg",
        breve: "Diseñada por Juan de Herrera, inacabada pero monumental.",
        info: "La Catedral de Nuestra Señora de la Asunción, iniciada en el siglo XVI, es un símbolo del centro histórico a pesar de no completarse."  
      },
      {
        nombre: "Estatua de Miguel de Cervantes",
        coords: [41.652780, -4.722220],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/e/e3/Estatua_Miguel_de_Cervantes_Valladolid_2018.jpg",
        breve: "Escultura en honor al autor de Don Quijote.",
        info: "Situada en la Plaza de Cervantes, conmemora la estancia de Miguel de Cervantes en Valladolid."  
      },
      {
        nombre: "Plaza Mayor de Valladolid",
        coords: [41.651983, -4.728469],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/3/3d/Plaza_Mayor_Valladolid.jpg",
        breve: "Plaza central histórica de la ciudad.",
        info: "La Plaza Mayor de Valladolid es el principal espacio urbano de la ciudad y sirve como punto de encuentro histórico y social."  
      },
      {
        nombre: "Monumento a Cristóbal Colón",
        coords: [41.658100, -4.713900],  
        imagen: "https://upload.wikimedia.org/wikipedia/commons/8/8d/Monumento_a_Col%C3%B3n_Valladolid.jpg",
        breve: "Homenaje a Colón en la Plaza de Colón.",
        info: "Monumento a Cristóbal Colón situado en la Plaza de Colón de Valladolid, inaugurado en 1905."  
      }
    ];

    monumentos.forEach(mon => {
      const icon = L.divIcon({
        className: 'custom‑marker',
        html: `<div style="background:#ffca28;width:20px;height:20px;border‑radius:50%;border:2px solid #212121;"></div>`,
        iconSize: [20, 20],
        iconAnchor: [10, 10]
      });

      const marker = L.marker(mon.coords, { icon }).addTo(map);

      marker.bindTooltip(
        `<div class="tooltip‑card">
           <img src="${mon.imagen}" alt="${mon.nombre}">
           <div><strong>${mon.nombre}</strong><br>${mon.breve}</div>
         </div>`,
        { direction: "top", offset: [0, -12], opacity: 0.95 }
      );

      marker.on("click", () => {
        document.getElementById("info‑imagen").src = mon.imagen;
        document.getElementById("info‑titulo").textContent = mon.nombre;
        document.getElementById("info‑texto").textContent = mon.info;
        document.getElementById("info‑panel").style.display = "flex";
      });
    });

    document.getElementById("close‑btn").addEventListener("click", () => {
      document.getElementById("info‑panel").style.display = "none";
    });
  </script>
</body>
</html>
