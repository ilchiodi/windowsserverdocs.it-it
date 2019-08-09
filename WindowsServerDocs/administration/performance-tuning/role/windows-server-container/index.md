---
title: Ottimizzazione delle prestazioni per i contenitori di Windows Server
description: Consigli per l'ottimizzazione delle prestazioni per i contenitori in Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: 072d71a7825907ada7d4bc02eb5390722692e81d
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863415"
---
# <a name="performance-tuning-windows-server-containers"></a>Ottimizzazione delle prestazioni per i contenitori di Windows Server

## <a name="introduction"></a>Introduzione
Windows Server 2016 è la prima versione di Windows che fornisce supporto per la tecnologia dei contenitori incorporata nel sistema operativo. In Server 2016 sono disponibili due tipi di contenitori: contenitori di Windows Server e contenitori di Hyper-V. Ogni tipo di contenitore supporta lo SKU Server Core o Nano Server di Windows Server 2016. 

Le implicazioni in termini di prestazioni di queste configurazioni sono diverse e vengono descritte di seguito per effettuare la scelta ottimale per i propri scenari. Verranno inoltre descritte nei dettagli le configurazioni che hanno impatto sulle prestazioni e i compromessi per ciascuna di queste opzioni.

### <a name="windows-server-container-and-hyper-v-containers"></a>Contenitore di Windows Server e contenitori di Hyper-V

Il contenitore di Windows Server e i contenitori di Hyper-V hanno in comune molti vantaggi in termini di portabilità e coerenza, ma differiscono per quanto riguarda le garanzie di isolamento e le caratteristiche delle prestazioni.

I **contenitori di Windows Server** offrono l'isolamento dell'applicazione attraverso una tecnologia di isolamento dei processi e dello spazio dei nomi. Un contenitore di Windows Server condivide un kernel con l’host contenitore e tutti i contenitori in esecuzione sull’host.

I **contenitori di Hyper-V** espandono l'isolamento fornito dai contenitori di Windows Server eseguendo ogni contenitore in una macchina virtuale altamente ottimizzata. In questa configurazione il kernel dell’host contenitore non viene condiviso con i contenitori di Hyper-V.

L'isolamento aggiuntivo fornito dai contenitori di Hyper-V è realizzato in larga parte da un livello hypervisor di isolamento tra il contenitore e l'host del contenitore. Ciò influisce sulla densità del contenitore dal momento che, a differenza dei contenitori di Windows Server, può verificarsi una minore condivisione dei file di sistema e dei file binari risultante in un maggiore footprint complessivo di archiviazione e memoria. Si verifica inoltre il sovraccarico aggiuntivo previsto in alcuni percorsi di rete, I/O di archiviazione e CPU.

### <a name="nano-server-and-server-core"></a>Nano Server e Server Core

I contenitori di Windows Server e Hyper-V offrono il supporto per Server Core e per una nuova opzione di installazione disponibile in Windows Server 2016: [Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server). 

Nano Server è un sistema operativo server amministrato da postazione remota e ottimizzato per data center e cloud privati. È simile a Windows Server in modalità Server Core, ma notevolmente più piccolo, non dispone di funzionalità di accesso locale e supporta solo agenti, strumenti e applicazioni a 64 bit. Occupa molto meno spazio su disco e si avvia più velocemente.

## <a name="container-start-up-time"></a>Tempo di avvio del contenitore
Il tempo di avvio del contenitore è una metrica fondamentale in molti degli scenari in cui i contenitori offrono i maggiori vantaggi. È essenziale pertanto conoscere le procedure per ottimizzare il tempo di avvio del contenitore. Di seguito vengono descritti alcuni compromessi di ottimizzazione da conoscere per migliorare il tempo di avvio.

### <a name="first-logon"></a>Primo accesso

Microsoft offre un'immagine di base sia per Server Core che per Nano Server. L'immagine di base che viene fornita per Server Core è stata ottimizzata rimuovendo il sovraccarico del tempo di avvio associato al primo accesso (Configurazione guidata). Ciò non avviene con l'immagine di base per Nano Server. Questo costo tuttavia può essere rimosso dalle immagini basate su Nano Server eseguendo il commit di almeno un livello per l'immagine del contenitore. Gli avvii successivi del contenitore a partire dall'immagine non comporteranno il costo del primo accesso.
### <a name="scratch-space-location"></a>Posizione dell'area scratch

Per impostazione predefinita, i contenitori usano un'area scratch temporanea sui supporti di memorizzazione delle unità del sistema dell'host del contenitore per l'archiviazione nell'ambito della durata del contenitore in esecuzione. Viene usata come unità del sistema del contenitore e di conseguenza molte delle operazioni di scrittura e lettura durante l'utilizzo del contenitore seguono questo percorso. Per i sistemi host in cui l'unità di sistema è presente su supporti di memorizzazione magnetici a dischi rotanti (HDD) ma è disponibile un supporto di memorizzazione più veloce (HDD o SSD più veloci), è possibile spostare l'area scratch del contenitore in un'unità diversa. A tale scopo viene usato il comando dockerd -g. Questo comando è globale e ha effetto su tutti i contenitori in esecuzione nel sistema.

