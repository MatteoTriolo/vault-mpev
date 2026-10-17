---
tags: [mpev, componente, allarmi]
---
# TCAE – Tag Calculation & Alarm Engine

Torna a [[00 - MPeV MOC]]. Legge/scrive il [[DIT]]; gli eventi vanno a [[AEC e AEV]]. Avviato dal [[SAL]].

Logiche su tag/elementi + motore allarmi integrato. Su tutti i DIT o uno solo (`/DIT`).

## Due schede
- **Calculations**: script di calcolo (conversioni, medie, combinazioni).
- **Alarms**: script vero/falso; vero → allarme.

## Regole
- Script in **VB.NET**, commenti con `'`.
- Lettura tag: buffer congelato a inizio script; scrittura committata a fine script.
- Variabili comuni tra calcoli e allarmi; assembly/namespace esterni; file esterni di funzioni.
- Config: `TCAE_DITx.INI` oppure `TCAE.INI` in `\CONFIG`.
- In stand-by, se abilitato, salva la configurazione allarmi.

## Esempio (ReadTag/WriteTag sono segnaposto)
```vb
Dim raw As Double = ReadTag("ZONA1.SONDA") ' es. 385
Dim t As Double = raw / 10                 ' 38,5 °C
WriteTag("ZONA1.TEMP_C", t)
Return (t > 35.0)                          ' allarme
```

## Finestra
Scheda Alarms: Select alarm, Description, Status, Condition Result (False = nessun allarme). Log "Alarm ... added with success", "Resume from Stand-by". Barra: MPeV Status Ready, # Alarms 516, # Calculations 3, Status Engine Running.

Diagnosi: [[Diagnosi rapida]]; prova script con "TCAE tester" in [[MPC e Manage]].
