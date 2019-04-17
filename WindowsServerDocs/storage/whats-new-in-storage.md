---
ms.assetid: 0f2a7f7b-aca8-4e5d-ad67-4258e88bc52f
title: "Novità nell'archiviazione in Windows Server"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: dongill
ms.technology: storage
ms.topic: article
author: kumudd
ms.date: 09/15/2016
ms.openlocfilehash: 9aab6246f7ddc86629834bf20a7d21cc4ce2ec8f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-storage-in-windows-server-2016"></a>Novità nell'archiviazione in Windows Server 2016

>Si applica a: Windows Server 2016

Questo argomento illustra le funzionalità nuove e modificate di Archiviazione in Windows Server 2016.

## <a name="s2d"></a>Spazi di archiviazione diretti  
Spazi di archiviazione diretta consente di creare un'archiviazione altamente disponibile e scalabile con server di archiviazione locale. Questa funzionalità semplifica la distribuzione e la gestione dei sistemi con archiviazione definita dal software e consente l'uso di nuove classi di dispositivi disco, ad esempio le unità SSD SATA e NVMe, che in precedenza non erano disponibili con spazi di archiviazione raggruppati in cluster che includevano i dischi condivisi.  

**Valore aggiunto da queste modifiche**  
Spazi di archiviazione diretta consente ai service provider e alle aziende di usare server di standard industriale con archiviazione locale per creare un'archiviazione altamente disponibile e scalabile definita dal software. L'uso di server con archiviazione locale riduce la complessità, aumenta la scalabilità e consente l'uso di dispositivi di archiviazione che non erano utilizzabili in precedenza, ad esempio i dischi a stato solido SATA per ridurre il costo dell'archiviazione flash o i dischi a stato solido NVMe per ottenere prestazioni migliori.  

Spazi di archiviazione diretta non richiede un'infrastruttura SAS condivisa e per questo semplifica i processi di distribuzione e configurazione. Essa sfrutta la rete come un'infrastruttura di archiviazione, usando SMB3 e SMB diretto (RDMA) per un'archiviazione efficiente con CPU ad alta velocità e bassa latenza. Per la scalabilità orizzontale, è sufficiente aggiungere più server per aumentare la capacità di archiviazione e le prestazioni delle operazioni di I/O  
Per altre informazioni, vedere [Storage Spaces Direct in Windows Server 2016](storage-spaces/storage-spaces-direct-overview.md) (Spazi di archiviazione diretta in Windows Server 2016).  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016.  

## <a name="storage-replica"></a>Replica archiviazione  
Replica di archiviazione consente di eseguire la replica sincrona a livello di blocco, indipendente dall'archiviazione, tra server o cluster per il ripristino di emergenza, nonché l'estensione di un cluster di failover tra siti. La replica sincrona consente il mirroring dei dati in siti fisici con volumi coerenti per arresto anomalo del sistema senza perdere dati a livello di file system. La replica asincrona consente l'estensione del sito oltre le aree metropolitane con la possibilità di perdita di dati.  

**Valore aggiunto da queste modifiche**  
Replica archiviazione consente di eseguire le operazioni seguenti:  

* Offrire una soluzione singola di ripristino di emergenza per le interruzioni pianificate e non pianificate di carichi di lavoro di importanza critica.
* Usare il trasporto SMB3 con prestazioni, scalabilità e affidabilità comprovati.
* Estendere i cluster di failover di Windows su distanze metropolitane.
* Usare i software Microsoft end-to-end per l'archiviazione e il clustering, ad esempio Hyper-V, Replica archiviazione, Spazi di archiviazione, cluster, File server di scalabilità orizzontale, SMB3, deduplicazione e NTFS o ReFS.
* Aiuta a ridurre costi e complessità come indicato di seguito: 
    * È indipendente dall'hardware e non necessita di una configurazione di archiviazione specifica come SAN o DAS.
    * Consente le tecnologie di rete e di archiviazione.
    * Presenta una gestione grafica semplice per nodi e cluster singoli tramite Gestione cluster di failover.
    * Include le opzioni di scripting complete e su larga scala tramite Windows PowerShell. 
