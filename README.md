# ðŸš— Vehicle Accident Detection & Alert System

> **Arduino UNO** based real-time crash detection system using accelerometer, GPS & GSM module.

![Circuit Wiring Diagram](arduino-uno-wiring.jpg)

---

## ðŸ“‹ Project Overview

This system detects vehicle accidents in real-time using an **ADXL335 accelerometer** to measure sudden impact/g-force. When a crash is detected:

1. ðŸ”” **Buzzer sounds** an alert immediately
2. ðŸ“ **GPS location** is captured via NEO-6M module
3. ðŸ“± **Phone call** is made to emergency contact via SIM800L GSM
4. ðŸ“© **SMS is sent** with Google Maps link of the crash location
5. â¹ï¸ **Cancel button** allows the driver to cancel false alerts within 30 seconds

---

## ðŸ”§ Components Required

| # | Component | Quantity | Description |
|---|-----------|----------|-------------|
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

## âš¡ Circuit Wiring Diagram

### Complete Pin Connection Table

| Component | Component Pin | Wire Color | Arduino UNO Pin | Description |
|-----------|--------------|------------|-----------------|-------------|
| **SIM800L GSM** | TX | ðŸŸ¢ Green | Digital Pin 2 (rxPin) | GSM transmits data â†’ Arduino receives (SoftwareSerial RX) |
| **SIM800L GSM** | RX | ðŸŸ¢ Green | Digital Pin 3 (txPin) | Arduino transmits AT commands â†’ GSM receives (SoftwareSerial TX) |
| **SIM800L GSM** | VCC | ðŸ”´ Red | 5V (via buck converter) | Power supply (needs 3.4V-4.4V) |
| **SIM800L GSM** | GND | âš« Black | GND | Common ground |
| **NEO-6M GPS** | TX | ðŸŸ¢ Green | Digital Pin 8 | GPS transmits NMEA data â†’ Arduino (AltSoftSerial fixed RX) |
| **NEO-6M GPS** | RX | ðŸŸ¢ Green | Digital Pin 9 | Arduino â†’ GPS commands (AltSoftSerial fixed TX) |
| **NEO-6M GPS** | VCC | ðŸ”´ Red | 5V | Power supply |
| **NEO-6M GPS** | GND | âš« Black | GND | Common ground |
| **ADXL335** | X | ðŸŸ¢ Green | Analog Pin A1 | X-axis acceleration output |
| **ADXL335** | Y | ðŸŸ¢ Green | Analog Pin A2 | Y-axis acceleration output |
| **ADXL335** | Z | ðŸŸ¢ Green | Analog Pin A3 | Z-axis acceleration output |
| **ADXL335** | VCC | ðŸ”´ Red | **3.3V** âš ï¸ | **MUST use 3.3V (NOT 5V!)** |
| **ADXL335** | GND | âš« Black | GND | Common ground |
| **I2C LCD 16x2** | SDA | ðŸŸ¢ Green | Analog Pin A4 | I2C data line |
| **I2C LCD 16x2** | SCL | ðŸŸ¢ Green | Analog Pin A5 | I2C clock line |
| **I2C LCD 16x2** | VCC | ðŸ”´ Red | 5V | Power supply |
| **I2C LCD 16x2** | GND | âš« Black | GND | Common ground |
| **Buzzer** | Signal (+) | ðŸŸ¢ Green | Digital Pin 12 | HIGH = buzzer ON |
| **Buzzer** | GND (-) | âš« Black | GND | Common ground |
| **Push Button** | Side 1 | ðŸŸ¢ Green | Digital Pin 11 | Cancel button (uses internal pullup) |
| **Push Button** | Side 2 | âš« Black | GND | Press pulls pin LOW |

> ðŸ“„ **Full wiring data also available as CSV:** [wiring.csv](wiring.csv)

---

## ðŸ“Œ Pin Summary (Quick Reference)

