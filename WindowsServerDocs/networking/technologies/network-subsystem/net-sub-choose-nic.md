---
title: Scelta di una scheda di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: 2e902f3aea4025afe4f475c45193710a8b474dcd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862224"
---
# <a name="choosing-a-network-adapter"></a>Scelta di una scheda di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per apprendere alcune delle funzionalità delle schede di rete che potrebbero influire sulle scelte di acquisto.

Per le applicazioni a elevato utilizzo di rete sono richieste schede di rete ad alte prestazioni. In questa sezione vengono illustrate alcune considerazioni per la scelta delle schede di rete, nonché la configurazione di impostazioni della scheda di rete diverse per ottenere le migliori prestazioni di rete.

> [!TIP]
>  È possibile configurare le impostazioni della scheda di rete usando Windows PowerShell. Per altre informazioni, vedere [cmdlet per le schede di rete in Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter).

##  <a name="offload-capabilities"></a><a name="bkmk_offload"></a>Funzionalità di offload

L'offload delle attività dall'unità di elaborazione centrale \(CPU\) alla scheda di rete può ridurre l'utilizzo della CPU sul server, migliorando così le prestazioni complessive del sistema.

Lo stack di rete nei prodotti Microsoft può eseguire l'offload di una o più attività in una scheda di rete se si seleziona una scheda di rete con le funzionalità di offload appropriate. La tabella seguente fornisce una breve panoramica delle diverse funzionalità di offload disponibili in Windows Server 2016.
  
|Tipo di offload|Descrizione|
|------------------|-----------------|  
|Calcolo del checksum per TCP|Lo stack di rete può eseguire l'offload del calcolo e della convalida di Transmission Control Protocol \(checksum TCP\) nei percorsi del codice di trasmissione e ricezione. Consente inoltre di eseguire l'offload del calcolo e della convalida dei checksum IPv4 e IPv6 nei percorsi del codice di trasmissione e ricezione.|  
|Calcolo del checksum per UDP |Lo stack di rete può eseguire l'offload del calcolo e della convalida del protocollo del datagramma utente \(checksum\) UDP nei percorsi del codice di trasmissione e ricezione.|
|Calcolo del checksum per IPv4 |Lo stack di rete può eseguire l'offload del calcolo e della convalida dei checksum IPv4 nei percorsi del codice di trasmissione e ricezione. |
|Calcolo del checksum per IPv6 |Lo stack di rete può eseguire l'offload del calcolo e della convalida dei checksum IPv6 nei percorsi del codice di trasmissione e ricezione. | 
|Segmentazione di pacchetti TCP di grandi dimensioni|Il livello trasporto TCP/IP supporta l'offload di invio di grandi dimensioni V2 (LSOv2). Con LSOv2, il livello di trasporto TCP/IP può scaricare la segmentazione dei pacchetti TCP di grandi dimensioni nella scheda di rete.|  
|Receive-Side Scaling \(RSS\)|RSS è una tecnologia di driver di rete che consente di distribuire in modo efficiente l'elaborazione della ricezione di rete tra più CPU nei sistemi multiprocessore. Ulteriori dettagli su RSS sono disponibili più avanti in questo argomento.|  
|Unione del segmento di ricezione \(RSC\)|RSC è la possibilità di raggruppare i pacchetti per ridurre al minimo l'elaborazione dell'intestazione necessaria per l'esecuzione dell'host. È possibile unire un massimo di 64 KB di payload ricevuti in un singolo pacchetto di dimensioni maggiori per l'elaborazione. Ulteriori dettagli su RSC sono disponibili più avanti in questo argomento.|  
  
###  <a name="receive-side-scaling"></a><a name="bkmk_rss"></a>Receive-Side Scaling

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 e Windows Server 2008 supportano Receive-Side Scaling \(\)RSS. 

