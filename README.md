<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bootstrap Cart</title>
     <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <style>
        .card {
            border: 1px solid black;
            box-shadow: 2px 2px 5px black, 2px 2px 5px white, 5px 5px 25px black;
            height: 100%;
        }

        h1 {
            text-align: center;
            text-shadow: 2px 2px 5px black, 5px 5px 5px black, 2px 2px 5px white;
            font-size: 80px;
            height: 30px;
            margin-top: 50px;
        }

        #countryCards {
            margin-top: 150px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>COUNTRY DETAILS</h1>
        <div id="countryCards" class="row"></div>
    </div>
    <script>
        // Function to fetch country data from Restcountries API
        function fetchCountryData() {
            fetch('https://restcountries.com/v3.1/all')
                .then(response => response.json())
                .then(data => {
                    data.forEach(country => {
                        createCountryCard(country);
                    });
                })
                .catch(error => {
                    console.log('Error:', error);
                });
        }

        // Function to fetch weather data from OpenWeatherMap API
        function fetchWeatherData(city, countryCode) {
            const apiKey = 'YOUR_OPENWEATHERMAP_API_KEY';
            const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${city},${countryCode}&appid=${apiKey}&units=metric`;

            fetch(apiUrl)
                .then(response => response.json())
                .then(data => {
                    displayWeatherData(city, data);
                })
                .catch(error => {
                    console.log('Error:', error);
                });
        }

        // Function to create Bootstrap card for each country
        function createCountryCard(country) {
            const countryCards = document.getElementById('countryCards');

            // Create card element
            const card = document.createElement('div');
            card.classList.add('col-md-4', 'mb-4');

            // Create card body
            const cardBody = document.createElement('div');
            cardBody.classList.add('card', 'text-center', 'h-100');

            // Create card title
            const cardTitle = document.createElement('h3');
            cardTitle.classList.add('card-header');
            cardTitle.textContent = country.name.common;

            // Create card content
            const cardContent = document.createElement('div');
            cardContent.classList.add('card-body');

            // Create capital element
            const capital = document.createElement('p');
            capital.textContent = `Capital: ${country.capital}`;

            // Create latlng element
            const latlng = document.createElement('p');
            latlng.textContent = `Latlng: ${country.latlng}`;

            // Create flag element
            const flag = document.createElement('img');
            flag.src = country.flags.png;
            flag.classList.add('card-img-top', 'mt-3');

            // Create weather button
            const weatherButton = document.createElement('button');
            weatherButton.classList.add('btn', 'btn-primary');
            weatherButton.textContent = 'Click for Weather';

            // Append elements to card content
            cardContent.appendChild(capital);
            cardContent.appendChild(latlng);
            cardContent.appendChild(weatherButton);

            // Append elements to card body
            cardBody.appendChild(cardTitle);
            cardBody.appendChild(flag);
            cardBody.appendChild(cardContent);

            // Append card body to card
            card.appendChild(cardBody);

            // Append card to country cards container
            countryCards.appendChild(card);

            // Add event listener to weather button
            weatherButton.addEventListener('click', function () {
                fetchWeatherData(country.capital, country.cca2);
            });
        }

        // Function to display weather data in the country card
        function displayWeatherData(city, data) {
            const countryCards = document.getElementById('countryCards').querySelectorAll('.card');

            for (let i = 0; i < countryCards.length; i++) {
                const cardTitle = countryCards[i].querySelector('.card-header').textContent;
                if (cardTitle === city) {
                    const weatherInfo = document.createElement('p');
                    const temperature = data.main.temp;
                    const weatherDescription = data.weather[0].description;
                    weatherInfo.textContent = `Temperature: ${temperature}°C, Weather: ${weatherDescription}`;
                    countryCards[i].querySelector('.card-body').appendChild(weatherInfo);
                    break;
                }
            }
        }

        // Call the fetchCountryData function on page load
        window.addEventListener('load', fetchCountryData);
    </script>

<script src="https://app.zenclass.in/sheets/v1/js/zen/suite/bundle.js"></script>

    <!-- Bootstrap JS -->
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
</body>
</html>
