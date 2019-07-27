---
title: Pianificare la scalabilità di Hyper-V in Windows Server 2016
description: Elenca il numero massimo supportato per i componenti che è possibile aggiungere o rimuovere da Hyper-V e dalle macchine virtuali, ad esempio la quantità di memoria e il numero di processori virtuali.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 534de49e50d7b415c9d64c32927418a4395f6f4f
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544744"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Pianificare la scalabilità di Hyper-V in Windows Server 2016

> Si applica a: Windows Server 2016, Windows Server 2019
  
Questo articolo fornisce informazioni dettagliate sulla configurazione massima per i componenti che è possibile aggiungere e rimuovere in un host Hyper-V o nelle relative macchine virtuali, ad esempio i processori virtuali o i checkpoint. Quando si pianifica la distribuzione, prendere in considerazione i valori massimi applicabili a ogni macchina virtuale, oltre a quelli che si applicano all'host Hyper-V. 

I massimi per i processori logici e di memoria sono i maggiori aumenti di Windows Server 2012, in risposta alle richieste di supporto di scenari più recenti, ad esempio machine learning e analisi dei dati. Blog di Windows Server recentemente pubblicato i risultati delle prestazioni di una macchina virtuale con 5.5 terabyte di memoria e 128 processori virtuali in esecuzione di database in memoria di 4 TB. Prestazioni superiori al 95% delle prestazioni di un server fisico. Per informazioni dettagliate, vedere [prestazioni della macchina Virtuale su larga scala di Windows Server 2016 Hyper-V per l'elaborazione delle transazioni in memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Altri numeri sono simili a quelli che si applicano a Windows Server 2012. \(I valori massimi per Windows Server 2012 R2 corrispondono a quelli di Windows Server 2012.\) 
  
> [!NOTE]  
> Per informazioni su System Center Virtual Machine Manager (VMM), vedere [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). VMM è un prodotto Microsoft, venduto separatamente, per la gestione di data center virtualizzati.  
  
## <a name="maximums-for-virtual-machines"></a>Numero massimo di macchine virtuali  
Questi valori massimi si applicano a ogni macchina virtuale. Non tutti i componenti sono disponibili in entrambe le generazioni di macchine virtuali. Per un confronto tra le generazioni, vedere la pagina relativa alla [creazione di una macchina virtuale di prima o seconda generazione in Hyper-V](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Componente|Massima|Note|  
|-------------|-----------|---------|  
|Checkpoint|50|Il numero effettivo potrebbe essere inferiore in base allo spazio di archiviazione disponibile. Ogni checkpoint viene archiviato come file con estensione avhd che usa l'archiviazione fisica.|  
|Memoria|12 TB per la generazione 2; <br>1 TB per la generazione 1|Per determinare la capacità minima e quella consigliata, esaminare i requisiti del sistema operativo specifico.|  
|Porte seriali (COM)|2|No.|  
|Dimensione dei dischi fisici collegati direttamente a una macchina virtuale|Variabile|La dimensione massima è determinata dal sistema operativo guest.|  
|Schede Fibre Channel virtuali|4|È consigliabile connettere ogni scheda Fibre Channel virtuale a una SAN virtuale diversa.|  
|Dispositivi floppy virtuali|1 unità floppy virtuale|No.|
|Capacità del disco rigido virtuale|64 TB per formato VHDX;<br>2040 GB per formato VHD|Ogni disco rigido virtuale è archiviato in un supporto fisico come file con estensione vhdx o vhd, a seconda del formato usato dal disco rigido virtuale.|  
|Dischi IDE virtuali|4|Il disco di avvio (talvolta denominato disco di avvio) deve essere collegato a uno dei dispositivi IDE. Può essere un disco rigido virtuale o un disco fisico collegato direttamente a una macchina virtuale.|  
|Processori virtuali|240 per la generazione 2;<br>64 per la generazione 1;<br>320 disponibile per il sistema operativo host (partizione radice)|Il numero di processori virtuali supportati da un sistema operativo guest potrebbe essere inferiore. Per informazioni dettagliate, vedere le informazioni pubblicate per il sistema operativo specifico.|
|Controller SCSI virtuali|4|L'uso di dispositivi SCSI virtuali richiede Integration Services, che sono disponibili per i sistemi operativi guest supportati. Per informazioni dettagliate sui sistemi operativi supportati, vedere [macchine virtuali Linux e FreeBSD supportate](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) e [sistemi operativi guest Windows supportati](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Dischi SCSI virtuali|256|Ogni controller SCSI supporta fino a 64 dischi, il che significa che per ogni macchina virtuale è possibile configurare fino a 256 dischi SCSI virtuali (64 dischi x 4 controller).|  
|Schede di rete virtuali|Windows Server 2016 supporta 12 totali:<br> -8 schede di rete specifiche di Hyper-V<br>-4 schede di rete legacy <br> Windows Server 2019 supporta 72 Totale: <br> -64 schede di rete specifiche di Hyper-V<br>-4 schede di rete legacy  |La scheda di rete specifica di Hyper-V garantisce prestazioni migliori e richiede un driver incluso in Integration Services. Per ulteriori informazioni, vedere [pianificare la rete Hyper-V in Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Valori massimi per gli host Hyper-V  
Questi valori massimi si applicano a ogni host Hyper-V.  
  
|Componente|Massima|Note|  
|-------------|-----------|---------|  
|Processori logici|512|Entrambi devono essere abilitati nel firmware:<br /><br />-Virtualizzazione assistita da hardware<br />-Protezione esecuzione programmi applicata dall'hardware<br /><br />Il sistema operativo host (partizione radice) visualizzerà solo i processori logici massimi 320|  
|Memoria|24 TB|No.|  
|Gruppo NIC|Nessun limite imposto da Hyper-V|Per informazioni dettagliate, vedere [Gruppo NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Schede di rete fisiche|Nessun limite imposto da Hyper-V|No.|  
|Macchine virtuali in esecuzione per server|1024|No.|  
|Archiviazione|Limitato da ciò che è supportato dal sistema operativo host. Nessun limite imposto da Hyper-V|**Nota:** Microsoft supporta l'archiviazione con connessione di rete (NAS) quando si usa SMB 3,0. L'archiviazione basata su NFS non è supportata.|
|Porte di commutatori di rete virtuali per server|Variabile, nessun limite imposto da Hyper-V|Il limite effettivo dipende dalle risorse di elaborazione disponibili.|  
|Processori virtuali per processore logico|Nessun rapporto imposto da Hyper-V|No.|  
|Processori virtuali per server|2048|No.|  
|Reti di archiviazione (SAN, Storage Area Network) virtuali|Nessun limite imposto da Hyper-V|No.|  
|Commutatori virtuali|Variabile, nessun limite imposto da Hyper-V|Il limite effettivo dipende dalle risorse di elaborazione disponibili.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Cluster di failover e Hyper-V  
Questa tabella elenca i valori massimi applicabili quando si usa Hyper-V e il clustering di failover. È importante eseguire la pianificazione della capacità per assicurarsi che vi siano risorse hardware sufficienti per eseguire tutte le macchine virtuali in un ambiente cluster.  

Per informazioni sugli aggiornamenti al clustering di failover, incluse le nuove funzionalità per le macchine virtuali, vedere Novità relative [al clustering di failover in Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Componente|Massima|Note|  
|-------------|-----------|---------|  
|Nodi per cluster|64|Tenere presenti il numero di nodi che si desidera riservare per il failover nonché le attività di manutenzione come l'installazione di aggiornamenti. È consigliabile pianificare un numero di risorse sufficienti in modo da poter riservare 1 nodo per il failover, il che significa che tale nodo rimane inattivo fino a quando un altro nodo non esegue il failover (il nodo inattivo è anche noto come nodo passivo). È possibile aumentare questo numero se si desidera riservare ulteriori nodi. Non è previsto un rapporto o un moltiplicatore di nodi riservati ai nodi attivi; l'unico requisito è che il numero totale di nodi in un cluster non può superare il valore massimo di 64.|  
|Macchine virtuali in esecuzione per cluster e per nodo|8\.000 per cluster|Molti fattori possono influire sul numero reale di macchine virtuali che è possibile eseguire contemporaneamente in un nodo, ad esempio:<br />-Quantità di memoria fisica utilizzata da ogni macchina virtuale.<br />-Larghezza di banda di rete e archiviazione.<br />: Numero di mandrini del disco, che influiscono sulle prestazioni di I/O del disco.|  
  

