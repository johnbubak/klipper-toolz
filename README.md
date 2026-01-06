# Klipper Toolz 🛠️

Eine umfassende Sammlung nützlicher Makros zur Wartung und Reinigung deines Klipper-Druckers.

---

## 🧰 Verfügbare Makros

### 1. 🧼 Nozzle Total Clean (Encapsulation + Cold Pull)

**Datei:** `encapsulation_clean.cfg`

Kombiniert zwei bewährte Reinigungsmethoden:
- **Cold Pull** (Innenreinigung): Zieht verbrannte Reste aus dem Schmelzkanal
- **Encapsulation** (Außenreinigung): Umschließt die Düse mit Plastik und zieht äußere Verklebungen ab

#### ⚠️ Wichtige Hinweise
- **Direct-Drive**: Automatische Pause, Hebel manuell öffnen vor Resume
- **Bowden**: Läuft automatisch durch
- Benötigt `[pause_resume]` Modul

#### Parameter
```gcode
TOTAL_CLEAN_COMBO BED_TEMP=60 PRINT_TEMP=220 PULL_TEMP=90 X_POS=10 Y_POS=10
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| `BED_TEMP` | 60 | Betttemperatur in °C |
| `PRINT_TEMP` | 220 | Drucktemperatur |
| `PULL_TEMP` | 90 | Abkühltemperatur (PLA: 80-90, PETG: 100-110, ABS: 130-140) |
| `X_POS` | 10 | X-Position der Reinigungsform |
| `Y_POS` | 10 | Y-Position der Reinigungsform |

---

### 2. ❄️ Cold Pull (Internal Cleaning)

**Datei:** `cold_pull.cfg`

Klassische Methode zur inneren Düsenreinigung. Nutzt das Filament selbst als "Reinigungspfropfen".

#### Ablauf
1. Heizt die Düse auf Schmelztemperatur
2. Kühlt kontrolliert auf Pull-Temperatur ab
3. Wartet auf **manuelle Filament-Extraktion** durch den Nutzer

#### Parameter
```gcode
COLD_PULL HEAT_TEMP=220 PULL_TEMP=90
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| `HEAT_TEMP` | 220 | Schmelztemperatur des Materials |
| `PULL_TEMP` | 90 | Temperatur zum Herausziehen (Material muss noch zäh sein) |

**💡 Tipp:** Bei hartnäckigen Verstopfungen 2-3 Durchgänge durchführen.

---

### 3. 🧹 Bed-Edge Wipe (External Cleaning)

**Datei:** `bed_wipe.cfg`

Nutzt die Bettkante als improvisierte "Bürste" für äußere Düsenreinigung.

#### Parameter
```gcode
CLEAN_NOZZLE_NO_TOOLS WIPE_TEMP=200 WIPE_LENGTH=50 X_START=0 Y_START=0
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| `WIPE_TEMP` | 200 | Wisch-Temperatur (Material soll zäh, nicht flüssig sein) |
| `WIPE_LENGTH` | 50 | Länge der Wischbewegung in mm |
| `X_START` | 0 | Startposition X (Ecke des Betts) |
| `Y_START` | 0 | Startposition Y (Ecke des Betts) |

**⚠️ Achtung:** Stelle sicher, dass die Bettkante frei von Clips oder anderen Hindernissen ist!

---

### 4. 🔧 Cleaning Pins (Mechanical Scrubbing)

**Datei:** `cleaning_pins.cfg`

Druckt eine Reihe kleiner Türmchen, durch die die Düse mehrfach fährt. Äußere Verklebungen bleiben am frischen Plastik hängen.

#### Parameter
```gcode
PRINT_CLEANING_PINS START_X=10 START_Y=10 PIN_COUNT=5 PIN_DISTANCE=8 PIN_HEIGHT=4 TEMP=210
```

| Parameter | Standard | Beschreibung |
|-----------|----------|--------------|
| `START_X` | 10 | Startposition X (freier Bettbereich!) |
| `START_Y` | 10 | Startposition Y |
| `PIN_COUNT` | 5 | Anzahl der Stäbchen |
| `PIN_DISTANCE` | 8 | Abstand zwischen Stäbchen in mm |
| `PIN_HEIGHT` | 4 | Höhe der Stäbchen in mm |
| `TEMP` | 210 | Drucktemperatur |

**💡 Beste Ergebnisse:** Bauteillüfter auf 100%, damit Pins schnell fest werden.

---

## 🚀 Installation & Update-Manager

### 1. Moonraker Update-Manager einrichten

Öffne deine `moonraker.conf` und füge am Ende hinzu:
```ini
[update_manager klipper-toolz]
type: git_repo
path: ~/klipper-toolz
origin: https://github.com/johnbubak/klipper-toolz.git
primary_branch: main
managed_services: klipper
```

**Save & Restart** in Fluidd/Mainsail.

### 2. Makros in Klipper aktivieren

Öffne deine `printer.cfg` und füge hinzu:
```yaml
# Klipper Toolz - Reinigungs-Makros
[include klipper-toolz/encapsulation_clean.cfg]
[include klipper-toolz/cold_pull.cfg]
[include klipper-toolz/bed_wipe.cfg]
[include klipper-toolz/cleaning_pins.cfg]
```

### 3. Voraussetzungen

Für die Pause-Funktion (Direct-Drive Detection):
```yaml
[pause_resume]
```

**Save & Restart Klipper.**

---

## 🎯 Workflow & Best Practices

### Welches Makro wann nutzen?

| Situation | Empfohlenes Makro | Grund |
|-----------|-------------------|-------|
| Düse innen verstopft | `COLD_PULL` | Zieht Partikel aus dem Schmelzkanal |
| Düse außen verklebt | `BED_WIPE` oder `CLEANING_PINS` | Mechanische Außenreinigung |
| Totalschaden (innen + außen) | `TOTAL_CLEAN_COMBO` | Kombinierte Reinigung in einem Durchgang |
| Vor jedem Druck (Wartung) | `BED_WIPE` | Schnelle äußere Reinigung |

### 🔧 Material-spezifische Temperaturen

| Material | PRINT_TEMP | PULL_TEMP | WIPE_TEMP |
|----------|------------|-----------|-----------|
| PLA | 200-220 | 80-90 | 180-200 |
| PETG | 230-250 | 100-110 | 200-220 |
| ABS | 240-260 | 130-140 | 220-240 |
| TPU | 210-230 | 70-80 | 190-210 |

---

## 🛠️ Update-Workflow

Nach der Einrichtung:
1. Änderungen werden auf GitHub committed
2. Fluidd/Mainsail zeigt im **Update Manager** eine Benachrichtigung
3. Ein Klick auf **Update** lädt die neuesten Makros

---

## 🤝 Beitragen

Pull Requests und Issues sind willkommen! Für größere Änderungen bitte vorher ein Issue öffnen.

---

## 📝 Lizenz

[MIT License](LICENSE)

---

## ⚠️ Haftungsausschluss

Nutzung auf eigene Gefahr. Keine Haftung für Hardware-Schäden. Neue Makros immer unter Aufsicht testen.

---

## 🙏 Credits

Basierend auf Community-Wissen aus:
- Klipper Discourse
- Voron Design
- RepRap Community
