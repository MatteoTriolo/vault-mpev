---
tags: [mpev, componente]
---
# SAL – System Applications Launcher

Torna a [[00 - MPeV MOC]]. Fa parte di [[Mappa componenti]].

- Avviatore e core del sistema; deve restare attivo per tutto l'uso di MPeV.
- Legge `SAL.INI` (in `\CONFIG`) e il database oggetti (in `\DATABASE`). Vedi [[File e cartelle]].
- Finestra **MPeV Settings**: General, Functions Activation, Folder Aliases, Security, Database, Available drivers, Apps.
- Per ogni app: abilitata, percorso eseguibile (es. `{SYSPATH}SeasXTMC.exe`), argomenti (es. `/CONTROLLED`, `/DIT`).
- Esempio: argomento `/DIT` nella riga del [[TCAE]] → lavora solo su un DIT; file `TCAE_DIT2.INI` per DIT 2.
- Associa i driver ai DIT con la riga `associateddrv` ([[Driver OPC MBD SIM]]).
- Avvio/chiusura: [[Avvio spegnimento riavvio]].
