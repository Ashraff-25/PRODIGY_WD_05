# PRODIGY_WD_05

#####HTML

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Weather App</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>

  <div class="container">
    <h1>🌦 Weather App</h1>
    <input type="text" id="cityInput" placeholder="Enter city name" />
    <button onclick="getWeatherByCity()">Search</button>
    <button onclick="getWeatherByLocation()">Use My Location</button>

    <div class="weather" id="weatherDisplay"></div>
  </div>

  <script src="script.js"></script>
</body>
</html>


###CSS

body {
  font-family: 'Arial', sans-serif;
  background: #e0f7fa;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.container {
  background: white;
  padding: 30px;
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 5px 15px rgba(0,0,0,0.2);
  width: 300px;
}

input {
  padding: 10px;
  margin: 10px 0;
  width: 80%;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  margin: 5px;
  padding: 10px 15px;
  background-color: teal;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.weather {
  margin-top: 20px;
  text-align: left;
}


##Javascript
const API_KEY = "YOUR_API_KEY"; // Replace with your OpenWeatherMap API key

function getWeatherByCity() {
  const city = document.getElementById("cityInput").value;
  if (!city) return alert("Please enter a city name.");
  fetchWeather(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`);
}

function getWeatherByLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(
      position => {
        const { latitude, longitude } = position.coords;
        fetchWeather(`https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=${API_KEY}&units=metric`);
      },
      error => alert("Unable to access location.")
    );
  } else {
    alert("Geolocation is not supported by this browser.");
  }
}

function fetchWeather(url) {
  fetch(url)
    .then(response => response.json())
    .then(data => displayWeather(data))
    .catch(() => alert("Error fetching weather data."));
}

function displayWeather(data) {
  if (data.cod !== 200) {
    document.getElementById("weatherDisplay").innerHTML = `<p>${data.message}</p>`;
    return;
  }

  const html = `
    <h2>${data.name}, ${data.sys.country}</h2>
    <p><strong>Temperature:</strong> ${data.main.temp}°C</p>
    <p><strong>Weather:</strong> ${data.weather[0].description}</p>
    <p><strong>Humidity:</strong> ${data.main.humidity}%</p>
    <p><strong>Wind:</strong> ${data.wind.speed} m/s</p>
  `;
  document.getElementById("weatherDisplay").innerHTML = html;
}
