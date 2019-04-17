---
title: Gestione delle macchine virtuali con Windows Admin Center
description: Gestire gli host Hyper-V e le macchine virtuali con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296753"
---
# Gestione delle macchine virtuali con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Lo strumento di macchine virtuali è disponibile nel [Server](manage-servers.md), [Cluster di Failover](manage-failover-clusters.md) o [Cluster iperconvergente](manage-hyper-converged.md) connessioni se è abilitato il ruolo Hyper-V nel server o cluster. È possibile utilizzare lo strumento di macchine virtuali per gestire gli host Hyper-V che eseguono Windows Server 2012 o versione successiva, sia installato con esperienza Desktop o come Server Core. Hyper-V Server 2012, sono supportati anche 2016 e 2019.

## Funzionalità principali

Effetti di evidenziazione dello strumento di macchine virtuali in Windows Admin Center includono:

- **Alto livello Hyper-V host il monitoraggio delle risorse.** Visualizzare complessiva utilizzo della CPU e memoria, le metriche delle prestazioni dei / o, avvisi di integrità della macchina virtuale e gli eventi per l'intero cluster o il server host Hyper-V in un singolo dashboard.
- **Esperienza unificata riunire funzionalità di gestione di Hyper-V e gestione Cluster di Failover.** Visualizzare tutte le macchine virtuali in un cluster e il drill-down una singola macchina virtuale per Gestione avanzata e risoluzione dei problemi.
- **Semplificata, ma potente i flussi di lavoro per la gestione delle macchine virtuali.** Nuove esperienze di interfaccia utente personalizzata per scenari di amministrazione IT per creare, gestire e replicare le macchine virtuali.

Ecco alcune delle attività di Hyper-V che è possibile eseguire in Windows Admin Center:

