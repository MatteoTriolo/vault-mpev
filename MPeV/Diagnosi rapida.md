---
tags: [mpev, diagnostica]
---
# Diagnosi rapida: sintomo → dove guardare

Torna a [[00 - MPeV MOC]]. Sempre prima: `LOGS` ([[File e cartelle]]).

| Sintomo | Dove | Controlla |
|---|---|---|
| Valore fermo/qualità non buona | MPC Drivers, Object View ([[MPC e Manage]]) | Driver Running? OPC server raggiungibile? Elemento sul registro giusto? ([[Driver OPC MBD SIM]]) |
| Valore sbagliato (385 vs 38,5) | Object View, script [[TCAE]] | Grezzo corretto? Scalatura mancante? |
| Allarme non scatta | Finestra TCAE, TCAE tester, Alarm Configuration | Status Engine Running? Condition Result? Soglia? |
| Slave non allineato | NET Comms, [[Component e Apps Monitoring]] | NETCOMM, rete, MRM ([[NETCOMM MRM e Failover]]) |
| Esterno non vede tag | opcs.ini | OPC=YES, BROWSABLE, SKIPOPCFILTER ([[OPCS]]) |
| Modifica senza effetto | SAL | Serve riavvio ([[Avvio spegnimento riavvio]]) |
| Luminosità non cambia | Finestra [[TMC]] | Qualità GOOD? Tag set true? Log |
| Componente Stopped | Monitoring, LOGS | Start/Restart, leggi il log |
