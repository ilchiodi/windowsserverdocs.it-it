---
title: Rete ad alte prestazioni
description: In questo argomento viene fornita una panoramica delle tecnologie di ottimizzazione in Windows Server 2016 e l'Offload e include collegamenti a indicazioni aggiuntive su queste tecnologie.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 09e18a41452baa0add8e055ceb22d2f5c2ad886e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815832"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Funzionalità e tecnologie per solo hardware (HO, Hardware Only)

Le accelerazioni hardware migliorare le prestazioni di rete in combinazione con il software ma non sono strettamente parte di qualsiasi funzionalità del software. Esempi includono regolazione di Interrupt, controllo di flusso e Receive-side dell'Offload Checksum IPv4.

>[!TIP]
>SH e HO funzionalità sono disponibili se supporta l'interfaccia di rete installata. Le descrizioni delle funzionalità seguente illustra le procedure indicare se la scheda di rete supporta la funzionalità.

### <a name="address-checksum-offload"></a>Offload Checksum indirizzo

Offload checksum indirizzo sono una funzionalità di interfaccia di rete che trasferisce il carico di calcolo del checksum di indirizzo (IP, TCP, UDP) per l'hardware di interfaccia di rete per inviare e ricevere.

Nel percorso di ricezione, l'offload checksum calcola i checksum nelle intestazioni IP TCP e UDP (se necessario) e indica al sistema operativo se i checksum passano, non è riuscita o non controllata. Se l'interfaccia di rete indica che i checksum siano validi, che il sistema operativo accetta il pacchetto non verificato. Se l'interfaccia di rete asserzioni che sono il checksum non valido o non verificato, lo stack IP/TCP/UDP internamente calcola anche in questo caso i valori di checksum. Se il valore di checksum calcolato non riesce, il pacchetto Ottiene eliminato.

Nel percorso di invio, l'offload checksum calcola e inserisce i valori di checksum nell'intestazione IP, TCP o UDP come appropriato.

La disabilitazione di Offload checksum nel percorso di invio non disabilita l'inserimento per i pacchetti inviati al driver miniport usando la funzionalità di grandi dimensioni Offload invio (LSO) e calcolo del checksum.  Per disabilitare tutti i calcoli di offload checksum, l'utente deve anche disabilitare LSO.

_**Gestire gli offload Checksum indirizzo**_

Nelle proprietà avanzate sono disponibili diverse proprietà distinte:

-   IPv4 Checksum Offload

-   TCP Checksum Offload (IPv4)

-   Offload Checksum TCP (IPv6)

-   Offload Checksum UDP (IPv4)

-   Offload Checksum UDP (IPv6)

Per impostazione predefinita, questi sono tutti sempre abilitati. È consigliabile abilitare sempre tutti questi Offload.

L'offload Checksum possono essere gestite tramite i cmdlet Enable-NetAdapterChecksumOffload e Disable-NetAdapterChecksumOffload. Ad esempio, il cmdlet seguente Abilita il TCP (IPv4) e il calcolo del checksum UDP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Suggerimenti sull'uso di offload Checksum indirizzo**_

Offload Checksum indirizzo deve essere sempre abilitato indipendentemente dal carico di lavoro o circostanza. Ciò offload basic la maggior parte di tutte le tecnologie sempre servirà a migliorare le prestazioni di rete. Offload checksum è necessaria anche per altri offload senza stato usare inclusi receive-side scaling (RSS), ricevere segmento coalescing (RSC) e l'offload di invio di grandi dimensioni (LSO).

### <a name="interrupt-moderation-im"></a>(IM) regolazione di interrupt

Messaggistica immediata di memorizza nel buffer più pacchetti ricevuti prima di interrompere il sistema operativo. Quando una scheda di rete riceve un pacchetto, viene avviato un timer. Quando il buffer è pieno o il timer scade, a seconda del valore raggiunto per primo, l'interfaccia di rete consente di interrompere il sistema operativo. 

Molte schede di rete supporta più di solo attivazione/disattivazione per la regolazione di Interrupt. La maggior parte delle interfacce di rete supporta i concetti di un basso, medio e ad alta velocità per la messaggistica immediata. I vari tassi di rappresentano i timer di brevi o più e sulle regolazioni per dimensioni del buffer appropriata per ridurre la latenza (regolazione di interrupt bassa) o ridurre gli interrupt (regolazione di interrupt alta).

È presente un saldo per essere equilibrio tra riducendo gli interrupt e ritardare eccessivamente recapito ordinato dei pacchetti. In generale, elaborazione dei pacchetti è più efficiente con regolazione di Interrupt abilitata. Applicazioni a bassa latenza o ad alte prestazioni potrebbe essere necessario valutare l'impatto della disabilitazione o la riduzione di regolazione di Interrupt.

### <a name="jumbo-frames"></a>Frame jumbo

I frame Jumbo è una funzionalità di interfaccia di rete e di rete che consente a un'applicazione per l'invio di frame che sono molto più grandi rispetto a quello predefinito 1500 byte. In genere il limite per i frame jumbo è circa 9000 byte, ma potrebbe essere più piccolo.

Non sono previste modifiche al supporto di frame jumbo in Windows Server 2012 R2.

In Windows Server 2016 che è un nuovo offload: MTU_for_HNV. Questo nuovo offload funziona con le impostazioni di Frame jumbo nell'ambito per verificare che il traffico incapsulato non richiede la segmentazione tra l'host e il commutatore adiacente. Questa nuova funzionalità dello stack SDN con l'interfaccia di rete calcolare automaticamente quali MTU per pubblicizzare e quali MTU da usare durante la trasmissione. Questi valori per il valore MTU variano se qualsiasi offload HNV è in uso. (Della tabella di compatibilità delle funzionalità, nella tabella 1, MTU_for_HNV avrà le stesse interazioni come gli Offload di HNVv2 offload hanno in quanto è direttamente correlato il HNVv2.)

### <a name="large-send-offload-lso"></a>LSO (Large Send Offload)

LSO consente a un'applicazione passare un grande blocco di dati per l'interfaccia di rete e le interruzioni di interfaccia di rete i dati in pacchetti che rientrano all'interno di unità di MTU (Maximum Transfer) di rete.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

Unione segmenti Receive, noto anche come grandi ricezione Offload è una funzionalità di interfaccia di rete che richiede i pacchetti che fanno parte dello stesso flusso che arriva tra interruzioni di rete e li assegna un singolo pacchetto prima di inviarli al sistema operativo. RSC non è disponibile nelle schede NIC che sono associate al commutatore virtuale Hyper-V. Per altre informazioni, vedere [Receive segmento Coalescing (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).