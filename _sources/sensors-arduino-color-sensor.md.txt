# Documentation for Setting Up Color Sensors with Adafruit TCS34725 and TCA9548A Multiplexer

This guide explains how to set up multiple Adafruit TCS34725 color sensors using a TCA9548A I2C multiplexer. The provided code initializes the sensors, reads their RGB values, and prints the data to the Serial Monitor.

---

## Prerequisites

1. **Hardware Requirements**:
   - Arduino board (e.g., Uno, Mega, etc.).
   - Two or more Adafruit TCS34725 RGB color sensors.
   - A TCA9548A I2C multiplexer.
   - Connecting wires (jumper wires).

2. **Software Requirements**:
   - Arduino IDE installed on your computer.
   - `Adafruit_TCS34725` library installed in the Arduino IDE.

---

## Hardware Setup

1. **Connect the TCA9548A Multiplexer**:
   - Connect `VIN` and `GND` of the multiplexer to the Arduino's `5V` and `GND` pins.
   - Connect the `SCL` and `SDA` pins of the multiplexer to the corresponding pins on the Arduino.

2. **Connect the TCS34725 Sensors**:
   - Connect each sensor’s `VIN` and `GND` to the multiplexer’s `VIN` and `GND`.
   - Connect the `SCL` and `SDA` pins of each sensor to the corresponding channels of the TCA9548A (e.g., channel 2 for `LEFTSENSOR` and channel 3 for `RIGHTSENSOR`).

3. **Configure Multiplexer Address**:
   - The TCA9548A default I2C address is `0x70`. If you have changed the address, update the `TCAADDR` constant in the code accordingly.

---

## Software Setup

1. **Install Required Libraries**:
   - Open the Arduino IDE.
   - Navigate to **Sketch** → **Include Library** → **Manage Libraries**.
   - Search for `Adafruit TCS34725` and click **Install**.

2. **Upload the Code**:
   - Open the provided code in the Arduino IDE.
   - Connect your Arduino to the computer.
   - Select the appropriate **Board** and **Port** under the **Tools** menu.
   - Click **Upload**.

---

## Code Explanation

### 1. **Library and Constants Setup**
```cpp
#include <Adafruit_TCS34725.h>

#define TCAADDR 0x70
#define LEFTSENSOR 2
#define RIGHTSENSOR 3
```
- The `Adafruit_TCS34725` library is included for sensor control.
- `TCAADDR` defines the I2C address of the multiplexer.
- `LEFTSENSOR` and `RIGHTSENSOR` correspond to the multiplexer channels connected to the sensors.

---

### 2. **RGB Variable Declaration**
```cpp
uint16_t r, g, b, c;
```
- These variables store the red, green, blue, and clear channel values.

---

### 3. **Multiplexer Channel Selection**
```cpp
void tcaSelect(uint8_t i) {
  if (i > 7) return;

  Wire.beginTransmission(TCAADDR);
  Wire.write(1 << i);
  Wire.endTransmission();  
}
```
- The `tcaSelect` function activates a specific multiplexer channel by writing a bit corresponding to the desired channel.

---

### 4. **Sensor Initialization**
```cpp
Adafruit_TCS34725 lightSensor = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_2_4MS, TCS34725_GAIN_1X);

void setup() {
  Serial.begin(115200);
  lightSensor.begin();
  tcaSelect(LEFTSENSOR);
  delay(100);
}
```
- The sensor is initialized with a 2.4ms integration time and a gain of `1X` for accuracy.

---

### 5. **Reading and Displaying Sensor Data**
```cpp
void loop() {
  tcaSelect(LEFTSENSOR);
  lightSensor.getRawData(&r,  &g,  &b,  &c);
  Serial.print("RED: "); Serial.println(r, DEC);
  Serial.print("GREEN: "); Serial.println(g, DEC);
  Serial.print("BLUE: "); Serial.println(b, DEC);
}
```
- The `loop` function alternates between reading data from the left and right sensors.
- RGB values are read and printed to the Serial Monitor with a 500ms delay.

---

## Testing the Setup

1. Open the Arduino IDE’s **Serial Monitor** (set baud rate to `115200`).
2. Observe the RGB data for both the left and right sensors.

---

## Troubleshooting

1. **No Data on Serial Monitor**:
   - Ensure the sensors are connected correctly to the multiplexer.
   - Check the I2C address of the multiplexer.
   - Verify the connections for `SCL` and `SDA`.

2. **Incorrect RGB Values**:
   - Ensure the sensors are not blocked.
   - Check the integration time and gain settings.

---

By following this guide, you should be able to successfully set up and use multiple Adafruit TCS34725 sensors with a TCA9548A multiplexer.