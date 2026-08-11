# 🌀 TCL Klimaanlagen-Integration für ESPHome / Home Assistant

> 🇩🇪 **Deutsche Version** – Vollständig ins Deutsche übersetzt, inklusive ausführlicher Verkabelungs- und Modul-Dokumentation.  
> ⚡ **Framework-Neutral**: Funktioniert sowohl mit **ESP-IDF** als auch mit dem **Arduino-Framework** (unterstützt ESP32 & ESP8266).  
> 🔒 **100 % Lokal & Cloud-frei**: Direkte Steuerung über den internen UART-Anschluss der TCL Klimaanlage.

---

## 📋 Inhaltsverzeichnis

- [Hardware & Was du brauchst](#-hardware--was-du-brauchst)
- [Verkabelung](#-verkabelung)
- [Framework-Wahl (ESP-IDF vs. Arduino)](#-framework-wahl-esp-idf-vs-arduino)
- [Einrichtung in Home Assistant](#-einrichtung-in-home-assistant)
- [Beschreibung aller Pakete (Packages)](#-beschreibung-aller-pakete-packages)
- [Steuerelemente & Funktionen](#-steuerelemente--funktionen)
- [Fehlerbehebung & Tipps](#-fehlerbehebung--tipps)

---

## 🛠️ Hardware & Was du brauchst

- **Mikrocontroller**:
  - **ESP32** (Empfohlen, z. B. ESP32-C3 DevKit, ESP32 WROOM32, NodeMCU ESP32)
  - **ESP8266** (z. B. D1 Mini, ESP-01S, ESP-12F)
- **USB-A Steckerkabel** (zum Anschließen an die Buchse des Innengeräts der Klimaanlage, z. B. USB-A auf offene Kabelenden).
- **Home Assistant** mit installiertem **ESPHome Add-on** (ab Version 2026.4.0+).

---

## 🔌 Verkabelung

Die Steuerung erfolgt über die interne USB-Buchse der TCL-Klimaanlage. Hierzu wird ein USB-A-Kabel mit dem Mikrocontroller verbunden:

| USB-A Pin | Standard-Kabelfarbe | → ESP32 / ESP8266 Pin | Beschreibung |
|:---:|:---:|:---:|:---|
| **VBUS / VCC** | Rot | **VIN / 5V** | Stromversorgung von der Klimaanlage (5V) |
| **GND** | Schwarz | **GND** | Masse |
| **D+** | Grün | **RXD** (z. B. GPIO3 / GPIO16) | Empfangsleitung (UART RX) |
| **D-** | Weiß / Grau | **TXD** (z. B. GPIO1 / GPIO17) | Sendeleitung (UART TX) |

> ⚠️ **Hinweis zu TXD/RXD**: Falls die Datenübertragung nicht klappt, versuche `TXD` und `RXD` kreuzweise zu tauschen.

---

## ⚡ Framework-Wahl (ESP-IDF vs. Arduino)

Diese Integration ist **framework-neutral** geschrieben (Standard-C++). In deiner Gerät-YAML kannst du das gewünschte Framework wählen:

### Variante A: ESP-IDF (Empfohlen für moderne ESP32)
```yaml
esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: esp-idf
```

### Variante B: Arduino-Framework (Klassisch / ESP8266)
```yaml
esp32:
  board: esp32dev
  framework:
    type: arduino
```

---

## 🧠 Einrichtung in Home Assistant

1. Öffne das **ESPHome Add-on** in Home Assistant.
2. Erstelle ein neues Gerät (*"New Device"*) und wähle deinen Mikrocontroller-Typ aus.
3. Füge den Inhalt der [`TCL-Conditioner.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/TCL-Conditioner.yaml) in dein ESPHome-Gerätedokument ein.
4. Passe die WLAN-Zugangsdaten (`wifi_ssid`, `wifi_password`) und GPIO-Pins an.
5. Klicke auf **Install** (Kompilieren & Flashen).

---

## 📦 Beschreibung aller Pakete (Packages)

Die Konfiguration ist modular aufgebaut. In der Datei `TCL-Conditioner.yaml` können im Abschnitt `packages` folgende Module aktiviert oder deaktiviert werden:

| Paket-Datei | Zweck & Funktion |
|:---|:---|
| **`packages/core.yaml`** | **Kernkomponente (Pflicht)**:<br>Enthält die eigentliche Steuerung der Klimaanlage, die Klima-Entität, alle Schalter für Piepser/Display und die Dropdown-Listen für Lamellensteuerung (Swing/Fixing). |
| **`packages/leds.yaml`** | **Status-LEDs (Optional)**:<br>Aktiviert physische LEDs am Mikrocontroller für die Anzeige von Datensenden (TX) und Datenempfang (RX). Die Pins werden über `receive_led` und `transmit_led` definiert. |
| **`packages/bad_connect.yaml`** | **Verbindungsoptimierung (Optional)**:<br>Aktiviert einen 3-fachen Wiederholungsmodus beim Senden von Befehlen. Hilfreich bei langer Verkabelung oder instabiler Signalqualität. |
| **`packages/uart_speed.yaml`** | **Baudraten-Umschalter (Optional)**:<br>Fügt eine Dropdown-Liste in Home Assistant hinzu, um die UART-Geschwindigkeit zur Laufzeit anzupassen (z. B. 9600, 38400, 115200 Baud). |
| **`packages/screen.yaml`** | **OLED-Display-Unterstützung (Optional)**:<br>Steuert ein kleines I2C-Display (SSD1306 128x32) an. Zeigt Uhrzeit, WLAN-Signalstärke, aktuellen Modus ("Aus", "Kühlen", "Heizen" etc.) und Status-Icons an. |

---

## 🎛️ Steuerelemente & Funktionen

Nach der Einbindung stehen in Home Assistant folgende Schalter und Optionen bereit:

### 1. Schalter (Switches)
* 🔔 **Beeper** (*Piepser*): Aktiviert/deaktiviert den Quittungston der Klimaanlage bei Befehlsempfang.
* 🖥️ **Display**: Schaltet die Temperaturanzeige am Gehäuse des Innengeräts ein oder aus.
* 💡 **Display on module**: Schaltet die LED-Statusanzeige auf dem Mikrocontroller-Modul ein/aus.
* ⚡ **Force config**: Erzwingt das wiederholte Senden der Einstellungen an die Klimaanlage.

### 2. Lamellensteuerung (Dropdowns / Selects)
* ↕️ **Vertical swing** (*Vertikale Schwingung*):
  * `Von oben nach unten`
  * `In der oberen Hälfte`
  * `In der unteren Hälfte`
* ↔️ **Horizontal swing** (*Horizontale Schwingung*):
  * `Von links nach rechts`
  * `Im linken Bereich`
  * `Im Zentrum`
  * `Im rechten Bereich`
* 📌 **Vertical fixing** (*Vertikale Arretierung*):
  * `Letzte Position`, `Ganz nach oben`, `In der oberen Hälfte`, `In der Mitte`, `In der unteren Hälfte`, `Ganz nach unten`.
* 📌 **Horizontal fixing** (*Horizontale Arretierung*):
  * `Letzte Position`, `Ganz nach links`, `In der linken Hälfte`, `In der Mitte`, `In der rechten Hälfte`, `Ganz nach rechts`.

---

## 🔍 Fehlerbehebung & Tipps

1. **Keine Reaktion der Klimaanlage**:
   - Prüfe, ob RX und TX richtig angeschlossen sind (ggf. TX und RX tauschen).
   - Stelle sicher, dass `logger: baud_rate: 0` in der Konfiguration gesetzt ist (UART-Logging muss deaktiviert sein, da derselbe Port für die Klimaanlage genutzt wird).
2. **Temperaturwerte unplausibel**:
   - Die Soll-Temperatur ist beim TCL-Protokoll auf 16 °C bis 31 °C begrenzt.