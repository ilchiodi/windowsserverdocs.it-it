---
title: Pianificare la scalabilità di Hyper-V in Windows Server 2016
description: Elenca il numero massimo numero supportato per i componenti è possibile aggiungere o rimuovere da Hyper-V e macchine virtuali, come la quantità di memoria e il numero di processori virtuale.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 03a464269c8aea29c226dee776f0dfacfe48743a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850262"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Pianificare la scalabilità di Hyper-V in Windows Server 2016

> Si applica a: Windows Server 2016
  
Questo articolo offre informazioni dettagliate sulla configurazione massima per i componenti è possibile aggiungere e rimuovere in un host Hyper-V o le macchine virtuali, ad esempio processori virtuali o i checkpoint. Quando si pianifica la distribuzione, prendere in considerazione le configurazioni massime per ogni macchina virtuale, nonché a quelle che si applicano all'host Hyper-V. 

Valori massimi per la memoria e processori logici sono l'aumento maggiore da Windows Server 2012, in risposta alle richieste per supportare scenari più recenti, ad esempio analitica di machine learning e data. Blog di Windows Server recentemente pubblicato i risultati delle prestazioni di una macchina virtuale con 5.5 terabyte di memoria e 128 processori virtuali in esecuzione di database in memoria di 4 TB. Le prestazioni era maggiore del 95% delle prestazioni di un server fisico. Per informazioni dettagliate, vedere [prestazioni della macchina Virtuale su larga scala di Windows Server 2016 Hyper-V per l'elaborazione delle transazioni in memoria](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Altri numeri sono simili a quelle che si applicano a Windows Server 2012. \(Valori massimi per Windows Server 2012 R2 sono identiche a Windows Server 2012.\) 
  
> [!NOTE]  
> Per informazioni su System Center Virtual Machine Manager (VMM), vedere [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). VMM è un prodotto Microsoft, venduto separatamente, per la gestione di data center virtualizzati.  
  
## <a name="maximums-for-virtual-machines"></a>Valori massimi per le macchine virtuali  
Questi limiti massimi applicano a ogni macchina virtuale. Non tutti i componenti sono disponibili in entrambe le generazioni di macchine virtuali. Per un confronto tra le generazioni, vedere [è necessario creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Component|Massimo|Note|  
|-------------|-----------|---------|  
|Checkpoint|50|Il numero effettivo potrebbe essere inferiore in base allo spazio di archiviazione disponibile. Ogni checkpoint vengono archiviati come file con estensione avhd che utilizza l'archiviazione fisica.|  
|Memoria|12 TB per la generazione 2; <br>1 TB per la generazione 1|Per determinare la capacità minima e quella consigliata, esaminare i requisiti del sistema operativo specifico.|  
|Porte seriali (COM)|2|Nessuno.|  
|Dimensione dei dischi fisici collegati direttamente a una macchina virtuale|Varia|La dimensione massima è determinata dal sistema operativo guest.|  
|Schede Fibre Channel virtuali|4|È consigliabile connettere ogni scheda Fibre Channel virtuale a una SAN virtuale diversa.|  
|Dispositivi floppy virtuali|1 unità floppy virtuale|Nessuno.|
|Capacità del disco rigido virtuale|64 TB per il formato VHDX;<br>2040 GB per il formato di disco rigido virtuale|Ogni disco rigido virtuale è archiviato in un supporto fisico come file con estensione vhdx o vhd, a seconda del formato usato dal disco rigido virtuale.|  
|Dischi IDE virtuali|4|Disco di avvio (talvolta denominato disco di avvio) deve essere collegato a uno dei dispositivi IDE. Può essere un disco rigido virtuale o un disco fisico collegato direttamente a una macchina virtuale.|  
|Processori virtuali|240 per la generazione 2;<br>64 per la generazione 1;<br>320 disponibili per l'host del sistema operativo (partizione radice)|Il numero di processori virtuali supportati da un sistema operativo guest potrebbe essere inferiore. Per informazioni dettagliate, vedere le informazioni pubblicate per il sistema operativo specifico.|
|Controller SCSI virtuali|4|Uso di dispositivi SCSI virtuali richiede servizi di integrazione, sono disponibili per i sistemi operativi guest supportati. Per informazioni dettagliate su quali sistemi operativi supportati, vedere [macchine virtuali Linux e FreeBSD supportate](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) e [sistemi operativi guest supportati Windows](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Dischi SCSI virtuali|256|Ogni controller SCSI supporta fino a 64 dischi, il che significa che per ogni macchina virtuale è possibile configurare fino a 256 dischi SCSI virtuali (64 dischi x 4 controller).|  
|Schede di rete virtuali|12 in totale:<br> -8 schede di rete specifici di Hyper-V<br>-4 schede di rete legacy|La scheda di rete specifici di Hyper-V offre prestazioni migliori e richiede un driver inclusi in servizi di integrazione. Per altre informazioni, vedere [piano per la rete di Hyper-V in Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Valori massimi per gli host Hyper-V  
Questi limiti massimi si applicano a ogni host Hyper-V.  
  
|Component|Massimo|Note|  
|-------------|-----------|---------|  
|Processori logici|512|Entrambi questi deve essere abilitate nel firmware:<br /><br />-Virtualizzazione assistita mediante hardware<br />-Imposta dall'hardware dati esecuzione programmi (DEP)<br /><br />L'host del sistema operativo (partizione radice) verrà visualizzati solo massimi 320 processori logici|  
|Memoria|24 TB|Nessuno.|  
|Gruppo NIC|Nessun limite imposto da Hyper-V|Per informazioni dettagliate, vedere [NIC Teaming](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Schede di rete fisiche|Nessun limite imposto da Hyper-V|Nessuno.|  
|Macchine virtuali in esecuzione per server|1024|Nessuno.|  
|Archiviazione|Limitazioni di frequenza da che cosa sono supportato dal sistema operativo host. Nessun limite imposto da Hyper-V|**Nota:** Microsoft supporta NAS (NA) quando si usa SMB 3.0. L'archiviazione basata su NFS non è supportata.|
|Porte di commutatori di rete virtuali per server|Variabile, nessun limite imposto da Hyper-V|Il limite effettivo dipende dalle risorse di elaborazione disponibili.|  
|Processori virtuali per processore logico|Nessun rapporto imposto da Hyper-V|Nessuno.|  
|Processori virtuali per server|2048|Nessuno.|  
|Reti di archiviazione (SAN, Storage Area Network) virtuali|Nessun limite imposto da Hyper-V|Nessuno.|  
|Commutatori virtuali|Variabile, nessun limite imposto da Hyper-V|Il limite effettivo dipende dalle risorse di elaborazione disponibili.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Cluster di failover e Hyper-V  
Questa tabella elenca i valori massimi applicabili quando si usa Hyper-V e Clustering di Failover. È importante eseguire operazioni di pianificazione della capacità per assicurarsi che siano presenti risorse hardware sufficienti per eseguire tutte le macchine virtuali in un ambiente cluster.  

Per altre informazioni sugli aggiornamenti per il Clustering di Failover, tra cui nuove funzionalità per le macchine virtuali, vedere [What ' s New in Failover Clustering in Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Component|Massimo|Note|  
|-------------|-----------|---------|  
|Nodi per cluster|64|Tenere presenti il numero di nodi che si desidera riservare per il failover nonché le attività di manutenzione come l'installazione di aggiornamenti. È consigliabile pianificare un numero di risorse sufficienti in modo da poter riservare 1 nodo per il failover, il che significa che tale nodo rimane inattivo fino a quando un altro nodo non esegue il failover (il nodo inattivo è anche noto come nodo passivo). È possibile aumentare questo numero se si desidera riservare ulteriori nodi. Non è rapporto consigliato o moltiplicatore di nodi riservati per i nodi attivi. l'unico requisito è che il numero totale di nodi in un cluster non può superare il numero massimo di 64.|  
|Macchine virtuali in esecuzione per cluster e per nodo|8.000 per cluster|Diversi fattori possono influenzare il numero reale di macchine virtuali che è possibile eseguire contemporaneamente in un nodo, ad esempio:<br />-Quantità di memoria fisica utilizzata da ogni macchina virtuale.<br />-Funzionalità di rete e larghezza di banda di archiviazione.<br />-Numero di assi del disco, che influisce sulle prestazioni dei / o disco.|  
  

