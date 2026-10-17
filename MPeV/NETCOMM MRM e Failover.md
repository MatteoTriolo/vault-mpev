---
tags: [mpev, componente, rete, ridondanza]
---
# NETCOMM, MRM e Failover

Torna a [[00 - MPeV MOC]]. Vedi [[Architettura a tre livelli]].

## NETCOMM
- Connessione di rete tra nodi CLIENT e SERVER; un solo componente, modalità server/client/dinamica. Parametri in SAL.INI ([[SAL]]). TCP/IP su Ethernet.
- Collegato al [[Core e bus]] via Core BUS.
- Con DIT in **STANDBY** gira in due istanze (client e server); l'**MRM** decide quale opera. Vale anche per driver e alcune app.
- Modalità sicura per DIT: SSL con certificati su tutte le macchine.
- Nell'impianto: DIT 1 STANDBY, DIT 0 e 11 SERVER (motivo da verificare, [[Punti da verificare]]).

## Failover
1. Normale: Master ha VIP e fa polling; heartbeat + copia dati (tag, stato DIT, ack allarmi) verso lo Slave.
2. Guasto: heartbeat assente, lo Slave se ne accorge.
3. Subentro: Slave diventa Master, prende VIP e polling. Operatore: nessuno stacco.
Dopo la riparazione il Server 1 rientra come Slave e si riallinea (verifica in NET Comms, [[MPC e Manage]], e [[Component e Apps Monitoring]]).
