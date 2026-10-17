---
tags: [mpev, architettura]
---
# Mappa componenti di un server

Torna a [[00 - MPeV MOC]]. Al centro il [[Core e bus]] con la memoria condivisa [[DIT]]; gli altri moduli gli girano attorno, avviati dal [[SAL]].

| Sigla | Nome | In una frase |
|---|---|---|
| [[SAL]] | System Applications Launcher | Avvia e chiude tutto |
| CORE | Nucleo MPeV | Comunicazione interna (Core BUS) |
| [[DIT]] | Driver Image Table | Immagine RAM dei registri PLC |
| OPCC / SEASXOPCC | OPC DA Client Classic | Driver che legge via OPC ([[Driver OPC MBD SIM]]) |
| [[TCAE]] | Tag Calculation & Alarm Engine | Calcoli e allarmi (VB.NET) |
| [[AEC e AEV]] | Alarm Event Collector / Viewer | Raccolta e vista allarmi |
| [[OPCS]] | OPC Server | OPC DA Classic verso terzi |
| NETCOMM | Network Communicator | TCP/IP client/server ([[NETCOMM MRM e Failover]]) |
| MRM | – | Decide ruolo server/client |
| [[DVW]] | Editor/visualizzatore grafico | Pagine operatore |
| [[MPC e Manage]] | Platform Control / SeasXManage | Diagnostica e ingegneria |
| [[TMC]] | Tag Monitors Control | Parametri monitor via tag |
| BAR | Backup & Restore | Backup dati storici |
