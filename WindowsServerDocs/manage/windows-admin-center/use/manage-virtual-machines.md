---
title: Gestione delle macchine virtuali con Windows Admin Center
description: La gestione di host Hyper-V e macchine virtuali con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 84e1ce7864f04550ee25253bcf038afdd7b919fe
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811681"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gestione delle macchine virtuali con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Lo strumento di macchine virtuali è disponibile nel [Server](manage-servers.md), [Cluster di Failover](manage-failover-clusters.md) oppure [Cluster Hyper-Converged](manage-hyper-converged.md) connessioni se il ruolo Hyper-V è abilitato nel server o del cluster. È possibile usare lo strumento di macchine virtuali per gestire gli host Hyper-V che eseguono Windows Server 2012 o versioni successive, installato uno con esperienza Desktop o Server Core. Hyper-V Server 2012, sono inoltre supportati 2016 e 2019.

## <a name="key-features"></a>Funzionalità principali

I vantaggi dello strumento di macchine virtuali in Windows Admin Center includono:

- **Monitoraggio ad alto livello Hyper-V host risorse.** Visualizzazione complessiva dell'utilizzo della CPU e memoria, le metriche delle prestazioni dei / o, gli avvisi sull'integrità della macchina virtuale e gli eventi per il server host Hyper-V o intero cluster in un singolo dashboard.
- **Esperienza unificata innovativo, riunisce funzionalità di gestione di Hyper-V e gestione Cluster di Failover.** Visualizzare tutte le macchine virtuali in un cluster e il drill-down in una singola macchina virtuale per Gestione avanzata e la risoluzione dei problemi.
- **Semplificati ma potenti flussi di lavoro per la gestione delle macchine virtuali.** Nuove esperienze dell'interfaccia utente su misura per scenari per l'amministrazione IT a creare, gestire e replicare le macchine virtuali.

Ecco alcune delle attività di Hyper-V che è possibile eseguire in Windows Admin Center:

