# 📡 ESP8266 Wi-Fi Deauther v3.0

> A powerful Wi-Fi security testing toolkit built for the ESP8266 microcontroller. Perform deauthentication attacks, beacon flooding, probe request spamming, and packet sniffing — all from a pocket-sized device with a built-in web interface, serial CLI, and optional OLED display.

---

## ⚠️ Legal Disclaimer

**This project is intended for authorized security testing and educational purposes only.**

Using this tool against networks you do not own or have explicit permission to test is **illegal** in most countries. The author takes no responsibility for misuse. Always obtain written authorization before testing any network.

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔧 **Deauthentication Attack** | Disconnect selected clients from Wi-Fi access points by sending deauth/disassociation frames |
| 📶 **Beacon Flood** | Create hundreds of fake Wi-Fi networks to confuse nearby devices |
| 🔍 **Probe Request Attack** | Flood the air with probe requests targeting specific SSIDs |
| 📡 **Packet Sniffer** | Passively scan and monitor nearby Wi-Fi traffic, detect deauth frames |
| 🌐 **Web Interface** | Full-featured web UI accessible at `192.168.4.1` — manage attacks, scan networks, configure settings |
| 💻 **Serial CLI** | Complete command-line interface over USB serial (115200 baud) |
| 🖥️ **OLED Display** | Optional SSD1306/SH1106 display support with button navigation |
| 💡 **LED Indicators** | Visual feedback via NeoPixel, DotStar, digital, or MY92xx LEDs |
| 💾 **Auto-Save** | Automatically saves settings, SSIDs, and device names to SPIFFS/EEPROM |
| 🔁 **Boot-Loop Protection** | Automatically formats storage if a boot-loop is detected |

---

## 🛠️ Supported Hardware

### ESP8266 Boards
- **NodeMCU** (ESP-12E/F)
- **Wemos D1 Mini**
- **Generic ESP8266** modules

### DSTIKE Boards (Plug & Play)
- Deauther V1 / V2 / V3 / V3.5
- Deauther OLED V1 – V6
- Deauther Monster V1 – V5
- Deauther Boy
- Deauther Watch / Watch V2
- Deauther Mini / Mini EVO
- USB Deauther / USB Deauther V2
- D-Duino B V5 (LED Ring)
- NodeMCU-07 / V2

### Other Supported Hardware
- **HackHeld Vega** (by SpacehuhnTech)
- **Maltronics** boards
- **Lyasi 7W E27 Lamp** (MY92xx LED)
- **Avatar 5W E14 Lamp** (MY92xx LED)

---

## 📦 Installation

