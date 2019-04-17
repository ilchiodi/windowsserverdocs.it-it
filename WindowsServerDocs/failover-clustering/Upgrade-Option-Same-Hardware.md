---
title: L'aggiornamento dei cluster di Failover con lo stesso Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Questo articolo descrive l'aggiornamento di un Cluster di Failover 2-nodo con lo stesso hardware
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121304"
---
# L'aggiornamento dei cluster di Failover nello stesso hardware

> Si applica a: Windows Server 2019, Windows Server 2016

Un cluster di failover è un gruppo di computer indipendenti che lavorano insieme per aumentare la disponibilità di applicazioni e servizi. I server inclusi nel cluster (detti nodi) sono connessi mediante cavi fisici e software. Se uno dei nodi del cluster non riesce, un altro nodo inizia a fornire il servizio (processo noto come failover). Un minimo di servizio risulterà esperienza degli utenti.

Questa guida descrive i passaggi per l'aggiornamento di nodi del cluster a Windows Server 2019 o Windows Server 2016 da una versione precedente con lo stesso hardware.

## Panoramica

L'aggiornamento del sistema operativo in un failover esistente cluster è supportato solo durante il passaggio da Windows Server 2016 a Windows 2019.  Se il cluster di failover è in esecuzione una versione precedente, ad esempio, ad esempio Windows Server 2012 R2 e versioni precedenti, l'aggiornamento durante l'eseguono dei servizi di cluster non consentirà l'aggiunta di nodi insieme.  Se si usa lo stesso hardware, è possono eseguire i passaggi ottenerla, per la versione più recente.  

