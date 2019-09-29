---
title: Gestione delle macchine virtuali con l'interfaccia di amministrazione di Windows
description: Gestione degli host Hyper-V e delle macchine virtuali con l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356864"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>Gestione delle macchine virtuali con l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Lo strumento macchine virtuali è disponibile in [Server](manage-servers.md), [cluster di failover](manage-failover-clusters.md) o connessioni [cluster iperconvergenti](manage-hyper-converged.md) se il ruolo Hyper-V è abilitato nel server o nel cluster. È possibile utilizzare lo strumento macchine virtuali per gestire gli host Hyper-V che eseguono Windows Server 2012 o versione successiva, installati con esperienza desktop o come server core. Sono supportati anche Hyper-V Server 2012, 2016 e 2019.

## <a name="key-features"></a>Funzionalità principali

Le caratteristiche principali dello strumento macchine virtuali nell'interfaccia di amministrazione di Windows includono:

- **Monitoraggio delle risorse dell'host Hyper-V di alto livello.** Visualizzare l'utilizzo complessivo della CPU e della memoria, le metriche delle prestazioni di i/o, gli avvisi di integrità della macchina virtuale e gli eventi per il server host Hyper-V o l'intero cluster in un unico dashboard.
- **Esperienza unificata con la console di gestione di Hyper-V e le funzionalità Gestione cluster di failover insieme.** Visualizzare tutte le macchine virtuali in un cluster ed eseguire il drill-down in un'unica macchina virtuale per la gestione e la risoluzione dei problemi avanzati.
- **Flussi di lavoro semplificati, ma potenti per la gestione delle macchine virtuali.** Nuove esperienze dell'interfaccia utente personalizzate per gli scenari di amministrazione IT per la creazione, la gestione e la replica di macchine virtuali.

Di seguito sono riportate alcune delle attività di Hyper-V che è possibile eseguire nell'interfaccia di amministrazione di Windows:

