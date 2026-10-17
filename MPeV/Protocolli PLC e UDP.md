---
tags: [plc, protocolli, rete, udp]
---
# Protocolli dei PLC e UDP

Torna a [[00 - MPeV MOC]]. Contesto: [[Driver OPC MBD SIM]], [[NETCOMM MRM e Failover]], [[Glossario MPeV]].

## 1. Livelli: dove sta cosa
| Livello | Esempi |
|---|---|
| Fisico / collegamento | RS-485, Ethernet, cavo UTP (è solo il cavo, non un protocollo) |
| Rete | IP |
| **Trasporto** | **TCP, UDP** |
| Applicazione | Modbus, EtherNet/IP, OPC, BACnet, S7comm, SNMP |

Il protocollo PLC (applicazione) viaggia sopra TCP o UDP, che viaggiano sopra IP.

## 2. Cos'è UDP
**User Datagram Protocol**: protocollo di trasporto *senza connessione*.
- Nessuna stretta di mano: si spedisce e basta.
- Nessuna conferma di ricezione, nessuna ritrasmissione.
- Nessun ordinamento garantito: i pacchetti possono arrivare invertiti, duplicati o non arrivare.
- Pochissimo overhead → bassa latenza.
- Supporta broadcast e multicast.

### TCP vs UDP
| | TCP | UDP |
|---|---|---|
| Connessione | Sì (handshake) | No |
| Affidabilità | Garantita (ack, ritrasmissioni) | Nessuna |
| Ordine | Garantito | Non garantito |
| Unità dati | Flusso di byte | Datagrammi separati |
| Overhead | Maggiore | Minimo (header 8 byte) |
| Uso tipico | Configurazione, file, comandi critici | Dati ciclici, tempo reale, discovery, log |

## 3. Cos'è un "messaggio UDP" (datagramma)
Un pacchetto autonomo: **header di 8 byte + payload**.

```
 0        16        32
 +---------+---------+
 | porta   | porta   |
 | origine | destin. |
 +---------+---------+
 |lunghezza|checksum |
 +---------+---------+
 |   payload (dati)  |
 +-------------------+
```
- **Porta origine / destinazione** (16 bit ciascuna): identificano le applicazioni.
- **Lunghezza**: header + dati.
- **Checksum**: rileva dati corrotti (opzionale in IPv4).
- **Payload**: qui sta il messaggio del protocollo applicativo (es. i valori di un PLC).

Proprietà chiave: **un invio = un datagramma = una ricezione**. I confini del messaggio si conservano (con TCP no, è un flusso e li devi gestire tu). Payload massimo teorico in IPv4: 65507 byte; nella pratica si resta sotto la MTU (~1472 byte su Ethernet) per evitare frammentazione.

## 4. Se perdi un messaggio?
UDP non lo dice: l'affidabilità si costruisce nell'applicazione.
- Numero di sequenza nel payload.
- Timeout e ritrasmissione applicativa.
- Heartbeat periodico: se non arriva per N cicli, si considera il nodo perso (idea simile al heartbeat Master/Slave in [[NETCOMM MRM e Failover]]).
- Per dati ciclici la perdita di un pacchetto conta poco: il prossimo arriva dopo pochi ms.

## 5. Protocolli PLC/automazione e trasporto
| Protocollo | Trasporto | Porta |
|---|---|---|
| Modbus TCP (usato dal driver MBD, [[Driver OPC MBD SIM]]) | TCP | 502 |
| Modbus RTU | seriale RS-485 | – |
| EtherNet/IP, I/O ciclico (implicito) | UDP | 2222 |
| EtherNet/IP, messaggi espliciti | TCP (e UDP) | 44818 |
| S7comm (Siemens) | TCP | 102 |
| OPC UA (client/server) | TCP | 4840 |
| BACnet/IP (HVAC) | UDP | 47808 |
| SNMP | UDP | 161 |
| NTP | UDP | 123 |
| PROFINET RT | Ethernet diretto (livello 2), non UDP | – |
| OPC DA Classic (usato da OPCC/OPCS, [[Driver OPC MBD SIM]]) | DCOM su TCP | dinamica |

Regola pratica: configurazione e comandi → TCP; dati ciclici e discovery → UDP.

## 6. Esempio (Python, valori ipotetici)
```python
import socket
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)   # UDP
s.sendto(b"TEMP=38.5", ("192.168.33.10", 5000))        # un datagramma
data, addr = s.recvfrom(1500)                          # una ricezione = un datagramma
```
Nessun `connect`/`accept`: è il segno della natura senza connessione.

## 7. Diagnosi con Wireshark
Filtro `udp.port == 47808` (esempio BACnet). Controlla: arrivano i datagrammi? Lunghezza e checksum corretti? Intervallo regolare? Perdite = buchi nei numeri di sequenza applicativi.

## Da approfondire (note future)
[[Modbus]] · [[EtherNet-IP]] · [[BACnet]] · [[TCP vs UDP]] · [[OPC DA e DCOM]]

## Da verificare
- Nell'esempio dell'help MPeV compare un componente "IPMSUDPGW" (Stopped): dal nome sembra un gateway UDP, ma la guida non lo descrive.
- Porte e protocolli sopra: da conoscenza generale, non dalla guida MPeV; confermare sui dispositivi reali dell'impianto.
