---
title: Aggiornamento di cluster di Failover utilizzando lo stesso Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Questo articolo descrive l'aggiornamento di un Cluster di Failover 2 nodi con lo stesso hardware
ms.localizationpriority: medium
ms.openlocfilehash: 77cde9e64fda385facd91d86483f4d7f749f30a1
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453056"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>L'aggiornamento di cluster di Failover sullo stesso hardware

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità di servizi e applicazioni. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno dei nodi si verifica un errore, il servizio verrà garantito da un altro nodo tramite un processo denominato failover. Per gli utenti l'interruzione del servizio risulterà minima.

Questa guida descrive i passaggi per l'aggiornamento di nodi del cluster a Windows Server 2019 o Windows Server 2016 da una versione precedente con lo stesso hardware.

## <a name="overview"></a>Panoramica

L'aggiornamento del sistema operativo in un caso di failover esistente cluster è supportato solo quando si passa da Windows Server 2016 per Windows 2019.  Se il cluster di failover è in esecuzione una versione precedente, ad esempio, ad esempio Windows Server 2012 R2 e versioni precedenti, l'aggiornamento mentre sono in esecuzione i servizi del cluster non consentirà unire i nodi.  Se si usa lo stesso hardware, è possono eseguire i passaggi per la versione più recente.  

