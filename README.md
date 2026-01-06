# Klipper Toolz 🛠️

Eine Sammlung nützlicher Makros zur Wartung deines Klipper-Druckers.

---

## 🧼 Nozzle Total Clean (Encapsulation + Cold Pull)

Dieses Makro kombiniert zwei bewährte Reinigungsmethoden:

- **Cold Pull** (Innenreinigung): Zieht verbrannte Reste und Partikel aus dem Schmelzkanal
- **Encapsulation** (Außenreinigung): Umschließt die Düse mit Plastik und zieht äußere Verklebungen beim Abkühlen ab

### ⚠️ Wichtige Hinweise

- **Direct-Drive Extruder**: Das Makro erkennt automatisch Direct-Drive (rotation_distance < 10) und pausiert. Du musst dann den Extruder-Hebel manuell öffnen, bevor du "Resume" klickst, um das Getriebe zu schützen.
- **Bowden Extruder**: Läuft automatisch durch
- Benötigt das `[pause_resume]` Modul in deiner Konfiguration

### 📊 Anpassbare Parameter

Das Makro unterstützt folgende optionale Parameter:
```gcode
TOTAL_CLEAN_COMBO BED_TEMP=60 PRINT_TEMP=220 PULL_TEMP=90 X_POS=10 Y_POS=10
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| `BED_TEMP` | 60 | Betttemperatur in °C |
| `PRINT_TEMP` | 220 | Drucktemperatur zum Formen der Kapsel |
| `PULL_TEMP` | 90 | Abkühltemperatur für den Cold-Pull (PLA: 80-90°C, PETG: 100-110°C, ABS: 130-140°C) |
| `X_POS` | 10 | X-Position für die Reinigungsform |
| `Y_POS` | 10 | Y-Position für die Reinigungsform |

**Beispiel für PETG:**
```gcode
TOTAL_CLEAN_COMBO PRINT_TEMP=240 PULL_TEMP=110
```

---

## 🚀 Installation & Update-Manager

Um **Klipper Toolz** in dein System zu integrieren und automatische Updates über Fluidd/Mainsail zu erhalten, folge diesen Schritten:

### 1. Moonraker Update-Manager einrichten

Öffne deine `moonraker.conf` und füge am Ende den folgenden Block hinzu:
```ini
[update_manager klipper-toolz]
type: git_repo
path: ~/klipper-toolz
origin: https://github.com/johnbubak/klipper-toolz.git
primary_branch: main
managed_services: klipper
```

Klicke anschließend auf **Save & Restart** in Fluidd/Mainsail.

### 2. Makro in Klipper aktivieren

Öffne deine `printer.cfg` und füge am Anfang (bei deinen anderen Includes) diese Zeile ein:
```yaml
[include klipper-toolz/encapsulation_clean.cfg]
```

### 3. Voraussetzungen prüfen (Wichtig für Direct-Drive!)

Damit die Pausen-Funktion korrekt funktioniert, muss das `pause_resume` Modul in deiner `printer.cfg` aktiv sein. Falls du es noch nicht hast, füge diese Zeile hinzu:
```yaml
[pause_resume]
```

Speichere die Konfiguration und starte Klipper neu.

---

## 🛠️ Workflow

Ab jetzt ist dein Drucker mit diesem Repository verbunden. Sobald Änderungen auf GitHub vorgenommen werden, wird dir in der Fluidd/Mainsail Oberfläche im **Update Manager** ein Update angeboten. Ein Klick genügt, um die neuesten Makros auf deinen Drucker zu laden.

---

## 🤝 Beitragen

Verbesserungsvorschläge und Pull Requests sind willkommen! Bitte öffne ein Issue, um größere Änderungen zu diskutieren.

---

## 📝 Lizenz

Dieses Projekt steht unter der [MIT License](LICENSE).

---

## ⚠️ Haftungsausschluss

Die Nutzung dieser Makros erfolgt auf eigene Gefahr. Der Autor übernimmt keine Haftung für Schäden an Hardware oder Drucken. Teste neue Makros immer zunächst unter Aufsicht.