```
ARDUINO UNO PIN MAP
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
 DIGITAL PINS:
   Pin 2  â† SIM800L TX  (SoftwareSerial RX)
   Pin 3  â†’ SIM800L RX  (SoftwareSerial TX)
   Pin 8  â† NEO-6M TX   (AltSoftSerial RX) [FIXED]
   Pin 9  â†’ NEO-6M RX   (AltSoftSerial TX) [FIXED]
   Pin 11 â† Push Button  (INPUT_PULLUP)
   Pin 12 â†’ Buzzer        (OUTPUT)

 ANALOG PINS:
   A1 â† ADXL335 X-axis
   A2 â† ADXL335 Y-axis
   A3 â† ADXL335 Z-axis
   A4 â†” I2C LCD SDA      [FIXED I2C]
   A5 â†” I2C LCD SCL      [FIXED I2C]

 POWER:
   5V   â†’ SIM800L, NEO-6M, LCD
   3.3V â†’ ADXL335 (âš ï¸ MUST be 3.3V!)
   GND  â†’ All components (common ground)
â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•â•
```

---

## ðŸ“ Project Structure

```
acc/
â”œâ”€â”€ README.md                          â† This file
â”œâ”€â”€ arduino-uno-wiring.jpg             â† Circuit wiring diagram
â”œâ”€â”€ wiring.csv                         â† Wiring connections in CSV format
â”œâ”€â”€ accident-alert-gsm/                â† Arduino NANO version
â”‚   â””â”€â”€ accident-alert-gsm.ino
â”œâ”€â”€ accident-alert-gsm-uno/            â† Arduino UNO version â­
â”‚   â””â”€â”€ accident-alert-gsm-uno.ino
â”œâ”€â”€ algorithm-test/                    â† Impact algorithm test sketch
â”‚   â””â”€â”€ algorithm-test.ino
â”œâ”€â”€ i2c-scanner/                       â† I2C address scanner utility
â”‚   â””â”€â”€ i2c-scanner.ino
â”œâ”€â”€ lcd-test/                          â† LCD display test sketch
â”‚   â””â”€â”€ lcd-test.ino
â”œâ”€â”€ libraries/                         â† Required libraries
â”‚   â”œâ”€â”€ AltSoftSerial-master/          â† GPS serial communication
â”‚   â”œâ”€â”€ Arduino-LiquidCrystal-I2C-library-master/  â† I2C LCD
â”‚   â””â”€â”€ TinyGPSPlus/                   â† GPS NMEA parsing
â””â”€â”€ Improved Crash Detection Algorithm for Vehicle Crash Detection.pdf
```

---

## ðŸš€ How to Upload (Arduino UNO)

1. **Open** `accident-alert-gsm-uno/accident-alert-gsm-uno.ino` in Arduino IDE
2. **Install Libraries** â€” Copy all folders from `libraries/` into your Arduino libraries folder:
   - `AltSoftSerial`
   - `LiquidCrystal_I2C`
   - `TinyGPSPlus`
3. **Set Emergency Phone Number** â€” Edit line in code:
   ```cpp
   const String EMERGENCY_PHONE = "+91XXXXXXXXXX";  // Your number with country code
   ```
4. **Select Board** â†’ `Tools` â†’ `Board` â†’ `Arduino UNO`
5. **Select Port** â†’ `Tools` â†’ `Port` â†’ Your COM port
6. **Upload** â†’ Click â–¶ï¸ Upload button
7. **Open Serial Monitor** at `9600 baud` to see debug output

---

