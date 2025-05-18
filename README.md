
# Smart Heater Control Module 🔥

This project is a smart safety and control module for electric heaters. Designed using KiCad, it includes temperature monitoring, relay-based control, safety cutoffs, and visual feedback, all powered by a microcontroller (e.g., ESP32 or Arduino).

---

## 🧩 Features

- **Real-Time Temperature Monitoring**
- **Relay Control for Heater Switch**
- **Safety Cut-off with Thermal Fuse**
- **OLED Display for Status Info**
- **Manual Override Switch**
- **PCB Designed using KiCad**

---

## 📐 Block Diagram
![Screenshot From 2025-05-19 02-45-44](https://github.com/user-attachments/assets/d3b5a882-02a3-451c-8951-0a6824dc8734)



---

## 🧪 Schematic

![Screenshot From 2025-05-19 04-06-10](https://github.com/user-attachments/assets/b756d44a-97ae-42f4-aee9-229b998851e7)


---

## 🛠️ PCB Layout Previews
![Screenshot From 2025-05-19 04-08-29](https://github.com/user-attachments/assets/c04f8c2c-1d04-4698-a80e-f60cbcee7fe1)
![Screenshot From 2025-05-19 04-07-51](https://github.com/user-attachments/assets/0d296524-fcb0-4bc2-b380-bd6ff490481c)
![Screenshot From 2025-05-19 04-07-22](https://github.com/user-attachments/assets/da25d9b0-3ef0-4b68-84b6-f995406cad7d)
![Screenshot From 2025-05-19 04-07-03](https://github.com/user-attachments/assets/23b37ba7-8d0b-4983-8964-1885ee26aa7d)



---

## 📋 Bill of Materials (BOM)

| Component             | Qty | Cost     |
|-----------------------|-----|----------|
| ESP32 Dev Board       | 1   | Rs. 300  |
| DS18B20 Temp Sensor   | 1   | Rs. 100  |
| 5V Relay Module       | 1   | Rs. 60   |
| OLED Display (0.96")  | 1   | Rs. 150  |
| 7805 Regulator        | 1   | Rs. 15   |
| Discrete Components   | -   | Rs. 50   |
| PCB Fabrication       | 1   | Rs. 200  |

---

## 🔌 System Explanation

### Power Flow
AC Mains → Step-down SMPS → 5V/12V Regulated Supply → Microcontroller → Relay Control

### Sensor and Relay Logic
- If temperature < threshold → Relay ON → Heater ON  
- If temperature ≥ threshold → Relay OFF → Heater OFF  

### Safety & Fallback
- Thermal fuse as a hardware cutoff  
- Watchdog timer in software  
- Manual override and warning LED/OLED  

---

## 🧪 Test Plan

1. Simulate gradual temperature increase
2. Observe cutoff at 60°C
3. Check relay OFF and OLED warning
4. Allow manual reset below 50°C

---

## 📁 Project Structure

```
├── SmartHeaterControl.kicad_sch
├── SmartHeaterControl.kicad_pcb
├── gerber/
├── screenshots/
├── BlockDiagram.png
├── Schematic.png
└── PCB_View_*.png
```

---
## 💻 MCU Programming Guide

### Hardware Pin Mapping (ESP32)

| Component             | MCU Pin          |
|----------------------|------------------|
| DS18B20 Temp Sensor  | GPIO 4           |
| Relay Module (IN)    | GPIO 13          |
| OLED Display (I2C)   | SDA = GPIO 21, SCL = GPIO 22 |
| Buzzer / LED         | GPIO 14          |
| Manual Reset Button  | GPIO 12          |

> Note: DS18B20 needs a 4.7kΩ pull-up resistor on the data line.

---

### Required Libraries (Arduino IDE)

- `OneWire.h`
- `DallasTemperature.h`
- `Adafruit_GFX.h`
- `Adafruit_SSD1306.h`

---

### ESP32 Board Setup in Arduino IDE

1. Install board support:
   - Board Manager URL:  
     `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
2. Select board: **ESP32 Dev Module**
3. Connect device via USB and select correct COM port
4. Install required libraries via Library Manager

---

### 🔌 Sample Arduino Code

```cpp
#include <OneWire.h>
#include <DallasTemperature.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define ONE_WIRE_BUS 4
#define RELAY_PIN 13
#define BUZZER_PIN 14
#define BUTTON_PIN 12

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

OneWire oneWire(ONE_WIRE_BUS);
DallasTemperature sensors(&oneWire);

float cutoffTemp = 60.0;
float resumeTemp = 50.0;
bool manualReset = false;

void setup() {
  Serial.begin(115200);
  sensors.begin();

  pinMode(RELAY_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(BUTTON_PIN, INPUT_PULLUP);

  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
}

void loop() {
  sensors.requestTemperatures();
  float tempC = sensors.getTempCByIndex(0);

  display.clearDisplay();
  display.setCursor(0,0);
  display.print("Temp: ");
  display.print(tempC);
  display.print(" C");

  if (!manualReset && tempC < cutoffTemp) {
    digitalWrite(RELAY_PIN, HIGH);
    digitalWrite(BUZZER_PIN, LOW);
    display.setCursor(0,20);
    display.print("Status: Heater ON");
  } else if (!manualReset && tempC >= cutoffTemp) {
    digitalWrite(RELAY_PIN, LOW);
    digitalWrite(BUZZER_PIN, HIGH);
    display.setCursor(0,20);
    display.print("OVERHEAT! Relay OFF");
    display.setCursor(0,30);
    display.print("Press button to reset");
    manualReset = true;
  }

  if (manualReset && digitalRead(BUTTON_PIN) == LOW && tempC <= resumeTemp) {
    manualReset = false;
    digitalWrite(BUZZER_PIN, LOW);
  }

  display.display();
  delay(1000);
}
## 👨‍💻 Author

**Soumabha Majumdar**  
📧 [soumabha.majumdar@gmail.com](mailto:soumabha.majumdar@gmail.com)  
📞 7003472159

---

## 📜 License

This project is open-source and free to use for academic and non-commercial purposes.
