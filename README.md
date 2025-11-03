<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Monumentos de Valladolid</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css">
  <style>
    body {
      margin: 0;
      font-family: "Segoe UI", sans-serif;
      background: #f4f6f9;
    }
    header {
      background: #283593;
      color: white;
      padding: 1rem;
      text-align: center;
      font-size: 1.5rem;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
      z-index: 1000;
      position: relative;
    }
    #map {
      height: 90vh;
      width: 100%;
    }
    .tooltip-custom {
      text-align: center;
    }
    .tooltip-custom img {
      width: 120px;
      height: 80px;
      border-radius: 8px;
      margin-bottom: 4px;
    }
    #info-panel {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(10, 10, 30, 0.9);
      color: white;
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
      width: 60%;
      max-width: 600px;
      border-radius: 15px;
      margin-bottom: 1.5rem;
      box-shadow: 0 0 15px rgba(255, 255, 255, 0.3);
    }
    #info-panel h2 {
      font-size: 2rem;
      margin-bottom: 1rem;
      color: #ffca28;
    }
    #info-panel p {
      font-size: 1.1rem;
      line-height: 1.6;
      max-width: 800px;
    }
    #close-btn {
      position: absolute;
      top: 20px;
      right: 30px;
      background: #ffca28;
      color: #212121;
      border: none;
      font-size: 1rem;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }
    #close-btn:hover {
      background: #ffd54f;
    }
  </style>
</head>
<body>
  <header>🗺️ Monumentos emblemáticos de Valladolid</header>
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
      center: [41.6540, -4.7235],
      zoom: 16,
      dragging: false,
      scrollWheelZoom: false,
      doubleClickZoom: false,
      boxZoom: false,
      keyboard: false,
      tap: false,
      touchZoom: false
    });

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);

    const monumentos = [
      {
        nombre: "Iglesia de San Pablo",
        coords: [41.656916, -4.724647],  // según recurso externo :contentReference[oaicite:0]{index=0}
        imagen: "https://upload.wikimedia.org/wikipedia/commons/2/2a/Iglesia_de_San_Pablo_de_Valladolid_2019.jpg",
        breve: "Obra maestra del gótico isabelino con una fachada impresionante del siglo XV.",
        info: "Construida a finales del siglo XV, la Iglesia de San Pablo es un ejemplo magistral del gótico isabelino. Su fachada, decorada con relieves de gran detalle, fue testigo de acontecimientos históricos como la coronación del rey Felipe II."
      },
      {
        nombre: "Museo Nacional de Escultura (Colegio de San Gregorio)",
        coords: [41.656900, -4.723610],  // según Wikipedia :contentReference[oaicite:1]{index=1}
        imagen: "https://upload.wikimedia.org/wikipedia/commons/7/79/Colegio_de_San_Gregorio%2C_Valladolid%2C_Espa%C3%B1a%2C_2015-12-30%2C_DD_49.JPG",
        breve: "Antiguo Colegio de San Gregorio, joya del arte plateresco y sede del Museo Nacional de Escultura.",
        info: "El conjunto monumental del Colegio de San Gregorio, conocido por sus 'cadenas', alberga el Museo Nacional de Escultura. Su fachada plateresca es una de las más representativas del arte renacentista español, con una riqueza decorativa excepcional."
      },
      {
        nombre: "Catedral de Valladolid",
        coords: [41.652678, -4.723415],  // según Wikipedia :contentReference[oaicite:2]{index=2}
        imagen: "https://upload.wikimedia.org/wikipedia/commons/0/0d/Catedral_de_Valladolid_2018.jpg",
        breve: "Diseñada por Juan de Herrera, combina los estilos herreriano y barroco. Inacabada pero monumental.",
        info: "La Catedral de Nuestra Señora de la Asunción comenzó a construirse en el siglo XVI según los planos de Juan de Herrera. Aunque nunca se terminó, su arquitectura imponente refleja el poder y la historia religiosa de Valladolid."
      },
      {
        nombre: "Estatua de Miguel de Cervantes",
        coords: [41.652850, -4.722200],  // aproximación mejor localizada
        imagen: "https://upload.wikimedia.org/wikipedia/commons/e/e3/Estatua_Miguel_de_Cervantes_Valladolid_2018.jpg",
        breve: "Escultura en honor al célebre autor de Don Quijote, situada en la Plaza de la Universidad.",
        info: "Situada en la Plaza de la Universidad, esta estatua conmemora la estancia del escritor en Valladolid. Es un punto muy visitado del centro histórico."
      }
    ];

    monumentos.forEach(monumento => {
      const marker = L.marker(monumento.coords).addTo(map);

      marker.bindTooltip(
        `<div class='tooltip‑custom'>
          <img src='${monumento.imagen}' alt='${monumento.nombre}'>
          <div><b>${monumento.nombre}</b><br>${monumento.breve}</div>
        </div>`,
        { direction: "top", offset: [0, -10], opacity: 0.95 }
      );

      marker.on("click", () => {
        document.getElementById("info-imagen").src = monumento.imagen;
        document.getElementById("info-titulo").textContent = monumento.nombre;
        document.getElementById("info-texto").textContent = monumento.info;
        document.getElementById("info-panel").style.display = "flex";
      });
    });

    document.getElementById("close-btn").addEventListener("click", () => {
      document.getElementById("info-panel").style.display = "none";
    });

  </script>
</body>
</html>
