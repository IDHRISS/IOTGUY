## WOKWI SIMULATOR
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 16
#define LCD_LINES   2

const int DHT_PIN = 15;

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  lcd.init();
  lcd.backlight();

}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");

  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");

  delay(1000);
}
#WEBDEVELOPMENT
const express = require('express');
const app = express();
const port = 3000;

// Serve static files (HTML, CSS, JavaScript)
app.use(express.static('public'));

// Endpoint to provide fixed temperature and humidity data
app.get('/api/data', (req, res) => {
    const data = {
        temperature: 24.0,
        humidity: 40.0,
    };
    res.json(data);
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
<!DOCTYPE html>
<html>
<head>
    <title>Real-Time Environment Data</title>
</head>
<body>
    <h1>Real-Time Environment Data</h1>
    <div>
        <p>Temperature: <span id="temperature">Loading...</span></p>
        <p>Humidity: <span id="humidity">Loading...</span></p>
    </div>
    <script>
        const temperatureElement = document.getElementById('temperature');
        const humidityElement = document.getElementById('humidity');

        // Function to fetch real-time data and update the web page
        function updateData() {
            fetch('/api/data')
                .then(response => response.json())
                .then(data => {
                    temperatureElement.textContent = data.temperature + ' °C';
                    humidityElement.textContent = data.humidity + '%';
                })
                .catch(error => {
                    console.error('Failed to fetch data:', error);
                });
        }

        // Periodically update data (e.g., every 5 seconds)
        setInterval(updateData, 5000);
    </script>
</body>
</html>
node server.js










