---
title: Scelta di una scheda di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875852"
---
# <a name="choosing-a-network-adapter"></a>Scelta di una scheda di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su alcune delle funzionalità delle schede di rete che potrebbero influire sulle scelte di acquisto.

Le applicazioni a elevato utilizzo di rete richiedono le schede di rete ad alte prestazioni. In questa sezione illustra alcune considerazioni per la scelta delle schede di rete, nonché come configurare le impostazioni di scheda di rete diversa per ottenere le migliori prestazioni di rete.

> [!TIP]
>  È possibile configurare le impostazioni della scheda di rete tramite Windows PowerShell. Per altre informazioni, vedere [cmdlet per scheda di rete in Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a> Eseguire l'offload delle funzionalità

L'offload delle attività da unità di elaborazione centrale \(CPU\) alla rete adapter può ridurre l'utilizzo della CPU sul server, che consente di migliorare le prestazioni complessive del sistema.

Lo stack di rete in prodotti Microsoft può scaricare uno o più attività a una scheda di rete se si seleziona una scheda di rete che è appropriato eseguire l'offload delle funzionalità. Nella tabella seguente fornisce una breve panoramica delle funzionalità di offload diversi che sono disponibili in Windows Server 2016.
  
|Tipo di offload|Descrizione|
|------------------|-----------------|  
|Calcolo del checksum per il protocollo TCP|Lo stack di rete può eseguire l'offload di calcolo e la convalida dei Transmission Control Protocol \(TCP\) checksum in inviare e ricevere i percorsi del codice. Può anche scaricare il calcolo e la convalida di IPv4 e IPv6 i valori di checksum su inviare e ricevere i percorsi del codice.|  
|Calcolo del checksum per UDP |Lo stack di rete può eseguire l'offload di calcolo e convalida di User Datagram Protocol \(UDP\) checksum in inviare e ricevere i percorsi del codice.|
|Calcolo del checksum per IPv4 |Lo stack di rete può eseguire l'offload di calcolo e convalida di IPv4, i valori di checksum su inviare e ricevere i percorsi del codice. |
|Calcolo del checksum per IPv6 |Lo stack di rete può eseguire l'offload di calcolo e convalida di IPv6 i valori di checksum su inviare e ricevere i percorsi del codice. | 
|Segmentazione di grandi dimensioni pacchetti TCP|Il livello di trasporto TCP/IP supporta v2 Large Send Offload (LSOv2). Con LSOv2, il livello di trasporto TCP/IP può eseguire l'offload di segmentazione dei pacchetti TCP grandi alla scheda di rete.|  
|Receive-Side Scaling \(RSS\)|RSS è una tecnologia di driver di rete che consente la distribuzione efficiente di rete ricevono l'elaborazione su più CPU nei sistemi multiprocessore. Più in dettaglio RSS viene fornito più avanti in questo argomento.|  
|Unione segmenti ricevuti \(RSC\)|RSC è la possibilità di pacchetti gruppo interagiscono per ridurre al minimo l'intestazione di elaborazione che è necessaria per l'host da eseguire. Un massimo di 64 KB di payload ricevuto può essere unito in un singolo pacchetto di dimensioni maggiori per l'elaborazione. Informazioni più dettagliate sulle funzionalità RSC viene fornita più avanti in questo argomento.|  
  
###  <a name="bkmk_rss"></a> Receive-Side Scaling

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 e Windows Server 2008 supportano Receive-Side Scaling \(RSS\). 

Alcuni server configurati con più processori logici che condividono le risorse hardware \(ad esempio un core fisici\) e che vengono trattati come multithreading simultaneo \(SMT\) peer. La tecnologia Intel Hyper-Threading è un esempio. RSS indirizza l'elaborazione di rete di fino a un processore logico per ogni core. In un server con Hyper-Threading di Intel, 4 core e 8 processori logici, ad esempio, RSS Usa non più di 4 processori logici per l'elaborazione di rete.  

RSS distribuisce i pacchetti dei / o di rete in ingresso tra processori logici in modo che i pacchetti che appartengono alla stessa connessione TCP sono elaborati sullo stesso processore logico, che mantiene l'ordinamento. 

RSS caricare anche il traffico multicast e unicast UDP di saldi e instrada i flussi correlati \(determinate eseguendo l'hashing gli indirizzi di origine e destinazione\) sul processore logico stesso, mantenendo l'ordine degli arrivi correlati. Ciò consente di migliorare scalabilità e prestazioni per scenari a elevato utilizzo di ricezione per i server con un minor numero di schede di rete che non idonei processori logici. 

#### <a name="configuring-rss"></a>Configurazione di RSS

In Windows Server 2016, è possibile configurare RSS tramite i cmdlet di Windows PowerShell e i profili RSS. 

È possibile definire profili RSS tramite le **– profilo** parametro del **Set-NetAdapterRss** cmdlet di Windows PowerShell.

**Comandi di Windows PowerShell per la configurazione di RSS**

I cmdlet seguenti consentono di visualizzare e modificare i parametri RSS per ogni scheda di rete.
  
>[!NOTE]
>Per un riferimento al comando dettagliati per ogni cmdlet, inclusi la sintassi e parametri, è possibile fare clic sui collegamenti seguenti. Inoltre, è possibile passare il nome del cmdlet **Get-Help** al prompt di Windows PowerShell per informazioni dettagliate su ogni comando.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Questo comando disabilita RSS nella scheda di rete specificato.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Questo comando abilita RSS nella scheda di rete specificato.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Questo comando recupera le proprietà RSS della scheda di rete specificato.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Questo comando imposta le proprietà RSS nella scheda di rete specificato.  

#### <a name="rss-profiles"></a>Profili RSS

È possibile usare la **-profilo** parametro del cmdlet Set-NetAdapterRss per specificare quali processori logici vengono assegnati a quale scheda di rete. I valori disponibili per questo parametro sono:

- **Più vicino**. Sono preferibili numeri processore logico che si avvicinano alla processore RSS di base della scheda di rete. Con questo profilo, il sistema operativo potrebbe ribilanciare dinamicamente in base al carico di processori logici.
  
- **ClosestStatic**. I numeri dei processori logici in prossimità del processore RSS di base della scheda di rete vengono preferiti. Con questo profilo, il sistema operativo non ribilanciare dinamicamente in base al carico di processori logici.
  
- **NUMA**. I numeri dei processori logici vengono in genere selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo potrebbe ribilanciare dinamicamente in base al carico di processori logici.
  
- **NUMAStatic**. Questo è il **profilo predefinito**. I numeri dei processori logici vengono in genere selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo non bilancerà processori logici dinamicamente in base al carico.

- **Conservativa**. RSS Usa il minor numero di processori per sostenere il carico possibile. Questa opzione consente di ridurre il numero di interrupt.

A seconda dello scenario e le caratteristiche del carico di lavoro, è anche possibile usare altri parametri del **Set-NetAdapterRss** cmdlet di Windows PowerShell per specificare quanto segue:

- In base all'adapter per ogni rete, il numero di processori logico è utilizzabile per RSS.
- L'offset iniziale dell'intervallo di processori logici.
- Nodo da cui la scheda di rete alloca la memoria.

Di seguito sono aggiuntiva **Set-NetAdapterRss** parametri che è possibile utilizzare configurazione di RSS:

>[!NOTE]
>La sintassi di esempio per ogni parametro di seguito, il nome della scheda di rete **Ethernet** viene utilizzato come un valore di esempio per il **– Name** parametro del **Set-NetAdapterRss** comando. Quando si esegue il cmdlet, assicurarsi che il nome di scheda di rete che usa sia appropriato per l'ambiente.

- **\* MaxProcessors**: Imposta il numero massimo di processori RSS da utilizzare. Ciò garantisce che il traffico dell'applicazione è associato a un numero massimo di processori in una determinata interfaccia. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**: Imposta il gruppo di processori di base di un nodo NUMA. Ciò ha effetto sulla matrice del processore utilizzata da RSS. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: Imposta il gruppo di processori massima di un nodo NUMA. Ciò ha effetto sulla matrice del processore utilizzata da RSS. Un gruppo di processori massimo impostando questo determinerebbe una limitazione in modo che il bilanciamento del carico è allineata all'interno di un gruppo k. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: Imposta il numero di processori di base di un nodo NUMA. Ciò ha effetto sulla matrice del processore utilizzata da RSS. In questo modo il partizionamento processori nelle schede di rete. Questo è il processore logico prima nei processori intervallo di RSS che viene assegnato a ogni scheda. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: Nodo NUMA che ogni scheda di rete può allocare memoria da. Può trattarsi di un gruppo k o da diversi gruppi k. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: Se i processori logici sembrano sottoutilizzati per la ricezione del traffico \(ad esempio, come visualizzato in Gestione attività\), è possibile provare ad aumentare il numero di code RSS dal valore predefinito di 2 al valore massimo supportato dalla scheda di rete . La scheda di rete può avere opzioni per modificare il numero di code RSS come parte del driver. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Per altre informazioni, fare clic sul collegamento seguente per scaricare [Scalable Networking: Eliminando ricevere l'elaborazione di un collo di bottiglia: Introduzione a RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) in formato Word.
  
#### <a name="understanding-rss-performance"></a>La comprensione delle prestazioni di RSS

RSS di ottimizzazione, è necessario conoscere la configurazione e la logica di bilanciamento del carico. Per verificare che le impostazioni di RSS sono diventate effettive, è possibile esaminare l'output quando si esegue la **Get-NetAdapterRss** cmdlet di Windows PowerShell. Seguito è riportato l'output di questo cmdlet.
  
```

PS C:\Users\Administrator> get-netadapterrss  
Name                           : testnic 2  
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8
  
IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
…   
(# indirection table entries are a power of 2 and based on # of processors)  
…   
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
```  

Oltre ai parametri che sono stati impostati di ripetizione, l'aspetto principale dell'output è l'output di tabella di riferimento indiretto. La tabella di riferimento indiretto consente di visualizzare i bucket nella tabella hash che vengono usati per distribuire il traffico in ingresso. In questo esempio, la notazione n:c designa il K Numa-coppia indice: CPU di gruppo che viene utilizzato per dirigere il traffico in ingresso. Vediamo esattamente 2 voci univoche (0:0 e 0:4), gruppo k. 0/cpu0 e gruppo k. 0/cpu 4, che rappresentano rispettivamente.

È presente un solo gruppo k per questo sistema (gruppo k 0) e un n (dove n < = 128) voce della tabella di riferimento indiretto. Poiché il numero di code di ricezione è impostato su 2, solo 2 processori (0:0, 0:4) vengono scelti - anche se i processori massimi è impostata su 8. In effetti, la tabella di riferimento indiretto è hashing il traffico in ingresso per l'uso solo 2 CPU da 8 che sono disponibili.

Per sfruttare appieno le CPU, il numero di code RSS di ricezione deve essere uguale o maggiore di processori Max. Nell'esempio precedente, la coda di ricezione deve essere impostata su 8 o versione successiva.

#### <a name="nic-teaming-and-rss"></a>Gruppo NIC e RSS

RSS può essere abilitata in una scheda di rete è raggruppata con un'altra scheda di interfaccia di rete utilizza gruppo NIC. In questo scenario, solo la scheda di rete fisica sottostante può essere configurata per l'utilizzo di RSS. Un utente non è possibile impostare i cmdlet di RSS nella scheda di rete raggruppata.
  
###  <a name="bkmk_rsc"></a> Receive Segment Coalescing (RSC)

Unione segmenti ricevuti \(RSC\) consente prestazioni riducendo il numero di intestazioni IP che vengono elaborati per una determinata quantità di dati ricevuti. Da utilizzare per ridimensionare le prestazioni dei dati ricevuti raggruppando \(o coalescing\) i pacchetti più piccoli in unità più grandi.

Questo approccio può influire sulla latenza con vantaggi principalmente visualizzati in incrementi di velocità effettiva. RSC è consigliabile aumentare la velocità effettiva per carichi di lavoro ricevuti. È consigliabile distribuire schede di rete che supportano RSC. 

In queste schede di rete, verificare la funzionalità RSC in \(questo è l'impostazione predefinita\), a meno che non si dispone di carichi di lavoro specifici \(bassa latenza, ad esempio, la rete di velocità effettiva bassa\) grado di sfruttare show RSC viene disattivata .

#### <a name="understanding-rsc-diagnostics"></a>Informazioni sulla diagnostica RSC

È possibile diagnosticare RSC mediante i cmdlet di Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

Seguito è riportato l'output quando si esegue il cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

Il **ottenere** cmdlet Mostra se la funzionalità RSC è abilitata nell'interfaccia e se TCP consente RSC in uno stato operativo. Il motivo dell'errore fornisce informazioni dettagliate sull'errore per abilitare RSC in quell'interfaccia.

Nello scenario precedente, RSC IPv4 è supportato e operativa nell'interfaccia. Per comprendere gli errori di diagnostica, si può osservare il byte fuse o le eccezioni causate. Ciò offre un'indicazione dei problemi di unione.

Seguito è riportato l'output quando si esegue il cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC e virtualizzazione

RSC è supportata solo in host fisici quando la scheda di rete host non è associata al commutatore virtuale Hyper-V. RSC è disabilitata per il sistema operativo quando l'host è associata al commutatore virtuale Hyper-V. Inoltre, le macchine virtuali non beneficiare di RSC poiché le schede di rete virtuale non supportano RSC.

La funzionalità RSC può essere abilitata per una macchina virtuale quando Single Root Input/Output Virtualization \(SR-IOV\) è abilitata. In questo caso, le funzioni virtuali supportano la funzionalità RSC; di conseguenza, le macchine virtuali ricevono inoltre il vantaggio di RSC.

##  <a name="bkmk_resources"></a> Risorse della scheda di rete

Alcune schede di rete gestiscono attivamente le risorse per ottenere prestazioni ottimali. Più schede di rete consentono di configurare manualmente le risorse usando il **Advanced Networking** scheda per l'adapter. Per questi adapter, è possibile impostare i valori di un numero di parametri, incluso il numero di buffer di ricezione e invio di buffer.

Configurazione delle risorse di scheda di rete viene semplificata dall'uso dei cmdlet di Windows PowerShell seguente.

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

Per altre informazioni, vedere [cmdlet per scheda di rete in Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Per collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni di rete sottosistema](net-sub-performance-top.md).