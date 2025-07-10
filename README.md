# weather-api<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: white;
            overflow-x: hidden;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        .header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        .search-container {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
            gap: 10px;
        }

        .search-input {
            padding: 12px 20px;
            font-size: 1rem;
            border: none;
            border-radius: 25px;
            width: 300px;
            outline: none;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .search-btn {
            padding: 12px 25px;
            font-size: 1rem;
            border: none;
            border-radius: 25px;
            background: white;
            color: #667eea;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            transition: all 0.3s ease;
        }

        .search-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.2);
        }

        .search-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .error {
            background: rgba(255,0,0,0.1);
            border: 1px solid rgba(255,0,0,0.3);
            padding: 15px;
            border-radius: 10px;
            margin: 20px auto;
            max-width: 500px;
            text-align: center;
        }

        .loading {
            text-align: center;
            font-size: 1.2rem;
            margin: 40px 0;
        }

        .weather-container {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 30px;
            margin-top: 30px;
        }

        .main-weather {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
        }

        .weather-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 30px;
        }

        .location h2 {
            font-size: 2rem;
            margin-bottom: 5px;
        }

        .location p {
            opacity: 0.8;
            font-size: 1.1rem;
        }

        .temperature {
            text-align: right;
        }

        .temperature .temp {
            font-size: 3.5rem;
            font-weight: bold;
            margin-bottom: 5px;
        }

        .temperature .feels-like {
            opacity: 0.8;
        }

        .weather-info {
            display: flex;
            align-items: center;
            gap: 20px;
            margin-bottom: 30px;
        }

        .weather-icon {
            font-size: 4rem;
        }

        .weather-desc h3 {
            font-size: 1.5rem;
            margin-bottom: 5px;
        }

        .weather-desc p {
            opacity: 0.8;
        }

        .weather-details {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
            gap: 15px;
        }

        .detail-item {
            background: rgba(255,255,255,0.1);
            padding: 20px;
            border-radius: 15px;
            text-align: center;
        }

        .detail-item .icon {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .detail-item .label {
            font-size: 0.9rem;
            opacity: 0.8;
            margin-bottom: 5px;
        }

        .detail-item .value {
            font-size: 1.2rem;
            font-weight: bold;
        }

        .forecast-container {
            background: rgba(255,255,255,0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0,0,0,0.1);
        }

        .forecast-container h3 {
            font-size: 1.5rem;
            margin-bottom: 20px;
        }

        .forecast-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px;
            background: rgba(255,255,255,0.1);
            border-radius: 10px;
            margin-bottom: 15px;
        }

        .forecast-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .forecast-icon {
            font-size: 2rem;
        }

        .forecast-day {
            font-weight: bold;
            margin-bottom: 3px;
        }

        .forecast-rain {
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .forecast-temps {
            text-align: right;
        }

        .forecast-high {
            font-weight: bold;
            font-size: 1.1rem;
        }

        .forecast-low {
            opacity: 0.8;
            font-size: 0.9rem;
        }

        .hidden {
            display: none;
        }

        @media (max-width: 768px) {
            .weather-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }

            .search-container {
                flex-direction: column;
                align-items: center;
            }

            .search-input {
                width: 100%;
                max-width: 400px;
            }

            .weather-header {
                flex-direction: column;
                text-align: center;
                gap: 20px;
            }

            .weather-info {
                flex-direction: column;
                text-align: center;
            }

            .weather-details {
                grid-template-columns: repeat(2, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Weather App</h1>
            <p>Get current weather and 5-day forecast</p>
        </div>

        <div class="search-container">
            <input type="text" id="cityInput" class="search-input" placeholder="Enter city name..." value="London">
            <button id="searchBtn" class="search-btn">Search</button>
        </div>

        <div id="error" class="error hidden"></div>
        <div id="loading" class="loading hidden">Loading...</div>

        <div id="weatherContainer" class="weather-container hidden">
            <div class="main-weather">
                <div class="weather-header">
                    <div class="location">
                        <h2 id="cityName">London</h2>
                        <p id="country">GB</p>
                    </div>
                    <div class="temperature">
                        <div id="temperature" class="temp">22°C</div>
                        <div id="feelsLike" class="feels-like">Feels like 25°C</div>
                    </div>
                </div>

                <div class="weather-info">
                    <div id="weatherIcon" class="weather-icon">☀️</div>
                    <div class="weather-desc">
                        <h3 id="condition">Sunny</h3>
                        <p id="description">Clear skies with light winds</p>
                    </div>
                </div>

                <div class="weather-details">
                    <div class="detail-item">
                        <div class="icon">💨</div>
                        <div class="label">Wind Speed</div>
                        <div id="windSpeed" class="value">12 km/h</div>
                    </div>
                    <div class="detail-item">
                        <div class="icon">💧</div>
                        <div class="label">Humidity</div>
                        <div id="humidity" class="value">65%</div>
                    </div>
                    <div class="detail-item">
                        <div class="icon">👁️</div>
                        <div class="label">Visibility</div>
                        <div id="visibility" class="value">10 km</div>
                    </div>
                    <div class="detail-item">
                        <div class="icon">🌡️</div>
                        <div class="label">Pressure</div>
                        <div id="pressure" class="value">1013 hPa</div>
                    </div>
                </div>
            </div>

            <div class="forecast-container">
                <h3>5-Day Forecast</h3>
                <div id="forecastList"></div>
            </div>
        </div>
    </div>

    <script>
        class WeatherApp {
            constructor() {
                this.apiKey = 'YOUR_API_KEY_HERE'; // Replace with your OpenWeatherMap API key
                this.baseUrl = 'https://api.openweathermap.org/data/2.5';
                this.init();
            }

            init() {
                this.cityInput = document.getElementById('cityInput');
                this.searchBtn = document.getElementById('searchBtn');
                this.errorDiv = document.getElementById('error');
                this.loadingDiv = document.getElementById('loading');
                this.weatherContainer = document.getElementById('weatherContainer');

                this.searchBtn.addEventListener('click', () => this.handleSearch());
                this.cityInput.addEventListener('keypress', (e) => {
                    if (e.key === 'Enter') this.handleSearch();
                });

                // Load initial weather for London
                this.fetchWeather('London');
            }

            async handleSearch() {
                const city = this.cityInput.value.trim();
                if (city) {
                    await this.fetchWeather(city);
                }
            }

            async fetchWeather(city) {
                this.showLoading();
                this.hideError();

                try {
                    const [weatherData, forecastData] = await Promise.all([
                        this.getCurrentWeather(city),
                        this.getForecast(city)
                    ]);

                    this.displayWeather(weatherData);
                    this.displayForecast(forecastData);
                    this.hideLoading();
                    this.showWeather();
                } catch (error) {
                    this.hideLoading();
                    this.showError(error.message);
                }
            }

            async getCurrentWeather(city) {
                // For demo purposes, using mock data
                // Replace this with actual API call
                return new Promise((resolve) => {
                    setTimeout(() => {
                        resolve({
                            name: city,
                            sys: { country: 'GB' },
                            main: {
                                temp: Math.round(Math.random() * 30 + 5),
                                feels_like: Math.round(Math.random() * 30 + 5),
                                humidity: Math.round(Math.random() * 50 + 30),
                                pressure: Math.round(Math.random() * 50 + 1000)
                            },
                            weather: [{
                                main: ['Sunny', 'Cloudy', 'Rainy', 'Snowy'][Math.floor(Math.random() * 4)],
                                description: 'Partly cloudy with light winds'
                            }],
                            wind: { speed: Math.round(Math.random() * 20 + 5) },
                            visibility: Math.round(Math.random() * 10 + 5) * 1000
                        });
                    }, 1000);
                });
            }

            async getForecast(city) {
                // Mock forecast data
                return new Promise((resolve) => {
                    setTimeout(() => {
                        const forecast = Array.from({ length: 5 }, (_, i) => ({
                            day: ['Today', 'Tomorrow', 'Sunday', 'Monday', 'Tuesday'][i],
                            high: Math.round(Math.random() * 25 + 15),
                            low: Math.round(Math.random() * 15 + 5),
                            condition: ['Sunny', 'Cloudy', 'Rainy', 'Snowy'][Math.floor(Math.random() * 4)],
                            precipitation: Math.round(Math.random() * 80)
                        }));
                        resolve(forecast);
                    }, 500);
                });
            }

            displayWeather(data) {
                document.getElementById('cityName').textContent = data.name;
                document.getElementById('country').textContent = data.sys.country;
                document.getElementById('temperature').textContent = `${data.main.temp}°C`;
                document.getElementById('feelsLike').textContent = `Feels like ${data.main.feels_like}°C`;
                document.getElementById('condition').textContent = data.weather[0].main;
                document.getElementById('description').textContent = data.weather[0].description;
                document.getElementById('windSpeed').textContent = `${data.wind.speed} km/h`;
                document.getElementById('humidity').textContent = `${data.main.humidity}%`;
                document.getElementById('visibility').textContent = `${Math.round(data.visibility / 1000)} km`;
                document.getElementById('pressure').textContent = `${data.main.pressure} hPa`;

                // Set weather icon
                const iconMap = {
                    'Sunny': '☀️',
                    'Cloudy': '☁️',
                    'Rainy': '🌧️',
                    'Snowy': '❄️'
                };
                document.getElementById('weatherIcon').textContent = iconMap[data.weather[0].main] || '☀️';
            }

            displayForecast(forecast) {
                const forecastList = document.getElementById('forecastList');
                forecastList.innerHTML = '';

                forecast.forEach(day => {
                    const forecastItem = document.createElement('div');
                    forecastItem.className = 'forecast-item';

                    const iconMap = {
                        'Sunny': '☀️',
                        'Cloudy': '☁️',
                        'Rainy': '🌧️',
                        'Snowy': '❄️'
                    };

                    forecastItem.innerHTML = `
                        <div class="forecast-left">
                            <div class="forecast-icon">${iconMap[day.condition] || '☀️'}</div>
                            <div>
                                <div class="forecast-day">${day.day}</div>
                                <div class="forecast-rain">${day.precipitation}% rain</div>
                            </div>
                        </div>
                        <div class="forecast-temps">
                            <div class="forecast-high">${day.high}°</div>
                            <div class="forecast-low">${day.low}°</div>
                        </div>
                    `;

                    forecastList.appendChild(forecastItem);
                });
            }

            showLoading() {
                this.loadingDiv.classList.remove('hidden');
                this.searchBtn.disabled = true;
                this.searchBtn.textContent = 'Loading...';
            }

            hideLoading() {
                this.loadingDiv.classList.add('hidden');
                this.searchBtn.disabled = false;
                this.searchBtn.textContent = 'Search';
            }

            showError(message) {
                this.errorDiv.textContent = message;
                this.errorDiv.classList.remove('hidden');
                this.weatherContainer.classList.add('hidden');
            }

            hideError() {
                this.errorDiv.classList.add('hidden');
            }

            showWeather() {
                this.weatherContainer.classList.remove('hidden');
            }
        }

        // Initialize the app
        new WeatherApp();
    </script>
</body>
</html>
