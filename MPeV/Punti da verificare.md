---
tags: [mpev, da-verificare]
---
# Punti da verificare sull'impianto

Torna a [[00 - MPeV MOC]].

- [[DIT]]: tre tabelle (oggetti/elementi/gruppi) vs sei aree (DATA, DAST, DACFG, AAST, AACFG, RETA): come si incastrano?
- Gruppi: raggruppano oggetti? (lettura mia)
- [[Driver OPC MBD SIM]]: MBD = driver Modbus e MBDn.ini sua config (lettura dai nomi).
- Flag `isSim = True` nel SAL.INI del DIT 11 (riga tagliata).
- [[NETCOMM MRM e Failover]]: DIT 1 STANDBY, DIT 0 e 11 SERVER: cosa decide la scelta?
- [[AEC e AEV]]: guida impianto "Alarm Event Calculator/Visualizer" vs help "Collector/Viewer" (uso l'help).
- Comando chiusura: "CLOSE MPEV" (guida) vs "Shutdown MPeV" (MPC), probabilmente lo stesso ([[Avvio spegnimento riavvio]]).
- VIP, heartbeat, data mirroring: dalla guida impianto, non dall'help.
- Sigle DAST/DACFG/AAST/AACFG: lettura mia, plausibile da SAL.INI.
- [[TCAE]]: nomi funzioni script sono segnaposto.
- Pagine non viste: MRM, HUB, Manage; coda di opcs.ini ([[OPCS]], UNCISGOOD tagliato).