### <a name="nested-hyper-v-containers"></a>Contenitori di Hyper-V annidati
Hyper-V per Windows Server 2016 introduce il supporto di hypervisor annidato. È ora possibile pertanto eseguire una macchina virtuale dall'interno di una macchina virtuale. In questo modo si aprono molti scenari utili, ma si accentuano anche alcuni effetti negativi sulle prestazioni prodotti dall'hypervisor. Sono presenti infatti due livelli di hypervisor in esecuzione al di sopra dell'host fisico.

Per i contenitori questo effetto negativo si verifica quando si esegue un contenitore di Hyper-V all'interno di una macchina virtuale. Poiché un contenitore di Hyper-V offre isolamento tramite un livello di hypervisor tra se stesso e l'host del contenitore, quando l'host del contenitore è una macchina virtuale basata su Hyper-V, si verifica un sovraccarico per le prestazioni in termini di tempo di avvio del contenitore, I/O di archiviazione, I/O e velocità effettiva della rete e CPU.

## <a name="storage"></a>Archiviazione
### <a name="mounted-data-volumes"></a>Volumi di dati montati

I contenitori offrono la possibilità di usare l'unità del sistema host del contenitore per l'area scratch del contenitore. L'area scratch del contenitore tuttavia ha una durata pari a quella del contenitore. Quando pertanto il contenitore viene arrestato, l'area scratch e tutti i dati associati vengono eliminati.

Esistono tuttavia molti scenari in cui si preferisce avere dati persistenti indipendenti dalla durata del contenitore. In questi casi è supportato il montaggio di volumi di dati dall'host del contenitore nel contenitore. Per i contenitori di Windows Server, il sovraccarico del percorso I/O associato ai volumi di dati montati è trascurabile (prestazioni quasi uguali a quelle native). Con il montaggio di volumi di dati in contenitori di Hyper-V invece si verifica una riduzione del livello delle prestazioni di I/O in tale percorso. Questo impatto viene inoltre accentuato se si eseguono i contenitori di Hyper-V all'interno di macchine virtuali.

### <a name="scratch-space"></a>Area scratch

Per impostazione predefinita, sia i contenitori di Windows Server sia i contenitori di Hyper-V forniscono un disco rigido virtuale dinamico da 20 GB per l'area scratch. Per entrambi i tipi di contenitore, il sistema operativo occupa una parte di tale spazio e questo vale per ogni contenitore avviato. È importante quindi ricordare che ogni contenitore avviato produce un certo impatto sulla memoria e a seconda del carico di lavoro può scrivere fino a 20 GB del supporto di archiviazione sottostante. Le configurazioni di archiviazione dei server devono essere progettate tenendo conto di questo comportamento
(possibilità di configurare le dimensioni dell'area scratch).

## <a name="networking"></a>Rete
I contenitori di Windows Server e di Hyper-V offrono un'ampia gamma di modalità di rete per soddisfare al meglio le esigenze delle diverse configurazioni di rete. Ognuna di queste opzioni presenta proprie caratteristiche in termini di prestazioni.

### <a name="windows-network-address-translation-winnat"></a>Windows Network Address Translation (WinNAT)

Ogni contenitore riceve un indirizzo IP da un prefisso IP interno e privato, ad esempio 172.16.0.0/12. Il port forwarding o il mapping delle porte dall'host contenitore agli endpoint del contenitore è supportato. Docker crea una rete NAT per impostazione predefinita quando viene eseguito prima dockerd.

Di queste tre modalità, la configurazione NAT è il percorso I/O di rete più oneroso, ma richiede meno operazioni di configurazione rispetto alle altre. 

I contenitori di Windows Server usano una scheda vNIC host per il collegamento al commutatore virtuale. I contenitori di Hyper-V usano una scheda NIC VM sintetica, non visibile alla VM delle utilità, per il collegamento al commutatore virtuale. Quando i contenitori comunicano con la rete esterna, i pacchetti sono instradati attraverso WinNAT con le conversioni di indirizzi applicate, il che comporta un certo sovraccarico.

### <a name="transparent"></a>Modalità trasparente

Ogni endpoint del contenitore è connesso direttamente alla rete fisica. È possibile assegnare gli indirizzi IP dalla rete fisica in modo statico o dinamico usando un server DHCP esterno.

La modalità trasparente è la meno onerosa in termini di percorso I/O di rete e i pacchetti esterni vengono passati direttamente attraverso la scheda NIC virtuale del contenitore fornendo accesso diretto alla rete esterna.

### <a name="l2-bridge"></a>L2 Bridge
Ogni endpoint del contenitore si troverà nella stessa subnet IP dell'host del contenitore. Gli indirizzi IP devono essere assegnati in modo statico con lo stesso prefisso dell'host contenitore. Tutti gli endpoint del contenitore nell'host hanno lo stesso indirizzo MAC a causa di una conversione degli indirizzi di secondo livello.

La modalità L2 Bridge è più efficiente rispetto alla modalità WinNAT poiché fornisce accesso diretto alla rete esterna, ma è meno efficiente rispetto alla modalità trasparente perché introduce anche la conversione degli indirizzi MAC.




