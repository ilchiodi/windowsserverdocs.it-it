---
title: Aggiornamento dei cluster di failover con lo stesso hardware
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Questo articolo descrive l'aggiornamento di un cluster di failover a 2 nodi con lo stesso hardware
ms.localizationpriority: medium
ms.openlocfilehash: 5fe93f1d43e0c3a1bc4269b585cb9d021d3461aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361405"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Aggiornamento dei cluster di failover nello stesso hardware

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è costituito da un gruppo di computer indipendenti che interagiscono tra di loro per migliorare la disponibilità di servizi e applicazioni. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se in uno dei nodi si verifica un errore, il servizio verrà garantito da un altro nodo tramite un processo denominato failover. Per gli utenti l'interruzione del servizio risulterà minima.

Questa guida descrive i passaggi per l'aggiornamento dei nodi del cluster a Windows Server 2019 o Windows Server 2016 da una versione precedente con lo stesso hardware.

## <a name="overview"></a>Panoramica

L'aggiornamento del sistema operativo in un cluster di failover esistente è supportato solo quando si passa da Windows Server 2016 a Windows 2019.  Se il cluster di failover esegue una versione precedente, ad esempio Windows Server 2012 R2 e versioni precedenti, l'aggiornamento durante l'esecuzione dei servizi cluster non consentirà di unire i nodi.  Se si usa lo stesso hardware, è possibile eseguire i passaggi per ottenere la versione più recente.  

Prima di qualsiasi aggiornamento del cluster di failover, consultare il [contenuto di aggiornamento di Windows Server](../upgrade/upgrade-overview.md).  Quando si esegue l'aggiornamento di Windows Server sul posto, si passa da una versione del sistema operativo esistente a una versione più recente, rimanendo nello stesso hardware. Windows Server può essere aggiornato sul posto almeno uno e talvolta in due versioni. Ad esempio, Windows Server 2012 R2 e Windows Server 2016 possono essere aggiornati sul posto a Windows Server 2019.  Tenere inoltre presente che è possibile utilizzare la [migrazione guidata cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) , ma è supportata solo fino a due versioni. Nell'immagine seguente vengono illustrati i percorsi di aggiornamento per Windows Server. Le frecce verso il basso rappresentano il percorso di aggiornamento supportato da versioni precedenti fino a Windows Server 2019.

