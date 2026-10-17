---
tags: [mpev, componente, driver]
---
# Driver di campo: OPC, MBD, SIM

Torna a [[00 - MPeV MOC]]. Alimentano il [[DIT]]; configurati nel [[SAL]].

## Tre cose da non confondere
| Cosa | Cos'è | Chi lo fornisce |
|---|---|---|
| OPC server | Traduttore protocollo PLC (es. Modbus) → OPC | Costruttore PLC (es. ABB AC800) |
| Driver SEASXOPCC | Client OPC di MPeV, polling e collegamento a elementi DIT | MPeV |
| Server Master/Slave | PC fisico su cui gira MPeV | Impianto |

## Tipi di driver
- **OPC** (es. DIT 1): passa da un OPC server (riga `name`, es. `ABB.AC800MC_OpcDaServer.3`).
- **MBD** (es. DIT 11): Modbus TCP diretto, `name` vuoto; config in `MBD11.ini`, un blocco `[SERVERn]` per dispositivo (Address, Port=502, Type=TCP, Timeout=2000). "SERVER" = dispositivo Modbus, non server MPeV.
- **SIM** (es. DIT 0): SEASXSIM, simulazione, nessun dispositivo.

## Un DIT per zona
- Un driver OPC → un solo OPC server per DIT.
- Più PLC dietro lo stesso OPC server → 1 driver, 1 DIT.
- PLC con OPC server proprio → driver e DIT in più.
- MBD: un DIT raccoglie tutti i dispositivi di `MBDn.ini`.

## Limiti e regole
- Fino a 32 DIT/driver contemporanei.
- Modifiche a SAL.INI/database lette solo all'avvio del driver; strutture complesse/oggetti nuovi → riavvio di tutto ([[Avvio spegnimento riavvio]]).
- Stazione server MPeV (driver + NETCOMM SERVER) vs stazione client (NETCOMM CLIENT): [[NETCOMM MRM e Failover]].
