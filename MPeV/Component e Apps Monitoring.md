---
tags: [mpev, diagnostica]
---
# Component & Apps Monitoring

Torna a [[00 - MPeV MOC]].

Mostra: driver di campo e rete per DIT ([[Driver OPC MBD SIM]]), App MPeV ([[AEC e AEV]], [[TMC]]), app non MPeV.
Apri da menu contestuale System o console MPeV ([[SAL]]).

Comandi (tasto destro): Show, Start, Stop, Set to STBY, Resume from STBY, Re-Initialize, Restart.

Stati: STOPPED → RUNNING ⇄ STANDBY (in esecuzione ma non opera: è lo stato dello Slave, verifica che l'MRM abbia agito, [[NETCOMM MRM e Failover]]).

Esempio: driver SIM Running, AEC e IPMSUDPGW Stopped → clic destro su AEC → Start.
