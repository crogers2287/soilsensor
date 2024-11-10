# ESPHome THC-S RS485 Soil Sensor Configuration

This repository contains the ESPHome configuration for the THC-S RS485 soil sensor, a low-cost Time Domain Reflectometry (TDR) sensor for comprehensive soil monitoring.

## Understanding TDR Technology

Time Domain Reflectometry (TDR) is a sophisticated measurement technique that works similar to radar. Here's how it works:

- The sensor sends an electromagnetic pulse along a probe inserted in the soil
- When this pulse hits changes in the soil (like water content), part of it reflects back
- By measuring these reflections, the sensor can determine soil properties

### About the THC-S RS485

The THC-S RS485 is a Chinese-manufactured low-cost alternative to expensive TDR sensors:
- Professional TDR sensors like Teros-12 can cost $200-500
- The THC-S typically costs $30-50
- Uses RS485 protocol for communication, which is industrial standard and noise-resistant

### Measurements

The sensor provides comprehensive soil data:

#### Soil Moisture
- Measures volumetric water content
- Uses dielectric permittivity of soil
- More accurate than capacitive sensors

#### Temperature
- Built-in temperature sensor
- Used for compensating other readings
- Important for plant health monitoring

#### EC (Electrical Conductivity)
- Measures soil's ability to conduct electricity
- Indicates nutrient availability
- Temperature compensated for accuracy

#### pH
- Measures soil acidity/alkalinity
- Important for nutrient availability

#### NPK
- Nitrogen (N): Essential for leaf growth
- Phosphorus (P): Important for root development
- Potassium (K): Key for overall plant health

### Advantages Over Simpler Sensors
- More accurate than capacitive sensors
- Less affected by soil salinity
- Provides multiple measurements in one device
- More durable due to better build quality
- RS485 allows longer cable runs (up to 1000m)

### Limitations
- Requires more complex setup than simple sensors
- Needs calibration for best accuracy
- More expensive than basic moisture sensors
- Requires RS485 to TTL converter for microcontroller use

## Hardware Requirements

- ESP32-C3 board (or similar ESP32 board)
- THC-S RS485 soil sensor
- TTL to RS485 converter
- Power supply for the sensor (12V recommended)
- Jumper wires or proper cabling for permanent installation

## Wiring

| ESP32-C3 | TTL-RS485 Converter |
|----------|-------------------|
| GPIO5    | TX               |
| GPIO6    | RX               |
| GPIO4    | DE/RE           |
| 3.3V     | VCC             |
| GND      | GND             |

| TTL-RS485 Converter | THC-S Sensor |
|---------------------|--------------|
| A+                  | A+           |
| B-                  | B-           |
| GND                 | GND          |
| --                  | 12V          |

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

2. Copy `soil-sensor.yaml` to your ESPHome directory

3. Create and update the `secrets.yaml`:
```yaml
wifi_ssid: "Your_SSID"
wifi_password: "Your_Password"
wifi_ssid_2: "Your_Backup_SSID"
wifi_password_2: "Your_Backup_Password"
```

4. Flash your ESP32:
```bash
esphome run soil-sensor.yaml
```

## Sensor Calculations and Calibrations

The configuration includes several important calibrations derived from testing and documentation:

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
Based on testing with known solutions:
- Nitrogen: -27 offset
- Phosphorus: -109 offset
- Potassium: -102 offset

## Troubleshooting

Common issues and solutions:

1. No readings from sensor:
   - Check wiring, especially A+ and B- connections
   - Verify 12V power supply
   - Confirm RS485 converter is working (LED indicators)

2. Erratic readings:
   - Check cable length and quality
   - Verify power supply stability
   - Ensure proper grounding

3. Connection issues:
   - Verify WiFi credentials
   - Check ESP32 antenna orientation
   - Consider signal strength and distance to router

## Contributing

Feel free to submit issues and pull requests if you have improvements or fixes to suggest. Please include:
- Clear description of changes
- Test results if applicable
- Any relevant documentation updates

## Credits

This configuration is based on research and documentation from:
- Original Arduino code by [Kromadg](https://github.com/kromadg/soil-sensor)
- ESPHome community contributions
- Personal testing and calibration

## License

MIT License - feel free to use and modify as needed. See [LICENSE](LICENSE) for more details.
