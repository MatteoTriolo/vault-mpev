---
tags: [rete, dhcp, protocolli, configurazione]
---
# DHCP e DHCP server

Collegamenti: [[00 - MPeV MOC]] · [[Protocolli PLC e UDP]] · [[Driver OPC MBD SIM]] · [[NETCOMM MRM e Failover]] · [[Glossario MPeV]]

## 1. Cosa significa la sigla
**D-H-C-P = Dynamic Host Configuration Protocol.**

| Sillaba | Significato | Perché |
|---|---|---|
| **Dynamic** | Dinamico | Gli indirizzi si assegnano al momento, non sono scritti a mano una volta per tutte |
| **Host** | Qualsiasi dispositivo in rete | PC, PLC, telefono, stampante, server: tutto ciò che ha un IP |
| **Configuration** | Configurazione | Non dà solo l'IP: dà anche maschera, gateway, DNS e altri parametri |
| **Protocol** | Protocollo | Insieme di regole e messaggi con cui client e server si parlano |

In una frase: un protocollo che **configura automaticamente** i dispositivi in rete.

## 2. A cosa serve
Senza DHCP ogni dispositivo va configurato a mano (IP statico): lento ed esposto a errori (due dispositivi con lo stesso IP = conflitto). Con DHCP un **server** tiene un elenco di indirizzi disponibili e li presta ai **client** che si collegano.

Cosa consegna al client:
- indirizzo IP
- maschera di sottorete
- gateway predefinito (router)
- server DNS
- durata del prestito (**lease**)
- altri parametri opzionali (NTP, dominio, server di boot...)

## 3. Come funziona: DORA
Il client appena acceso non ha IP e non sa chi sia il server, quindi parla in **broadcast**. Lo scambio ha quattro messaggi:

```
Client (0.0.0.0)                    Server DHCP
   |--- 1. DISCOVER (broadcast) ------->|   "C'è un server DHCP?"
   |<-- 2. OFFER ----------------------|   "Ti propongo 192.168.33.101"
   |--- 3. REQUEST (broadcast) -------->|   "Accetto quell'indirizzo"
   |<-- 4. ACK ------------------------|   "Confermato, lease 8 ore"
```

**D**iscover, **O**ffer, **R**equest, **A**cknowledge.

