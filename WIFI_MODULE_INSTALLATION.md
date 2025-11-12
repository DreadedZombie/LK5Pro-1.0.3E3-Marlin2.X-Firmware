# MKS WiFi Module Installation Guide
## For MKS Robin E3 V1.1 (Longer LK5 Pro)

---

## Module Purchased
**MKS Robin WiFi v1.0** from AliExpress
- Link: https://www.aliexpress.us/item/2251832630567942.html
- Based on ESP8266 (ESP-12 module)
- Official Makerbase product

---

## What's in the Box
- MKS Robin WiFi module (ESP8266 board)
- Usually comes pre-soldered with header pins
- Some versions include mounting bracket

---

## Finding the WiFi Connector on Your Board

The MKS Robin E3 v1.1 board should have a **4-5 pin header** for WiFi/UART connection. It's typically labeled one of these ways:
- **WIFI**
- **UART**
- **AUX** or **AUX-1**
- **Serial** or **UART2**

### Common Locations to Check:

1. **Near the USB port** - Often WiFi headers are near the USB connector
2. **Along the edge of the board** - Check all edges, especially near stepper drivers
3. **Between the USB and SD card slots**
4. **Near the display connector** - Sometimes grouped with other serial interfaces

### What the Header Looks Like:

```
┌─────────────────┐
│ ○ ○ ○ ○ (○)    │  ← 4-5 pins in a row
│  |  |  |  |     │
│ 5V GND TX RX    │  ← Usually labeled (may have Reset/GPIO pins too)
└─────────────────┘
```