Alcuni server sono configurati con più processori logici che condividono risorse hardware \(come un core fisico\) e che vengono trattati come multi-threading simultaneo \(i peer\) SMT. Un esempio è la tecnologia Intel Hyper-Threading. RSS indirizza l'elaborazione di rete a un processore logico per core. Ad esempio, in un server con Hyper-Threading Intel, 4 core e 8 processori logici, RSS USA non più di 4 processori logici per l'elaborazione di rete.  

RSS distribuisce i pacchetti I/O di rete in ingresso tra i processori logici in modo che i pacchetti che appartengono alla stessa connessione TCP vengano elaborati nello stesso processore logico, che consente di mantenere l'ordinamento. 

RSS inoltre bilancia il carico del traffico multicast e unicast UDP e instrada i flussi correlati \(che sono determinati dall'hashing degli indirizzi di origine e di destinazione\) allo stesso processore logico, mantenendo l'ordine degli arrivi correlati. Ciò consente di migliorare la scalabilità e le prestazioni per gli scenari con utilizzo intensivo di ricezione per i server che dispongono di un numero inferiore di schede di rete rispetto a quelli che 

#### <a name="configuring-rss"></a>Configurazione di RSS

In Windows Server 2016 è possibile configurare RSS usando i cmdlet di Windows PowerShell e i profili RSS. 

È possibile definire i profili RSS usando il parametro **– profile** del cmdlet **set-NetAdapterRss** di Windows PowerShell.

**Comandi di Windows PowerShell per la configurazione RSS**

I cmdlet seguenti consentono di visualizzare e modificare i parametri RSS per ogni scheda di rete.
  
>[!NOTE]
>Per un riferimento dettagliato ai comandi per ogni cmdlet, inclusa la sintassi e i parametri, è possibile fare clic sui collegamenti seguenti. Inoltre, è possibile passare il nome del cmdlet a **Get-Help** al prompt di Windows PowerShell per informazioni dettagliate su ogni comando.  

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Disable-NetAdapterRss). Questo comando Disabilita RSS nella scheda di rete specificata.

- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterRss). Questo comando Abilita RSS nella scheda di rete specificata.
  
- [Get-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Get-NetAdapterRss). Questo comando recupera le proprietà RSS della scheda di rete specificata.
  
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss). Questo comando imposta le proprietà RSS nella scheda di rete specificata.  

#### <a name="rss-profiles"></a>Profili RSS

È possibile utilizzare il parametro **– profile** del cmdlet Set-NetAdapterRss per specificare i processori logici a cui è assegnata la scheda di rete. I valori disponibili per questo parametro sono:

- **Più vicino**. Sono preferibili i numeri di processori logici vicini al processore RSS di base della scheda di rete. Con questo profilo, il sistema operativo potrebbe ribilanciare i processori logici in modo dinamico in base al carico.
  
- **ClosestStatic**. Sono preferibili i numeri di processori logici vicini al processore RSS di base della scheda di rete. Con questo profilo, il sistema operativo non ribilancia i processori logici in modo dinamico in base al carico.
  
- **NUMA**. I numeri di processori logici vengono in genere selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo potrebbe ribilanciare i processori logici in modo dinamico in base al carico.
  
- **NUMAStatic**. Questo è il **profilo predefinito**. I numeri di processori logici vengono in genere selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo non ribilancia i processori logici in modo dinamico in base al carico.

- **Conservativa**. RSS utilizza il minor numero possibile di processori per sostenere il carico. Questa opzione consente di ridurre il numero di interrupt.

A seconda dello scenario e delle caratteristiche del carico di lavoro, è anche possibile usare altri parametri del cmdlet **set-NetAdapterRss** di Windows PowerShell per specificare quanto segue:

- Per ogni scheda di rete, il numero di processori logici che è possibile usare per RSS.
- Offset iniziale per l'intervallo di processori logici.
- Nodo da cui la scheda di rete alloca memoria.

Di seguito sono riportati i parametri **set-NetAdapterRss** aggiuntivi che è possibile usare per configurare RSS:

