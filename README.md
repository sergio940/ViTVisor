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

    .leaflet-popup-content {
      font-size: 0.95rem;
      color: #333;
      line-height: 1.4;
    }

    .leaflet-popup-content b {
      color: #283593;
    }
  </style>
</head>
<body>
  <header>🗺️ Monumentos emblemáticos de Valladolid</header>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    // Inicializar el mapa centrado en Valladolid
    const map = L.map('map').setView([41.6528, -4.7245], 15);

    // Capa base de OpenStreetMap
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);

    // Monumentos principales
    const monumentos = [
      {
        nombre: "Iglesia de San Pablo",
        coords: [41.6576, -4.7202],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/2/2a/Iglesia_de_San_Pablo_de_Valladolid_2019.jpg",
        breve: "Obra maestra del gótico isabelino con una fachada impresionante del siglo XV.",
        info: "<b>Iglesia de San Pablo</b><br>Construida a finales del siglo XV, la Iglesia de San Pablo es un ejemplo magistral del gótico isabelino. Su fachada, decorada con relieves de gran detalle, fue testigo de acontecimientos históricos como la coronación del rey Felipe II."
      },
      {
        nombre: "Museo Nacional de Escultura (Cadenas de San Gregorio)",
        coords: [41.6587, -4.7218],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/7/79/Colegio_de_San_Gregorio%2C_Valladolid%2C_Espa%C3%B1a%2C_2015-12-30%2C_DD_49.JPG",
        breve: "Antiguo Colegio de San Gregorio, joya del arte plateresco y sede del Museo Nacional de Escultura.",
        info: "<b>Cadenas de San Gregorio</b><br>El conjunto monumental del Colegio de San Gregorio, conocido por sus 'cadenas', alberga el Museo Nacional de Escultura. Su fachada plateresca es una de las más representativas del arte renacentista español."
      },
      {
        nombre: "Catedral de Valladolid",
        coords: [41.6514, -4.7243],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/0/0d/Catedral_de_Valladolid_2018.jpg",
        breve: "Diseñada por Juan de Herrera, combina los estilos herreriano y barroco. Inacabada pero monumental.",
        info: "<b>Catedral de Valladolid</b><br>La Catedral de Nuestra Señora de la Asunción comenzó a construirse en el siglo XVI según los planos de Juan de Herrera. Aunque nunca se terminó, su arquitectura imponente refleja el poder y la historia religiosa de la ciudad."
      },
      {
        nombre: "Estatua de Miguel de Cervantes",
        coords: [41.6553, -4.7275],
        imagen: "https://upload.wikimedia.org/wikipedia/commons/e/e3/Estatua_Miguel_de_Cervantes_Valladolid_2018.jpg",
        breve: "Escultura en honor al célebre autor de Don Quijote, situada en la plaza que lleva su nombre.",
        info: "<b>Estatua de Miguel de Cervantes</b><br>Situada en la Plaza de Cervantes, esta estatua conmemora la estancia del escritor en Valladolid, donde publicó la primera parte de 'Don Quijote de la Mancha' en 1605."
      }
    ];

    // Añadir marcadores con tooltips e info extendida
    monumentos.forEach(monumento => {
      const marker = L.marker(monumento.coords).addTo(map);

      // Tooltip con imagen y descripción breve
      marker.bindTooltip(
        `<div class='tooltip-custom'>
          <img src='${monumento.imagen}' alt='${monumento.nombre}'>
          <div><b>${monumento.nombre}</b><br>${monumento.breve}</div>
        </div>`,
        { direction: "top", offset: [0, -10], opacity: 0.95 }
      );

      // Popup con información completa
      marker.bindPopup(
        `<div style="text-align:center;">
          <img src="${monumento.imagen}" alt="${monumento.nombre}" style="width:200px; border-radius:10px; margin-bottom:8px;">
          <p>${monumento.info}</p>
        </div>`
      );
    });
  </script>
</body>
</html>
