[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/madeliauskasa/anma-water-meter/blob/main/README.md)


# 🌊 ANMA vandens skaitiklis
### Dviejų kanalų skaitiklių sekimas ir nuotėkio detektoriaus valdiklis

**ANMA vandens skaitiklis** – tai ESP32 pagrindu sukurtas komunalinių paslaugų monitorius, skirtas integruoti paprastus (neišmanius) vandens skaitiklius ir nuotėkio daviklius tiesiai į **Home Assistant** sistemą per **ESPHome** platformą.

---

## 🛠 Aparatinės įrangos apžvalga

*   **M1 jungtis:** 1-ojo skaitiklio jungtis (SN04-N NPN artumo jutiklis)
*   **M2 jungtis:** 2-ojo skaitiklio jungtis (SN04-N NPN artumo jutiklis)
*   **L jungtis:** Nuotėkio jutiklio jungtis (XH2.54 pasyvus zondas)
*   **R mygtukas (Reset):** Įrenginio perkrovimas
*   **B mygtukas (Boot):** Programinės įrangos įrašymo režimas (Firmware flash)
*   **USB Type-C:** 5V DC maitinimas ir duomenų perdavimas

---

## 📶 Pirminė konfigūracija

1.  **Maitinimas:** Prijunkite įrenginį prie 5V USB-C maitinimo šaltinio.
2.  **Prisijungimas:** Savo telefone ar kompiuteryje prisijunkite prie „Wi-Fi“ tinklo pavadinimu `Water-Meter-Setup`.
3.  **Nustatymų langas:** Prisijungimo puslapis turėtų atsidaryti automatiškai. Jei ne, naršyklėje įveskite adresą `192.168.4.1`.
4.  **„Wi-Fi“ konfigūravimas:** Pasirinkite savo namų tinklą ir įveskite slaptažodį. Įrenginis persikraus ir prisijungs prie jūsų vietinio tinklo.

---

## 🏠 Home Assistant integracija

### 1. Pridėjimas (Adoption)
Prisijungus prie „Wi-Fi“, „Home Assistant“ automatiškai aptiks naują įrenginį.
*   Eikite į **Settings** > **Devices & Services**.
*   Spustelėkite **Configure** ties nauju ESPHome mazgu.
*   **Šifravimo raktas:** Nuskaitykite ant korpuso esantį **QR kodą** ir įveskite raktą, kad užbaigtumėte pridėjimą.

### 2. Skaitiklio („Utility Meter“) konfigūravimas
Norėdami sekti vandens suvartojimą (dienos/mėnesio sumas), sukurkite **Utility Meter** pagalbininką:
1.  Eikite į **Settings** > **Helpers** > **Create Helper**.
2.  Pasirinkite **Utility Meter**.
3.  **Input Sensor:** Pasirinkite `M1 Water Pulses`.
4.  **Meter Reset Cycle:** Nustatykite `No cycle`.
5.  **Meter Reset Offset:** Nustatykite `0`.
6.  **Net Consumption:** `Išjungta` (Disabled).
7.  **Delta Values:** `Išjungta` (Disabled).
8.  **Periodically Resetting:** `Išjungta` (Disabled).
9.  **Sensor Always Available:** `Įjungta` (Enabled).

### 3. Pradinės vertės nustatymas (Kalibravimas)
Kadangi **ANMA vandens skaitiklis** pradeda skaičiuoti nuo nulio, turite jį sinchronizuoti su dabartiniais fizinio skaitiklio rodmenimis.

1.  Eikite į **Developer Tools** > **Services** (arba **Actions** naujesnėse HA versijose).
2.  Ieškokite paslaugos: `utility_meter.calibrate`.
3.  **Target:** Pasirinkite savo sukurtą pagalbininką (pvz., `sensor.daily_water_usage`).
4.  **Value:** Įveskite tikslų skaičių, kurį šiuo metu rodo jūsų fizinio skaitiklio ciferblatas.
5.  Spustelėkite **Perform Action**. Jūsų skaitmeninė lenta dabar sutampa su fiziniais rodmenimis.

### 4. Pridėjimas prie Energijos skydelio (Energy Dashboard)
1.  Eikite į **Settings** > **Dashboards** > **Energy**.
2.  Skiltyje **Water Consumption** spustelėkite **Add Water Source**.
3.  Pasirinkite 3-ajame žingsnyje sukurtą **Utility Meter** pagalbininką.

---

## ⚠️ Atsakomybės apribojimas
*Tai yra mėgėjiškas „pasidaryk pats“ (DIY) įrenginis, skirtas informaciniams tikslams. Tai nėra sertifikuotas gyvybės saugos ar oficialus apsaugos nuo užliejimo prietaisas. Kūrėjas neatsako už jokią žalą vandeniu ar duomenų praradimą.*