![Diagramma di aggiornamento sul posto](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

Di seguito è riportato un esempio di passaggio da un server del cluster di failover di Windows Server 2012 a Windows Server 2019 utilizzando lo stesso hardware.  

Prima di avviare qualsiasi aggiornamento, verificare che sia stato eseguito un backup corrente, incluso lo stato del sistema.  Assicurarsi inoltre che tutti i driver e il firmware siano stati aggiornati ai livelli certificati per il sistema operativo che verrà utilizzato.  Queste due note non verranno analizzate qui.

Nell'esempio seguente il nome del cluster di failover è CLUSTER e i nomi dei nodi sono NODE1 e NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Passaggio 1: Rimuovere il primo nodo ed eseguire l'aggiornamento a Windows Server 2016

1. In Gestione cluster di failover, svuotare tutte le risorse da NODE1 a NODE2 facendo clic con il pulsante destro del mouse sul nodo e selezionando **Sospendi** e **Svuota ruoli**.  In alternativa, è possibile usare il comando di PowerShell [Suspend-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Svuota nodo](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Rimuovere NODE1 dal cluster facendo clic con il pulsante destro del mouse sul nodo e selezionando **altre azioni** e **Rimuovi**.  In alternativa, è possibile usare il comando PowerShell [Remove-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Svuota nodo](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Per precauzione, scollegare NODE1 dalla risorsa di archiviazione in uso.  In alcuni casi, è sufficiente disconnettere i cavi di archiviazione dal computer.  Se necessario, rivolgersi al fornitore del sistema di archiviazione per i passaggi di scollegamento appropriati.  Questa operazione potrebbe non essere necessaria a seconda dell'archiviazione.

4. Ricompilare NODE1 con Windows Server 2016.  Assicurarsi di aver aggiunto tutti i ruoli, le funzionalità, i driver e gli aggiornamenti della sicurezza necessari.

5. Creare un nuovo cluster denominato CLUSTER1 con NODE1.  Aprire Gestione cluster di failover e nel riquadro **gestione** scegliere **Crea cluster** e seguire le istruzioni della procedura guidata.

    ![Svuota nodo](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Una volta creato il cluster, sarà necessario eseguire la migrazione dei ruoli dal cluster originale al nuovo cluster.  Nel nuovo cluster fare clic con il pulsante destro del mouse sul nome del cluster (CLUSTER1) e selezionare **altre azioni** e **copiare i ruoli del cluster**.  Eseguire la procedura guidata per eseguire la migrazione dei ruoli.

    ![Svuota nodo](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Una volta eseguita la migrazione di tutte le risorse, spegnere NODE2 (cluster originale) e disconnettere lo spazio di archiviazione in modo da non causare alcuna interferenza.  Connettere lo spazio di archiviazione a NODE1.  Al termine della connessione, portare tutte le risorse online e verificare che funzionino come previsto.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Passaggio 2: Ricompilare il secondo nodo in Windows Server 2019

Dopo aver verificato che tutto funzioni come previsto, NODE2 può essere ricompilato in Windows Server 2019 e aggiunto al cluster.

1. Eseguire un'installazione pulita di Windows Server 2019 in NODE2. Assicurarsi di aver aggiunto tutti i ruoli, le funzionalità, i driver e gli aggiornamenti della sicurezza necessari.

2. Ora che il cluster originale (CLUSTER) non è più disponibile, è possibile lasciare il nome del nuovo cluster come CLUSTER1 oppure tornare al nome originale.  Se si desidera tornare al nome originale, attenersi alla procedura seguente:
   
   a. Gestione cluster di failover in NODE1 fare clic con il pulsante destro del mouse sul nome del cluster (CLUSTER1) e scegliere **Proprietà**.
   
   b. Nella scheda **generale** rinominare il CLUSTER in cluster.

   c. Quando si sceglie OK o applica, viene visualizzata la finestra popup di seguito.

    ![Svuota nodo](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. Il servizio cluster verrà arrestato e sarà necessario avviarlo di nuovo per completare la ridenominazione.

3. In NODE1 aprire Gestione cluster di failover.  Fare clic con il pulsante destro del mouse sui **nodi** e scegliere **Aggiungi nodo**.  Eseguire la procedura guidata aggiungendo NODE2 al cluster.

4. Alleghi lo spazio di archiviazione a NODE2. Questo potrebbe includere la riconnessione dei cavi di archiviazione. 

5. Svuotare tutte le risorse da NODE1 a NODE2 facendo clic con il pulsante destro del mouse sul nodo e selezionando **Sospendi** e **Svuota ruoli**.  In alternativa, è possibile usare il comando di PowerShell [Suspend-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Verificare che tutte le risorse siano online e funzionino come previsto.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Passaggio 3: Ricompilare il primo nodo in Windows Server 2019

1. Rimuovere NODE1 dal cluster e disconnettere l'archiviazione dal nodo nel modo in cui è stato eseguito in precedenza.

2. Ricompilare o aggiornare NODE1 a Windows Server 2019.  Assicurarsi di aver aggiunto tutti i ruoli, le funzionalità, i driver e gli aggiornamenti della sicurezza necessari.

3. Ricollegare lo spazio di archiviazione e aggiungere nuovamente NODE1 al cluster.

4. Spostare tutte le risorse in NODE1 e assicurarsi che vengano riportate online e funzionino secondo necessità.

5. Il livello di funzionalità del cluster corrente rimane in Windows 2016.  Aggiornare il livello di funzionalità a Windows 2019 con il comando PowerShell [Update-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

A questo punto si esegue un cluster di failover di Windows Server 2019 completamente funzionante.

## <a name="additional-notes"></a>Note aggiuntive

- Come spiegato in precedenza, la disconnessione dello spazio di archiviazione potrebbe essere necessaria o meno.  Nella documentazione, è opportuno prestare attenzione a un errore.  Rivolgersi al fornitore del sistema di archiviazione.
- Se il punto di partenza è costituito da cluster Windows Server 2008 o 2008 R2, potrebbe essere necessario eseguire ulteriori passaggi.
- Se il cluster esegue macchine virtuali, assicurarsi di aggiornare il livello della macchina virtuale una volta che il livello di funzionalità del cluster è stato eseguito con il comando PowerShell [Update-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Si noti che se si esegue un'applicazione come SQL Server, Exchange Server e così via, l'applicazione non verrà migrata con la copia guidata ruoli cluster.  Per la corretta procedura di migrazione dell'applicazione, è necessario consultare il fornitore dell'applicazione.