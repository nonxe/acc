# Vehicle Accident Detection & Alert System

**Arduino UNO** based real-time crash detection system using accelerometer, GPS & GSM module.

![Circuit Wiring Diagram](arduino-uno-wiring.jpg)

---

## Project Overview

This system detects vehicle accidents in real-time using an ADXL335 accelerometer to measure sudden impact. When a crash is detected:

- Buzzer sounds an alert immediately
- GPS location is captured via NEO-6M module
- Phone call is made to emergency contact via SIM800L GSM
- SMS is sent with Google Maps link of the crash location
- Cancel button allows the driver to cancel false alerts within 30 seconds

---

## Components Required

| # | Component | Qty | Description |
|---|-----------|-----|-------------|
| 1 | Arduino UNO (ATmega328P) | 1 | Main microcontroller board |
| 2 | SIM800L GSM Module | 1 | For making calls & sending SMS |
| 3 | NEO-6M GPS Module | 1 | For getting GPS coordinates |
| 4 | ADXL335 Accelerometer | 1 | 3-axis analog accelerometer for impact detection |
| 5 | 16x2 I2C LCD Display | 1 | For displaying status (I2C address: 0x27) |
| 6 | Buzzer (Active) | 1 | For audible crash alert |
| 7 | Push Button | 1 | Cancel button to dismiss false alerts |
| 8 | SIM Card (2G) | 1 | For GSM module (must support 2G network) |
| 9 | GPS Antenna | 1 | Usually comes with NEO-6M module |
| 10 | Jumper Wires | ~20 | Male-to-Male & Male-to-Female |
| 11 | Breadboard | 1 | For prototyping connections |
| 12 | Power Supply | 1 | 5V USB or battery pack |

---

## Circuit Wiring

### Complete Pin Connections

| Component | Component Pin | Arduino UNO Pin | Description |
|-----------|--------------|-----------------|-------------|
| SIM800L GSM | TX | Digital Pin 2 | GSM transmits data to Arduino (SoftwareSerial RX) |
| SIM800L GSM | RX | Digital Pin 3 | Arduino transmits AT commands to GSM (SoftwareSerial TX) |
| SIM800L GSM | VCC | 5V (via buck converter) | Power supply (needs 3.4V-4.4V) |
| SIM800L GSM | GND | GND | Common ground |
| NEO-6M GPS | TX | Digital Pin 8 | GPS transmits NMEA data to Arduino (AltSoftSerial RX, fixed pin) |
| NEO-6M GPS | RX | Digital Pin 9 | Arduino sends commands to GPS (AltSoftSerial TX, fixed pin) |
| NEO-6M GPS | VCC | 5V | Power supply |
| NEO-6M GPS | GND | GND | Common ground |
| ADXL335 | X | Analog Pin A1 | X-axis acceleration output |
| ADXL335 | Y | Analog Pin A2 | Y-axis acceleration output |
| ADXL335 | Z | Analog Pin A3 | Z-axis acceleration output |
| ADXL335 | VCC | 3.3V | **Must use 3.3V, not 5V** |
| ADXL335 | GND | GND | Common ground |
| I2C LCD 16x2 | SDA | Analog Pin A4 | I2C data line |
| I2C LCD 16x2 | SCL | Analog Pin A5 | I2C clock line |
| I2C LCD 16x2 | VCC | 5V | Power supply |
| I2C LCD 16x2 | GND | GND | Common ground |
| Buzzer | Signal (+) | Digital Pin 12 | HIGH to activate alert sound |
| Buzzer | GND (-) | GND | Common ground |
| Push Button | Side 1 | Digital Pin 11 | Cancel button (uses internal pullup) |
| Push Button | Side 2 | GND | Press pulls pin LOW |

Full wiring data also available as CSV: [wiring.csv](wiring.csv)

---

### Pin Summary

