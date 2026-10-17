---
tags: [mpev, flusso-dati]
---
# Viaggio di un dato

Torna a [[00 - MPeV MOC]].

Sensore → PLC → OPC server → driver → DIT → TCAE → uscite.

1-2. Sensore e PLC: il PLC scrive la misura in un registro Modbus.
3. **OPC server** (del costruttore): traduce il protocollo PLC in OPC. Può stare nel PLC, su PC separato o sul server MPeV. Non esiste con driver MBD ([[Driver OPC MBD SIM]]).
4. **Driver SEASXOPCC**: polling ciclico dell'OPC server.
5. **[[DIT]]**: l'elemento collegato al registro riceve il valore grezzo. Flusso valido anche in scrittura (setpoint/uscite).
6. **[[TCAE]]**: scalatura e soglie.
7. Uscite: [[DVW]] (operatori), [[AEC e AEV]] (allarmi), [[OPCS]] (sistemi terzi).

Esempio (valori ipotetici): registro 385 → TCAE scala a 38,5 °C → soglia 35 °C superata → allarme. Vedi [[Esempi completi]].

Nota: il DIT non contiene "la temperatura" ma elementi collegati a registri; il significato lo dà l'oggetto.