- [Monitorare le prestazioni e le risorse di host Hyper-V](#monitor-hyper-v-host-resources-and-performance)
- [Inventario della macchina virtuale visualizzazione](#view-virtual-machine-inventory)
- [Creare una nuova macchina virtuale](#create-a-new-virtual-machine)
- [Modificare le impostazioni di macchina virtuale](#change-virtual-machine-settings)
- [Live una macchina virtuale di eseguire la migrazione a un altro nodo del cluster](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [Modificare le impostazioni di host Hyper-V](#change-hyper-v-host-settings)
- [Visualizzare i registri eventi di Hyper-V](#view-hyper-v-event-logs)
- [Proteggere le macchine virtuali con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)

## Monitorare le prestazioni e le risorse di host Hyper-V

![Schermata Riepilogo di macchine virtuali](../media/manage-virtual-machines/virtual-machines-summary.png)

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Esistono due schede nella parte superiore dello strumento di **macchine virtuali** , la scheda di **Riepilogo** e la scheda di **inventario** . Scheda **Riepilogo** fornisce una visione olistica delle risorse di host Hyper-V e le prestazioni per il server corrente o l'intero cluster, inclusi i seguenti:
    - Il numero di macchine virtuali raggruppati per stato - in esecuzione, off, pausa e salvato
    - Gli avvisi di integrità recenti o gli eventi di registro eventi Hyper-V (gli avvisi sono solo disponibili per i cluster iperconvergenti che eseguono Windows Server 2016 o versione successiva)
    - Utilizzo della CPU e memoria con suddivisione guest in host Visual Studio
    - Macchine virtuali principali più risorse di CPU e memoria
    - I dati reali e cronologici di riga grafici per la velocità effettiva e IOPS (grafici riga delle prestazioni di archiviazione sono solo disponibili per i cluster iperconvergenti che eseguono Windows Server 2016 o versione successiva. I dati storici sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2019)

## Inventario della macchina virtuale visualizzazione

![Schermata di inventario di macchine virtuali](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Esistono due schede nella parte superiore dello strumento di **macchine virtuali** , la scheda di **Riepilogo** e la scheda di **inventario** . La scheda di **inventario** sono elencate le macchine virtuali disponibile per il server corrente o l'intero cluster e fornisce i comandi per gestire le macchine virtuali singoli. È possibile:
    - Visualizzare un elenco delle macchine virtuali in esecuzione nel server corrente o al cluster.
    - Visualizzare i server di stato e l'host della macchina virtuale se stai visualizzando le macchine virtuali per un cluster. Anche visualizzare l'utilizzo della CPU e memoria dal punto di vista host, tra cui pressione della memoria, la richiesta di memoria e memoria assegnata e la macchina virtuale tempo di attività, lo stato di heartbeat e lo stato di protezione tramite Azure Site Recovery.
    - [Crea una nuova macchina virtuale](#create-a-new-virtual-machine).
    - Eliminare, avviare, disattivare, arrestare, sospendere, riprendere, reimpostare o rinominare una macchina virtuale. Anche salvare la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni per una macchina virtuale](#change-virtual-machine-settings).
    - Connettersi a una console di macchina virtuale con VMConnect tramite l'host Hyper-V.
    - [Replicare una macchina virtuale con Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).
    - Per le operazioni che possono essere eseguite su più macchine virtuali, ad esempio Start, arresto, Salva, pausa, eliminare, ripristino, è possibile selezionare più macchine virtuali ed eseguire l'operazione alla volta.

Nota: Se si è connessi a un cluster, lo strumento di macchina virtuale visualizzerà solo macchine virtuali del cluster. Prevediamo di mostrare anche le macchine virtuali non in cluster in futuro.

## Creare una nuova macchina virtuale

![Creare una nuova macchina virtuale schermata](../media/manage-virtual-machines/new-vm.png)

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegli la scheda di **inventario** e quindi fai clic di **Nuovo** per creare una nuova macchina virtuale.
3. Immetti il nome della macchina virtuale e scegliere tra le macchine virtuali della generazione 1 e 2.
4. Se stai creando una macchina virtuale in un cluster, è possibile scegliere quale host inizialmente creare la macchina virtuale in. Se si esegue Windows Server 2016 o versioni successive, lo strumento fornirà un Consiglio sull'host per te.
5. Scegliere un percorso per i file della macchina virtuale. Scegli un volume nell'elenco a discesa o fai clic su **Sfoglia** per selezionare una cartella tramite selezione cartelle. Il file di configurazione macchina virtuale e file di disco rigido virtuale verrà salvate in una singola cartella sotto la `\Hyper-V\\[virtual machine name]` percorso del volume selezionato o del percorso.

>[!Tip]
> Nel selettore di cartella, è possibile passare qualsiasi condivisione SMB disponibili nella rete immettendo il percorso nel campo **nome della cartella** come ```\\server\share```. Con una condivisione di rete per l'archiviazione della macchina virtuale richiederà [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp).

6. Scegli il numero di processori virtuali, se vuoi virtualizzazione annidata abilitata, configurare le impostazioni della memoria, le schede di rete, i dischi rigidi virtuali e scegliere se si desidera installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fai clic su **Crea** per creare la macchina virtuale.
8. Una volta che la macchina virtuale viene creata e viene visualizzato nell'elenco di macchina virtuale, è possibile avviare la macchina virtuale.
9. Una volta che viene avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco, fai clic su **altre** > **Connect** per scaricare il file RDP. Apri il file RDP nell'app di connessione Desktop remoto. Dato che questo si connette alla console della macchina virtuale, dovrai immettere le credenziali di amministratore dell'host Hyper-V.

## Modificare le impostazioni di macchina virtuale

![Schermata delle impostazioni di macchina virtuale](../media/manage-virtual-machines/vm-settings.png)

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere la scheda di **inventario** scegliere una macchina virtuale dall'elenco e fai clic su **altre** > **Impostazioni**.
3. Passare tra la scheda **Generale**, **sicurezza**, **memoria**, **processori**, **i dischi**, **reti**, **ordine di avvio** e **checkpoint** , configurare le impostazioni necessarie, quindi fare clic su **Save** per salvare impostazioni della scheda corrente. Le impostazioni disponibili variano a seconda generazione della macchina virtuale. Inoltre, alcune impostazioni non possono essere modificati per l'esecuzione di macchine virtuali ed è necessario arrestare prima di tutto la macchina virtuale.

## Live una macchina virtuale di eseguire la migrazione a un altro nodo del cluster

Se si è connessi a un cluster, è possibile migrare animati una macchina virtuale in un altro nodo del cluster.

1. Da un Cluster di Failover o una connessione di cluster iperconvergenti, fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere la scheda di **inventario** scegliere una macchina virtuale dall'elenco e fai clic su **altre** > **spostare**.
3. Scegliere un server dall'elenco dei nodi del cluster disponibili e **spostare**.
4. Le notifiche per lo stato di avanzamento di movimento verranno visualizzate nell'angolo superiore destro di Windows Admin Center. Se lo spostamento ha esito positivo, vedrai il nome del server Host modificato nell'elenco macchina virtuale.

## Gestione avanzata e risoluzione dei problemi per una singola macchina virtuale

![Schermata di dettagli singola macchina virtuale](../media/manage-virtual-machines/vm-details.png)

Puoi visualizzare informazioni dettagliate e grafici delle prestazioni per una singola macchina virtuale dalla macchina virtuale singola pagina.

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegli la scheda di **inventario** clic sul nome di una macchina virtuale nell'elenco di macchina virtuale.
3. Dalla pagina singola macchina virtuale, è possibile:
    - Visualizzare informazioni dettagliate per la macchina virtuale.
    - Visualizzare i dati storici riga grafici e Live per CPU, memoria, rete, la velocità effettiva e IOPS (dati cronologici sono disponibili solo per i cluster iperconvergenti che eseguono Windows Server 2019)
    - Visualizzare, creare, applicare, Rinomina ed eliminare i checkpoint.
    - Visualizzare i dettagli per i file di disco rigido virtuale (VHD), le schede di rete e server host della macchina virtuale.
    - Eliminare, avviare, disattivare, arrestare, sospendere, riprendere, reimpostare o rinominare la macchina virtuale. Anche salvare la macchina virtuale, eliminare uno stato salvato o creare un checkpoint.
    - [Modificare le impostazioni per la macchina virtuale](#change-virtual-machine-settings).
    - Connettersi alla console di macchina virtuale usando VMConnect tramite l'host Hyper-V.
    - [La replica della macchina virtuale tramite Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery).

## Gestire una macchina virtuale tramite l'host Hyper-V (VMConnect)

![Macchina virtuale si connettono tramite il browser web](../media/manage-virtual-machines/vm-connect.png)

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere la scheda di **inventario** scegliere una macchina virtuale dall'elenco e fai clic su **altre** > **Connect** o **file scaricare RDP**. **Connetti** consentirà di interagire con il macchina virtuale guest tramite la console di web Desktop remoto, integrata in Windows Admin Center. **RDP scaricare file** verrà scaricato un file RDP che è possibile aprire con l'applicazione di connessione Desktop remoto (mstsc.exe). Entrambe le opzioni userà VMConnect per connettersi alla macchina virtuale guest tramite l'host Hyper-V e verranno richiesto di immettere le credenziali di amministratore per il server host Hyper-V.

## Modificare le impostazioni di host Hyper-V

![Schermata delle impostazioni host Hyper-V](../media/manage-virtual-machines/host-settings.png)

1. Su una connessione Server, Hyper-converged Cluster o Cluster di Failover, fai clic sul menu delle **Impostazioni** nella parte inferiore del riquadro di spostamento a sinistra.
2. In un server host Hyper-V o cluster, vedrai un gruppo di **Impostazioni Host Hyper-V** con le sezioni seguenti:
    - Generale: Percorso file macchine virtuali e dischi rigidi virtuale modifica e hypervisor pianificare tipo (se supportato)
    - Modalità sessione avanzata
    - I valori NUMA
    - Migrazione in tempo reale
    - Migrazione dell'archiviazione
3. Se apporti modifiche dell'impostazione in connessione a un Cluster iperconvergente o Cluster di Failover qualsiasi host di Hyper-V, le modifiche verranno applicate a tutti i nodi del cluster.

## Visualizzare i registri eventi di Hyper-V

Puoi visualizzare i registri eventi di Hyper-V direttamente dallo strumento di macchine virtuali.

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere la scheda di **Riepilogo** . Nella sezione, gli eventi superiore destra, fai clic su **Tutti gli eventi di visualizzazione**.
3. Lo strumento Visualizzatore eventi visualizza i canali di evento di Hyper-V nel riquadro sinistro. Scegliere un canale di visualizzare gli eventi nel riquadro destro. Se si sta gestendo un cluster di failover o di un cluster iperconvergente, i registri eventi verranno visualizzati gli eventi per tutti i nodi del cluster, Visualizza il server host nella colonna computer.

## Proteggere le macchine virtuali con Azure Site Recovery

È possibile utilizzare Windows Admin Center per configurare Azure Site Recovery e replica delle macchine virtuali in locale in Azure. [Altre informazioni](../azure/azure-site-recovery.md)

## Altre prossime

Gestione delle macchine virtuali in Windows Admin Center è attivamente in fase di sviluppo e verranno aggiunte nuove funzionalità in futuro. Visualizzare lo stato e vota per funzionalità UserVoice:

|Richiesta di funzionalità|
|-------|
|[Importazione/esportazione di macchine virtuali](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[Macchine virtuali di ordinamento nelle cartelle](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[Il supporto delle impostazioni aggiuntive macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[Supporto di Replica Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[Delegare la proprietà di macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[Clonare macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[Creare un modello da una macchina virtuale esistente](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[Visualizza le macchine virtuali in host Hyper-V](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[Configurare VLAN nel riquadro nuova macchina virtuale](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**Vedi tutti o proporre nuove funzionalità**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