- [Monitorare le prestazioni e le risorse dell'host Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Visualizza inventario macchina virtuale](#view-virtual-machine-inventory)
- [Crea una nuova macchina virtuale](#create-a-new-virtual-machine)
- [Modificare le impostazioni della macchina virtuale](#change-virtual-machine-settings)
- [Eseguire la migrazione in tempo reale di una macchina virtuale in un altro nodo del cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modificare le impostazioni dell'host Hyper-V](#change-hyper-v-host-settings)
- [Visualizzare i log eventi di Hyper-V](#view-hyper-v-event-logs)
- [Proteggi le macchine virtuali con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>Monitorare le prestazioni e le risorse dell'host Hyper-V

![Schermata di riepilogo delle macchine virtuali](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Sono disponibili due schede nella parte superiore dello strumento **macchine virtuali** , la scheda **Riepilogo** e la scheda **inventario** . La scheda **Riepilogo** fornisce una visualizzazione olistica delle risorse e delle prestazioni dell'host Hyper-V per il server corrente o l'intero cluster, inclusi i seguenti:
    - Numero di macchine virtuali raggruppate in base allo stato, in esecuzione, disattivata, sospesa e salvata
    - Avvisi di integrità recenti o eventi del registro eventi di Hyper-V (gli avvisi sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2016 o versioni successive)
    - Utilizzo della CPU e della memoria con la suddivisione host rispetto a Guest
    - Prime macchine virtuali che utilizzano la maggior parte delle risorse di CPU e memoria
    - Grafici a linee di dati Live e cronologici per IOPS e velocità effettiva di i/o (i grafici a linee delle prestazioni di archiviazione sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2016 o versioni successive. I dati cronologici sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2019

## <a name="view-virtual-machine-inventory"></a>Visualizza inventario macchina virtuale

![Schermata di inventario delle macchine virtuali](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Sono disponibili due schede nella parte superiore dello strumento **macchine virtuali** , la scheda **Riepilogo** e la scheda **inventario** . La scheda **inventario** elenca le macchine virtuali disponibili nel server corrente o nell'intero cluster e fornisce i comandi per gestire le singole macchine virtuali. È possibile:
    - Visualizzare un elenco delle macchine virtuali in esecuzione nel server o nel cluster corrente.
    - Se si visualizzano macchine virtuali per un cluster, visualizzare lo stato e il server host della macchina virtuale. Visualizzare inoltre l'utilizzo della CPU e della memoria dal punto di vista dell'host, tra cui la quantità di memoria, la richiesta di memoria e la memoria assegnata, nonché lo stato di tempo di esecuzione della macchina virtuale, lo stato heartbeat e la protezione usando Azure Site Recovery
    - [Creare una nuova macchina virtuale](#create-a-new-virtual-machine).
    - Elimina, avvia, disattiva, arresta, Sospendi, Riprendi, Reimposta o rinomina una macchina virtuale. Salvare anche la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni di una macchina virtuale](#change-virtual-machine-settings).
    - Connettersi a una console della macchina virtuale usando VMConnect tramite l'host Hyper-V.
    - [Replicare una macchina virtuale utilizzando Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Per le operazioni che possono essere eseguite su più macchine virtuali, ad esempio avvio, arresto, salvataggio, pausa, eliminazione, reimpostazione, è possibile selezionare più macchine virtuali ed eseguire l'operazione in una sola volta.

NOTA: Se si è connessi a un cluster, lo strumento macchina virtuale visualizzerà solo le macchine virtuali in cluster. Si prevede di visualizzare anche le macchine virtuali non in cluster in futuro.

## <a name="create-a-new-virtual-machine"></a>Creare una nuova macchina virtuale

![Schermata Crea nuova macchina virtuale](../media/manage-virtual-machines/new-vm.png)

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** , quindi fare clic su **nuova** per creare una nuova macchina virtuale.
3. Immettere il nome della macchina virtuale e scegliere tra le macchine virtuali di prima e seconda generazione.
4. Se si sta creando una macchina virtuale in un cluster, è possibile scegliere su quale host creare inizialmente la macchina virtuale. Se si esegue Windows Server 2016 o versione successiva, lo strumento fornirà una raccomandazione per l'host.
5. Scegliere un percorso per i file della macchina virtuale. Scegliere un volume dall'elenco a discesa o fare clic su **Sfoglia** per scegliere una cartella tramite la selezione cartelle. I file di configurazione della macchina virtuale e il file del disco rigido virtuale verranno salvati in una singola `\Hyper-V\\[virtual machine name]` cartella nel percorso del volume o del percorso selezionato.

   >[!Tip]
   > Nella selezione cartelle è possibile passare a qualsiasi condivisione SMB disponibile in rete immettendo il percorso nel campo **nome cartella** come ```\\server\share```. Per usare una condivisione di rete per l'archiviazione delle macchine virtuali, è necessario [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Scegliere il numero di processori virtuali, se si desidera abilitare la virtualizzazione annidata, configurare le impostazioni della memoria, le schede di rete, i dischi rigidi virtuali e scegliere se installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fare clic su **Crea** per creare la macchina virtuale.
8. Dopo aver creato la macchina virtuale e averla visualizzata nell'elenco delle macchine virtuali, è possibile avviare la macchina virtuale.
9. Una volta avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco e fare clic su **altro** > **Connetti** per scaricare il file con estensione RDP. Aprire il file con estensione RDP nell'app Connessione Desktop remoto. Poiché si sta effettuando la connessione alla console della macchina virtuale, sarà necessario immettere le credenziali di amministratore dell'host Hyper-V.

## <a name="change-virtual-machine-settings"></a>Modificare le impostazioni della macchina virtuale

![Schermata delle impostazioni della macchina virtuale](../media/manage-virtual-machines/vm-settings.png)

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** . Scegliere una macchina virtuale dall'elenco e fare clic su **altre** **Impostazioni** > .
3. Passare tra la scheda **generale**, **sicurezza**, **memoria**, **processori**, **dischi**, **reti**, **ordine di avvio** e **Checkpoint** , configurare le impostazioni necessarie, quindi fare clic su **Salva** per salvare impostazioni della scheda corrente. Le impostazioni disponibili variano a seconda della generazione della macchina virtuale. Inoltre, alcune impostazioni non possono essere modificate per le macchine virtuali in esecuzione ed è necessario prima arrestare la macchina virtuale.

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>Eseguire la migrazione in tempo reale di una macchina virtuale in un altro nodo del cluster

Se si è connessi a un cluster, è possibile eseguire la migrazione in tempo reale di una macchina virtuale a un altro nodo del cluster.

1. Da un cluster di failover o una connessione cluster iperconvergente, fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** . Scegliere una macchina virtuale dall'elenco e fare clic su **altro** > **Sposta**.
3. Scegliere un server dall'elenco dei nodi del cluster disponibili e fare clic su **Sposta**.
4. Le notifiche per lo spostamento dello stato di avanzamento verranno visualizzate nell'angolo superiore destro dell'interfaccia di amministrazione di Windows. Se lo spostamento ha esito positivo, verrà visualizzato il nome del server host modificato nell'elenco della macchina virtuale.

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale

![Schermata dei dettagli di una singola macchina virtuale](../media/manage-virtual-machines/vm-details.png)

È possibile visualizzare informazioni dettagliate e grafici delle prestazioni per una singola macchina virtuale dalla pagina singola macchina virtuale.

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** . Fare clic sul nome di una macchina virtuale nell'elenco della macchina virtuale.
3. Dalla pagina singola macchina virtuale è possibile:
    - Visualizzare informazioni dettagliate per la macchina virtuale.
    - Visualizza i grafici a linee di dati Live e cronologici per CPU, memoria, rete, IOPS e velocità effettiva di i/o (i dati cronologici sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2019)
    - Visualizza, crea, applica, Rinomina ed Elimina i checkpoint.
    - Visualizzare i dettagli per i file del disco rigido virtuale (con estensione VHD) della macchina virtuale, le schede di rete e il server host.
    - Eliminare, avviare, disattivare, arrestare, sospendere, riprendere, reimpostare o rinominare la macchina virtuale. Salvare anche la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni per la macchina virtuale](#change-virtual-machine-settings).
    - Connettersi alla console della macchina virtuale usando VMConnect tramite l'host Hyper-V.
    - Eseguire [la replica della macchina virtuale utilizzando Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)

![Connessione della macchina virtuale tramite il Web browser](../media/manage-virtual-machines/vm-connect.png)

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** . Scegliere una macchina virtuale dall'elenco e fare clic su **altro** > **Connetti** o **Scarica file RDP**. **Connect** consente di interagire con la macchina virtuale guest tramite la console web di desktop remoto, integrata nell'interfaccia di amministrazione di Windows. **Scarica file RDP** consente di scaricare un file con estensione RDP che è possibile aprire con l'applicazione connessione Desktop remoto (mstsc. exe). Entrambe le opzioni utilizzeranno VMConnect per connettersi alla macchina virtuale guest tramite l'host Hyper-V e sarà necessario immettere le credenziali di amministratore per il server host Hyper-V.

## <a name="change-hyper-v-host-settings"></a>Modificare le impostazioni dell'host Hyper-V

![Schermata Impostazioni host Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. In un server, un cluster iperconvergente o una connessione cluster di failover, fare clic sul menu **Impostazioni** nella parte inferiore del riquadro di spostamento a sinistra.
2. In un cluster o un server host Hyper-V viene visualizzato un gruppo di **Impostazioni host Hyper-v** con le sezioni seguenti:
    - Generale Modificare i dischi rigidi virtuali e il percorso del file delle macchine virtuali e il tipo di pianificazione hypervisor (se supportato)
    - Modalità sessione avanzata
    - Spanning NUMA
    - Migrazione in tempo reale
    - Migrazione dell'archiviazione
3. Se si apportano modifiche alle impostazioni di un host Hyper-V in un cluster iperconvergente o in una connessione cluster di failover, la modifica verrà applicata a tutti i nodi del cluster.

## <a name="view-hyper-v-event-logs"></a>Visualizzare i log eventi di Hyper-V

È possibile visualizzare i log eventi di Hyper-V direttamente dallo strumento macchine virtuali.

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **Riepilogo** . Nella sezione in alto a destra fare clic su **Visualizza tutti gli eventi**.
3. Lo strumento Visualizzatore eventi mostrerà i canali degli eventi Hyper-V nel riquadro sinistro. Scegliere un canale per visualizzare gli eventi nel riquadro destro. Se si gestisce un cluster di failover o un cluster iperconvergente, nei registri eventi verranno visualizzati gli eventi per tutti i nodi del cluster, visualizzando il server host nella colonna computer.

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>Proteggi le macchine virtuali con Azure Site Recovery

È possibile usare l'interfaccia di amministrazione di Windows per configurare Azure Site Recovery e replicare le macchine virtuali locali in Azure. [Altre informazioni](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>Ulteriori informazioni

La gestione delle macchine virtuali nell'interfaccia di amministrazione di Windows è attivamente in fase di sviluppo e le nuove funzionalità verranno aggiunte a breve in futuro. È possibile visualizzare lo stato e votare le funzionalità in UserVoice:

- [Importazione/esportazione di macchine virtuali](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [Ordina macchine virtuali in cartelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [Supportare le impostazioni della macchina virtuale aggiuntive](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Supporto della replica Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [Delega proprietà macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [Clona macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [Creare un modello da una macchina virtuale esistente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [Visualizzare le macchine virtuali negli host Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [Configurare VLAN nel riquadro di una nuova macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[Vedere tutte le nuove funzionalità o proponerne di nuove](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D).