>[!NOTE]
>Nella sintassi di esempio per ogni parametro riportato di seguito, il nome della scheda di rete **Ethernet** viene usato come valore di esempio per il parametro **– Name** del comando **set-NetAdapterRss** . Quando si esegue il cmdlet, assicurarsi che il nome della scheda di rete utilizzato sia appropriato per l'ambiente in uso.

- **\* MaxProcessors**: imposta il numero massimo di processori RSS da usare. Ciò garantisce che il traffico dell'applicazione venga associato a un numero massimo di processori in una determinata interfaccia. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessors <value>`

- **\* BaseProcessorGroup**: imposta il gruppo di processori di base di un nodo NUMA. Questa operazione influisca sull'array del processore usato da RSS. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: imposta il gruppo di processori massimo di un nodo NUMA. Questa operazione influisca sull'array del processore usato da RSS. L'impostazione di questa opzione consente di limitare un gruppo di processori massimo in modo che il bilanciamento del carico sia allineato all'interno di un gruppo k. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: imposta il numero del processore di base di un nodo NUMA. Questa operazione influisca sull'array del processore usato da RSS. In questo modo è possibile partizionare i processori tra le schede di rete. Si tratta del primo processore logico nell'intervallo di processori RSS assegnato a ogni adapter. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: nodo NUMA da cui ogni scheda di rete può allocare memoria. Può trovarsi all'interno di un gruppo k o di gruppi k diversi. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: se i processori logici sembrano sottoutilizzati per il traffico di ricezione \(ad esempio, come visualizzato in gestione attività\), è possibile provare ad aumentare il numero di code RSS dal valore predefinito 2 al valore massimo supportato dalla scheda di rete. È possibile che la scheda di rete disponga delle opzioni per modificare il numero di code RSS come parte del driver. Esempio di sintassi:

     `Set-NetAdapterRss –Name "Ethernet" –NumberOfReceiveQueues <value>`