**Important:** The header might be:
- ✅ **Populated** - Pins already soldered to the board
- ❌ **Unpopulated** - Just holes in the PCB (you'll need to solder header pins)

---

## Pin Identification

### Standard MKS WiFi Pinout:

| Pin | Label | Function | Notes |
|-----|-------|----------|-------|
| 1 | 5V | Power | 5V supply from board |
| 2 | GND | Ground | Common ground |
| 3 | TX | Transmit | Board TX → WiFi RX |
| 4 | RX | Receive | Board RX → WiFi TX |
| 5 | RST (optional) | Reset | WiFi module reset |

**Critical:** TX/RX are **CROSSED** - WiFi TX connects to Board RX, and vice versa.

---

## Installation Steps

### Step 1: Locate the Connector

**ACTION NEEDED:** Take a clear photo of your MKS Robin E3 v1.1 board and I can help identify the exact location!

**How to take the photo:**
1. Remove printer cover/enclosure
2. Take photo from directly above the board
3. Make sure all labels are readable
4. Get both the full board and a close-up

### Step 2: Check if Header is Populated

- **If pins are already there:** Skip to Step 3
- **If just holes in PCB:** You'll need to solder 2.54mm header pins
  - Purchase: 2.54mm pitch male header pins
  - Tools needed: Soldering iron, solder
  - Tutorial: [How to solder header pins](https://learn.adafruit.com/adafruit-guide-excellent-soldering/tools)

### Step 3: Connect WiFi Module

1. **Power off printer completely** (unplug from wall)
2. Orient the WiFi module correctly:
   - Match the pin labels (5V to 5V, GND to GND, etc.)
   - TX on module → RX on board
   - RX on module → TX on board
3. Firmly press module onto header pins
4. **Double-check orientation before powering on!**

### Step 4: Secure the Module (Optional)

- Some boards have mounting holes for a bracket
- Alternatively, use a zip tie or double-sided tape to secure
- Make sure WiFi antenna (if external) is not touching metal parts

---

## Firmware Configuration

### 1. Enable MKS WiFi Module

Edit `Configuration.h` (line ~2881):

```cpp
// Uncomment this line:
#define MKS_WIFI_MODULE
```

### 2. Configure Serial Port for WiFi

Add these lines to `Configuration.h` (around line 164):

```cpp
#define SERIAL_PORT_2 3      // WiFi uses UART3
#define BAUDRATE_2 115200    // WiFi communication speed
```

**Note:** The exact serial port number may vary. Common options are `1`, `2`, or `3`. If one doesn't work, try the others.

### 3. Rebuild Firmware

```bash
cd /path/to/LK5Pro-Firmware/LK5-Dblower-BLtouch
pio run -e mks_robin_e3_maple
```

### 4. Flash Updated Firmware

1. Copy `.pio/build/mks_robin_e3_maple/Robin_e3.bin` → rename to `firmware.bin`
2. Copy to SD card root
3. Insert SD, power on printer
4. Wait for firmware update to complete

---

## WiFi Module Firmware

The WiFi module itself needs firmware. There are two options:

### Option A: Auto-Flash from SD Card (Easiest)

1. Download MKS WiFi firmware: [MKS-WIFI GitHub](https://github.com/makerbase-mks/MKS-WIFI/releases)
2. Extract `MksWifi.bin`
3. Copy `MksWifi.bin` to SD card root (along with `firmware.bin`)
4. Power on printer with WiFi module installed
5. Module will auto-flash (LED will blink rapidly)
6. Wait ~2 minutes for completion

### Option B: ESP3D Firmware (Advanced Alternative)

ESP3D is an alternative firmware with more features:
- [ESP3D GitHub](https://github.com/luc-github/ESP3D)
- Requires manual flashing via USB-to-serial adapter
- More web interface features

**Recommendation:** Start with MKS WiFi firmware (Option A)

---

## Testing & Verification

### 1. Power On and Check LED

After installation and firmware updates:
- WiFi module LED should light up
- LED may blink slowly (searching for network)

### 2. Find the WiFi Network

1. WiFi module creates its own access point on first boot
2. Look for WiFi network named: **MKS_WIFI_XXXX** (XXXX = unique ID)
3. Default password: `12345678`

### 3. Connect and Configure

1. Connect your phone/PC to the MKS_WIFI network
2. Open browser and go to: `http://192.168.4.1`
3. You should see the MKS WiFi configuration page

### 4. Configure Your WiFi

In the web interface:
- Go to WiFi settings
- Connect module to your home WiFi network
- Enter your WiFi password
- Save settings
- Module will reboot and join your network

### 5. Find Module on Your Network

- Check your router's DHCP client list
- Look for device named "MKS_WIFI" or "ESP_XXXXXX"
- Note the IP address
- Access via browser: `http://[IP_ADDRESS]`

---

## Web Interface Features

Once connected, you can:

✅ **Upload G-code files** - Drag and drop to upload prints
✅ **Monitor prints** - Real-time temperature, progress, status
✅ **Control printer** - Start, pause, stop prints remotely
✅ **Adjust settings** - Temperature, fan speed, etc.
✅ **View file list** - Browse SD card files
✅ **Send G-code commands** - Manual control via terminal

---

## Troubleshooting

### WiFi Module LED Not Lighting

- Check power connections (5V and GND)
- Verify module is firmly seated on header
- Test with multimeter: Should see 5V between 5V and GND pins

### Can't Find MKS_WIFI Network

- Wait 2-3 minutes after power on
- Module may need to flash firmware first (if `MksWifi.bin` on SD)
- Try power cycling printer
- Check if LED is blinking (indicates boot/flash mode)

### Module Connects but No Web Interface

- Verify correct IP address
- Try `http://192.168.4.1` (default AP mode)
- Check firewall settings on your PC
- Try different browser (Chrome recommended)

### Connection Drops Frequently

- WiFi module too far from router - add external antenna
- Check for interference (metal printer enclosure)
- Update WiFi module firmware
- Reduce WiFi traffic during prints

### Marlin Shows "Communication Error"

- Wrong serial port in firmware - try SERIAL_PORT_2 = 1 or 2 instead
- Baud rate mismatch - verify both Marlin and WiFi use 115200
- TX/RX swapped - double-check connections
- WiFi module not fully seated

### Can't Upload Large Files

- MKS WiFi has limited buffer size
- Files over 10MB may fail
- Compress G-code or reduce file size
- Alternative: Use SD card for large prints

---

## Serial Port Reference

MKS Robin E3 v1.1 has multiple UART ports. If WiFi doesn't work with one, try others:

| SERIAL_PORT | Hardware UART | Typical Use |
|-------------|---------------|-------------|
| 1 | USART1 (PA9/PA10) | USB serial |
| 2 | USART2 | WiFi or other |
| 3 | USART3 | WiFi or display |

In `Configuration.h`, try each:
```cpp
#define SERIAL_PORT_2 1
// or
#define SERIAL_PORT_2 2
// or
#define SERIAL_PORT_2 3
```

---

## Alternative: OctoPrint Instead

If WiFi module installation seems complex, consider **OctoPrint**:

### Advantages:
- No hardware modification needed
- More features and plugins
- Larger community support
- Better monitoring (webcam support)
- Works with existing USB connection

### Setup:
1. Get Raspberry Pi Zero 2 W (~$15)
2. Flash OctoPi image to SD card
3. Connect Pi to printer via USB
4. Access via web browser on local network

**OctoPrint Guide:** https://octoprint.org/download/

---

## Resources

- [MKS-WIFI Official GitHub](https://github.com/makerbase-mks/MKS-WIFI)
- [MKS Robin E3 Documentation](https://github.com/makerbase-mks/MKS-Robin-E3-E3D)
- [Mischianti WiFi Installation Tutorial](https://mischianti.org/2021/11/17/mks-wifi-for-makerbase-robin-boards-and-how-to-wiring-esp12-nodemcu-1/)
- [ESP3D Alternative Firmware](https://github.com/luc-github/ESP3D)
- [MKS Discord Community](https://discord.gg/4uar57NEyU)

---

## Next Steps

1. **Take a photo of your board** - I'll help identify the WiFi connector
2. Order the WiFi module (link above)
3. While waiting, we can prepare the firmware changes
4. When module arrives, follow this guide for installation

**Questions?** Share a photo of your MKS Robin E3 v1.1 board and I'll mark exactly where the WiFi connector is!

---

**Last Updated:** 2025-01-12
**Module:** MKS Robin WiFi v1.0
**Board:** MKS Robin E3 V1.1
**Printer:** Longer LK5 Pro
