---
title: atmadm
description: Argomento i comandi di Windows per **atmadm** -monitorare le connessioni e indirizzi che sono registrati per il atM chiamano Manager in una rete di trasferimento asincrono (atM mode).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a8a8883bad9aa2abdc90d5db5702ef42f46c8a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831772"
---
# <a name="atmadm"></a>atmadm

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Monitorare le connessioni e indirizzi che sono registrati per il atM chiamano Manager in una rete di trasferimento asincrono (atM mode). È possibile usare **atmadm** per visualizzare le statistiche per le chiamate in ingresso e in uscita sulle schede atM. Se utilizzato senza parametri, **atmadm** Visualizza le statistiche per monitorare lo stato delle connessioni attive atM. 

## <a name="syntax"></a>Sintassi

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|/c|Consente di visualizzare informazioni sulle chiamate per tutte le connessioni correnti per la scheda di rete atM installato nel computer.|
|/a|Visualizza l'accesso al servizio di rete registrato atM punti indirizzo (NSAP) per ogni adapter installato nel computer.|
|/s|Visualizza le statistiche per monitorare lo stato delle connessioni attive atM.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Il **/c atmadm** comando genera un output simile al seguente:

    ```
    Windows atM call Manager Statistics
    atM Connections on Interface : [009] Olicom atM PCI 155 Adapter
       Connection   VPI/VCI   remote address/
                              Media Parameters (rates in bytes/sec)
       In  PMP SVC    0/193   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/192   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  PMP SVC    0/191   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       Out P-P SVC    0/190   47000580FFE1000000F21A2E180020481A2E180B
                              Tx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 1516
       In  P-P SVC    0/475   47000580FFE1000000F21A2E180000C110081501
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9188
       Out PMP SVC    0/194   47000580FFE1000000F21A2E180000C110081501 (0)
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9180
                              Rx:UBR,Peak 0,Avg 0,MaxSdu 0
       Out P-P SVC    0/474   4700918100000000613E5BFE010000C110081500
                              Tx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
                              Rx:UBR,Peak 16953984,Avg 16953984,MaxSdu 9188
       In  PMP SVC    0/195   47000580FFE1000000F21A2E180000C110081500
                              Tx:UBR,Peak 0,Avg 0,MaxSdu 0
                              Rx:UBR,Peak 16953936,Avg 16953936,MaxSdu 9180
    ```
    
    Nella tabella seguente contiene le descrizioni di ogni elemento di **/c atmadm** output di esempio.
    
    |tipo di dati|Visualizzazione su schermo|Descrizione|
    |--------|---------|--------|
    |Informazioni di connessione|In/Out|direzione della chiamata.  **In** consiste nella scheda di rete atM da un altro dispositivo.  **Out** è dalla scheda di rete atM su un altro dispositivo.|
    ||PMP|Chiamata di Point-to-multipoint.|
    ||P-P|Chiamata da punto a punto.|
    ||SVC|La connessione è in un circuito virtuale disattivato.|
    ||PVC|La connessione è in un circuito virtuale permanente.|
    |Informazioni VPI/VCI|VPI/VCI|Percorso virtuale e channel virtuale della chiamata in ingresso o in uscita.|
    |indirizzo remoto/file multimediali parametri|47000580FFE1000000F21A2E180000C110081500|Indirizzo NSAP del chiamante **(In)** o chiamato **(Out)** dispositivo atM.|
    ||**Tx**|Il **Tx** parametro include tre elementi seguenti:<br /><br />-Il valore predefinito o specificato tipo di velocità in bit (registro UBR, routing basato sul contenuto, VBR o ABR)<br />-Il valore predefinito o specificato velocità della linea<br />-Dimensioni dell'unità (SDU) dei dati di servizio specificato|
    ||**Rx**|Il **Rx** parametro include tre elementi seguenti:<br /><br />-Il valore predefinito o specificato tipo di velocità in bit (registro UBR, routing basato sul contenuto, VBR o ABR)<br />-Il valore predefinito o specificato velocità della linea<br />-Dimensione SDU specificata|
    
- Il **atmadm /a** comando genera un output simile al seguente:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- Il **/s atmadm** comando genera un output simile al seguente:

    ```
    Windows atM call Manager Statistics
    atM call Manager statistics for Interface : [009] Olicom atM PCI 155 Adapter
    Current active calls                        = 4
    Total successful Incoming calls             = 1332
    Total successful Outgoing calls             = 1297
    Unsuccessful Incoming calls                 = 1
    Unsuccessful Outgoing calls                 = 1
    calls Closed by remote                      = 1302
    calls Closed Locally                        = 1323
    Signaling and ILMI Packets Sent            = 33655
    Signaling and ILMI Packets Received        = 34989
    ```
    
    Nella tabella seguente contiene le descrizioni di ogni elemento di **/s atmadm** output di esempio.
    
    |chiamare Manager statistica|Descrizione|
    |-------------|--------|
    |Chiamate attive correnti|chiama attualmente attiva sulla scheda atM installata nel computer.|
    |Totale chiamate in ingresso|chiamate ricevute correttamente da altri dispositivi su questa rete atM.|
    |Totale chiamate in uscita riuscite|chiamate completate per altri dispositivi atM su questa rete da questo computer.|
    |Le chiamate in ingresso non riuscite|Chiamate in ingresso che non è riuscito a connettersi al computer.|
    |Chiamate in uscita non riuscite|Chiamate in uscita che non è riuscito a connettersi a un altro dispositivo di rete.|
    |chiamate chiuso da remoto|chiamate chiuse da un dispositivo remoto nella rete.|
    |chiama chiuso in locale|chiamate chiuse da questo computer.|
    |I segnali e ILMI pacchetti inviati|Numero di gestione locale integrata interfaccia ILMI pacchetti inviati al commutatore a cui il computer sta provando a connettersi.|
    |I segnali e ILMI pacchetti ricevuti|Numero di pacchetti ILMI ricevuti dal commutatore atM.|
    
## <a name="BKMK_Examples"></a>Esempi

Per visualizzare informazioni sulle chiamate per tutte le connessioni correnti per la scheda di rete atM installata nel computer, digitare:

```
atmadm /c
```

Per visualizzare l'indirizzo atM registrati network service access punto (NSAP) per ogni adapter installato nel computer, digitare:

```
atmadm /a
```

Per visualizzare le statistiche per monitorare lo stato delle connessioni attive atM, digitare:

```
atmadm /s
```

## <a name="additional-references"></a>Altri riferimenti

- [Chiave sintassi della riga di comando](command-line-syntax-key.md)