### Prerequisites
- [Arduino IDE](https://www.arduino.cc/en/software) (v1.8+ or v2.x)
- USB cable (Micro-USB or USB-C depending on your board)

### Step 1 — Add ESP8266 Board to Arduino IDE

1. Open Arduino IDE → **File** → **Preferences**
2. In **Additional Board Manager URLs**, add:
   ```
   https://raw.githubusercontent.com/SpacehuhnTech/arduino/main/package_spacehuhn_index.json
   ```
3. Go to **Tools** → **Board** → **Board Manager**
4. Search for `deauther` and install **Deauther ESP8266 Boards**
5. Select your board under **Tools** → **Board** → **Deauther ESP8266 Boards**

### Step 2 — Configure Your Board

1. Open `A_config.h`
2. Uncomment the `#define` line that matches your hardware (e.g., `#define NODEMCU`)
3. Comment out `#define DEFAULT_ESP8266` if using a specific board

### Step 3 — Flash the Firmware

1. Open `esp8266_deauther.ino` in Arduino IDE
2. Select the correct **Board** and **Port** under **Tools**
3. Set **Flash Size** to `4MB (FS:1MB OTA:~1019KB)` or equivalent
4. Click **Upload** (→ button)

### Step 4 — Upload Web Interface (SPIFFS)

1. Install the [Arduino ESP8266 LittleFS Filesystem Uploader](https://github.com/earlephilhower/arduino-esp8266littlefs-plugin)
2. Go to **Tools** → **ESP8266 LittleFS Data Upload**
3. This uploads the `data/` folder content to the ESP8266's filesystem

---

## 🚀 Usage

### Web Interface
1. After flashing, the ESP8266 creates a Wi-Fi network:
   - **SSID:** `NetSpectre` (configurable in `A_config.h`)
   - **Password:** `HRM323232` (configurable in `A_config.h`)
2. Connect to this network from your phone/laptop
3. Open a browser and navigate to: `http://192.168.4.1`
4. Use the tabs to **Scan**, **Select Targets**, **Launch Attacks**, **Manage SSIDs**, and **Change Settings**

### Serial CLI
1. Open Arduino IDE Serial Monitor (or any terminal at `115200` baud)
2. Type `help` to see all available commands
3. Example commands:
   ```
   scan -t ap                 # Scan for access points
   scan -t st                 # Scan for stations (clients)
   select -a                  # Select all APs
   attack -t deauth           # Start deauth attack
   attack -t beacon           # Start beacon flood
   stop                       # Stop all attacks
   save                       # Save settings
   ```

### OLED Display (if connected)
- **UP / DOWN buttons** — Navigate menus
- **Button A** — Select / Confirm
- **Button B** — Back (if available)
- Hold **Reset Button** for 5 seconds to factory reset

---

## 🏗️ Project Architecture

```
esp8266_deauther/
├── esp8266_deauther.ino   # Main entry point (setup + loop)
├── A_config.h             # Board & feature configuration (edit this!)
│
├── Attack.cpp / .h        # Attack engine (deauth, beacon, probe)
├── Scan.cpp / .h          # WiFi scanner (APs + stations)
├── Accesspoints.cpp / .h  # Access point data management
├── Stations.cpp / .h      # Client station tracking
├── SSIDs.cpp / .h         # SSID list management (for beacon/probe)
├── Names.cpp / .h         # Device naming & aliasing
├── CLI.cpp / .h           # Serial command-line interface
├── DisplayUI.cpp / .h     # OLED display UI & button handling
│
├── wifi.cpp / .h          # WiFi AP management & web server
├── settings.cpp / .h      # Settings persistence (EEPROM)
├── led.cpp / .h           # LED control (NeoPixel/DotStar/Digital/MY92)
├── functions.h             # Utility functions (MAC, file I/O, etc.)
├── language.h              # UI strings & translations
├── oui.h                   # OUI (vendor) lookup database
├── webfiles.h              # Compressed web interface (served from flash)
├── EEPROMHelper.h          # EEPROM read/write helpers
├── SimpleList.h            # Lightweight linked-list template
├── debug.h                 # Debug macro definitions
│
├── data/web/               # Web interface source files
│   ├── index.html          # Dashboard
│   ├── scan.html           # Scan page
│   ├── attack.html         # Attack controls
│   ├── ssids.html          # SSID manager
│   ├── settings.html       # Settings page
│   ├── info.html           # Device information
│   ├── style.css           # Stylesheet
│   ├── js/                 # JavaScript files
│   └── lang/               # Language files (21 languages)
│
└── src/                    # Bundled libraries
    ├── ArduinoJson-v5.13.5/
    ├── Adafruit_NeoPixel-1.7.0/
    ├── Adafruit_DotStar-1.1.4/
    ├── esp8266-oled-ssd1306-4.1.0/
    ├── SimpleButton/
    ├── DS3231-1.0.3/
    └── my92xx-3.0.3/
```

---

## ⚙️ Configuration Reference (`A_config.h`)

### Board Selection
Uncomment **one** board definition at the top of the file:
```cpp
#define DEFAULT_ESP8266   // Generic ESP8266
// #define NODEMCU          // NodeMCU
// #define WEMOS_D1_MINI    // Wemos D1 Mini
// #define HACKHELD_VEGA    // HackHeld Vega
// #define DSTIKE_DEAUTHER_OLED_V5  // DSTIKE OLED V5
```

### Key Settings

| Setting | Default | Description |
|---|---|---|
| `AP_SSID` | `"NetSpectre"` | Access point name |
| `AP_PASSWD` | `"HRM323232"` | Access point password |
| `AP_HIDDEN` | `false` | Hide the access point |
| `DEAUTHS_PER_TARGET` | `25` | Deauth packets per target per cycle |
| `ATTACK_TIMEOUT` | `600` | Auto-stop attacks after N seconds |
| `BEACON_INTERVAL_100MS` | `true` | Beacon interval (true=100ms, false=1s) |
| `WEB_ENABLED` | `true` | Enable/disable web interface |
| `CLI_ENABLED` | `true` | Enable/disable serial CLI |
| `CH_TIME` | `200` | Channel hop time during scanning (ms) |

---

## 📚 Bundled Libraries

| Library | Version | Purpose |
|---|---|---|
| ArduinoJson | 5.13.5 | JSON parsing for settings & web API |
| Adafruit NeoPixel | 1.7.0 | WS2812B / NeoPixel LED control |
| Adafruit DotStar | 1.1.4 | APA102 / DotStar LED control |
| esp8266-oled-ssd1306 | 4.1.0 | SSD1306 / SH1106 OLED display driver |
| SimpleButton | — | Button input handling with debounce |
| DS3231 | 1.0.3 | RTC (Real-Time Clock) support |
| my92xx | 3.0.3 | MY9291/MY9231 LED driver (smart bulbs) |

---

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

- Original concept & core codebase by [SpacehuhnTech](https://github.com/SpacehuhnTech/esp8266_deauther)
- Hardware designs by [DSTIKE](https://dstike.com/)
- Maintained by **mehraniqbalgp**

---

> **Remember:** Only use this tool on networks you own or have explicit written permission to test. Stay legal, stay ethical. 🛡️
