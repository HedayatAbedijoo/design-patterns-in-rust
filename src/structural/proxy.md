# Proxy Pattern

Provides a surrogate or placeholder for another object to control access to it. It allows for additional functionalities or restrictions to be applied to an object without changing its core implementation.

### Example

---

```rust
use std::collections::HashMap;

// Subject: Weather Service
trait WeatherService {
    fn get_temperature(&mut self, city: &str) -> f32;
}

// Real Subject: OpenWeatherMap API
struct OpenWeatherMap {
    api_key: String,
}

impl WeatherService for OpenWeatherMap {
    fn get_temperature(&mut self, city: &str) -> f32 {
        // Implementation to call the OpenWeatherMap API and fetch the temperature for the given city
        // This can involve making HTTP requests or any other necessary operations
        println!(
            "Fetching temperature for {} from OpenWeatherMap API...",
            city
        );

        // For simplicity, let's return a placeholder value
        25.0
    }
}

// Proxy: Cached Weather Service
struct CachedWeatherService {
    weather_service: OpenWeatherMap,
    cache: HashMap<String, f32>,
}

impl CachedWeatherService {
    fn new(weather_service: OpenWeatherMap) -> Self {
        CachedWeatherService {
            weather_service,
            cache: HashMap::new(),
        }
    }
}

impl WeatherService for CachedWeatherService {
    fn get_temperature(&mut self, city: &str) -> f32 {
        // Check if the temperature is available in the cache
        if let Some(&temperature) = self.cache.get(city) {
            println!("Retrieving temperature for {} from cache...", city);
            return temperature;
        }

        // If not found in cache, fetch the temperature using the real weather service
        let temperature = self.weather_service.get_temperature(city);
        println!(
            "Retrieving temperature for {} from weather service...",
            city
        );

        // Store the temperature in the cache for future use
        self.cache.insert(city.to_string(), temperature);

        temperature
    }
}

// Client code
fn main() {
    // Create the real weather service and wrap it with the cached proxy
    let weather_service = OpenWeatherMap {
        api_key: "YOUR_API_KEY".to_string(),
    };
    let mut cached_weather_service = CachedWeatherService::new(weather_service);

    // Access the temperature using the cached proxy
    let city = "London";
    let temperature1 = cached_weather_service.get_temperature(city);
    let temperature2 = cached_weather_service.get_temperature(city);

    println!(
        "Temperature in {} is {} degrees Celsius",
        city, temperature1
    );
    println!("Retrieved temperature from cache: {}", temperature2);
}

```
