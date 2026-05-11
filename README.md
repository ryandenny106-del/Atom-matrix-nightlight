# M5Stack Atom Matrix — ESPHome Night Light

A smart PIR night light built with the [M5Stack Atom Matrix](https://docs.m5stack.com/en/core/ATOM%20Matrix) and [ESPHome](https://esphome.io/), integrated with Home Assistant.

Detects motion and lights up the 5×5 RGB LED matrix with different behaviours depending on the time of day — a warm amber glow at night, and a cyan sweep animation during the day.

---

## Features

- **Motion-triggered** — PIR sensor activates the LEDs automatically
- **Day / Night modes** — uses real sun elevation (sunrise/sunset) to switch behaviour
  - 🌙 **Night** — warm amber glow at 70% brightness
  - ☀️ **Day** — cyan wipe animation across all 25 LEDs, then fades out
- **Auto-off timeout** — LEDs turn off after 2 minutes of no motion (configurable)
- **Manual button override** — press the Atom Matrix button to toggle on/off
- **Home Assistant integration** — full API + OTA support
- **Captive portal fallback** — if Wi-Fi fails, a hotspot allows reconfiguration

---

## Hardware

| Component | Details |
|---|---|
| Microcontroller | M5Stack Atom Matrix (ESP32-PICO-D4) |
| LED Matrix | 5×5 WS2812C NeoPixels on GPIO27 |
| PIR Sensor | M5Stack PIR Unit via ToUnit Base (IO2 → GPIO22) |
| Button | Built-in Atom Matrix button on GPIO39 |

> ⚠️ **Brightness warning:** M5Stack recommend keeping the matrix at ≤20% brightness when running all 25 LEDs from USB power. This config uses 70% for night mode (motion-triggered, brief use) — monitor heat if you increase further.

---

## Getting Started

### Prerequisites

- [ESPHome add-on](https://esphome.io/guides/getting_started_hassio) installed in Home Assistant
- M5Stack Atom Matrix connected via USB for the first flash

### Installation

1. **Add the device in ESPHome**
   - Open Home Assistant → **ESPHome** → click **+ New Device**
   - Give it a name and select **ESP32** as the platform
   - ESPHome will generate a basic config — you'll replace it in the next step

2. **Paste the config**
   - Click **Edit** on your new device
   - Replace the entire contents with the code from [`atom-matrix-nightlight.yaml`](atom-matrix-nightlight.yaml)

3. **Set up your secrets**
   - In the ESPHome dashboard click the **⋮ menu → Secrets**
   - Add each key from [`secrets.yaml.example`](secrets.yaml.example) with your real values:
     - `wifi_ssid`, `wifi_password`, `fallback_password`
     - `api_key` — ESPHome can generate this for you, or use **Settings → Generate Key**
     - `latitude`, `longitude` — find yours at [latlong.net](https://www.latlong.net/)

4. **Flash the device**
   - Plug the Atom Matrix in via USB
   - Click **Install → Plug into this computer** for the first flash
   - Subsequent updates can be done wirelessly via **Install → Wirelessly** (OTA)

---

## Configuration

Key values are set via `substitutions` at the top of the YAML and are easy to change:

| Variable | Default | Description |
|---|---|---|
| `pir_pin` | `GPIO22` | PIR sensor digital output pin |
| `timeout_minutes` | `2` | Minutes before LEDs turn off after last motion |
| `log_level` | `INFO` | ESPHome log verbosity |

---

## Wiring

```
M5Stack Atom Matrix
│
├── GPIO27  →  WS2812C LED Matrix (built-in)
├── GPIO39  →  Built-in button (built-in)
└── GPIO22  →  PIR Unit digital output (white wire)
               via ToUnit Base IO2
```

---

## Secrets

All sensitive values live in `secrets.yaml` (never committed to git). See [`secrets.yaml.example`](secrets.yaml.example) for the required keys:

- `wifi_ssid` / `wifi_password` — your network credentials
- `api_key` — ESPHome API encryption key (base64, 32 bytes)
- `fallback_password` — captive portal AP password
- `latitude` / `longitude` — your location for sunrise/sunset

---

## License

MIT — do whatever you like with it.
