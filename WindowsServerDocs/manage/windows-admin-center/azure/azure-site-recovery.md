---
title: Proteggi le tue macchine virtuali Hyper-V con Azure Site Recovery e l'interfaccia di amministrazione di Windows
description: Usa Windows Admin Center (Project Honolulu) per proteggere le macchine virtuali Hyper-V con Azure Site Recovery.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server
ms.openlocfilehash: 4995ed433d34fddfa91548fa42d67eea3a319c1f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357347"
---
# <a name="protect-your-hyper-v-virtual-machines-with-azure-site-recovery-and-windows-admin-center"></a>Proteggi le tue macchine virtuali Hyper-V con Azure Site Recovery e l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center Preview, interfaccia di amministrazione di Windows

[Altre informazioni sull'integrazione di Azure con l'interfaccia di amministrazione di Windows.](../plan/azure-integration-options.md)

Windows Admin Center semplifica il processo di replica delle macchine virtuali nei server o nei cluster Hyper-V, facilitando l'uso delle funzionalità di Azure dal proprio data center. Per automatizzare l'installazione, puoi connettere il gateway di Windows Admin Center ad Azure.

Usa le informazioni seguenti per configurare le impostazioni di replica e creare un piano di ripristino dal portale di Azure, consentendo a Windows Admin Center di avviare la replica della macchina virtuale e di proteggere le macchine virtuali.

## <a name="what-is-azure-site-recovery-and-how-does-it-work-with-windows-admin-center"></a>Che cos'è Azure Site Recovery e come funziona con Windows Admin Center? 

**Azure Site Recovery** è un servizio di Azure che esegue la replica dei carichi di lavoro in esecuzione sulle macchine virtuali in modo che l'infrastruttura critica dell'azienda sia protetta in caso di emergenze.  [Altre informazioni su Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).

Azure Site Recovery è costituito da due componenti: **replica** e **failover**. La parte della replica protegge le macchine virtuali in caso di emergenza replicando il disco rigido virtuale della macchina virtuale di destinazione in un account di archiviazione di Azure. Puoi quindi eseguire il failover di queste macchine virtuali ed eseguirle in Azure in caso di emergenza. Puoi anche eseguire un failover di test senza influire sulle macchine virtuali principali per testare il processo di ripristino in Azure.

Il solo completamento dell'installazione per il componente di replica è sufficiente per proteggere la macchina virtuale in caso di emergenza. Tuttavia, non potrai avviare la macchina virtuale in Azure finché non configuri la parte di failover. La parte di failover può essere configurata al momento di eseguire il failover in una macchina virtuale di Azure, non è necessario configurarla inizialmente. Se il server host diventa non disponibile e non è stato ancora configurato il componente di failover, puoi configurarlo al momento e accedere ai carichi di lavoro della macchina virtuale protetta. Tuttavia, si consiglia di configurare le impostazioni di failover prima di una situazione di emergenza.
 

## <a name="prerequisites-and-planning"></a>Prerequisiti e pianificazione

- I server di destinazione che ospitano le macchine virtuali da proteggere devono avere l'accesso a Internet per la replica in Azure.
- [Connetti il gateway di Windows Admin Center ad Azure](azure-integration.md).
- [Esamina lo strumento di pianificazione della capacità per valutare i requisiti per completare correttamente replica e failover](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-capacity).

## <a name="step-1-set-up-vm-protection-on-your-target-host"></a>Passaggio 1: Configurare la protezione delle macchine virtuali nell'host di destinazione

> [!NOTE] 
> Devi eseguire questo passaggio una volta per ogni server o cluster host contenente le macchine virtuali che vuoi proteggere.

1. Passa al server o al cluster che ospita le macchine virtuali che vuoi proteggere (con Server Manager o Gestione di cluster iperconvergenti).
2. Passa a **Macchine virtuali** > **Inventario**.
3. Seleziona una qualsiasi macchina virtuale, non necessariamente quella da proteggere.
4. Seleziona **Altro** > **Configura protezione macchina virtuale**.
5. Accedi all'Account Azure.
6. Immetti le informazioni richieste:

   - **Abbonamento** Sottoscrizione di Azure che si vuole usare per la replica delle macchine virtuali in questo host.
   - **Percorso:** Area di Azure in cui devono essere create le risorse di ASR.
   - **Account di archiviazione:** L'account di archiviazione in cui verranno salvati i carichi di lavoro delle VM replicati in questo host.
   - **Credenziali** Scegliere un nome per l'insieme di credenziali Azure Site Recovery per le macchine virtuali protette in questo host.

7. Seleziona **Configura ASR**.
8. Attendere finché non viene visualizzata la notifica: **Impostazione Site Recovery completata**.
 