* Consente di ridurre i tempi di inattività e di aumentare l'affidabilità e la produttività di Windows.  
* Offre possibilità di supporto, metriche delle prestazioni e funzionalità di diagnostica.  

Per altre informazioni, vedere [Replica archiviazione in Windows Server 2016](storage-replica/storage-replica-overview.md).  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016.  

## <a name="storage-qos"></a>QoS di archiviazione  
È ora possibile usare Qualità del servizio (QoS) di archiviazione per monitorare in modo centralizzato le prestazioni di archiviazione end-to-end e creare criteri di gestione usando cluster Hyper-V e CSV in Windows Server 2016.  

**Valore aggiunto da queste modifiche**  
È ora possibile creare criteri QoS di archiviazione in un cluster CSV e assegnarli a uno o più dischi virtuali in macchine virtuali Hyper-V. Le prestazioni di archiviazione vengono regolate automaticamente in modo da soddisfare i criteri man mano che cambiano i carichi di lavoro e di archiviazione.  

* Ogni criterio specifica una riserva (minima) e/o un limite (massimo) da applicare a una raccolta di flussi di dati, ad esempio un disco rigido virtuale, una macchina virtuale singola o un gruppo di macchine virtuali, un servizio o un tenant.  
* Usando Windows PowerShell o WMI è possibile eseguire le attività seguenti:  
    * Creare criteri in un cluster CSV.
    * Enumerare i criteri disponibili in un cluster CSV.
    * Assegnare un criterio a un disco rigido virtuale in una macchina virtuale Hyper-V. 
    * Monitorare le prestazioni di ogni flusso e lo stato all'interno del criterio.  
* Se più dischi rigidi virtuali condividono lo stesso criterio, le prestazioni vengono distribuite equamente per soddisfare la richiesta nell'intervallo del valore minimo e massimo del criterio. Pertanto, un criterio consente di gestire un disco rigido virtuale, una macchina virtuale, più macchine virtuali che comprendono un servizio o tutte le macchine virtuali di proprietà di un tenant.  

**Differenze di funzionamento**  
Questa funzionalità è stata introdotta in Windows Server 2016. La gestione delle riserve minime, il monitoraggio dei flussi di tutti i dischi virtuali nel cluster tramite un comando singolo e la gestione centralizzata basata su criteri non erano possibili nelle versioni precedenti di Windows Server.  

Per altre informazioni, vedere [QoS di archiviazione](storage-qos/storage-qos-overview.md)

