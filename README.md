
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

![Block Diagram](BlockDiagram.png)

---

## 🧪 Schematic

![Schematic](Schematic.png)

---

## 🛠️ PCB Layout Previews

![Top Copper](PCB_View_TopCopper.png)
![Bottom Copper](PCB_View_BottomCopper.png)
![Drill](PCB_View_Drill.png)
![Edge Cuts](PCB_View_EdgeCuts.png)

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

## 👨‍💻 Author

**Soumabha Majumdar**  
📧 [soumabha.majumdar@gmail.com](mailto:soumabha.majumdar@gmail.com)  
📞 7003472159

---

## 📜 License

This project is open-source and free to use for academic and non-commercial purposes.
