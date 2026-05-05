[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/madeliauskasa/anma-water-meter/blob/main/README.md)
[![en](https://img.shields.io/badge/lang-lt-green.svg)](https://github.com/madeliauskasa/anma-water-meter/blob/main/README.lt.md)


# ANMA water meter
### Dual Water Meter & Leak Detection sensor tracker

ANMA water meter is an ESP32-powered utility monitor designed to integrate non-smart water meters and leak probes directly into **Home Assistant** via **ESPHome**.

---

## 🛠 Hardware Overview

*   **M1 Port:** Meter 1 Connection (SN04-N NPN Proximity Sensor)
*   **M2 Port:** Meter 2 Connection (SN04-N NPN Proximity Sensor)
*   **L Port:** Leak Sensor Connection (XH2.54 passive probe)
*   **R Button:** Reset (Reboot device)
*   **B Button:** Boot (Firmware flashing mode)
*   **USB Type-C:** 5V DC Power Supply & Data

---

## 📶 First-Time Setup

1.  **Power On:** Plug the device into a 5V USB-C power source.
2.  **Connect to Hotspot:** On your phone or PC, connect to the Wi-Fi network named `Water-Meter-Setup`.
3.  **Captive Portal:** A login page should automatically open. If it doesn't, navigate to `192.168.4.1` in your browser.
4.  **Configure Wi-Fi:** Select your home network and enter your credentials. The device will reboot and join your local network.

---

## 🏠 Home Assistant Integration

### 1. Adoption
Once connected to your Wi-Fi, Home Assistant will auto-discover the device.
*   Navigate to **Settings** > **Devices & Services**.
*   Click **Configure** on the new ESPHome node.
*   **Encryption Key:** Scan the **QR Code** on the device case and  enter the key to finalize adoption.

### 2. Configure Utility Meter
To track water usage (Daily/Monthly totals), create a **Utility Meter Helper**:
1.  Go to **Settings** > **Helpers** > **Create Helper**.
2.  Select **Utility Meter**.
3.  **Input Sensor:** Choose `M1 Water Pulses`.
4.  **Meter Reset Cycle:** Set to `No cycle`.
5.  **Meter Reset Offset:** Set to `0`.
6.  **Meter Reset Cycle:** Set to `No cycle`.
7.  **Net Consumption:** Set to `Disabled`.
8.  **Delta Values:** Set to `Disabled`.
9.  **Periodically Resetting:** Set to `Disabled`.
10.  **Sensor Always Available:** Set to `Enabled`.

### 3. Setting the Initial Meter Value (Calibration) 
Since the **ANMA Water Meter** starts counting from zero, you must synchronize it with your physical water meter's current reading.

1.  Go to **Developer Tools** > **Services** (or **Actions** in newer HA versions).
2.  Search for the service: `utility_meter.calibrate`.
3.  **Target:** Select the helper you created (e.g., `sensor.daily_water_usage`).
4.  **Value:** Enter the exact numerical reading currently shown on your physical meter dial.
5.  Click **Perform Action**. Your digital dashboard now matches your physical meter.

### 4. Add to Energy Dashboard
1.  Go to **Settings** > **Dashboards** > **Energy**.
2.  Under **Water Consumption**, click **Add Water Source**.
3.  Select the **Utility Meter helper** created in the 3rd step.

---

## ⚠️ Disclaimer
*This is a DIY hobbyist device for informational purposes. It is not a certified life-safety or flood-prevention device. The developer is not responsible for any water damage or data loss.*