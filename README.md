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
  <header>🗺️ Monumentos de Valladolid</header>
  <div id="map"></div>

  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
  <script>
    // Inicialización del mapa centrado en Valladolid
    const map = L.map('map').setView([41.6528, -4.7245], 14);

    // Capa base (OpenStreetMap)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '© OpenStreetMap contributors',
      maxZoom: 19
    }).addTo(map);

    // Marcadores de monumentos
    const monumentos = [
      {
        nombre: "Plaza Mayor de Valladolid",
        coords: [41.6528, -4.7284],
        info: "<b>Plaza Mayor de Valladolid</b><br>Centro neurálgico de la ciudad. Fue una de las primeras plazas mayores regulares de España, modelo para otras como la de Madrid."
      },
      {
        nombre: "Catedral de Valladolid",
        coords: [41.6514, -4.7243],
        info: "<b>Catedral de Nuestra Señora de la Asunción</b><br>Diseñada por Juan de Herrera, esta catedral inacabada combina estilo herreriano y barroco."
      },
      {
        nombre: "Iglesia de San Pablo",
        coords: [41.6576, -4.7202],
        info: "<b>Iglesia de San Pablo</b><br>Ejemplo destacado del gótico isabelino. Su fachada es una joya escultórica del siglo XV."
      },
      {
        nombre: "Campo Grande",
        coords: [41.6485, -4.7285],
        info: "<b>Campo Grande</b><br>El pulmón verde de Valladolid, con pavos reales, fuentes y monumentos entre frondosos paseos."
      },
      {
        nombre: "Museo Nacional de Escultura",
        coords: [41.6587, -4.7218],
        info: "<b>Museo Nacional de Escultura</b><br>Ubicado en el Colegio de San Gregorio, alberga una de las mejores colecciones de escultura policromada de España."
      }
    ];

    // Añadir los marcadores al mapa
    monumentos.forEach(monumento => {
      L.marker(monumento.coords)
        .addTo(map)
        .bindPopup(monumento.info);
    });
  </script>
</body>
</html>

