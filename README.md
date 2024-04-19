# Lidar Sensor on Raspberry Pi

This repository guides you through setting up and using a Lidar sensor with a Raspberry Pi for distance measurement and mapping applications. It includes hardware setup instructions, software installation steps, and example Python code to get you started.

## Prerequisites

- Raspberry Pi (any model with GPIO pins)
- Lidar sensor (e.g., Garmin LIDAR-Lite v3)
- Python 3 installed on Raspberry Pi
- Wiring accessories (jumper wires, breadboard)

## Installation

### Hardware Setup

1. Connect the Lidar sensor's power and ground wires to the Raspberry Pi's 5V and GND pins, respectively.
2. Connect the Lidar's SCL and SDA wires to the Raspberry Pi's SCL and SDA GPIO pins for I2C communication.

### Software Installation

Install the necessary libraries for I2C communication:

```bash
sudo apt update
sudo apt install python3-smbus
pip3 install RPi.GPIO
```

## Example - Reading Distance from Lidar

This example demonstrates how to read distance measurements from the Lidar sensor.

### `read_distance.py`

```python
import smbus
import time

# Setup I2C bus
bus = smbus.SMBus(1)
LIDAR_ADDRESS = 0x62

def read_distance():
    # Write 0x04 to register 0x00 to start measurement
    bus.write_byte_data(LIDAR_ADDRESS, 0x00, 0x04)

    # Wait for the measurement to complete
    time.sleep(0.02)

    # Read two bytes from register 0x8f (autoincrement mode)
    distance = bus.read_i2c_block_data(LIDAR_ADDRESS, 0x8f, 2)
    return (distance[0] << 8) + distance[1]

# Example usage
while True:
    print("Distance: {} cm".format(read_distance()))
    time.sleep(1)
```

## Contributing

Contributions to improve the code, add new features, or enhance the documentation are welcome. Please fork the repository, make your changes, and submit a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for more details.
