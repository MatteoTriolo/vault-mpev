---
tags: [mpev, esempi]
---
# Esempi completi (valori ipotetici)

Torna a [[00 - MPeV MOC]]. Flusso generale: [[Viaggio di un dato]].

## A · Allarme alta temperatura
Sonda 38,5 °C → PLC scrive 385 → OPC server → driver aggiorna elemento SONDA in [[DIT]] → [[TCAE]] scala e valuta t > 35 → vero → [[AEC e AEV]] e [[DVW]] → Master copia sullo Slave ([[NETCOMM MRM e Failover]]) → [[OPCS]] per SCADA esterni.

## B · Cambio setpoint
Da [[DVW]] setpoint Zona 1 = 24 °C → cambia elemento SETPOINT → driver scrive nel registro PLC ([[Driver OPC MBD SIM]]) → PLC regola.

## C · Guasto Master
Vedi failover in [[NETCOMM MRM e Failover]]. Dopo riparazione Server 1 rientra Slave.

## D · Luminosità monitor
Tag value = 70, qualità GOOD → [[TMC]] "Set Brightness to 70".

## E · Aggiungere un PLC
Oggetti/elementi nel database; stesso OPC server → stesso DIT; OPC server proprio → nuovo driver (SAL.INI) e DIT. Modifiche lette solo all'avvio: Shutdown e riavvio ([[Avvio spegnimento riavvio]]). Verifica driver Running ([[Component e Apps Monitoring]]) e qualità in Object View ([[MPC e Manage]]).
