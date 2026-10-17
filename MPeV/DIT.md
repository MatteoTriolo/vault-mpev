---
tags: [mpev, componente, dit]
---
# DIT – Driver Image Table

Torna a [[00 - MPeV MOC]]. Alimentato dai [[Driver OPC MBD SIM]]; letto da [[TCAE]], [[OPCS]], [[DVW]].

Immagine in RAM dei registri dei PLC; area di scambio tra driver e componenti.

## Tre tabelle
- **Oggetti**: la "scatola" (Zona 1 condizionamento, UTA, ventilconvettore).
- **Elementi**: i "piedini", collegati a registri (ingresso, uscita, setpoint).
- **Gruppi**: raggruppano oggetti (lettura mia, da verificare → [[Punti da verificare]]).

## Lettura/scrittura
- Lettura: il driver aggiorna l'elemento col valore grezzo (es. 385); la conversione la fa il [[TCAE]].
- Scrittura: cambia un elemento di uscita/setpoint → il driver scrive nel registro PLC.

## DIT vs database
| | DIT | Database oggetti |
|---|---|---|
| Dove | RAM | Disco `\DATABASE` (MDB/DBX/DBM) |
| Contenuto | Valori correnti, allarmi, retain | Definizione oggetti, tipi, gruppi, tag |
| Cambia | A ogni polling | Solo se lo modifichi |
| Letto | Sempre | All'avvio |

## Aree interne (SAL.INI)
DATA (runtime), DAST/DACFG/AAST/AACFG (allarmi, interfacce ALMDA/ALMAA, soglie LOLO/LO/HI/HIHI), RETA (retain). Sono aree di scambio e replica ([[NETCOMM MRM e Failover]]). Come si incastrano con le tre tabelle: da verificare.

## Più DIT
Ogni sorgente dati ha il suo DIT; TCAE, STANDBY e SSL valgono per singolo DIT; distribuiscono il carico.
