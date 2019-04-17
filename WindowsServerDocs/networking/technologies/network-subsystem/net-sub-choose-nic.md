---
title: Scelta di una scheda di rete
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>Scelta di una scheda di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su alcune delle funzionalità di schede di rete che potrebbero influire sulle scelte di acquisto.

Applicazioni di risorse di rete richiede schede di rete ad alte prestazioni. In questa sezione illustra alcune considerazioni per la scelta di schede di rete, nonché come configurare le impostazioni della scheda rete diversa per ottenere le migliori prestazioni di rete.

> [!TIP]
>  È possibile configurare le impostazioni della scheda di rete utilizzando Windows PowerShell. Per ulteriori informazioni, vedere [cmdlet degli adattatori di rete in Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a>Funzionalità di offload

Offload delle attività da unità di elaborazione centrale \(CPU\) alla scheda di rete può ridurre l'utilizzo della CPU sul server, che migliora le prestazioni generali del sistema.

Lo stack di rete dei prodotti Microsoft può eseguire l'offload di uno o più attività per una scheda di rete se si seleziona una scheda di rete con appropriati delle funzionalità di offload. Nella tabella seguente fornisce una breve panoramica delle funzionalità di offload diversi che sono disponibili in Windows Server 2016.
  
|Tipo di offload|Descrizione|
|------------------|-----------------|  
|Calcolo del checksum per TCP|Lo stack di rete può eseguire l'offload di calcolo e la convalida di checksum \(TCP\) Transmission Control Protocol in inviare e ricevere i percorsi del codice. È possibile anche offload di calcolo e la convalida di IPv4 e IPv6 checksum nel inviare e ricevere i percorsi del codice.|  
|Calcolo del checksum per UDP |Lo stack di rete può eseguire l'offload di calcolo e la convalida di checksum \(UDP\) User Datagram Protocol in inviare e ricevere i percorsi del codice.|
|Calcolo del checksum per IPv4 |Lo stack di rete può eseguire l'offload di calcolo e la convalida di IPv4 checksum in inviare e ricevere i percorsi del codice. |
|Calcolo del checksum per IPv6 |Lo stack di rete può eseguire l'offload il calcolo e la convalida di checksum su inviare e ricevere i percorsi del codice di IPv6. | 
|Segmentazione dei pacchetti TCP grandi dimensioni|Il layer di trasporto TCP/IP supporta Large Send Offload v2 (LSOv2). Con LSOv2, il livello di trasporto TCP/IP può eseguire l'offload di segmentazione di grandi dimensioni pacchetti TCP alla scheda di rete.|  
|Receive Side Scaling \(RSS\)|RSS è una tecnologia di driver di rete che consente la distribuzione di rete efficiente ricevere elaborazione su più CPU nei sistemi multiprocessore. Informazioni più dettagliate sugli RSS viene fornita più avanti in questo argomento.|  
|Ricevere segmento Coalescing \(RSC\)|RSC è la possibilità di pacchetti di gruppo in modo da ridurre al minimo l'intestazione di elaborazione che è necessaria per l'host eseguire. Un singolo pacchetto più grande per l'elaborazione, è possibile fuse un massimo di 64 KB di payload ricevuti. Informazioni più dettagliate sugli RSC viene fornita più avanti in questo argomento.|  
  
###  <a name="bkmk_rss"></a>Ricevere Side Scaling

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 e Windows Server 2008 supportano Receive-Side Scaling \(RSS\). 

Alcuni server sono configurati con più processori logici che condividono risorse hardware \ (ad esempio un core\ fisico) e che vengono considerati come \(SMT\) multithreading simultaneo peer. La tecnologia Hyper-Threading di Intel è un esempio. RSS indirizza l'elaborazione di rete di fino a un processore logico per ogni core. In un server con Hyper-Threading di Intel, 4 core e 8 processori logici, ad esempio, RSS utilizza non più di 4 processori logici per l'elaborazione di rete.  

RSS distribuisce i pacchetti dei / o di rete in ingresso tra processori logici in modo che i pacchetti che appartengono alla stessa connessione TCP vengono elaborati in dello stesso processore logico che mantiene l'ordinamento. 

RSS anche caricare i saldi UDP unicast e il traffico multicast e viene instradato flussi correlati \ (che sono determinati dall'hashing addresses\ l'origine e di destinazione) allo stesso processore logico, mantenendo l'ordine di arrivi correlati. Ciò consente di migliorare la scalabilità e prestazioni di ricezione intensiva per i server che hanno un minor numero di schede di rete che non idonei processori logici. 

#### <a name="configuring-rss"></a>Configurazione di RSS

In Windows Server 2016, è possibile configurare RSS tramite i cmdlet di Windows PowerShell e i profili RSS. 

È possibile definire i profili RSS tramite il **– profilo** parametro del **Set-NetAdapterRss** cmdlet di Windows PowerShell.

**Comandi di Windows PowerShell per la configurazione di RSS**

I cmdlet seguenti consentono di visualizzare e modificare i parametri di RSS per ogni scheda di rete.
  
>[!NOTE]
>Per informazioni di riferimento dettagliate di comando per ogni cmdlet, inclusa la sintassi e parametri, è possibile scegliere i collegamenti seguenti. Inoltre, è possibile passare il nome del cmdlet per **Get-Help** al prompt di Windows PowerShell per informazioni dettagliate su ogni comando.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Questo comando disabilita RSS nella scheda di rete specificato.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Questo comando abilita RSS nella scheda di rete specificato.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Questo comando recupera le proprietà RSS della scheda di rete specificato.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Questo comando imposta le proprietà RSS nella scheda di rete specificato.  

#### <a name="rss-profiles"></a>Profili RSS

È possibile utilizzare il **– profilo** parametro del cmdlet Set-NetAdapterRss per specificare quali processori logici vengono assegnati alla scheda di rete. I valori disponibili per questo parametro sono:

- **Più vicino**. I numeri dei processori logici presenti nelle vicinanze del processore RSS base della scheda di rete sono preferiti. Con questo profilo, il sistema operativo potrebbe ribilanciare processori logici in modo dinamico in base al caricamento.
  
- **ClosestStatic**. I numeri dei processori logici in prossimità del processore RSS base della scheda di rete sono preferiti. Con questo profilo, il sistema operativo non ribilanciare processori logici in modo dinamico in base al caricamento.
  
- **NUMA**. In genere, i numeri dei processori logici sono selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo potrebbe ribilanciare processori logici in modo dinamico in base al caricamento.
  
- **NUMAStatic**. Questo è il **profilo predefinito**. In genere, i numeri dei processori logici sono selezionati in nodi NUMA diversi per distribuire il carico. Con questo profilo, il sistema operativo non verrà ribilanciare processori logici in modo dinamico in base al caricamento.

- **Conservativo**. RSS utilizza il minor numero di processori possibile per sostenere il carico. Questa opzione consente di ridurre il numero di interrupt.

A seconda dello scenario e le caratteristiche del carico di lavoro, è possibile usare anche altri parametri del **Set-NetAdapterRss** cmdlet di Windows PowerShell per specificare quanto segue:

- Per una scheda per ogni rete, il numero di processori logici è utilizzabile per RSS.
- L'offset iniziale per l'intervallo di processori logici.
- Il nodo da cui viene allocata la scheda di rete.

Di seguito sono aggiuntiva **Set-NetAdapterRss** parametri che è possibile utilizzare per configurare RSS:

>[!NOTE]
>Nella sintassi di esempio per ogni parametro, sotto il nome della scheda rete **Ethernet** viene utilizzato come un valore di esempio per il **– nome** parametro del **Set-NetAdapterRss** comando. Quando si esegue il cmdlet, assicurarsi che il nome della scheda di rete che usi sia appropriato per l'ambiente.

- **\ * MaxProcessors**: imposta il numero massimo di processori RSS da utilizzare. Ciò garantisce che il traffico delle applicazioni è associato a un numero massimo di processori di un'interfaccia. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**: imposta il gruppo di base del processore di un nodo NUMA. Questo influisce sulla matrice del processore utilizzata da RSS. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**: imposta il gruppo di processore Max di un nodo NUMA. Questo influisce sulla matrice del processore utilizzata da RSS. Questa impostazione potrebbe limitare un gruppo di massima del processore in modo che il bilanciamento del carico viene allineato all'interno di un gruppo di k. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**: imposta il numero di processori di base di un nodo NUMA. Questo influisce sulla matrice del processore utilizzata da RSS. In questo modo il partizionamento processori su schede di rete. Questo è il primo processore logico nei processori intervallo di RSS assegnato a ogni scheda. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**: nodo NUMA di ogni scheda di rete può allocare memoria da. Può trattarsi di un gruppo di k o da diversi gruppi-k. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**: se i processori logici sembrano sottoutilizzati per ricevere il traffico \ (ad esempio, come vengono visualizzati in Task Manager), è possibile provare ad aumentare il numero di code RSS dal valore predefinito di 2 al massimo supportato dalla scheda di rete. La scheda di rete potrebbe essere opzioni per modificare il numero di code RSS come parte del driver. Sintassi di esempio:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Per ulteriori informazioni, fare clic sul collegamento seguente per scaricare [rete scalabile: eliminando il collo di elaborazione ricevere: Introduzione a RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) in formato Word.
  
#### <a name="understanding-rss-performance"></a>Informazioni sulle prestazioni di RSS

Per l'ottimizzazione di RSS, è necessario comprendere la configurazione e la logica di bilanciamento del carico. Per verificare che le impostazioni di RSS siano state applicate, è possibile esaminare l'output quando si esegue il **Get-NetAdapterRss** cmdlet di Windows PowerShell. Di seguito è esempio dell'output di questo cmdlet.
  
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

Oltre a ripetizione di parametri che sono stati impostati, l'aspetto principale dell'output è l'output di tabella di indirizzamento indiretto. Nella tabella di indirizzamento indiretto Visualizza i bucket tabella hash che vengono utilizzati per distribuire il traffico in ingresso. In questo esempio, la notazione n:c designa il K Numa-coppia di indice di gruppo: CPU che viene utilizzato per indirizzare il traffico in ingresso. Vediamo esattamente 2 voci univoche (0:0 e 0:4), che rappresentano k gruppo 0/cpu0 e 0-gruppo/cpu 4, rispettivamente.

Esiste un solo gruppo k per questo sistema (gruppo k 0) e un n (dove n < = 128) voce della tabella di indirizzamento indiretto. Perché il numero di code di receive è impostato su 2, solo 2 processori (0:0, 0:4) vengono scelti - anche se i processori massima è impostata su 8. In effetti, la tabella di indirizzamento indiretto è hashing del traffico in ingresso per utilizzare solo 2 CPU fuori 8 che sono disponibili.

Per usare appieno le CPU, il numero di code RSS ricevere deve essere uguale o maggiore rispetto ai processori Max. Nell'esempio precedente, la coda di ricezione deve essere impostata su 8 o versione successiva.

#### <a name="nic-teaming-and-rss"></a>Gruppo NIC e RSS

RSS può essere abilitata in una scheda di rete che è raggruppata con un'altra scheda di interfaccia di rete utilizza gruppo NIC. In questo scenario, solo la scheda di rete fisica sottostante può essere configurata per utilizzare RSS. Un utente non può impostare i cmdlet RSS nella scheda di rete in team.
  
###  <a name="bkmk_rsc"></a>Receive Segment Coalescing (RSC)

Ricevere Segment Coalescing \(RSC\) consente prestazioni riducendo il numero di intestazioni IP che vengono elaborati per una determinata quantità di dati ricevuti. Da utilizzare per migliorare le prestazioni di dati ricevuti da raggruppamento \(or coalescing\) i pacchetti più piccoli in unità più grande.

Questo approccio può influire sulla latenza vantaggi principalmente appare in incrementi di velocità effettiva. RSC è consigliabile aumentare la velocità effettiva ricevuti con carichi di lavoro. Valutare la distribuzione di schede di rete che supportano RSC. 

In queste schede di rete, verificare che RSC è in \ (questo è il setting\ predefinito), a meno che non si dispone di carichi di lavoro specifici \ (ad esempio, a bassa latenza, bassa velocità effettiva networking\) che mostra vantaggiose RSC viene disattivato.

#### <a name="understanding-rsc-diagnostics"></a>Informazioni sulla diagnostica RSC

È possibile diagnosticare RSC mediante i cmdlet di Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

Di seguito è l'output di esempio quando si esegue il cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

Il **ottenere** cmdlet Mostra se RSC è abilitata nell'interfaccia e se TCP consente RSC in uno stato operativo. La causa dell'errore fornisce informazioni dettagliate sull'errore per abilitare RSC su tale interfaccia.

Nello scenario precedente, RSC IPv4 è supportata e operativa nell'interfaccia. Per comprendere gli errori di diagnostica, si può osservare le eccezioni causate o equivale a un byte. Ciò fornisce un'indicazione dei problemi di aggregazione.

Di seguito è l'output di esempio quando si esegue il cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC e virtualizzazione

RSC è supportata solo negli host fisico quando la scheda di rete host non viene associata al commutatore virtuale Hyper-V. RSC è disabilitata per il sistema operativo quando l'host è associata al commutatore virtuale Hyper-V. Inoltre, le macchine virtuali non usufruire del vantaggio della RSC perché RSC non supportano le schede di rete virtuale.

RSC può essere abilitata per una macchina virtuale quando è abilitata \(SR-IOV\) Single Root Input/Output Virtualization. In questo caso, le funzioni virtuali supportano la funzionalità RSC; di conseguenza, le macchine virtuali visualizzato anche il vantaggio di RSC.

##  <a name="bkmk_resources"></a>Risorse della scheda di rete

Alcune schede di rete gestiscono attivamente le risorse per ottenere prestazioni ottimali. Più schede di rete consentono di configurare manualmente le risorse utilizzando il **avanzate rete** scheda per la scheda. Per tali schede, è possibile impostare i valori di un numero di parametri, compresi il numero di buffer di ricezione e inviare i buffer.

Configurazione delle risorse della scheda di rete è semplificata grazie all'utilizzo dei cmdlet di Windows PowerShell seguente.

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

Per ulteriori informazioni, vedere [cmdlet degli adattatori di rete in Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Per i collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).