---
tags: [mpev, componente, diagnostica]
---
# MPC e Manage

Torna a [[00 - MPeV MOC]].

- **MPC** (Platform Control, SeasXMPeVPC): si connette al [[Core e bus]] in modo nativo, più oneroso.
- **Manage** (SeasXManage): alter ego via HUB, più leggero; preferirlo dove le funzioni coincidono.
- Strumenti per il configuratore. Menu da icona traybar o CTRL+F9.

| Strumento | Uso |
|---|---|
| Object View | Stato real-time oggetti/tag ([[DIT]]) |
| Element View | Navigazione elementi |
| Alarm Configuration | Soglie e config allarmi ([[TCAE]]) |
| Alarm Summary | Riepilogo allarmi |
| Retain Data | Dati ritenuti |
| System | Chiusura/riavvio |
| Tools | Strumenti vari |
| Drivers | Stato driver ([[Driver OPC MBD SIM]]) |
| NET Comms | Stato rete ([[NETCOMM MRM e Failover]]) |
| Apps | Stato app |
| TCAE tester | Prova script |
| Logon | Accesso utente ([adm]) |

CTRL+F10: Shutdown MPeV, Close Platform Control, Restart Drivers, Cancel ([[Avvio spegnimento riavvio]]).
