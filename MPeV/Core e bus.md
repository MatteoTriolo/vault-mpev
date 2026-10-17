---
tags: [mpev, componente]
---
# Core e bus interno

Torna a [[00 - MPeV MOC]].

- Il Core gestisce la comunicazione tra i componenti sul **Core BUS**.
- Ogni componente parla col Core tramite una classe di interfaccia (es. NETCOMM: "MPeV Core Class IF").
- Per questo i componenti si avviano/fermano/riavviano in modo indipendente ([[Component e Apps Monitoring]]).
- Contiene la memoria condivisa [[DIT]]. Avviato dal [[SAL]].
