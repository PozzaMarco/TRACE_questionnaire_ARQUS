<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>TRACE Emotional Mapping Form</title>

  <!-- Bootstrap CSS -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">

  <!-- Leaflet CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />

  <!-- Leaflet Control Geocoder CSS -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.css" />

  <style>
    #map {
      height: 400px;
      width: 100%;
      margin-bottom: 20px;
    }
  </style>
</head>
<body class="bg-light">

  <div class="container mt-5">
    <div class="card shadow-lg">
      <div class="card-header bg-primary text-white">
        <h4 class="mb-0">TRACE Emotional Mapping</h4>
      </div>
      <div class="card-body">

        <div id="map" class="mb-4 rounded border"></div>

        <form id="mappingForm" method="POST" target="_blank"
              action="https://docs.google.com/forms/d/e/1FAIpQLSclgxvAxCD9i9Vb0YOelNukNERuxKP-RyGiSLoRFsFci8yq2g/formResponse">

          <!-- Hidden fields for latitude and longitude -->
          <input type="hidden" name="entry.1904858787" id="latInput">
          <input type="hidden" name="entry.224005917" id="lngInput">

          <div class="mb-3">
            <label for="city" class="form-label">City</label>
            <select name="entry.1494796080" id="city" class="form-select" required>
              <option value="">-- Select a City --</option>
              <option value="Padova">Padova</option>
              <option value="Granada">Granada</option>
            </select>
          </div>

          <div class="mb-3">
            <label for="place" class="form-label">Place Description</label>
            <input type="text" name="entry.517676228" id="place" class="form-control" placeholder="e.g., Main square, park..." required pattern=".{3,}" title="Please enter at least 3 characters.">
          </div>

          <div class="row">
            <div class="col-md-4 mb-3">
              <label for="type" class="form-label">Type of Place</label>
              <select name="entry.274883972" id="type" class="form-select" required>
                <option value="">-- Select Type --</option>
                <option value="Social">Social</option>
                <option value="Academic">Academic</option>
                <option value="Historical">Historical</option>
                <option value="Natural">Natural</option>
              </select>
            </div>

            <div class="col-md-4 mb-3">
              <label for="emotion" class="form-label">Emotion</label>
              <select name="entry.700718928" id="emotion" class="form-select" required>
                <option value="">-- Select Emotion --</option>
                <option value="Positive">Positive</option>
                <option value="Neutral">Neutral</option>
                <option value="Negative">Negative</option>
              </select>
            </div>
          </div>

          <div class="mb-3">
            <label for="reflection" class="form-label">Why is this emotion important? (Optional)</label>
            <textarea name="entry.580084989" id="reflection" class="form-control" rows="3" placeholder="Your thoughts..."></textarea>
          </div>

          <button type="submit" class="btn btn-success">Submit</button>
        </form>

      </div>
    </div>
  </div>

  <!-- Leaflet JS -->
  <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>

  <!-- Leaflet Control Geocoder JS -->
  <script src="https://unpkg.com/leaflet-control-geocoder/dist/Control.Geocoder.js"></script>

  <!-- Bootstrap JS Bundle -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

  <script>
    const map = L.map('map').setView([41.9, 12.5], 5);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors'
    }).addTo(map);

    let marker;

    map.on('click', function(e) {
      if (marker) map.removeLayer(marker);
      marker = L.marker(e.latlng).addTo(map);
      document.getElementById('latInput').value = e.latlng.lat;
      document.getElementById('lngInput').value = e.latlng.lng;
    });

    L.Control.geocoder({
      defaultMarkGeocode: false
    })
    .on('markgeocode', function(e) {
      const center = e.geocode.center;
      const bbox = e.geocode.bbox;

      if (marker) map.removeLayer(marker);
      marker = L.marker(center).addTo(map);
      map.fitBounds(bbox);

      document.getElementById('latInput').value = center.lat;
      document.getElementById('lngInput').value = center.lng;
    })
    .addTo(map);

    const form = document.getElementById('mappingForm');
    form.addEventListener('submit', function(e) {
      const lat = document.getElementById('latInput').value.trim();
      const lng = document.getElementById('lngInput').value.trim();
      const place = document.getElementById('place').value.trim();

      if (!lat || !lng) {
        e.preventDefault();
        alert("Please select a location on the map.");
        return;
      }

      document.getElementById('place').value = place;
    });
  </script>

</body>
</html>
