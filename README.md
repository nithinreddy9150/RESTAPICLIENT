# RESTAPICLIENT
## 1. Introduction
This Java program fetches real-time weather data using the OpenWeatherMap API. It retrieves weather details such as temperature, humidity, and description for a given city.

## 2. Requirements
- Java Development Kit (JDK) 8 or later
- Internet connection
- OpenWeatherMap API key (Replace API_KEY in the code)

## 3. Implementation Details
- Sends an HTTP request to the OpenWeatherMap API.
- Parses the JSON response to extract weather information.
- Displays the temperature, humidity, and weather description.

## 4. Code Explanation
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import org.json.JSONObject;

public class Main {
    private static final String API_KEY = "d440db18d66c20e6675e9e1452672055"; // Replace with your OpenWeatherMap API key
    private static final String BASE_URL = "https://api.openweathermap.org/data/2.5/weather?q=";

    public static void main(String[] args) {
        String city = "chennai"; // Change city name as needed
        fetchWeatherData(city);
    }

    private static void fetchWeatherData(String city) {
        try {
            String urlString = BASE_URL + city + "&appid=" + API_KEY + "&units=metric";
            URL url = new URL(urlString);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            conn.setRequestMethod("GET");

            int responseCode = conn.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) {
                BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
                String inputLine;
                StringBuilder response = new StringBuilder();

                while ((inputLine = in.readLine()) != null) {
                    response.append(inputLine);
                }
                in.close();

                parseAndDisplayWeather(response.toString());
            } else {
                System.out.println("Error: Unable to fetch weather data");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static void parseAndDisplayWeather(String jsonResponse) {
        JSONObject obj = new JSONObject(jsonResponse);
        String cityName = obj.getString("name");
        JSONObject main = obj.getJSONObject("main");
        double temperature = main.getDouble("temp");
        int humidity = main.getInt("humidity");
        JSONObject weather = obj.getJSONArray("weather").getJSONObject(0);
        String description = weather.getString("description");

        System.out.println("Weather Data for " + cityName + ":");
        System.out.println("Temperature: " + temperature + "°C");
        System.out.println("Humidity: " + humidity + "%");
        System.out.println("Description: " + description);
    }
}
5. How to Run the Program
Replace "your_api_key" with a valid API key from OpenWeatherMap.
Save the file as Main.java.
Open a terminal or command prompt.
Navigate to the directory where the Java file is saved.
Compile the program using:
javac Main.java
Run the program using:
java Main
6. Sample Output
Weather Data for Chennai:
Temperature: 30.5°C
Humidity: 75%
Description: clear sky
7. Conclusion
This program demonstrates how to fetch and process weather data using a REST API in Java. It provides real-time weather updates for any specified city.