## <a name="dedup"></a>Deduplicazione dati  
| Funzionalità | Novità o aggiornamento | Descrizione |
|---------------|----------------|-------------|
| [Supporto per volumi di grandi dimensioni](data-deduplication/whats-new.md#large-volume-support) | Aggiornamento | Prima di Windows Server 2016 i volumi dovevano essere ridimensionati in modo specifico per la varianza prevista e i volumi di dimensioni superiori a 10 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 la deduplicazione dati supporta dimensioni di volume **fino a 64 TB**. |
| [Supporto per file di grandi dimensioni](data-deduplication/whats-new.md#large-file-support) | Aggiornamento | Prima di Windows Server 2016 i file che raggiungevano 1 TB non erano buoni candidati per la deduplicazione. In Windows Server 2016 i file **fino a 1 TB** sono completamente supportati. |
| [Supporto per Nano Server](data-deduplication/whats-new.md#nano-server-support) | Nuovo | La deduplicazione dati è disponibile e completamente supportata nella nuova opzione di distribuzione Nano Server per Windows Server 2016. |
| [Supporto del backup semplificato](data-deduplication/whats-new.md#simple-backup-support) | Nuovo | In Windows Server 2012 R2 le applicazioni di backup virtualizzato come [Data Protection Manager](https://technet.microsoft.com/en-us/library/hh758173.aspx) di Microsoft erano supportati grazie a una serie di passaggi di configurazione manuale. In Windows Server 2016 è stato aggiunto il nuovo tipo di utilizzo predefinito "Backup", per una distribuzione semplice della deduplicazione dati per le applicazioni di backup virtualizzato. |
| [Supporto per gli aggiornamenti in sequenza del sistema operativo cluster](data-deduplication/whats-new.md#cluster-upgrade-support) | Nuovo | La deduplicazione dati supporta la nuova funzionalità [Aggiornamento in sequenza del sistema operativo cluster](..//failover-clustering/cluster-operating-system-rolling-upgrade.md) di Windows Server 2016. |

## <a name="smb-hardening-improvements"></a>Miglioramenti della protezione avanzata di SMB per le connessioni SYSVOL e NETLOGON  
In Windows 10 e Windows Server 2016 le connessioni client alle condivisioni SYSVOL e NETLOGON predefinite di Active Directory Domain Services sui controller di dominio richiedono ora la firma SMB e l'autenticazione reciproca (ad esempio Kerberos).   

**Valore aggiunto da queste modifiche**  
Questa modifica riduce il rischio di attacchi man-in-the-middle.   

**Cosa funziona in modo diverso?**  
Se la firma SMB e l'autenticazione reciproca non sono disponibili,un computer Windows 10 o Windows Server 2016 non eseguirà i criteri di gruppo e gli script basati su dominio.  

> [!NOTE]  
> I valori del Registro di sistema per queste impostazioni non sono presenti per impostazione predefinita, ma le regole di protezione avanzata si applicano comunque fino a quando non sono oggetto di override da criteri di gruppo o altri valori del Registro di sistema.  

Per altre informazioni su questi miglioramenti alla sicurezza, noti anche come protezione avanzata UNC, vedere l'articolo della Knowledge Base di Microsoft [3000483](http://support.microsoft.com/kb/3000483) e [MS15-011 & MS15-014: Hardening Group Policy](http://blogs.technet.microsoft.com/srd/2015/02/10/ms15-011-ms15-014-hardening-group-policy) (MS15-011 & MS15-014: protezione avanzata dei criteri di gruppo).  

## <a name="work-folders"></a>Cartelle di lavoro
Miglioramento della notifica delle modifiche quando il server di Cartelle di lavoro esegue Windows Server 2016 e il client di Cartelle di lavoro è Windows 10.

**Valore aggiunto da queste modifiche**<br>
Con Windows Server 2012 R2, quando le modifiche ai file vengono sincronizzate con il server di Cartelle di lavoro, i client non ricevono una notifica delle modifiche e l'aggiornamento può arrivare anche 10 minuti dopo.  Con Windows Server 2016, l'invio delle notifiche ai client di Windows 10 da parte del server di Cartelle di lavoro e la sincronizzazione conseguente delle modifiche dei file sono immediati.

**Differenze di funzionamento**<br>
Questa funzionalità è stata introdotta in Windows Server 2016. Richiede un server Cartelle di lavoro con Windows Server 2016 e il client deve essere Windows 10.

Se si usa un client precedente o il server Cartelle di lavoro è Windows Server 2012 R2, il client continuerà a eseguire il polling delle modifiche ogni 10 minuti.

## <a name="refs"></a>ReFS 
La successiva iterazione di ReFS offre il supporto per distribuzioni di archiviazione su larga scala, con carichi di lavoro diversificati, al fine di fornire affidabilità, resilienza e scalabilità per i dati.     

**Valore aggiunto da queste modifiche**<br>
ReFS introduce i seguenti miglioramenti:

* ReFS implementa la nuova funzionalità di livelli di archiviazione, consentendo così prestazioni più rapide e maggiore capacità di archiviazione. La nuova funzionalità offre le seguenti caratteristiche:
    * Più tipi di resilienza sullo stesso disco virtuale (con la possibilità di utilizzare il mirroring nel livello delle prestazioni e la parità nel livello della capacità, ad esempio).
    * Maggiore velocità di risposta ai working set di deriva. 
    * Supporto dei supporti di memorizzazione SMR (Shingled Magnetic Recording). 
* L'introduzione della clonazione dei blocchi migliora in modo sostanziale le prestazioni delle operazioni VM, ad esempio le operazioni di unione del checkpoint .vhdx.
* Il nuovo strumento di analisi ReFS consente di ripristinare una risorsa di archiviazione persa e di recuperare i dati da danneggiamenti critici. 

**Differenze di funzionamento**<br>
Queste funzionalità sono nuove in Windows Server 2016. 

## <a name="see-also"></a>Vedere anche  
* [Novità di Windows Server 2016](../get-started/what-s-new-in-windows-server-2016.md)  