Prima di qualsiasi aggiornamento del cluster di failover, contatta il [Centro di aggiornamento di Windows](https://www.microsoft.com/upgradecenter).  Quando si aggiorna un Windows Server sul posto, passare da una versione del sistema operativo esistente a una versione più recente, sempre nello stesso hardware. Windows Server può essere aggiornato posto almeno uno e talvolta due versioni in avanti. Ad esempio, possono essere aggiornati Windows Server 2012 R2 e Windows Server 2016 sul posto a Windows Server 2019.  Tieni anche presente che la [Procedura guidata di migrazione di Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) può essere usato, ma è supportata solo fino a due versioni indietro. Nell'immagine seguente mostra i percorsi di aggiornamento per Windows Server. Le frecce di puntamento verso il basso rappresentano il percorso di aggiornamento supportato lo spostamento da versioni precedenti fino a Windows Server 2019.

![Diagramma di aggiornamento sul posto](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

I passaggi seguenti sono un esempio di passare da un server di cluster di failover di Windows Server 2012 a Windows Server 2019 con lo stesso hardware.  

Prima di iniziare qualsiasi aggiornamento, assicurati è stato eseguito un backup corrente, tra cui lo stato del sistema.  Assicurati anche tutti i driver e firmware sono stati aggiornati per i livelli di certificati per il sistema operativo si utilizzerà.  Queste due note non verrà illustrate qui.

Nell'esempio seguente, il nome del cluster di failover è CLUSTER e i nomi di nodo vengono nodo 1 e il nodo 2.

## Passaggio 1: Rimuovere il primo nodo e l'aggiornamento a Windows Server 2016

1. In Gestione Cluster di Failover, Svuota tutte le risorse dal nodo 1 per il nodo 2 destro del mouse, facendo clic sul nodo e selezionando **Pausa** e **Svuota ruoli**.  In alternativa, è possibile utilizzare il comando di PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Svuota nodo](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. Rimuovere il nodo 1 dal Cluster destro del mouse, facendo clic sul nodo e selezionando **Più azioni** e **operazioni**.  In alternativa, è possibile utilizzare il comando di PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Svuota nodo](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. Come misura, scollegare il nodo 1 dall'archivio che usi.  In alcuni casi, la disconnessione i cavi di archiviazione da computer è sufficiente.  Rivolgiti al tuo fornitore di archiviazione per la procedura corretta disconnessione, se necessario.  A seconda il dispositivo di archiviazione, questo potrebbe non essere necessario.

4. Ricompila il nodo 1 con Windows Server 2016.  Assicurati di che avere aggiunto tutti i ruoli necessari, funzionalità, driver e gli aggiornamenti della sicurezza.

5. Creare un nuovo cluster denominato CLUSTER1 con il nodo 1.  Apri Gestione Cluster di Failover e nel riquadro di **Gestione** , scegli di **Creare Cluster** e segui le istruzioni della procedura guidata.

    ![Svuota nodo](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. Dopo aver creato il Cluster, i ruoli saranno necessario eseguire la migrazione a questo nuovo cluster dal cluster originale.  Nel nuovo cluster, pulsante destro del mouse sul **Più azioni** nome del cluster (CLUSTER1) e la selezione e **Copia i ruoli del Cluster**.  Segui della procedura guidata per eseguire la migrazione di ruoli.

    ![Svuota nodo](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  Una volta che sono state migrate tutte le risorse, spegnere il nodo 2 (cluster originale) e disconnettere l'archiviazione in modo da non deve causare qualsiasi interferenze.  Collega lo spazio di archiviazione al nodo 1.  Una volta collegato, il tutto portare online tutte le risorse e assicurati che funzionano come dovrebbe.

## Passaggio 2: Ricompila secondo nodo di Windows Server 2019

Dopo aver verificato che tutto funzioni come dovrebbe, il nodo 2 possono essere ricompilato per Windows Server 2019 e aggiunto al cluster.

1. Eseguire un'installazione pulita di Windows Server 2019 sul nodo 2. Assicurati di che avere aggiunto tutti i ruoli necessari, funzionalità, driver e gli aggiornamenti della sicurezza.

2. Ora che è più visibili del cluster originale (CLUSTER), puoi lasciare il nuovo nome del cluster come CLUSTER1 o tornare al nome originale.  Se vuoi tornare al nome originale, segui questi passaggi:
   
   a. Nel nodo 1, fare clic sul nome del cluster (CLUSTER1) in Gestione Cluster di Failover destro del mouse e scegli **proprietà**.
   
   b. Nella scheda **Generale** , Rinomina il cluster a CLUSTER.

   c. Quando scegli OK o applica, verrà visualizzato il seguente popup finestra di dialogo.

    ![Svuota nodo](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. Verrà arrestato e necessari per essere avviata di nuovo per la ridenominazione completare il servizio Cluster.

3. Nel nodo 1, Apri Gestione Cluster di Failover.  Clic del mouse sui **nodi** pulsante destro del mouse e seleziona **l'Aggiunta di nodi**.  Passare attraverso la procedura guidata di aggiungere il nodo 2 al cluster.

4. Collegare l'archiviazione a nodo 2. Questo può includere la riconnessione i cavi di archiviazione. 

5. Svuota tutte le risorse dal nodo 1 per il nodo 2 destro del mouse, facendo clic sul nodo e selezionando **Pausa** e **Svuota ruoli**.  In alternativa, è possibile utilizzare il comando di PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Assicurati di tutte le risorse siano in linea e funzionano come dovrebbe.

## Passaggio 3: Ricompila primo nodo di Windows Server 2019

1. Rimuovere il nodo 1 dal cluster e disconnettere l'archiviazione dal nodo in modo da cui è in precedenza.

2. Ricompila o aggiorna il nodo 1 a Windows Server 2019.  Assicurati di che avere aggiunto tutti i ruoli necessari, funzionalità, driver e gli aggiornamenti della sicurezza.

3. Associare nuovamente lo spazio di archiviazione e aggiungere il nodo 1 al cluster.

4. Spostare tutte le risorse per il nodo 1 e assicurarti che si connettono e funzioni in base alle esigenze.

5. Il livello funzionale del cluster corrente rimane in Windows 2016.  Aggiornare il livello di funzionalità di Windows 2019 con il comando di PowerShell [CLUSTERFUNCTIONALLEVEL aggiornamento](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Ora di esecuzione con un Cluster di Failover di Windows Server 2019 completamente funzionante.

## Note aggiuntive

- Come spiegato in precedenza, la disconnessione lo spazio di archiviazione può anche potrebbe non essere necessaria.  Nella documentazione, vogliamo prestare particolare attenzione.  Contatta il fornitore dell'archiviazione.
- Se il punto di partenza è Windows Server 2008 o 2008 R2 cluster, potrebbe essere necessarie un'esecuzione tramite altra passaggi.
- Se il cluster è in esecuzione di macchine virtuali, assicurati che effettuare l'aggiornamento, il livello di macchina virtuale è stato eseguito il livello funzionale del cluster con il comando di PowerShell [VMVERSION di aggiornamento](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Tieni presente che se si esegue un'applicazione, ad esempio SQL Server, Server Exchange e così via, l'applicazione non verrà eseguita con la procedura guidata ruoli Cluster copia.  Devi consultare il fornitore dell'applicazione per la procedura di migrazione corretta dell'applicazione.