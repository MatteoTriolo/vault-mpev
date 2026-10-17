---
tags: [mpev, file]
---
# File e cartelle

Torna a [[00 - MPeV MOC]]. Prima regola di diagnosi: guarda in `LOGS` ([[Diagnosi rapida]]).

| Cartella | Tipo | Contenuto |
|---|---|---|
| AEC | Base | Binari [[AEC e AEV]] collector |
| AEV | Base | Binari viewer |
| APPS | Base | Applicativi terzi |
| BAR | Base | Backup & Restore storici |
| BIN | Base | Binari [[Core e bus]] |
| CONFIG | Progetto | SAL.INI, TCAE.INI, opcs.ini ([[SAL]], [[TCAE]], [[OPCS]]) |
| DATABASE | Progetto | MDB/DBX/DBM ([[DIT]]) |
| DVW | Base | Binari [[DVW]] |
| DVWPRJ | Progetto | Pagine video del progetto |
| HELP | Base/progetto | Guida |
| HTC, HTV | Base | Binari |
| LOGS | Progetto | Log giornalieri, uno per componente |
| MULTIPRJ | Progetto | Multiprogetto: sottocartelle CONFIG, DATABASE, DVWPRJ |
| RUNTIME | Progetto | File di lavoro (riservata) |

Dalla release R1: modalità multiprogetto (più impianti sulla stessa macchina, uno per volta).
