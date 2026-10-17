---
tags: [mpev, componente, opc]
---
# OPCS – OPC Server

Torna a [[00 - MPeV MOC]]. Espone i tag del [[Core e bus]]/[[DIT]] a sistemi terzi (SCADA, gateway). Lanciato dal [[SAL]] o in automatico alla connessione di un client OPC.

Config: `opcs.ini`, sezione OPCGATEWAY ([[File e cartelle]]).

| Parametro | Default | Funzione |
|---|---|---|
| OPCSERVER_RATE | 100 ms | Rinfresco dati interni |
| MPEVTAGSCAN_RATE | 500 ms | Scansione tag MPeV |
| PORTUPDATEDATATIME | 500 ms (≥200) | Rinfresco dal coreport |
| UPDATETAGSALWAYSONTIMER | False | True: aggiorna item a ogni ciclo |
| BROWSABLE | True | True: crea subito tutti gli item; False: solo su richiesta |
| DEVICEREAD_INT | True | Lettura DEVICE via chiamata interna |
| SKIPOPCFILTER | False | True: ignora filtro OPC=YES |
| UNCISGOOD | False | True: qualità uncertain = GOOD |

Sistema esterno non vede un tag: controlla OPC=YES, BROWSABLE, SKIPOPCFILTER ([[Diagnosi rapida]]).