Per ulteriori informazioni, fare clic sul collegamento seguente per scaricare [la rete scalabile: eliminazione del collo di bottiglia per l'elaborazione della ricezione, introduzione a RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) in formato Word.
  
#### <a name="understanding-rss-performance"></a>Informazioni sulle prestazioni RSS

Per ottimizzare RSS è necessario conoscere la configurazione e la logica di bilanciamento del carico. Per verificare che le impostazioni RSS siano state applicate, è possibile esaminare l'output quando si esegue il cmdlet **Get-NetAdapterRss** di Windows PowerShell. Di seguito è riportato un esempio di output di questo cmdlet.
  
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

Oltre ai parametri di eco impostati, l'aspetto chiave dell'output è l'output della tabella di riferimento indiretto. La tabella di riferimento indiretto Visualizza i bucket della tabella hash utilizzati per distribuire il traffico in ingresso. In questo esempio la notazione n:c designa la coppia NUMA K-Group: CPU index usata per indirizzare il traffico in ingresso. Vengono visualizzate esattamente 2 voci univoche (0:0 e 0:4), che rappresentano rispettivamente k-Group 0/CPU0 e k-Group 0/CPU 4.

Per questo sistema è presente un solo gruppo k (k-Group 0) e una voce della tabella di riferimento indiretto n (dove n < = 128). Poiché il numero di code di ricezione è impostato su 2, vengono scelti solo 2 processori (0:0, 0:4), anche se il numero massimo di processori è impostato su 8. In effetti, la tabella di riferimento indiretto esegue l'hashing del traffico in ingresso per usare solo 2 CPU fuori dagli 8 disponibili.

Per utilizzare completamente le CPU, il numero di code di ricezione RSS deve essere maggiore o uguale al numero massimo di processori. Nell'esempio precedente, la coda di ricezione deve essere impostata su un valore maggiore o uguale a 8.

#### <a name="nic-teaming-and-rss"></a>Gruppo NIC e RSS

RSS può essere abilitato in una scheda di rete raggruppata con un'altra scheda di interfaccia di rete che utilizza il gruppo NIC. In questo scenario è possibile configurare solo la scheda di rete fisica sottostante per l'utilizzo di RSS. Un utente non può impostare i cmdlet RSS nella scheda di rete di gruppo.
  
###  <a name="receive-segment-coalescing-rsc"></a><a name="bkmk_rsc"></a>Unione segmento di ricezione (RSC)

L'Unione dei segmenti di ricezione \(RSC\) facilita le prestazioni riducendo il numero di intestazioni IP elaborate per una determinata quantità di dati ricevuti. Deve essere usato per semplificare la scalabilità delle prestazioni dei dati ricevuti raggruppando \(o coalesone\) i pacchetti più piccoli in unità più grandi.

Questo approccio può influire sulla latenza con i vantaggi che si riscontrano principalmente nei miglioramenti della velocità effettiva. È consigliabile usare RSC per aumentare la velocità effettiva per carichi di lavoro elevati ricevuti. Prendere in considerazione la distribuzione di schede di rete che supportano RSC. 

In queste schede di rete, assicurarsi che RSC sia attivo \(questa è l'impostazione predefinita\), a meno che non siano presenti carichi di lavoro specifici \(ad esempio, bassa latenza, rete a velocità effettiva bassa\) che mostrino i vantaggi offerti da RSC.

#### <a name="understanding-rsc-diagnostics"></a>Informazioni sulla diagnostica RSC

È possibile diagnosticare RSC usando i cmdlet di Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

Di seguito è riportato un esempio di output quando si esegue il cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

Il cmdlet **Get** indica se RSC è abilitato nell'interfaccia e se TCP consente a RSC di trovarsi in uno stato operativo. Il motivo dell'errore fornisce informazioni dettagliate sull'errore di abilitazione di RSC su tale interfaccia.

Nello scenario precedente, IPv4 RSC è supportato e operativo nell'interfaccia. Per comprendere gli errori di diagnostica, è possibile visualizzare i byte o le eccezioni Uniti. In questo modo viene fornita un'indicazione dei problemi di Unione.

Di seguito è riportato un esempio di output quando si esegue il cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics "myAdapter"   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC e virtualizzazione

RSC è supportato solo nell'host fisico quando la scheda di rete host non è associata al Commuter virtuale Hyper-V. RSC viene disabilitato dal sistema operativo quando l'host è associato al Commuter virtuale Hyper-V. Inoltre, le macchine virtuali non ottengono il vantaggio di RSC perché le schede di rete virtuali non supportano RSC.

RSC può essere abilitato per una macchina virtuale quando è abilitata la funzionalità Single root input/output Virtualization \(SR-IOV\). In questo caso, le funzioni virtuali supportano la funzionalità RSC; di conseguenza, le macchine virtuali ricevono anche il vantaggio di RSC.

##  <a name="network-adapter-resources"></a><a name="bkmk_resources"></a>Risorse scheda di rete

Alcune schede di rete gestiscono attivamente le proprie risorse per ottenere prestazioni ottimali. Diverse schede di rete consentono di configurare manualmente le risorse utilizzando la scheda **rete avanzata** per la scheda. Per tali schede è possibile impostare i valori di un numero di parametri, incluso il numero di buffer di ricezione e di invio.

La configurazione delle risorse della scheda di rete è semplificata mediante l'utilizzo dei cmdlet di Windows PowerShell seguenti.

- [Get-NetAdapterAdvancedProperty](https://docs.microsoft.com/powershell/module/netadapter/Get-NetAdapterAdvancedProperty)

- [Set-NetAdapterAdvancedProperty](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterAdvancedProperty)

- [Enable-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapte)

- [Enable-NetAdapterBinding](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterBinding)

- [Enable-NetAdapterChecksumOffload](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Enable-NetAdapterIPSecOffload](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Enable-NetAdapterLso](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterLso)

- [Enable-NetAdapterPowerManagement](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterPowerManagement)

- [Enable-NetAdapterQos](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterQos)

- [Enable-NetAdapterRDMA](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterRDMA)

- [Enable-NetAdapterSriov](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterSriov)

Per altre informazioni, vedere [cmdlet per le schede di rete in Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter).

Per i collegamenti a tutti gli argomenti di questa guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).
