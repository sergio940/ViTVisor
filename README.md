<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Monumentos de Valladolid</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css">
  <style>
    body {
      margin: 0;
      font-family: "Lato", "Segoe UI", sans‑serif;
      background: #fafafa;
      color: #333;
    }
    header {
      background: #00695c;
      color: white;
      padding: 1.2rem;
      text-align: center;
      font-size: 1.8rem;
      box-shadow: 0 3px 10px rgba(0,0,0,0.15);
    }
    #map {
      height: 85vh;
      width: 100%;
      border-top: 4px solid #004d40;
    }
    .tooltip-custom {
      text-align: center;
      font-size: 0.9rem;
      line-height: 1.2;
    }
    .tooltip-custom img {
      width: 100px;
      height: 70px;
      border-radius: 6px;
      margin-bottom: 6px;
      object-fit: cover;
    }
    /* Panel de información a pantalla completa */
    #info-panel {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 50, 50, 0.9);
      color: #fff;
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      z-index: 2000;
      padding: 2rem;
      overflow-y: auto;
    }
    #info-panel img {
      width: 70%;
      max-width: 650px;
      border-radius: 12px;
      margin-bottom: 2rem;
      box-shadow: 0 0 20px rgba(0,0,0,0.4);
    }
    #info-panel h2 {
      font-size: 2.4rem;
      margin-bottom: 1rem;
      color: #ffcc80;
    }
    #info-panel p {
      font-size: 1.15rem;
      line-height: 1.6;
      max-width: 850px;
    }
    #close-btn {
      position: absolute;
      top: 25px;
      right: 35px;
      background: #ffcc80;
      color: #00332d;
      border: none;
      font-size: 1.1rem;
      padding: 12px 25px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }
    #close-btn:hover {
      background: #ffd699;
    }
  </style>
</head>
<body>
  <header>🗺️ Monumentos emblemáticos en Valladolid</header>
  <div id="map"></div>

  <div id="info-panel">
    <button id="close-btn">Cerrar</button>
    <img id="info-imagen" src="" alt="">
    <h2 id="info-titulo"></h2>
    <p id="info-texto"></p>
  </div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    const map = L.map('map', {
      center: [41.6540, -4.7240],
      zoom: 15,
      dragging: true,
      scrollWheelZoom: true,
      doubleClickZoom: true,
      boxZoom: true,
      keyboard: true,
      tap: true,
      touchZoom: true
    });

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);

    const monumentos = [
      {
        nombre: "Iglesia de San Pablo",
        coords: [41.657000, -4.724500],  // fuente Wikipedia: 41°39′25″N 4°43′28″O ≈ 41.657, -4.7245 :contentReference[oaicite:0]{index=0}
        imagen: "https://upload.wikimedia.org/wikipedia/commons/2/2a/Iglesia_de_San_Pablo_de_Valladolid_2019.jpg",
        breve: "Obra maestra del gótico isabelino con magnífica fachada plateresca.",
        info: "Construida a finales del siglo XV, la Iglesia de San Pablo representa uno de los ejemplos más destacados del gótico isabelino en Castilla y León. Fue lugar de bautizo de reyes y su exterior es una obra de arte decorativa."
      },
      {
        nombre: "Museo Nacional de Escultura (Colegio de San Gregorio)",
        coords: [41.657118, -4.723707],  // aproximado: según fuente 41.6571178, -4.7237065 :contentReference[oaicite:1]{index=1}
        imagen: "https://upload.wikimedia.org/wikipedia/commons/7/79/Colegio_de_San_Gregorio%2C_Valladolid%2C_Espa%C3%B1a%2C_2015-12-30%2C_DD_49.JPG",
        breve: "Joyas del arte plateresco y sede del Museo Nacional de Escultura.",
        info: "El antiguo Colegio de San Gregorio, hoy sede del Museo Nacional de Escultura, es un edificio emblemático del Renacimiento español. Su fachada y patios son tan interesantes como las colecciones que alberga."
      },
      {
        nombre: "Catedral de Valladolid",
        coords: [41.652222, -4.723611],  // estimación aproximada, revisable
        imagen: "https://upload.wikimedia.org/wikipedia/commons/0/0d/Catedral_de_Valladolid_2018.jpg",
        breve: "Proyecto de Juan de Herrera; inacabada pero imponente.",
        info: "La Catedral de Nuestra Señora de la Asunción, iniciada en el siglo XVI, es un símbolo de la ciudad. Aunque nunca se llegó a completar, su presencia domina el horizonte del centro histórico."
      },
      {
        nombre: "Estatua de Miguel de Cervantes",
        coords: [41.655300, -4.727500],  // aproximación para la plaza de Cervantes
        imagen: "https://upload.wikimedia.org/wikipedia/commons/e/e3/Estatua_Miguel_de_Cervantes_Valladolid_2018.jpg",
        breve: "Monumento al autor de ‘Don Quijote’ en plena plaza de la Universidad.",
        info: "Esta estatua honra a Miguel de Cervantes y se encuentra en la Plaza de la Universidad de Valladolid, lugar de gran tránsito y valor histórico en la ciudad."
      }
    ];

    monumentos.forEach(mon => {
      const marker = L.marker(mon.coords).addTo(map);

      marker.bindTooltip(
        `<div class="tooltip-custom">
           <img src="${mon.imagen}" alt="${mon.nombre}">
           <div><strong>${mon.nombre}</strong><br>${mon.breve}</div>
         </div>`,
        { direction: "top", offset: [0, -12], opacity: 0.9 }
      );

      marker.on("click", () => {
        document.getElementById("info-imagen").src = mon.imagen;
        document.getElementById("info-titulo").textContent = mon.nombre;
        document.getElementById("info-texto").textContent = mon.info;
        document.getElementById("info-panel").style.display = "flex";
      });
    });

    document.getElementById("close-btn").addEventListener("click", () => {
      document.getElementById("info-panel").style.display = "none";
    });
  </script>
</body>
</html>