Prima di eseguire qualsiasi aggiornamento del cluster di failover, consultare il [centro aggiornamento Windows](https://www.microsoft.com/upgradecenter).  Quando si aggiorna un Server di Windows sul posto, si sposta da una versione del sistema operativo esistente a una versione più recente rimanendo sullo stesso hardware. Windows Server può essere aggiornato sul posto almeno uno e a volte due versioni di rollforward. Ad esempio, Windows Server 2012 R2 e Windows Server 2016 possono essere aggiornati sul posto a Windows Server 2019.  Inoltre, tenere presente che il [migrazione guidata Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) può essere usato ma è supportato solo un massimo di due versioni nuovamente. La figura seguente illustra i percorsi di aggiornamento per Windows Server. Le frecce verso il basso di puntamento rappresentano il percorso di aggiornamento supportato lo spostamento da versioni precedenti fino a Windows Server 2019.

![Diagramma di aggiornamento sul posto](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

I passaggi seguenti sono un esempio di passare da un server di cluster di failover di Windows Server 2012 a Windows Server 2019 Usa lo stesso hardware.  

Prima di avviare qualsiasi aggiornamento, verificare un backup corrente, incluso lo stato del sistema è stato eseguito.  Inoltre assicurarsi che tutti i driver e firmware sono stati aggiornati ai livelli di certificati per il sistema operativo si intende utilizzare.  Queste due note non verrà illustrate di seguito.

Nell'esempio seguente, il nome del cluster di failover è CLUSTER e i nomi dei nodi sono NODE1 e NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Passaggio 1: Rimuovere il primo nodo e l'aggiornamento a Windows Server 2016

1. In Gestione Cluster di Failover, svuotare tutte le risorse da NODE1 a NODE2 destro del mouse facendo clic sul nodo e selezionando **pausa** e **svuotare ruoli**.  In alternativa, è possibile usare il comando di PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Svuotamento nodo](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Rimuovere il nodo 1 dal Cluster destro del mouse, scegliere il nodo, quindi selezionare **altre azioni** e **rimozione**.  In alternativa, è possibile usare il comando di PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Svuotamento nodo](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Come precauzione, scollegare NODE1 dalla risorsa di archiviazione in uso.  In alcuni casi, disconnettendo i cavi dell'archiviazione dalla macchina è sufficiente.  Rivolgersi al fornitore di archiviazione per i passaggi la disconnessione appropriata se necessario.  A seconda delle risorse di archiviazione, ciò potrebbe non essere necessario.

4. Ricompilare NODE1 con Windows Server 2016.  Assicurarsi di che aver aggiunto tutti i necessari ruoli, funzionalità, i driver e gli aggiornamenti della sicurezza.

5. Creare un nuovo cluster denominato CLUSTER1 con NODE1.  Aprire Gestione Cluster di Failover e il **Management** riquadro, scegliere **crea Cluster** e seguire le istruzioni della procedura guidata.

    ![Svuotamento nodo](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Dopo aver creato il Cluster, i ruoli dovrà essere eseguita la migrazione dal cluster originale al nuovo cluster.  Nel nuovo cluster, con il pulsante destro del mouse fare clic sul nome del cluster (CLUSTER1) e selezionando **altre azioni** e **copia ruoli Cluster**.  Seguire la procedura guidata per eseguire la migrazione di ruoli.

    ![Svuotamento nodo](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Dopo che tutte le risorse sono state migrate, spegnere NODE2 (cluster originale) e scollegare la risorsa di archiviazione in modo da non causare eventuali interferenze.  Connettere l'archiviazione a NODE1.  Dopo tutto è connesso, connettere tutte le risorse e assicurarsi che funzionino come necessario.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Passaggio 2: Ricompilare secondo nodo per Windows Server 2019

Dopo avere verificato che funzionino come previsto, ricompilabile a Windows Server 2019 NODE2 e aggiunti al Cluster.

1. Eseguire un'installazione pulita di Windows Server 2019 in NODE2. Assicurarsi di che aver aggiunto tutti i necessari ruoli, funzionalità, i driver e gli aggiornamenti della sicurezza.

2. Ora che il cluster originale (CLUSTER) è ancora attivo, è possibile lasciare il nuovo nome del cluster come CLUSTER1 o tornare indietro con il nome originale.  Se si vuole tornare a quello originale, seguire questa procedura:
   
   a. In NODE1, in Gestione Cluster di Failover destro del mouse sul nome del cluster (CLUSTER1) e scegliere **proprietà**.
   
   b. Nel **generale** scheda, rinominare i cluster a CLUSTER.

   c. Quando si sceglie OK o applica, verrà visualizzato il seguente popup della finestra.

    ![Svuotamento nodo](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. Il servizio Cluster verrà arrestato e deve essere riavviato per completare la ridenominazione.

3. In NODE1, aprire Gestione Cluster di Failover.  Fare clic destro del mouse su **i nodi** e selezionare **AddNode**.  Ripetere la procedura guidata aggiungere NODE2 al Cluster.

4. Collegare la risorsa di archiviazione a NODE2. Ad esempio riconnettere i cavi dell'archiviazione. 

5. Svuotare tutte le risorse da NODE1 a NODE2 destro del mouse facendo clic sul nodo e selezionando **pausa** e **svuotare ruoli**.  In alternativa, è possibile usare il comando di PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Assicurarsi che tutte le risorse siano online e funzionino come necessario.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Passaggio 3: Ricompilare primo nodo a Windows Server 2019

1. Rimuovere il nodo 1 dal cluster e disconnettere la risorsa di archiviazione dal nodo in modo da cui è in precedenza.

2. Ricompilazione o aggiornare Nodo1 a Windows Server 2019.  Assicurarsi di che aver aggiunto tutti i necessari ruoli, funzionalità, i driver e gli aggiornamenti della sicurezza.

3. Collegare nuovamente lo spazio di archiviazione e aggiungere il nodo 1 al cluster.

4. Spostare tutte le risorse a NODE1 e assicurarsi che sono online e funzionano in base alle esigenze.

5. Livello funzionale del cluster corrente rimane in Windows 2016.  Aggiornare il livello di funzionalità a Windows 2019 con il comando di PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Ora sono in esecuzione con un Cluster di Failover di Windows Server 2019 completamente funzionale.

## <a name="additional-notes"></a>Note aggiuntive

- Come spiegato in precedenza, disconnessione della risorsa di archiviazione può o non sia necessaria.  Nella documentazione, si vuole prestare particolare attenzione.  Consultare il fornitore di archiviazione.
- Se il punto di partenza è Windows Server 2008 o 2008 R2 cluster, potrebbe essere necessaria un'esecuzione aggiuntiva tramite dei passaggi.
- Se il cluster è in esecuzione le macchine virtuali, assicurarsi di aggiornare il livello di macchina virtuale dopo che il livello funzionale del cluster è stati condotti con il comando di PowerShell [UPDATE-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Si noti che se si esegue un'applicazione, ad esempio SQL Server, Exchange Server, e così via, l'applicazione non verrà eseguita con la procedura guidata copia ruoli Cluster.  È consigliabile consultare il fornitore dell'applicazione per i passaggi di migrazione appropriata dell'applicazione.