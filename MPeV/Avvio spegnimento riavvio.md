---
tags: [mpev, operativita]
---
# Avvio, spegnimento, riavvio

Torna a [[00 - MPeV MOC]].

## Avvio (lettura logica, non procedura ufficiale)
1. [[SAL]] legge SAL.INI e database.
2. Avvia driver, [[TCAE]], app, NETCOMM, [[OPCS]].
3. NETCOMM collega, l'MRM assegna i ruoli ([[NETCOMM MRM e Failover]]).
4. I driver fanno polling e riempiono il [[DIT]]; TCAE e allarmi lavorano.

## Spegnimento
**Mai** killare dal Task Manager né togliere alimentazione. Usa Shutdown MPeV (CTRL+F10, [[MPC e Manage]]; in guida impianto "CLOSE MPEV"). Attendi chiusura di grafica, driver, servizi memoria; poi riavvia.

## Quando riavviare
| Modifica | Effetto |
|---|---|
| Parametri driver | Letti al riavvio del driver |
| Strutture complesse/oggetti nuovi | Riavvio intero marine portal |
| Componente che non risponde | Prima Restart/Re-Initialize ([[Component e Apps Monitoring]]) |
