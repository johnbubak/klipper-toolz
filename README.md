# Klipper Toolz 🛠️

Eine Sammlung nützlicher Makros zur Wartung deines Klipper-Druckers.

### 🧼 Nozzle Total Clean (Encapsulation + Cold Pull)
Dieses Makro druckt eine kleine Form, umschließt die Düse mit Plastik und zieht beim Abkühlen sowohl den inneren Schmutz (Cold-Pull) als auch äußere Reste (Encapsulation) ab.

**Wichtig:** 
- Bei **Direct-Drive** Extrudern pausiert das Makro. Du musst den Hebel manuell öffnen, bevor du auf "Resume" klickst.
- Benötigt `[pause_resume]` in deiner Konfiguration.

## 🚀 Installation & Update-Manager

Um **Klipper Toolz** in dein System zu integrieren und automatische Updates über Fluidd/Mainsail zu erhalten, folge diesen Schritten:

### Moonraker Update-Manager einrichten
Öffne deine `moonraker.conf` und füge am Ende den folgenden Block hinzu. 
> **Wichtig:** Ersetze `DEIN_USER` durch deinen tatsächlichen GitHub-Benutzernamen.

```ini
[update_manager klipper-toolz]
type: git_repo
path: ~/klipper-toolz
origin: github.com
primary_branch: main
managed_services: klipper
```

Makro in Klipper aktivieren
Öffne deine printer.cfg und füge am Anfang (bei deinen anderen Includes) diese Zeile ein:

```ini
[include ../klipper-toolz/encapsulation_clean.cfg]
```

### Direct-Drive Extruder ! Voraussetzungen prüfen
Damit die Pausen-Funktion (besonders wichtig für Direct-Drive Extruder) korrekt funktioniert, muss das pause_resume Modul in deiner printer.cfg aktiv sein. Falls du es noch nicht hast, füge diese Zeile einfach hinzu:

```ini
[pause_resume]
```

