---
title: atmadm
description: 'Argomento dei comandi di Windows per **atmadm** : monitora le connessioni e gli indirizzi registrati da ATM Call Manager in una rete di modalità di trasferimento asincrona (ATM).'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: fdb0070063f2bfc6be7b762d71e2f30216f9d8d0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382634"
---
# <a name="atmadm"></a>atmadm

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il monitoraggio delle connessioni e degli indirizzi registrati da Gestione chiamate atM in una rete atM (Asynchronous Transfer Mode). È possibile utilizzare **atmadm** per visualizzare le statistiche per le chiamate in ingresso e in uscita sugli adapter atM. Usato senza parametri, **atmadm** Visualizza le statistiche per il monitoraggio dello stato delle connessioni atM attive. 

## <a name="syntax"></a>Sintassi

```
atmadm [/c][/a][/s]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------|--------|
|/c|Visualizza le informazioni sulle chiamate per tutte le connessioni correnti alla scheda di rete atM installata nel computer.|
|/a|Consente di visualizzare l'indirizzo del punto di accesso al servizio di rete atM (NSAP) registrato per ogni scheda installata nel computer.|
|/s|Visualizza le statistiche per il monitoraggio dello stato delle connessioni atM attive.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

- Il comando **atmadm/c** produce un output simile al seguente:

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
    
    La tabella seguente contiene le descrizioni di ogni elemento nell'output di esempio **atmadm/c** .
    
    |tipo di dati|Visualizzazione schermo|Descrizione|
    |--------|---------|--------|
    |Informazioni di connessione|In/Out|direzione della chiamata.  **In** è alla scheda di rete atM da un altro dispositivo.  **Out** è dalla scheda di rete atM a un altro dispositivo.|
    ||PMP|Chiamata da punto a MultiPoint.|
    ||P-P|Chiamata da punto a punto.|
    ||SVC|La connessione si trova in un circuito virtuale a commutazione.|
    ||PVC|La connessione si trova in un circuito virtuale permanente.|
    |Informazioni su VPI/VCI|VPI/VCI|Percorso virtuale e canale virtuale della chiamata in ingresso o in uscita.|
    |Indirizzo remoto/parametri multimediali|47000580FFE1000000F21A2E180000C110081500|Indirizzo NSAP del dispositivo atM chiamante **(in)** o chiamato **(out)** .|
    ||**TX**|Il parametro **TX** include i tre elementi seguenti:<br /><br />-Tipo di velocità in bit predefinito o specificato (UBR, CBR, VBR o ABR)<br />-Velocità della riga predefinita o specificata<br />-Dimensioni SDU (Service Data Unit) specificate|
    ||**RX**|Il parametro **RX** include i tre elementi seguenti:<br /><br />-Tipo di velocità in bit predefinito o specificato (UBR, CBR, VBR o ABR)<br />-Velocità della riga predefinita o specificata<br />-Dimensioni SDU specificate|
    
- Il comando **atmadm/a** produce un output simile al seguente:

    ```
    Windows atM call Manager Statistics
    atM addresses for Interface : [009] Olicom atM PCI 155 Adapter
    47000580FFE1000000F21A2E180000C110081500
    ```
    
- Il comando **atmadm/s** genera un output simile al seguente:

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
    
    La tabella seguente contiene le descrizioni di ogni elemento nell'output di esempio **atmadm/s** .
    
    |Statistica di Call Manager|Descrizione|
    |-------------|--------|
    |Chiamate attive correnti|chiama attualmente attivo sulla scheda atM installata nel computer.|
    |Totale chiamate in ingresso riuscite|chiamate ricevute da altri dispositivi in questa rete atM.|
    |Totale chiamate in uscita riuscite|le chiamate sono state completate in altri dispositivi atM in questa rete da questo computer.|
    |Chiamate in ingresso non riuscite|Chiamate in ingresso che non sono in grado di connettersi al computer.|
    |Chiamate in uscita non riuscite|Chiamate in uscita che non sono in grado di connettersi a un altro dispositivo sulla rete.|
    |Chiamate chiuse da remoto|chiama chiuse da un dispositivo remoto sulla rete.|
    |Chiamate chiuse localmente|Chiamate chiuse dal computer.|
    |Segnalazione e pacchetti ILMI inviati|Numero di pacchetti ILMI (Integrated Local Management Interface) inviati allo switch a cui questo computer sta tentando di connettersi.|
    |Segnalazione e pacchetti ILMI ricevuti|Numero di pacchetti ILMI ricevuti dall'opzione atM.|
    
## <a name="BKMK_Examples"></a>Esempi

Per visualizzare le informazioni sulle chiamate per tutte le connessioni correnti alla scheda di rete atM installata nel computer, digitare:

```
atmadm /c
```

Per visualizzare l'indirizzo del punto di accesso del servizio di rete atM (NSAP) registrato per ogni scheda installata nel computer, digitare:

```
atmadm /a
```

Per visualizzare le statistiche per il monitoraggio dello stato delle connessioni atM attive, digitare:

```
atmadm /s
```

## <a name="additional-references"></a>Altri riferimenti

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