- Il REQUEST è in broadcast anche perché, se rispondono più server, avvisa gli altri che l'offerta è stata scelta.
- Altri messaggi: **NAK** (il server rifiuta), **DECLINE** (il client scopre un conflitto e rifiuta), **RELEASE** (il client restituisce l'IP), **INFORM** (il client ha già un IP e chiede solo i parametri).

### Trasporto
DHCP usa **UDP** (vedi [[Protocolli PLC e UDP]]): porta **67** lato server, porta **68** lato client. Il client usa origine 0.0.0.0 e destinazione 255.255.255.255 finché non ha un IP.

### Lease e rinnovo
L'indirizzo è un prestito a tempo.
- A **T1 = 50%** della durata il client chiede il rinnovo direttamente al suo server (unicast).
- A **T2 = 87,5%**, se il server non risponde, chiede a qualunque server (broadcast).
- Scaduto il lease senza rinnovo, il client deve smettere di usare l'IP.

### Relay agent
Il broadcast non attraversa i router. Per usare **un server DHCP centrale per più VLAN/sottoreti** si configura un **relay agent** (su router o switch L3, "ip helper-address") che inoltra le richieste in unicast al server, aggiungendo il proprio indirizzo nel campo *giaddr* così il server sa da quale sottorete arriva la richiesta.

## 4. Il DHCP server: concetti di configurazione
| Concetto | Significato |
|---|---|
| **Scope / pool** | Intervallo di indirizzi distribuibili (es. .100-.199) |
| **Maschera, gateway, DNS** | Opzioni consegnate al client |
| **Lease time** | Durata del prestito |
| **Esclusioni** | Indirizzi dentro lo scope che il server non deve dare mai |
| **Prenotazioni (reservation)** | Associa un MAC a un IP fisso: il dispositivo riceve sempre lo stesso indirizzo |
| **Opzioni** | Numeri standard: 1 maschera, 3 router, 6 DNS, 15 dominio, 42 NTP, 51 lease, 53 tipo messaggio, 54 id server |

### Regole di buona progettazione
- Dimensiona lo scope con margine (non al 100%).
- Tieni **fuori dal pool** (o escludi) gli indirizzi statici: server, gateway, stampanti.
- Per dispositivi che devono avere sempre lo stesso IP usa le **prenotazioni** invece di IP statici sparsi.
- Su una stessa rete **un solo server DHCP** attivo, altrimenti due server danno indirizzi diversi (DHCP "rogue"). Per la ridondanza usa la modalità failover prevista dal server.
- Lease lunghi (giorni) per reti stabili, brevi (ore) per reti con molti ospiti.

### Esempio 1: ISC dhcpd (`dhcpd.conf`)
Nota: ISC DHCP ha terminato la manutenzione (2022); il successore dello stesso progetto è **Kea**.
```
subnet 192.168.33.0 netmask 255.255.255.0 {
  range 192.168.33.100 192.168.33.199;
  option routers 192.168.33.1;
  option domain-name-servers 192.168.33.1;
  default-lease-time 86400;     # 1 giorno
  max-lease-time 172800;        # 2 giorni
}
host plc1 {
  hardware ethernet 00:11:22:33:44:55;
  fixed-address 192.168.33.226;
}
```

### Esempio 2: dnsmasq (`dnsmasq.conf`, leggero, comune su router/Linux)
```
dhcp-range=192.168.33.100,192.168.33.199,255.255.255.0,12h
dhcp-option=option:router,192.168.33.1
dhcp-option=option:dns-server,192.168.33.1
dhcp-host=00:11:22:33:44:55,plc1,192.168.33.226
```

### Esempio 3: Windows Server (PowerShell)
```powershell
Install-WindowsFeature DHCP -IncludeManagementTools
Add-DhcpServerv4Scope -Name "HVAC" -StartRange 192.168.33.100 -EndRange 192.168.33.199 `
  -SubnetMask 255.255.255.0 -LeaseDuration 8.00:00:00
Set-DhcpServerv4OptionValue -ScopeId 192.168.33.0 -Router 192.168.33.1 -DnsServer 192.168.33.1
Add-DhcpServerv4ExclusionRange -ScopeId 192.168.33.0 -StartRange 192.168.33.100 -EndRange 192.168.33.109
Add-DhcpServerv4Reservation -ScopeId 192.168.33.0 -IPAddress 192.168.33.226 `
  -ClientId "00-11-22-33-44-55" -Name "PLC1"
```
Su Windows il server in dominio va anche **autorizzato** in Active Directory.

## 5. Diagnosi
| Sintomo | Causa probabile |
|---|---|
| IP 169.254.x.x (APIPA) | Il client non ha ricevuto risposta DHCP |
| Due IP diversi in rete / gateway sbagliato | Server DHCP non autorizzato (rogue) |
| Client in un'altra VLAN senza IP | Manca il relay agent |
| "Conflict detected" | Un IP statico cade dentro lo scope |
| Il dispositivo cambia IP al riavvio | Manca la prenotazione o il lease è scaduto |

Strumenti: Wireshark con filtro `bootp` (o `udp.port == 67 || udp.port == 68`) per vedere DORA; log del server; tabella dei lease.

## 6. Collegamento con l'impianto MPeV (mia lettura, da verificare)
- Nella guida i dispositivi Modbus del DIT 11 sono raggiunti per **indirizzo IP** (`MBD11.ini`, es. 192.168.33.226/227, porta 502, vedi [[Driver OPC MBD SIM]]). Se un PLC ottenesse un IP diverso da DHCP, il driver non lo troverebbe più. Per PLC e server MPeV conviene IP statico o prenotazione DHCP.
- L'**IP virtuale** dei server ridondanti ([[NETCOMM MRM e Failover]]) è un indirizzo gestito dal cluster: va tenuto fuori dallo scope DHCP.
- La guida MPeV non tratta DHCP: sul tuo impianto va verificato se esiste un server DHCP e con quale scope.

## Da approfondire (note future)
[[DNS]] · [[ARP]] · [[VLAN e relay]] · [[Indirizzamento IP e sottoreti]] · [[Kea DHCP]]