**Digital Pins:**
- Pin 2 â€” SIM800L TX (SoftwareSerial RX)
- Pin 3 â€” SIM800L RX (SoftwareSerial TX)
- Pin 8 â€” NEO-6M TX (AltSoftSerial RX, fixed)
- Pin 9 â€” NEO-6M RX (AltSoftSerial TX, fixed)
- Pin 11 â€” Push Button (INPUT_PULLUP)
- Pin 12 â€” Buzzer (OUTPUT)

**Analog Pins:**
- A1 â€” ADXL335 X-axis
- A2 â€” ADXL335 Y-axis
- A3 â€” ADXL335 Z-axis
- A4 â€” I2C LCD SDA (fixed)
- A5 â€” I2C LCD SCL (fixed)

**Power:**
- 5V â€” SIM800L, NEO-6M, LCD
- 3.3V â€” ADXL335 only
- GND â€” All components (common ground)

---

## How It Works

1. Accelerometer readings are taken every 2 milliseconds
2. System calculates the magnitude of change using the formula: magnitude = sqrt(Î”xÂ² + Î”yÂ² + Î”zÂ²)
3. If magnitude crosses the sensitivity threshold (default: 20), an impact is detected
4. On impact â€” buzzer turns ON, GPS location is captured, and crash info is shown on LCD
5. A 30-second countdown starts â€” if the driver presses the cancel button, the alert is dismissed
6. If no cancellation happens â€” system makes a phone call and sends an SMS with the Google Maps location link

---

## Project Structure

| Folder/File | Description |
|-------------|-------------|
| `accident-alert-gsm/` | Arduino Nano version |
| `accident-alert-gsm-uno/` | Arduino UNO version |
| `algorithm-test/` | Impact algorithm test sketch |
| `i2c-scanner/` | I2C address scanner utility |
| `lcd-test/` | LCD display test sketch |
| `libraries/` | Required libraries (AltSoftSerial, LiquidCrystal_I2C, TinyGPSPlus) |
| `wiring.csv` | Pin connections in CSV format |
| `arduino-uno-wiring.jpg` | Circuit wiring diagram |

---

## How to Upload (Arduino UNO)

1. Open `accident-alert-gsm-uno/accident-alert-gsm-uno.ino` in Arduino IDE
2. Copy all folders from `libraries/` into your Arduino libraries folder
3. Set your emergency phone number in the code:
   ```cpp
   const String EMERGENCY_PHONE = "+91XXXXXXXXXX";
   ```
4. Go to Tools â†’ Board â†’ Select "Arduino UNO"
5. Go to Tools â†’ Port â†’ Select your COM port
6. Click Upload
7. Open Serial Monitor at 9600 baud to see debug output

---

## Important Notes

| Note | Details |
|------|---------|
| ADXL335 Voltage | Connect to 3.3V only â€” 5V will damage the sensor |
| SIM800L Power | Needs 3.4V-4.4V at 2A peak â€” use a buck converter or LiPo battery |
| I2C Address | Default LCD address is 0x27 â€” run `i2c-scanner.ino` to verify |
| AltSoftSerial Pins | Pins 8 & 9 are fixed for AltSoftSerial on UNO â€” cannot be changed |
| 2G Network | SIM800L only supports 2G â€” ensure your carrier supports it |
| GPS First Fix | First GPS fix can take 1-5 minutes outdoors with clear sky view |
| Sensitivity | Default is 20 â€” increase for less sensitive, decrease for more |
| Alert Delay | 30 second delay before sending alert â€” driver can cancel with button |

---

## SMS Alert Format

When an accident is detected, the emergency contact receives:

```
Accident Alert!!
http://maps.google.com/maps?q=loc:28.612912,77.229510
```

Clicking the link opens Google Maps with the exact crash location.

You can also send `get gps` as an SMS from the emergency number to get the current location.

---

## Nano vs UNO

Both Arduino Nano and UNO use the same ATmega328P chip with identical pin mapping, 32KB flash, 2KB SRAM, and 16MHz clock. The code works on both boards without any changes â€” only the physical board size differs.

---

This project is open source for educational purposes.