Ciò potrebbe richiedere fino a 10 minuti. Puoi controllare lo stato di avanzamento passando a **Notifiche** (l'icona a forma di campana in alto a destra).

>[!NOTE]
> Questo passaggio installa automaticamente l'agente ASR sul server o sui nodi di destinazione (se la configurazione è in un cluster) e crea un **Gruppo di risorse** con **l'account di archiviazione** e l'**insieme di credenziali** specificati nella **posizione** indicata. Viene anche registrato l'host di destinazione con il servizio di ASR e configurato un criterio di replica predefinito.

## <a name="step-2-select-virtual-machines-to-protect"></a>Passaggio 2: Selezionare le macchine virtuali da proteggere

1. Torna al server o al cluster configurato nel passaggio 2 e vai a **Macchine virtuali > Inventario**.
2. Seleziona la macchina virtuale da proteggere.
3. Seleziona **Altro** > **Proteggi macchina virtuale**.
4. Esamina i [requisiti di capacità per la protezione della macchina virtuale](https://docs.microsoft.com/azure/site-recovery/site-recovery-capacity-planner).

    Per usare un account di archiviazione Premium, [creane uno nel portale di Azure](https://docs.microsoft.com/azure/storage/common/storage-premium-storage). L'opzione **Crea nuovo** fornita nel riquadro Windows Admin Center consente di creare un account di archiviazione standard.

5. Immetti il nome dell'**account di archiviazione** da usare per la replica della macchina virtuale e seleziona **Proteggi macchina virtuale**. Questo passaggio consente la replica per la macchina virtuale selezionata. 

6. ASR avvia la replica. La replica viene completata e la macchina virtuale risulta protetta quando il valore nella colonna **Protetto** della griglia dell'**inventario della macchina virtuale** viene impostato su **Sì**. L'operazione può richiedere diversi minuti.  

## <a name="step-3-configure-and-run-a-test-failover-in-the-azure-portal"></a>Passaggio 3: Configurare ed eseguire un failover di test nella portale di Azure

 Anche se non è necessario completare questo passaggio all'avvio della replica della macchina virtuale (la macchina virtuale è già protetta solo dalla replica), è consigliabile configurare le impostazioni di failover quando configuri Azure Site Recovery. Se vuoi preparare il failover in una macchina virtuale di Azure, completa questa procedura:

1. [Configura una rete di Azure](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure) La macchina virtuale con failover verrà collegata a questa rete virtuale. Gli altri passaggi elencati nella pagina collegata vengono completati automaticamente da Windows Admin Center. È sufficiente impostare la rete di Azure.

2. [Esegui il failover di test](https://docs.microsoft.com/azure/site-recovery/hyper-v-site-walkthrough-test-failover).

## <a name="step-4-create-recovery-plans"></a>Passaggio 4: Creare piani di ripristino

**Piano di ripristino** è una funzionalità in Azure Site Recovery che consente di eseguire il failover e il ripristino di un'intera applicazione che comprende una raccolta di macchine virtuali. Le macchine virtuali protette singolarmente possono essere ripristinate, ma aggiungendo le macchine virtuali che comprendono un'applicazione a un piano di ripristino, è possibile eseguire il failover dell'intera applicazione con il piano di ripristino. Puoi anche usare la funzionalità di failover di test del piano di ripristino per testare il ripristino dell'applicazione. La funzionalità Piano di ripristino consente di raggruppare le macchine virtuali, impostare l'ordine con cui vengono elaborate durante un failover e automatizzare i passaggi aggiuntivi da eseguire nell'ambito del processo di ripristino. Dopo aver protetto le macchine virtuali, puoi passare all'insieme di credenziali di Azure Site Recovery nel portale di Azure e creare piani di ripristino per le macchine virtuali. [Altre informazioni sui piani di ripristino](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans).

## <a name="monitoring-replicated-vms-in-azure"></a>Monitoraggio delle macchine virtuali replicate in Azure ##

Per verificare che non ci siano errori di registrazione del server, vai al **portale di Azure** > **Tutte le risorse** > **Insieme di credenziali di Servizi di ripristino** (specificato nel passaggio 2) > **Processi** > **Processi di Site Recovery**.

Puoi monitorare la replica della macchine virtuale da **Insieme di credenziali di Servizi di ripristino** > **Elementi replicati**.

Per visualizzare tutti i server che vengono registrati nell'insieme di credenziali, passa a **Insieme di credenziali di Servizi di ripristino** > **Infrastruttura di Site Recovery**  > **Host Hyper-V** (nella sezione dei siti di Hyper-V).

## <a name="known-issue"></a>Problema noto ##

Durante la registrazione di ASR con un cluster, se un nodo non riesce a installare ASR o a registrarlo nel servizio ASR, le macchine virtuali potrebbero non essere protette. Verifica che tutti i nodi del cluster vengano registrati nel portale di Azure passando a **Insieme di credenziali di Servizi di ripristino** > **Processi** > **Processi di Site Recovery**.