# HA-BluePrint-FanControl

Home Assistant Blueprint, das einen Lüfter nach Raumluft regelt: CO2, VOC, Luftfeuchte und Temperatur. Mit Nachtsperre über Zeitfenster, Sonnenstand und/oder Schalter.

[![Blueprint in Home Assistant importieren](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2FMissenberger79%2FHA-BluePrint-FanControl%2Fblob%2Fmain%2Fluefter_raumluft.yaml)

## Was es macht

- Jeder der vier Sensoren ist optional und hat einen eigenen Grenzwert. Pro Sensor wählst du, ob "über" oder "unter" dem Wert auslöst.
- Ist ein Grenzwert für X Minuten verletzt, geht der Lüfter auf die eingestellte Stufe.
- Er läuft, bis **alle** konfigurierten Werte um X % Abstand zum Grenzwert wieder im grünen Bereich sind (Hysterese). Bei 1000 ppm und 10 % also unter 900 ppm.
- Nach einer einstellbaren Wartezeit geht er auf eine Grundstufe zurück. 0 % heißt ausschalten. Es wird nur heruntergeregelt, nie hochgeschaltet.
- Fällt ein Sensor aus, läuft der Lüfter weiter, statt blind abzuschalten.

## Nachtsperre

Solange "Nacht" ist, macht die Automation gar nichts. Nacht kann über drei Quellen definiert werden, einzeln oder kombiniert. Es reicht, wenn eine davon Nacht meldet:

- **Zeitfenster** mit Von- und Bis-Zeit, auch über Mitternacht
- **Sonnenstand** zwischen Sonnenuntergang und Sonnenaufgang, jeweils mit Offset in Minuten
- **Schalter**, Helfer (input_boolean) oder Binärsensor

Ist das Zeitfenster aktiv, hat dessen Endzeit am Morgen Vorrang vor dem Sonnenaufgang. Beispiel mit 22:00 bis 06:00: Im Winter darf der Lüfter ab 6:00 wieder laufen, obwohl die Sonne erst später aufgeht. Im Sommer bleibt er bis 6:00 gesperrt, auch wenn die Sonne früher da ist.

Nach Nachtende wird nachgeholt: Ist dann ein Grenzwert verletzt, startet der Lüfter. Ist alles im grünen Bereich, geht er auf die Grundstufe.

## Voraussetzungen

- Home Assistant 2024.10 oder neuer
- Eine `fan`-Entität, die Prozentstufen kann
- Mindestens ein Sensor

## Installation

Den Button oben nutzen oder die Datei `luefter_raumluft.yaml` nach `config/blueprints/automation/` kopieren. Danach unter Einstellungen, Automatisierungen, Blueprints eine neue Automation daraus anlegen.

## Hinweise

- Bei VOC muss der Grenzwert zur Einheit des Sensors passen (ppb, µg/m³ oder Index).
- Der Standardwert von 500 beim VOC-Grenzwert ist nur ein Platzhalter.

## Lizenz

MIT
