<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Weather App</title>

<style>

body {
  font-family: Arial;
  background: #86a7c8;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
}

.container {
  background: rgb(14, 190, 161);
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
  text-align: center;
  width: 350px;
}

button {
  padding: 10px 20px;
  border: none;
  background: #0079c1;
  color: rgb(255, 255, 255);
  border-radius: 4px;
  cursor: pointer;
  margin-top: 5px;
  font-size: 15px;
}

button:hover {
  background: #033c5f;
}

.loader {
  border: 4px solid #b74d4d;
  border-top: 4px solid #0079c1;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin: 20px auto;
  display: none;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

.error {
  color: red;
  display: none;
  margin-top: 10px;
}

.success {
  display: none;
  margin-top: 20px;
  background: #d4edda;
  border: 1px solid #28a745;
  border-radius: 8px;
  padding: 15px;
  color: #155724;
}

.success .checkmark {
  font-size: 40px;
}

.success p {
  margin: 8px 0 0;
  font-size: 16px;
  font-weight: bold;
}

.success small {
  color: #555;
  font-size: 13px;
}

</style>
</head>

<body>

<div class="container">

  <h2>📍 Location App</h2>

  <button id="locationBtn">Get My Location</button>

  <div id="loading" class="loader"></div>

  <div id="error" class="error"></div>

  <div id="successBox" class="success">
    <div class="checkmark">✅</div>
    <p>Your location is successfully taken!</p>
    <small id="coordsText"></small>
  </div>

</div>

<script>

const locationBtn = document.getElementById("locationBtn")
const loading     = document.getElementById("loading")
const errorDiv    = document.getElementById("error")
const successBox  = document.getElementById("successBox")
const coordsText  = document.getElementById("coordsText")


function showLoading() {
  loading.style.display    = "block"
  successBox.style.display = "none"
  errorDiv.style.display   = "none"
  locationBtn.disabled     = true
}

function hideLoading() {
  loading.style.display = "none"
  locationBtn.disabled  = false
}

function showError(msg) {
  errorDiv.innerText     = msg
  errorDiv.style.display = "block"
}

function showSuccess(lat, lon) {
  coordsText.innerText     = `Lat: ${lat.toFixed(5)} | Lon: ${lon.toFixed(5)}`
  successBox.style.display = "block"
}


locationBtn.addEventListener("click", () => {
  if (!navigator.geolocation) {
    showError("Geolocation is not supported by your browser.")
    return
  }

  showLoading()

  navigator.geolocation.getCurrentPosition(
    (position) => {
      hideLoading()
      showSuccess(position.coords.latitude, position.coords.longitude)
    },
    (err) => {
      hideLoading()
      showError("Location permission denied. Please allow access and try again.")
    }
  )
})

</script>

</body>
