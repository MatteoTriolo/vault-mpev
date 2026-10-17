---
tags: [mpev, componente, app]
---
# TMC – Tag Monitors Control

Torna a [[00 - MPeV MOC]].

- Gestisce via tag i parametri dei monitor: luminosità, contrasto, retroilluminazione, ingressi.
- App MPeV configurata nel [[SAL]]: `{SYSPATH}SeasXTMC.exe`, argomento `/CONTROLLED`.
- Applica il parametro quando cambia la tag "value" (qualità GOOD) o quando una tag "set" booleana va a true.
- Esempio: TEST_SIMLB, Brightness = 70 → log "Set Brightness to 70" (monitor DURAMON).
- Diagnosi: [[Diagnosi rapida]].
