# 🌀 TCL Klimaanlagen-Integration für ESPHome / Home Assistant

> 🇩🇪 **Deutsche Version** – Vollständig übersetzt und erweitert basierend auf dem originalen Repository.  
> ![Zuletzt aktualisiert](https://img.shields.io/github/last-commit/Pommel4711/tclac/german?label=Zuletzt%20aktualisiert&color=blue&style=flat-square)  
> 🔗 **Original-Repository**: [I-am-nightingale/tclac](https://github.com/I-am-nightingale/tclac)  
> 🇩🇪 **Deutsches Haupt-Repository**: [Pommel4711/tclac](https://github.com/Pommel4711/tclac)  
> ⚡ **Framework-Neutral**: Unterstützt **ESP-IDF** und das **Arduino-Framework** (ESP32, ESP32-C6 & ESP8266).  
> 🔒 **100 % Lokal & Cloud-frei**: Direkte Steuerung über den internen UART/USB-Anschluss der TCL Klimaanlage.

---

## 📋 Inhaltsverzeichnis

- [Über das Projekt & Kompatibilität](#-über-das-projekt--kompatibilität)
  - [Repositories & Pull-Request-Status](#repositories--pull-request-status)
  - [Upstream-Update-Check & Synchronisation](#-upstream-update-check--synchronisation)
  - [Unterstützte Klimaanlagen-Modelle](#unterstützte-klimaanlagen-modelle)
  - [Protokoll & Voraussetzungen](#protokoll--voraussetzungen)
- [Hardware & Verkabelung](#-hardware--verkabelung)
- [Konfigurationsdateien & Navigation](#-konfigurationsdateien--navigation)
- [Plattform-Einrichtung (Boards & Mikrocontroller)](#-plattform-einrichtung-boards--mikrocontroller)
  - [Besonderheit beim ESP32-C6 (ESP-IDF)](#5-esp32-c6-nur-mit-esp-idf-framework)
- [Netzwerk-Konfiguration (Manuelle IP-Adresse)](#-netzwerk-konfiguration-manuelle-ip-adresse)
- [Einbindung via Remote Packages (Modular)](#-einbindung-via-remote-packages-modular)
  - [Beschreibung der Pakete](#beschreibung-der-pakete)
  - [Hinweis zur Remote-Package URL](#-ausführliche-erklärung-der-wählbaren-remote-package-urls)
  - [Code-Beispiele: Richtig vs. Falsch](#code-beispiele-richtig-vs-falsch)
- [Steuerelemente & Funktionen in Home Assistant](#-steuerelemente--funktionen-in-home-assistant)
- [Fehlerbehebung & Tipps](#-fehlerbehebung--tipps)
- [Weiterführende Links & Autor-Dank](#-weiterführende-links--autor-dank)

---

## ℹ️ Über das Projekt & Kompatibilität

Dieses ESPHome-Paket ermöglicht die direkte Anbindung von TCL-Klimaanlagen (und baugleichen Modellen anderer Marken) an **Home Assistant**. Die Anbindung erfolgt lokal über die interne UART/USB-Schnittstelle des Klimageräts.

> ⚠️ **Hinweis zur Hardware-Varianz**: Vorab exakt zu garantieren, ob eine bestimmte Klimaanlage kompatibel ist, ist aufgrund unterschiedlicher Ausstattungs-Revisionen schwierig. Selbst baugleiche Modelle können sich darin unterscheiden, ob ein WLAN-Modul ab Werk verbaut ist, ein USB-Kabel herausgeführt wurde oder der UART-Stecker auf der Platine verlötet ist.

### Repositories & Pull-Request-Status

- 🇩🇪 **Deutsches Haupt-Repository (Empfohlen)**: [https://github.com/Pommel4711/tclac](https://github.com/Pommel4711/tclac) (Branch: `german`)  
- 🇷🇺 **Originales Upstream-Repository**: [https://github.com/I-am-nightingale/tclac](https://github.com/I-am-nightingale/tclac) (Branch: `master`)  

> ℹ️ **Warum dieses deutsche Repository (`Pommel4711/tclac`)?**  
> Dieses deutsche Repository ([Pommel4711/tclac](https://github.com/Pommel4711/tclac)) wurde erstellt, um alle Anpassungen (vollständige deutsche Übersetzungen aller Entitäten sowie umfassende Framework-Neutralität für neuere Mikrocontroller-Chips wie den **ESP32-C6**) gebündelt bereitzustellen.  
> 
> **Hintergrund zur Framework-Unterstützung (ESP-IDF vs. Arduino)**:  
> Neuere ESP32-Chips (wie der ESP32-C6) besitzen in ESPHome / Home Assistant häufig noch keine native **Arduino-Framework-Unterstützung**. Damit auch diese modernen Chips ohne Einschränkungen genutzt werden können, wurden die Komponenten framework-neutral gestaltet. Sie unterstützen nun nativ das **ESP-IDF Framework** zusätzlich zum **Arduino-Framework**.  
> 
> **Pull-Request Status & Rückfall-Ebene bei Updates:**  
> Diese Erweiterungen sind als Pull-Request (PR) beim originalen Upstream-Repository eingereicht.  
> *Rückfall-Option:* Falls ESPHome in Zukunft grundlegende C++ API-Änderungen einführt und das deutsche Repository temporär noch nicht nachgepflegt wurde, kann in der Konfiguration jederzeit flexibel auf das originale Upstream-Repository (`https://github.com/I-am-nightingale/tclac.git`) umgestellt werden, da der Hauptentwickler *I-am-nightingale* dort C++ Updates direkt einpflegt.

---

### 🔄 Upstream-Update-Check & Synchronisation

- 📅 **Letzter Stand / Commit im deutschen Repository**: ![Zuletzt aktualisiert](https://img.shields.io/github/last-commit/Pommel4711/tclac/german?label=Stand&color=blue&style=flat-square)
- 🔍 **Live-Commits vergleichen**:  
  👉 [**GitHub Compare: Upstream (I-am-nightingale:master) vs. German (Pommel4711:german)**](https://github.com/I-am-nightingale/tclac/compare/master...Pommel4711:tclac:german)

Über diesen Direktlink zeigt GitHub dir sofort an, ob der Original-Autor *I-am-nightingale* seit der letzten Synchronisation neue Funktionen, Bugfixes oder Protokoll-Anpassungen veröffentlicht hat.

### Unterstützte Klimaanlagen-Modelle

Folgende Modelle wurden (mit oder ohne Lötarbeiten) von der Community erfolgreich getestet:

- **Axioma**: ASX09H1/ASB09H1
- **Ballu**: BSAI-12HN1_15Y, Discovery DC BSVI-07HN8, BSVI-09HN8, BSVI-12HN8
- **Daichi**: AIR20AVQ1/AIR20FV1, AIR25AVQS1R-1/AIR25FVS1R-1, AIR35AVQS1R-1/AIR35FVS1R-1, DA35EVQ1-1/DF35EV1-1
- **Dantex**: RK-12SATI/RK-12SATIE
- **Ecostar**: Radium KVS-RAD09CH
- **iFFALCON**: F1 18
- **Royal Clima**: Gloria Inverter, Pandora RC-PDC28HN
- **Tesla**: TT27TP61S-0932IAWUV
- **TCL**:
  - ELI ONF 12
  - Liferise ONF 09
  - TAC-CT09INV/R
  - One Inverter TACM-09HRID/E1 *(möglicherweise abweichende Pin-Belegung)*
  - TAC-07CHSA/TPG-W
  - TAC-09CHSA/TPG
  - TAC-09CHSA/DSEI-W
  - TAC-09HRID/E1
  - TAC-12CHSA/TPG
  - TAC-12CHSA/TPGI
  - TAC-XAL24I
  - TPG31IHB

### Protokoll & Voraussetzungen

- **Nachrichtenlängen**: Die Komponente unterstützt vom Klimagerät gesendete Nachrichtenlängen von **61, 65 und 68 Bytes** *(vollständig getestet und verifiziert mit 61-Byte-Nachrichten)*.
- **Mindestanforderungen**: Home Assistant und ESPHome in Version **2026.4.0 oder neuer**.

---

## 🔌 Hardware & Verkabelung

Die Steuerung erfolgt über die interne USB-A-Buchse der Klimaanlage. Diese nutzt kein USB-Protokoll, sondern ein **5V UART-Seriell-Protokoll**. Dazu wird ein einfaches USB-A-Kabel mit dem Mikrocontroller verbunden:

### 1. Physikalische Pin-Zuordnung (USB-A ↔ ESP32 / ESP8266)

| USB-A Pin | Kabelfarbe (Standard) | Signal der Klimaanlage | → ESP32 / ESP8266 Pin | Funktion |
|:---:|:---:|:---:|:---:|:---|
| **VBUS / VCC** | **Rot** | 5V Stromversorgung | **VIN / 5V** | 5V Stromversorgung für den ESP |
| **GND** | **Schwarz** | Masse (0V) | **GND** | Gemeinsame Masse |
| **D+** | **Grün** | TX (Klimaanlage sendet) | **RXD (GPIO3)** | Empfangsleitung des ESP |
| **D-** | **Weiß / Grau** | RX (Klimaanlage empfängt) | **TXD (GPIO1)** | Sendeleitung des ESP |

> 🚨 **Wichtigster Sicherheitshinweis**:  
> **Rot (VBUS / 5V)** und **Schwarz (GND)** dürfen **niemals** vertauscht werden! Die Stromversorgung muss immer exakt an 5V/VIN und GND anliegen, um Beschädigungen zu vermeiden.

---

### 2. Pins anpassen in `TCL-Conditioner.yaml` (Je nach ESP-Modell)

Je nachdem, welchen Mikrocontroller du einsetzt (z. B. ESP32 WROOM32, ESP32-C3, ESP32-C6 oder ESP8266 / D1 Mini), musst du in der Konfigurationsdatei [`TCL-Conditioner.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/TCL-Conditioner.yaml) die GPIO-Pins unter `substitutions` anpassen:

```yaml
substitutions:
  # Für Standard ESP32 (WROOM32 / NodeMCU):
  uart_rx: GPIO3 # Empfängt Daten von D+ (Grün)
  uart_tx: GPIO1 # Sendet Daten an D- (Weiß/Grau)

  # Beispiel für ESP32-C3 / ESP32-C6 (falls andere HW-UART-Pins genutzt werden):
  # uart_rx: GPIO20
  # uart_tx: GPIO21
```

---

### 3. Warum herrscht Verwirrung bei RX & TX?

In der seriellen Kommunikation (UART) werden Daten **immer gekreuzt** übertragen:
- **TX** (Sender der Klimaanlage, `D+`) muss an **RX** (Empfänger des ESP, z. B. `GPIO3`).
- **RX** (Empfänger der Klimaanlage, `D-`) muss an **TX** (Sender des ESP, z. B. `GPIO1`).

**Ursachen für Verwirrung in anderen Repositories (z. B. bei `sorz2122`):**
1. In manchen älteren READMEs wurden beim Formatieren der Tabellen versehentlich Zeilen verschoben (z. B. `GND → VIN`).
2. Auf manchen ESP-Boards bezeichnet der Aufdruck `RX` den eigenen Empfänger-Pin (`GPIO3`), während andere Board-Hersteller den Pin beschriften, an den der externe Empfänger angeschlossen werden soll.

---

### 4. Funktions-Test & Diagnose bei RX/TX-Vertauschung

Falls nach dem Flashen keine Daten von der Klimaanlage empfangen werden, musst du keine Sorge haben: **UART-Datenleitungen nehmen bei Vertauschung keinen Schaden!**

#### 🔍 So prüfst du die Verbindung Schritt für Schritt:

1. Schließe **D+ (Grün)** an `GPIO3` (RXD) und **D- (Weiß/Grau)** an `GPIO1` (TXD) an.
2. Schalte das Klimagerät ein und öffne die **ESPHome-Logs** in Home Assistant.
3. **Ergebnis-Diagnose:**
   - ✅ **Richtig verkabelt**: Im Log erscheinen alle paar Sekunden Datenpakete (61 Bytes) von der Klimaanlage. Temperatur und Status werden in Home Assistant angezeigt.
   - ❌ **Vertauscht (RX/TX falsch)**: Im Log erscheinen keine Daten oder Fehlermeldungen wie `Climate unit not responding`.

#### 🛠️ Lösung bei vertauschten Datenleitungen:
Du hast zwei einfache Möglichkeiten:
1. **Hardware-seitig**: Tausche einfach das grüne (`D+`) und das weiß/graue (`D-`) Kabel am ESP32 untereinander.
2. **Software-seitig**: Ändere in deiner [`TCL-Conditioner.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/TCL-Conditioner.yaml) die beiden Zeilen auf:
   ```yaml
   uart_rx: GPIO1
   uart_tx: GPIO3
   ```

---

## 📁 Konfigurationsdateien & Navigation

Im Repository stehen vorbereitete Konfigurationsdateien zur Verfügung. Lade sie herunter oder kopiere den Inhalt in dein ESPHome-Projekt:

- [`TCL-Conditioner.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/TCL-Conditioner.yaml) – **Vollständige Vorlage**: Enthält ausführliche Kommentare zu jedem Einstellungsfeld.
- [`Sample_conf.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/Sample_conf.yaml) – **Kompakte Basiskonfiguration**: Schnelle Einrichtung ohne lange Erklärungen.
- Pakete im Ordner [`packages/`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages):
  - [`packages/core.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/core.yaml) – Kernsteuerung (Pflicht)
  - [`packages/leds.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/leds.yaml) – Status-LEDs
  - [`packages/bad_connect.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/bad_connect.yaml) – 3-fach Befehlswiederholung
  - [`packages/uart_speed.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/uart_speed.yaml) – Baudraten-Umschalter
  - [`packages/screen.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/screen.yaml) – OLED-Display (SSD1306)

---

## ⚙️ Plattform-Einrichtung (Boards & Mikrocontroller)

Je nach verwendeter Hardware muss der passende Plattform-Abschnitt in der YAML-Datei ausgewählt werden.

> ⚠️ **Wichtig**: Es darf immer nur **ein** Plattform-Block aktiv sein! Kommentiere nicht benötigte Plattformzeilen aus oder lösche sie.

### Beispiels-Konfigurationen:

#### 1. ESP-01S (ESP8266)
```yaml
esp8266:
  board: esp01_1m
```

#### 2. Hommyn HDN/WFN-02-01 Modul (ESP32-C3 mit Arduino-Framework)
```yaml
esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: arduino
```

#### 3. ESP32 WROOM32 / NodeMCU-32S
```yaml
esphome:
  platform: ESP32
  board: nodemcu-32s
```

#### 4. Wemos D1 Mini / NodeMCU (ESP8266 / ESP-12F)
```yaml
esphome:
  platform: ESP8266
  board: esp12e
```

#### 5. ESP32-C6 (Nur mit ESP-IDF Framework!)
> ⚠️ **Wichtiger Hinweis zum ESP32-C6 & Compilier-Fehlern**: Für den ESP32-C6 existiert in ESPHome / Home Assistant derzeit **keine Arduino-Framework-Unterstützung**.  
> Falls beim Kompilieren in Home Assistant / ESPHome Fehler auftreten, die sich auf Arduino-Bibliotheken beziehen (z. B. `fatal error: Arduino.h: No such file or directory`, Fehler mit `String`-Bibliotheken oder fehlender Board-Support), muss die Konfiguration unter `esp32:` zwingend auf **`esp-idf`** umgestellt werden:

```yaml
esp32:
  board: esp32-c6-devkitc-1
  framework:
    type: esp-idf
```

---

## 🌐 Netzwerk-Konfiguration (Manuelle IP-Adresse)

Standardmäßig bezieht der Mikrocontroller seine IP-Adresse automatisch per DHCP. Um eine feste (statische) IP-Adresse zu vergeben, kann am Ende der Konfigurationsdatei folgender Block hinzugefügt werden:

```yaml
wifi:
  manual_ip:
    static_ip: 192.168.1.4
    gateway: 192.168.1.1
    subnet: 255.255.255.0
```

---

## 📦 Einbindung via Remote Packages (Modular)

Die Konfiguration nutzt das `packages`-Feature von ESPHome. Dadurch werden die Logik-Bausteine direkt von GitHub geladen. Dies ermöglicht einfache Updates, ohne den lokalen Code manuell pflegen zu müssen.

### Beschreibung der Pakete

- **[`packages/core.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/core.yaml)** *(Pflicht)*: Kernlogik der Klimasteuerung, Klima-Entität, Piepser-/Display-Schalter sowie Lamellensteuerung.
- **[`packages/leds.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/leds.yaml)** *(Optional)*: Steuerung physischer Sende-/Empfangs-LEDs (`receive_led`, `transmit_led`).
- **[`packages/bad_connect.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/bad_connect.yaml)** *(Optional)*: Sendet jeden Steuerbefehl 3-fach an die Klimaanlage. Empfohlen bei langen Kabelwegen oder Verbindungsproblemen.
- **[`packages/uart_speed.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/uart_speed.yaml)** *(Optional)*: Fügt eine Dropdown-Liste in Home Assistant hinzu, um die UART-Baudrate dynamisch anzupassen (z. B. 9600, 38400, 115200 Baud).
- **[`packages/screen.yaml`](file:///C:/Users/philipp.fiedler/Desktop/Aufgaben/eig/tclac/packages/screen.yaml)** *(Optional)*: Steuerung eines SSD1306 OLED-Displays (128x32).

---

### 💡 Ausführliche Erklärung der wählbaren Remote-Package URLs

In der Konfiguration unter `packages.remote_package.url` kannst du festlegen, aus welchem Repository die Pakete geladen werden sollen.

#### 📊 Entscheidungshilfe auf einen Blick:

| Anwendungsfall / Anforderung | Empfohlenes Repository | URL | `ref` |
|:---|:---|:---|:---|
| 🇩🇪 **Standard & Deutsch** (Deutsche Benennung, ESP32-C6, ESP-IDF & Arduino) | **Pommel4711/tclac** *(Empfohlen)* | `https://github.com/Pommel4711/tclac.git` | `german` |
| 🇷🇺 **Neueste Upstream-Features / C++ Fixes** (Original-Autor *I-am-nightingale*) | **I-am-nightingale/tclac** | `https://github.com/I-am-nightingale/tclac.git` | `master` |

---

#### 🔍 Wann wählst du welches Repository?

1. **Wähle `https://github.com/Pommel4711/tclac.git` (`ref: german`) WENN:**
   - Du alle Schalter, Modi und Optionen in Home Assistant auf **Deutsch** haben möchtest.
   - Du einen **ESP32-C6** oder ein anderes Board nutzt, das das **ESP-IDF Framework** erfordert (da der ESP32-C6 in ESPHome noch keine Arduino-Unterstützung hat).
   - Du maximale Framework-Neutralität (Arduino & ESP-IDF) benötigst.

2. **Wähle `https://github.com/I-am-nightingale/tclac.git` (`ref: master`) WENN:**
   - ESPHome in Zukunft grundlegende C++ API-Änderungen durchführt und das deutsche Repository temporär noch nicht aktualisiert wurde – so erhältst du sofort die neuesten C++ Fixes des Original-Autors.
   - Das originale russische Repository von *I-am-nightingale* in der Zukunft **neue Funktionen oder Protokoll-Updates** erhält, die im deutschen Branch noch nicht eingepflegt wurden.
   - Der Pull-Request im Hauptprojekt offiziell gemergt wurde.

---

### Code-Beispiele: Richtig vs. Falsch

> ⚠️ **Einrückung beachten**: YAML reagiert empfindlich auf Leerzeichen. Alle Zeilen unter `files:` müssen exakt an derselben Spalte ausgerichtet sein!

#### ✅ RICHTIG: Standard-Einbindung (Deutsche Version – Empfohlen)
```yaml
packages:
  remote_package:
    url: https://github.com/Pommel4711/tclac.git
    ref: german
    files:
    # v - Exakte Spalten-Ausrichtung beachten!
      - packages/core.yaml # Kernkomponente (Pflicht)
      - packages/leds.yaml
    refresh: 1d
```

#### ✅ RICHTIG: Standard-Einbindung (Originales Upstream-Repository)
```yaml
packages:
  remote_package:
    url: https://github.com/I-am-nightingale/tclac.git
    ref: master
    files:
    # v - Exakte Spalten-Ausrichtung beachten!
      - packages/core.yaml # Kernkomponente (Pflicht)
      - packages/leds.yaml
    refresh: 30s
```

#### ✅ RICHTIG: Mit Wiederholungs-Modus (`bad_connect.yaml`)
```yaml
packages:
  remote_package:
    url: https://github.com/Pommel4711/tclac.git
    ref: german
    files:
    # v - Exakte Spalten-Ausrichtung beachten!
      - packages/core.yaml
      - packages/leds.yaml
      - packages/bad_connect.yaml
    refresh: 1d
```

#### ✅ RICHTIG: Mit Baudraten-Umschalter (`uart_speed.yaml`)
```yaml
packages:
  remote_package:
    url: https://github.com/Pommel4711/tclac.git
    ref: german
    files:
    # v - Exakte Spalten-Ausrichtung beachten!
      - packages/core.yaml
      - packages/leds.yaml
      - packages/uart_speed.yaml
    refresh: 1d
```

#### ❌ FALSCH: Defekte Einrückung (Syntaxfehler in ESPHome)
```yaml
# ❌ FALSCH: leds.yaml ist zu weit eingerückt!
packages:
  remote_package:
    url: https://github.com/Pommel4711/tclac.git
    ref: german
    files:
    # v - Ausrichtung fehlerhaft!
      - packages/core.yaml
        - packages/leds.yaml # ❌ ESPHome meldet einen Fehler!
    refresh: 1d
```

---

## 🎛️ Steuerelemente & Funktionen in Home Assistant

Nach der Integration stehen folgende Steuerelemente zur Verfügung:

### 1. Schalter (Switches)
* 🔔 **Beeper** (*Piepser*): Aktiviert/deaktiviert den Quittungston der Klimaanlage bei Befehlsempfang.
* 🖥️ **Display**: Schaltet die Temperaturanzeige am Gehäuse des Innengeräts ein oder aus.
* 💡 **Display on module**: Schaltet die Status-LED auf dem ESP-Modul ein/aus.
* ⚡ **Force config**: Erzwingt das erneute Senden aller Einstellungen an die Klimaanlage.

### 2. Lamellensteuerung (Dropdowns / Selects)
* ↕️ **Vertical swing** (*Vertikale Schwingung*):
  * `Von oben nach unten`, `In der oberen Hälfte`, `In der unteren Hälfte`
* ↔️ **Horizontal swing** (*Horizontale Schwingung*):
  * `Von links nach rechts`, `Im linken Bereich`, `Im Zentrum`, `Im rechten Bereich`
* 📌 **Vertical fixing** (*Vertikale Arretierung*):
  * `Letzte Position`, `Ganz nach oben`, `In der oberen Hälfte`, `In der Mitte`, `In der unteren Hälfte`, `Ganz nach unten`
* 📌 **Horizontal fixing** (*Horizontale Arretierung*):
  * `Letzte Position`, `Ganz nach links`, `In der linken Hälfte`, `In der Mitte`, `In der rechten Hälfte`, `Ganz nach rechts`

---

## 🔍 Fehlerbehebung & Tipps

1. **Keine Kommunikation mit der Klimaanlage**:
   - Stelle sicher, dass `RX` und `TX` korrekt verkabelt sind (versuchsweise tauschen).
   - In der Konfiguration muss `logger: baud_rate: 0` gesetzt sein, da der UART-Port exklusiv für die Klimaanlage genutzt wird.
2. **Kompilier-Fehler bezüglich Arduino-Bibliotheken (besonders beim ESP32-C6)**:
   - Sollten beim Kompilieren in Home Assistant Fehler bezüglich Arduino-Bibliotheken auftreten (z. B. `fatal error: Arduino.h: No such file or directory`, Fehler mit `String`-Klassen oder fehlendem Framework-Support), liegt das daran, dass neuere Microcontroller (wie der ESP32-C6) unter ESPHome noch kein Arduino-Framework unterstützen.
   - Stelle in deiner Konfiguration unter `esp32:` das Framework zwingend auf `type: esp-idf` um.
3. **Temperaturbereich**:
   - Die Soll-Temperatur des TCL-Protokolls liegt zwischen **16 °C und 31 °C**.

---

## 🔗 Weiterführende Links & Autor-Dank

- 🇩🇪 **Deutsches Haupt-Repository**: [Pommel4711/tclac](https://github.com/Pommel4711/tclac) (Deutsche Variante, ESP-IDF & ESP32-C6 Unterstützung)
- 🌟 **Originales Upstream-Repository**: [I-am-nightingale/tclac](https://github.com/I-am-nightingale/tclac) (Russisches Originalprojekt)
- 📡 **MQTT-Alternative**: Wer die Klimaanlage nicht über ESPHome, sondern über MQTT anbinden möchte: [TCL-TAC-07-WiFi von pavel211](https://github.com/pavel211/TCL-TAC-07-WiFi)
- 📝 **Artikel & Anleitung auf Dzen**: [Blogartikel des Autors auf Dzen](https://dzen.ru/a/ZmdoyUNswXWnulhg)
