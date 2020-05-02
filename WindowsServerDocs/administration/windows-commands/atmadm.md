---
title: atmadm
description: Argomento di riferimento per il comando atmadm, che consente di monitorare le connessioni e gli indirizzi registrati da atM Call Manager in una rete atM (Asynchronous Transfer Mode).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 37156c2e-c4d4-4fd8-a03d-245fb60bf996
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32dad00e5a4d03c905f95c48e112f512a9dbc2e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718913"
---
# <a name="atmadm"></a>atmadm

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il monitoraggio delle connessioni e degli indirizzi registrati da Gestione chiamate atM in una rete atM (Asynchronous Transfer Mode). È possibile utilizzare **atmadm** per visualizzare le statistiche per le chiamate in ingresso e in uscita sugli adapter atM. Usato senza parametri, **atmadm** Visualizza le statistiche per il monitoraggio dello stato delle connessioni atM attive.

## <a name="syntax"></a>Sintassi

```
atmadm [/c][/a][/s]
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| /C | Visualizza le informazioni sulle chiamate per tutte le connessioni correnti alla scheda di rete atM installata nel computer. |
| /a | Consente di visualizzare l'indirizzo del punto di accesso al servizio di rete atM (NSAP) registrato per ogni scheda installata nel computer. |
| /s | Visualizza le statistiche per il monitoraggio dello stato delle connessioni atM attive. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="remarks"></a>Osservazioni

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

    | Tipo di dati | Visualizzazione schermo | Descrizione |
    | -------- | --------- | -------- |
    | Informazioni di connessione | Ingresso/Uscita | Direzione della chiamata. **In** è alla scheda di rete atM da un altro dispositivo.  **Out** è dalla scheda di rete atM a un altro dispositivo. |
    | PMP | Chiamata da punto a MultiPoint. |
    | P-P | Chiamata da punto a punto. |
    | SVC | La connessione si trova in un circuito virtuale a commutazione. |
    | PVC | La connessione si trova in un circuito virtuale permanente. |
    | Informazioni su VPI/VCI | VPI/VCI | Percorso virtuale e canale virtuale della chiamata in ingresso o in uscita. |
    | Indirizzo remoto/parametri multimediali | 47000580FFE1000000F21A2E180000C110081500 | Indirizzo NSAP del dispositivo atM chiamante **(in)** o chiamato **(out)** . |
    | TX | Il parametro **TX** include i tre elementi seguenti:<ul><li>Tipo di velocità in bit predefinito o specificato (UBR, CBR, VBR o ABR)</li><li>Velocità riga predefinita o specificata</li><li>Dimensioni SDU (Service Data Unit) specificate.</li></ul> |
    | Rx | Il parametro **RX** include i tre elementi seguenti:<ul><li>Tipo di velocità in bit predefinito o specificato (UBR, CBR, VBR o ABR)</li><li>Velocità riga predefinita o specificata</li><li>Dimensioni SDU specificate.</li></ul> |

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

    | Statistica di Call Manager | Descrizione |
    | ------------- | -------- |
    | Chiamate attive correnti | Chiama attualmente attivo sulla scheda atM installata nel computer. |
    | Totale chiamate in ingresso riuscite | Chiamate ricevute da altri dispositivi in questa rete atM. |
    | Totale chiamate in uscita riuscite | Le chiamate sono state completate in altri dispositivi atM in questa rete da questo computer. |
    | Chiamate in ingresso non riuscite | Chiamate in ingresso che non sono in grado di connettersi al computer. |
    | Chiamate in uscita non riuscite | Chiamate in uscita che non sono in grado di connettersi a un altro dispositivo sulla rete. |
    | Chiamate chiuse da remoto | Chiama chiuse da un dispositivo remoto sulla rete. |
    | Chiamate chiuse localmente | Chiamate chiuse dal computer. |
    | Segnalazione e pacchetti ILMI inviati | Numero di pacchetti ILMI (Integrated Local Management Interface) inviati allo switch a cui questo computer sta tentando di connettersi. |
    | Segnalazione e pacchetti ILMI ricevuti | Numero di pacchetti ILMI ricevuti dall'opzione atM. |

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
