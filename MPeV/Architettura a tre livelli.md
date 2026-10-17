---
tags: [mpev, architettura]
---
# Architettura a tre livelli

Torna a [[00 - MPeV MOC]].

1. **Livello campo** (HVAC Control Network): PLC e controllori (zone MVZ1, MVZ2) con sonde, valvole, attuatori.
2. **Livello supervisione** (server ridondati HVAC/SMS): Server 1 (Master) e Server 2 (Slave/Standby). Qui gira MPeV. Vedi [[Mappa componenti]].
3. **Livello operatore** (Supervision Network): workstation HMI con pagine grafiche [[DVW]].

## Master e Slave
- Solo il Master interroga il campo e gestisce le logiche.
- Lo Slave è in standby e riceve una copia continua dei dati (data mirroring) via [[NETCOMM MRM e Failover]].
- I client puntano a un **IP virtuale (VIP)** posseduto dal Master.
- Chi decide i ruoli: la logica esterna **MRM**.

Analogia sala di controllo: [[SAL]] accende/spegne, [[Driver OPC MBD SIM]] parlano con gli impianti, [[DIT]] è il tabellone, [[TCAE]] fa conti e soglie, [[AEC e AEV]] gli allarmi, [[OPCS]] parla con l'esterno, [[NETCOMM MRM e Failover]] collega le sale, [[DVW]] è lo schermo, [[MPC e Manage]] gli strumenti del tecnico.
