---
title: Rete ad alte prestazioni
description: In questo argomento viene fornita una panoramica delle tecnologie di offload e ottimizzazione di Windows Server 2016 e sono inclusi collegamenti a ulteriori indicazioni su tali tecnologie.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 5cf6a5057151c696bc1c29a1dcf6fc18c776605a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405751"
---
## <a name="hardware-only-ho-features-and-technologies"></a>Funzionalità e tecnologie per solo hardware (HO, Hardware Only)

Queste accelerazioni hardware migliorano le prestazioni di rete insieme al software, ma non fanno parte di alcuna funzionalità software. Alcuni esempi includono la moderazione di interrupt, il controllo di flusso e l'offload di checksum IPv4 sul lato di ricezione.

>[!TIP]
>Le funzionalità SH e HO sono disponibili se la scheda di interfaccia di rete installata la supporta. Le descrizioni delle funzionalità seguenti illustrano come stabilire se la scheda di interfaccia di rete supporta la funzionalità.

### <a name="address-checksum-offload"></a>Offload di checksum degli indirizzi

Gli offload di checksum degli indirizzi sono una funzionalità NIC che esegue l'offload del calcolo dei checksum degli indirizzi (IP, TCP, UDP) nell'hardware della scheda di interfaccia di rete per l'invio e la ricezione.

Nel percorso di ricezione, l'offload di checksum calcola i checksum nelle intestazioni IP, TCP e UDP (a seconda dei casi) e indica al sistema operativo se i checksum sono stati superati, non superati o non controllati. Se la scheda di interfaccia di rete dichiara che i checksum sono validi, il sistema operativo accetta il pacchetto in questione. Se la scheda di interfaccia di rete dichiara che i checksum non sono validi o non sono controllati, lo stack IP/TCP/UDP calcola internamente i checksum. Se il checksum calcolato ha esito negativo, il pacchetto viene ignorato.

Nel percorso di trasmissione, l'offload di checksum calcola e inserisce i checksum nell'intestazione IP, TCP o UDP, a seconda dei casi.

La disabilitazione degli offload di checksum sul percorso di trasmissione non disabilita il calcolo e l'inserimento di checksum per i pacchetti inviati al driver miniport utilizzando la funzionalità di offload di invio (LSO).  Per disabilitare tutti i calcoli di offload di checksum, l'utente deve anche disabilitare LSO.

_**Gestisci offload di checksum degli indirizzi**_

Nelle proprietà avanzate sono disponibili diverse proprietà distinte:

-   Offload Checksum IPv4

-   Offload checksum TCP (IPv4)

-   Offload checksum TCP (IPv6)

-   Offload checksum UDP (IPv4)

-   Offload checksum UDP (IPv6)

Per impostazione predefinita, tutti i valori sono sempre abilitati. È consigliabile abilitare sempre tutti questi offload.

È possibile gestire gli offload di checksum usando i cmdlet Enable-NetAdapterChecksumOffload e Disable-NetAdapterChecksumOffload. Ad esempio, il cmdlet seguente Abilita i calcoli di checksum TCP (IPv4) e UDP (IPv4):

```PowerShell
Enable-NetAdapterChecksumOffload –Name * -TcpIPv4 -UdpIPv4
```

_**Suggerimenti sull'utilizzo degli offload di checksum degli indirizzi**_

Gli offload di checksum degli indirizzi devono essere sempre abilitati indipendentemente dal carico di lavoro o dalle circostanze. Questa più base di tutte le tecnologie di offload migliora sempre le prestazioni della rete. L'offload di checksum è necessario anche per il funzionamento di altri offload senza stato, tra cui Receive-Side Scaling (RSS), Receiver Segment coalesation (RSC) e Large Send Offload (LSO).

### <a name="interrupt-moderation-im"></a>Moderazione interrupt (IM)

IM memorizza nel buffer più pacchetti ricevuti prima di interrompere il sistema operativo. Quando una scheda di interfaccia di rete riceve un pacchetto, viene avviato un timer. Quando il buffer è pieno o il timer scade, a seconda di quale sia il primo, la NIC interrompe il sistema operativo. 

Molte schede di rete supportano più di solo on/off per la moderazione delle interruzioni. La maggior parte delle schede di rete supporta i concetti di bassa, media e alta velocità di messaggistica immediata. Le diverse frequenze rappresentano timer più brevi o più lunghi e regolazioni appropriate per la dimensione del buffer per ridurre la latenza (moderazione di interrupt ridotta) o ridurre gli interrupt (moderazione di interrupt elevata).

Si verifica un equilibrio tra la riduzione degli interrupt e il ritardo eccessivo del recapito del pacchetto. L'elaborazione dei pacchetti è in genere più efficiente con la moderazione di interrupt abilitata. È possibile che le applicazioni ad alte prestazioni o a bassa latenza debbano valutare l'effetto della disabilitazione o della riduzione della moderazione degli interrupt.

### <a name="jumbo-frames"></a>Frame jumbo

I frame Jumbo sono una scheda di interfaccia di rete e una funzionalità di rete che consente a un'applicazione di inviare frame di dimensioni molto maggiori rispetto ai 1500 byte predefiniti. Il limite per i frame Jumbo è in genere pari a circa 9000 byte, ma può essere minore.

Non sono state apportate modifiche al supporto di frame Jumbo in Windows Server 2012 R2.

In Windows Server 2016 è disponibile un nuovo Offload: MTU_for_HNV. Questo nuovo offload funziona con le impostazioni del frame Jumbo per garantire che il traffico incapsulato non richieda la segmentazione tra l'host e l'opzione adiacente. Questa nuova funzionalità dello stack SDN prevede che la scheda di interfaccia di rete calcoli automaticamente la MTU da pubblicizzare e la MTU da usare in rete. Questi valori per MTU sono diversi se è in uso un HNV offload. Nella tabella di compatibilità delle funzionalità della tabella 1, MTU_for_HNV avrà le stesse interazioni degli offload HNVv2, poiché è direttamente correlato al HNVv2 offload.

### <a name="large-send-offload-lso"></a>LSO (Large Send Offload)

LSO consente a un'applicazione di passare un blocco di dati di grandi dimensioni alla scheda di interfaccia di rete e la scheda di interfaccia di rete suddivide i dati in pacchetti che rientrano nell'unità di trasferimento massima (MTU) della rete.

### <a name="receive-segment-coalescing-rsc"></a>Receive Segment Coalescing (RSC)

L'Unione dei segmenti di ricezione, nota anche come offload di ricezione di grandi dimensioni, è una funzionalità NIC che accetta pacchetti che fanno parte dello stesso flusso proveniente da interruzioni di rete e li unisce in un unico pacchetto prima di distribuirli al sistema operativo. RSC non è disponibile sulle schede di rete associate al Commuter virtuale Hyper-V. Per ulteriori informazioni, vedere la pagina relativa all'Unione dei [segmenti Receive (RSC)](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).