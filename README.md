# ESPHome THC-S RS485 Soil Sensor Configuration

This repository contains the ESPHome configuration for the THC-S RS485 soil sensor, a low-cost Time Domain Reflectometry (TDR) sensor that measures:
- Soil Moisture
- Temperature
- Electrical Conductivity (EC)
- pH
- NPK (Nitrogen, Phosphorus, Potassium) levels

## Hardware Requirements

- ESP32-C3 board (or similar ESP32 board)
- THC-S RS485 soil sensor
- TTL to RS485 converter
- Power supply for the sensor

## Wiring

| ESP32-C3 | TTL-RS485 |
|----------|-----------|
| GPIO5    | TX        |
| GPIO6    | RX        |
| GPIO4    | DE/RE     |
| 3.3V     | VCC       |
| GND      | GND       |

## Features

- Direct integration with Home Assistant via ESPHome
- Temperature readings in both Celsius and Fahrenheit
- Temperature-compensated EC readings
- Calibrated soil moisture readings using dielectric constant calculations
- Offset-corrected NPK readings
- Multiple WiFi network support with fallback AP
- Over-the-air updates

## Installation

1. Install ESPHome if you haven't already:
```bash
pip install esphome
```

2. Copy the `soil-sensor.yaml` to your ESPHome directory

3. Update the WiFi credentials in your `secrets.yaml`:
```yaml
wifi_ssid: "Your_SSID"
wifi_password: "Your_Password"
```

4. Flash your ESP32:
```bash
esphome run soil-sensor.yaml
```

## Sensor Calculations

The configuration includes several important calibrations:

### Soil Moisture
Uses a quadratic approximation for the apparent dielectric constant:
```
soil_apparent_dieletric_constant = 1.3088 + (0.1439 * raw_value) + (0.0076 * raw_value * raw_value)
```

### EC (Electrical Conductivity)
Includes temperature compensation:
```
soil_ec = 1.93 * raw_ec - 270.8
soil_ec = soil_ec / (1.0 + 0.019 * (temp - 25.0))
```

### NPK Offsets
- Nitrogen: -27
- Phosphorus: -109
- Potassium: -102

## Contributing

Feel free to submit issues and pull requests if you have improvements or fixes to suggest.

## Credits

This configuration is based on research and documentation from:
- Original Arduino code by [Kromadg](https://github.com/kromadg/soil-sensor)
- [Detailed blog post about the setup](your-blog-post-if-you-have-one)

## License

MIT License - feel free to use and modify as needed.