## âš™ï¸ How It Works

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                    SYSTEM FLOWCHART                      â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚                                                         â”‚
â”‚  START â†’ Read Accelerometer (every 2ms)                 â”‚
â”‚            â”‚                                            â”‚
â”‚            â”œâ”€ Calculate magnitude of change              â”‚
â”‚            â”‚   magnitude = âˆš(Î”xÂ² + Î”yÂ² + Î”zÂ²)          â”‚
â”‚            â”‚                                            â”‚
â”‚            â”œâ”€ magnitude â‰¥ 20 (sensitivity)?             â”‚
â”‚            â”‚     â”‚                                      â”‚
â”‚            â”‚     â”œâ”€ YES â†’ Impact Detected!              â”‚
â”‚            â”‚     â”‚         â”œâ”€ Buzzer ON ðŸ””              â”‚
â”‚            â”‚     â”‚         â”œâ”€ Get GPS Location ðŸ“       â”‚
â”‚            â”‚     â”‚         â”œâ”€ Show on LCD ðŸ“º            â”‚
â”‚            â”‚     â”‚         â”œâ”€ Wait 30 seconds â±ï¸        â”‚
â”‚            â”‚     â”‚         â”‚    â”‚                        â”‚
â”‚            â”‚     â”‚         â”‚    â”œâ”€ Button pressed?       â”‚
â”‚            â”‚     â”‚         â”‚    â”‚   â””â”€ YES â†’ Cancel âŒ   â”‚
â”‚            â”‚     â”‚         â”‚    â”‚                        â”‚
â”‚            â”‚     â”‚         â”‚    â””â”€ Timer expired?        â”‚
â”‚            â”‚     â”‚         â”‚        â””â”€ YES â†’             â”‚
â”‚            â”‚     â”‚         â”‚          â”œâ”€ Make Call ðŸ“ž    â”‚
â”‚            â”‚     â”‚         â”‚          â””â”€ Send SMS ðŸ“©    â”‚
â”‚            â”‚     â”‚         â”‚            (with Maps link) â”‚
â”‚            â”‚     â”‚                                      â”‚
â”‚            â”‚     â””â”€ NO  â†’ Continue monitoring            â”‚
â”‚            â”‚                                            â”‚
â”‚            â””â”€ Loop back â†©ï¸                               â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

---

## âš ï¸ Important Notes

| Note | Details |
|------|---------|
| **ADXL335 Voltage** | Connect to **3.3V ONLY** â€” connecting to 5V will damage the sensor! |
| **SIM800L Power** | Needs 3.4V-4.4V at 2A peak â€” use a buck converter or LiPo battery |
| **I2C Address** | Default LCD address is `0x27` â€” run `i2c-scanner.ino` to verify |
| **AltSoftSerial Pins** | Pins 8 & 9 are **FIXED** for AltSoftSerial on UNO â€” cannot be changed |
| **2G Network** | SIM800L only supports 2G â€” ensure your carrier supports 2G |
| **GPS First Fix** | First GPS fix can take 1-5 minutes outdoors with clear sky view |
| **Sensitivity** | Default sensitivity is `20` â€” increase for less sensitive, decrease for more |
| **Alert Delay** | 30 second delay before sending alert â€” driver can cancel with button |

---

## ðŸ“ž SMS Alert Format

When accident is detected, the emergency contact receives:
```
Accident Alert!!
http://maps.google.com/maps?q=loc:28.612912,77.229510
```
*(Clicking the link opens Google Maps with exact crash location)*

---

## ðŸ“ž "Get GPS" SMS Command

Send SMS with text `get gps` from the emergency phone number to get current location:
```
GPS Location Data
http://maps.google.com/maps?q=loc:28.612912,77.229510
```

---

## ðŸ”Œ Arduino Nano vs Arduino UNO

| Feature | Arduino Nano | Arduino UNO |
|---------|-------------|-------------|
| Microcontroller | ATmega328P | ATmega328P |
| Digital I/O Pins | 14 | 14 |
| Analog Input Pins | 8 | 6 |
| Flash Memory | 32 KB | 32 KB |
| SRAM | 2 KB | 2 KB |
| Clock Speed | 16 MHz | 16 MHz |
| **Pin Mapping** | **Same** | **Same** |
| **Code Compatibility** | âœ… | âœ… |

> Both boards use the **same ATmega328P** chip â€” the code is functionally identical. Only the board form factor differs.

---

## ðŸ“œ License

This project is open source for educational purposes.

---

*Made with â¤ï¸ for road safety*