- [Monitorare le prestazioni e risorse dell'host Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Visualizzare l'inventario macchina virtuale](#view-virtual-machine-inventory)
- [Creare una nuova macchina virtuale](#create-a-new-virtual-machine)
- [Modificare le impostazioni della macchina virtuale](#change-virtual-machine-settings)
- [Migrazione in tempo reale di una macchina virtuale a un altro nodo del cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modificare le impostazioni di host Hyper-V](#change-hyper-v-host-settings)
- [Visualizzare i registri eventi di Hyper-V](#view-hyper-v-event-logs)
- [Proteggere le macchine virtuali con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Monitorare le prestazioni e risorse dell'host Hyper-V

![Schermata di riepilogo di macchine virtuali](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Sono disponibili due schede nella parte superiore del **macchine virtuali** dello strumento, il **riepilogo** scheda e il **inventario** scheda. Il **riepilogo** scheda fornisce una visione olistica di risorse dell'host Hyper-V e delle prestazioni per il server corrente o intero cluster, inclusi i seguenti:
    - Il numero di macchine virtuali raggruppate per lo stato, in esecuzione, impostata su off, in pausa e salvato
    - Gli avvisi di integrità recenti o gli eventi del registro eventi di Hyper-V (gli avvisi sono solo disponibili per i cluster iperconvergente che esegue Windows Server 2016 o versioni successive)
    - Utilizzo della CPU e memoria con suddivisione di host e guest
    - Principali macchine virtuali utilizzano più risorse di CPU e memoria
    - Riga di dati in tempo reale e cronologici grafici per la velocità effettiva dei / o e IOPS (grafici a linee delle prestazioni di archiviazione sono solo disponibili per i cluster iperconvergente che esegue Windows Server 2016 o versioni successive. I dati cronologici sono disponibili solo per i cluster iperconvergente che esegue Windows Server 2019)

## <a name="view-virtual-machine-inventory"></a>Visualizzare l'inventario macchina virtuale

![Schermata di inventario di macchine virtuali](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Sono disponibili due schede nella parte superiore del **macchine virtuali** dello strumento, il **riepilogo** scheda e il **inventario** scheda. Il **inventario** scheda sono elencate le macchine virtuali disponibili nel server corrente o intero cluster, nonché i comandi per gestire singole macchine virtuali. È possibile:
    - Visualizzare un elenco delle macchine virtuali in esecuzione nel server corrente o del cluster.
    - Visualizzare il server di stato e l'host della macchina virtuale se si siano visualizzando le macchine virtuali per un cluster. Visualizzare anche l'utilizzo di CPU e memoria dalla prospettiva dell'host, tra cui utilizzo elevato della memoria, la richiesta di memoria e memoria assegnata e della macchina virtuale il tempo di attività, lo stato di heartbeat e lo stato di protezione tramite Azure Site Recovery.
    - [Creare una nuova macchina virtuale](#create-a-new-virtual-machine).
    - Elimina, avviare, disattivare, arrestare, sospendere, riprendere, reimpostare o rinominare una macchina virtuale. Inoltre salvare la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni per una macchina virtuale](#change-virtual-machine-settings).
    - Connettersi a una console di macchina virtuale tramite VMConnect tramite l'host Hyper-V.
    - [Replicare una macchina virtuale usando Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Per le operazioni che possono essere eseguite in più macchine virtuali, ad esempio avviare, arrestare, salvare, Pause, Elimina, Ripristina, è possibile selezionare più macchine virtuali ed eseguire l'operazione in una sola volta.

NOTA: Se si è connessi a un cluster, lo strumento di macchina virtuale verrà visualizzati solo macchine virtuali del cluster. Si prevede di visualizzare anche le macchine virtuali non cluster in futuro.

## <a name="create-a-new-virtual-machine"></a>Creare una nuova macchina virtuale

![Creare una nuova macchina virtuale schermata](../media/manage-virtual-machines/new-vm.png)

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** tab, quindi fare clic su **New** per creare una nuova macchina virtuale.
3. Immettere il nome della macchina virtuale e scegliere tra le macchine virtuali di generazione 1 e 2.
4. Se si sta creando una macchina virtuale in un cluster, è possibile scegliere quale host da creare inizialmente la macchina virtuale su. Se si esegue Windows Server 2016 o versione successiva, lo strumento fornirà un Consiglio per gli host per l'utente.
5. Scegliere un percorso per i file della macchina virtuale. Scegliere un volume nell'elenco a discesa oppure fare clic su **esplorare** per scegliere una cartella utilizzando il selettore di cartella. Il file di configurazione macchina virtuale e il file di disco rigido virtuale verrà salvati in una singola cartella sotto la `\Hyper-V\\[virtual machine name]` percorso del volume selezionato o del percorso.

   >[!Tip]
   > Nel selettore di cartella, è possibile passare a una condivisione SMB disponibile in rete immettendo il percorso nel **nomecartella** campo come ```\\server\share```. Usa una condivisione di rete per l'archiviazione della macchina virtuale richiederà [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Scegliere il numero di processori virtuali, se si desidera che la virtualizzazione annidata abilitata, configurare le impostazioni della memoria, schede di rete, dischi rigidi virtuali e scegliere se si desidera installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fare clic su **Crea** per creare la macchina virtuale.
8. Una volta che la macchina virtuale viene creata e visualizzato nell'elenco di macchine virtuali, è possibile avviare la macchina virtuale.
9. Una volta che viene avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco, fare clic su **altre** > **Connect** per scaricare il file con estensione rdp. Nell'app connessione Desktop remoto, aprire il file con estensione rdp. Poiché questo si connette alla console della macchina virtuale, sarà necessario immettere le credenziali di amministratore dell'host Hyper-V.

## <a name="change-virtual-machine-settings"></a>Modificare le impostazioni della macchina virtuale

![Schermata delle impostazioni della macchina virtuale](../media/manage-virtual-machines/vm-settings.png)

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** scheda. Scegliere una macchina virtuale dall'elenco e fare clic su **altre** > **impostazioni**.
3. Spostarsi tra i **generali**, **Security**, **memoria**, **processori**, **dischi**, **Networks**, **ordine di avvio** e **checkpoint** scheda, configurare le impostazioni necessarie, quindi fare clic su **Salva** per salvare la scheda corrente Impostazioni. Le impostazioni disponibili variano a seconda della generazione della macchina virtuale. Inoltre, alcune impostazioni non possono essere modificate per l'esecuzione di macchine virtuali e sarà necessario arrestare la macchina virtuale prima di tutto.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Migrazione in tempo reale di una macchina virtuale a un altro nodo del cluster

Se si è connessi a un cluster, è possibile migrare una macchina virtuale in tempo reale a un altro nodo del cluster.

1. Da una connessione cluster iperconvergente o un Cluster di Failover, fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** scheda. Scegliere una macchina virtuale dall'elenco e fare clic su **altre** > **spostare**.
3. Scegliere un server dall'elenco dei nodi del cluster disponibile e fare clic su **spostare**.
4. Le notifiche per lo stato di avanzamento di spostamento verranno visualizzate nell'angolo superiore destro di Windows Admin Center. Se lo spostamento ha esito positivo, si verrà visualizzato il nome del server Host modificato nell'elenco di macchine virtuali.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale

![Schermata dei dettagli macchina virtuale singola](../media/manage-virtual-machines/vm-details.png)

È possibile visualizzare informazioni dettagliate e grafici delle prestazioni per una singola macchina virtuale dalla macchina virtuale singola pagina.

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** scheda. Fare clic sul nome di una macchina virtuale dall'elenco di macchine virtuali.
3. Dalla pagina singola macchina virtuale, è possibile:
    - Visualizzare informazioni dettagliate per la macchina virtuale.
    - Visualizzazione Live e i grafici a linee i dati cronologici per CPU, memoria, rete e velocità effettiva dei / o e IOPS (i dati cronologici sono disponibili solo per i cluster iperconvergente che esegue Windows Server 2019)
    - Visualizzare, creare, applicare, rinominare ed eliminare i checkpoint.
    - Visualizzare i dettagli per i file di disco rigido virtuale (VHD), schede di rete e server host della macchina virtuale.
    - Elimina, avviare, disattivare, arrestare, sospendere, riprendere, reimpostare o rinominare la macchina virtuale. Inoltre salvare la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni per la macchina virtuale](#change-virtual-machine-settings).
    - Connettersi alla console macchina virtuale tramite VMConnect tramite l'host Hyper-V.
    - [Replicare la macchina virtuale usando Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)

![Macchina virtuale di connettersi tramite il web browser](../media/manage-virtual-machines/vm-connect.png)

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** scheda. Scegliere una macchina virtuale dall'elenco e fare clic su **altre** > **Connect** oppure **file Download RDP**. **Connettere** consentirà all'utente di interagire con il guest macchina virtuale tramite la console di web Desktop remoto, integrata in Windows Admin Center. **Scaricare il file RDP** scaricherà un file con estensione rdp che può essere aperto con l'applicazione connessione Desktop remoto (mstsc.exe). Entrambe le opzioni userà VMConnect per connettersi alla macchina virtuale guest tramite l'host Hyper-V e richiederanno di immettere le credenziali di amministratore per il server host Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Modificare le impostazioni di host Hyper-V

![Schermata delle impostazioni di host Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. In una connessione Server, Cluster iperconvergente o Cluster di Failover, fare clic sui **impostazioni** menu nella parte inferiore del riquadro di spostamento sinistra.
2. In un server host Hyper-V o cluster, si noterà una **impostazioni di Host Hyper-V** gruppo con le sezioni seguenti:
    - Generale: Tipo pianificazione percorso file delle macchine virtuali e dischi rigidi virtuale modifica e hypervisor (se supportato)
    - Modalità sessione avanzata
    - Spanning NUMA
    - Migrazione in tempo reale
    - Migrazione dell'archiviazione
3. Se si apportano le modifiche alle impostazioni in un Cluster iperconvergente o Cluster di Failover di una connessione qualsiasi host di Hyper-V, la modifica verrà applicata a tutti i nodi del cluster.

## <a name="view-hyper-v-event-logs"></a>Visualizzare i registri eventi di Hyper-V

È possibile visualizzare i registri eventi di Hyper-V direttamente dallo strumento di macchine virtuali.

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **riepilogo** scheda. Nella sezione superiore gli eventi di destra, fare clic su **tutti gli eventi di visualizzazione**.
3. Lo strumento Visualizzatore eventi visualizzerà i canali di eventi di Hyper-V nel riquadro sinistro. Scegliere un canale per visualizzare gli eventi nel riquadro di destra. Se si gestisce un cluster di failover o un cluster iperconvergente, i registri eventi verranno visualizzati gli eventi per tutti i nodi del cluster, la visualizzazione del server host nella colonna macchina.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Proteggere le macchine virtuali con Azure Site Recovery

Per configurare Azure Site Recovery e la replica delle macchine virtuali locali in Azure, è possibile usare Windows Admin Center. [Altre informazioni](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Ulteriori provenienti

Gestione delle macchine virtuali in Windows Admin Center è attivamente in fase di sviluppo e verranno aggiunte nuove funzionalità nel prossimo futuro. In UserVoice, è possibile visualizzare lo stato e votare funzionalità:

- [Importazione/esportazione di macchine virtuali](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Macchine virtuali di ordinamento nelle cartelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Supporta le impostazioni di macchina virtuale aggiuntiva](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Supporto per la Replica Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Delegare la proprietà macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Clonazione della macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Creare un modello da una macchina virtuale esistente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Visualizzare le macchine virtuali tra host Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurazione delle VLAN nel riquadro nuova macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Vedi tutto o proporre nuove funzionalità](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
