---
title: Aggiornare l'Host di virtualizzazione Desktop remoto a Windows Server 2016
description: In questo articolo viene descritto come aggiornare le distribuzioni di Servizi Desktop remoto esistente a Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 17bf3a49155d29960684acebea870c0b51f664a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812032"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Aggiornare l'Host di virtualizzazione Desktop remoto a Windows Server 2016

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Aggiornamenti del sistema operativo è supportato con ruolo Servizi Desktop REMOTO
Gli aggiornamenti a Windows Server 2016 sono supportati solo da Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Server Host di virtualizzazione Desktop remoto nella distribuzione di macchine virtuali sono archiviate in locale
Questi server devono essere aggiornati contemporaneamente. Seguire i passaggi seguenti per eseguire l'aggiornamento:

1. Disconnettere tutti gli utenti.
1. Attivare o disattivare salvare tutte le macchine virtuali in ogni host. 
1. Aggiornare i server a Windows Server 2016. 
1. Tutte le raccolte devono essere disponibili e funzionale una volta completati gli aggiornamenti.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Server Host di virtualizzazione Desktop remoto nella distribuzione di macchine virtuali sono archiviate in volumi condivisi Cluster (CSV) 

1. Determinare una strategia di aggiornamento in cui alcuni server RDVH verrà aggiornato e alcuni continueranno a ospitare le macchine virtuali in Windows Server 2012 R2.  
1. Isolare uno o più server RDVH, di destinazione per il round iniziale dell'aggiornamento, eseguendo la migrazione di tutte le macchine virtuali loro ' per non essere ancora aggiornato' Server RDVH che rimangono parte del cluster 2012 R2 originale.
    1. Aprire Gestione Cluster di Failover. 
    1. Fare clic su **Ruoli**. 
    1. Selezionare uno o più macchine virtuali. Fare doppio clic per aprire il menu di scelta rapida. 
    1. Fare clic su **spostare** e scegliere **Live** oppure **Quick Migration** per spostare le macchine virtuali a uno o più dei server Host di virtualizzazione Desktop remoto che non fanno parte dell'aggiornamento iniziale. Uso **Live** oppure **rapido** migrazione dipende da fattori quali la compatibilità hardware o requisiti online. 
1. Rimuovere i server RDVH, preparati per l'aggiornamento, dal cluster originale. 
1. Aggiornare i server RDVH isolati. 
1. Dopo aver aggiornato correttamente il server di destinazione RDVH, creare un nuovo cluster e CSV, che deve trovarsi in un volume SAN completamente diverso.
1. Aggiungere tutti i server RDVH aggiornati al nuovo cluster. 
1. Creare una struttura di cartelle nel nuovo volume condiviso cluster che riproduce la struttura di cartelle esistente nel file CSV esistente. Ciò include le cartelle di raccolta e le cartelle per secondari a livello superiore di ciascuna macchina virtuale. 
1. Dalle cartelle di raccolta di macchine Virtuali diverse nel CSV originale, copiare la cartella /IMGS e il contenuto per le nuove cartelle di raccolta nelle stesse posizioni sul nuovo volume condiviso cluster. 
1. Nella macchina di origine RDVH, utilizzare Gestione Cluster per rimuovere la configurazione della macchina virtuale per la disponibilità elevata:
    1. Avviare Gestione Cluster. 
    1. Fare clic su **Ruoli**. 
    1. Fare doppio clic sugli oggetti macchina virtuale e quindi fare clic su **rimuovere**. 
1. In uno dei server RDVH non aggiornato, utilizzare Gestione Hyper-V per spostare tutte le macchine virtuali in uno dei server RDVH aggiornati e nuovi Cluster CSV:
    1. Aprire la console di gestione di Hyper-V. 
    1. Selezionare uno dei server RDVH non aggiornato. 
    1. Fare doppio clic su una delle VM da spostare e quindi fare clic su **spostare**. 
    1. Scegli **spostare la macchina virtuale**, quindi fare clic su **successivo**. 
    1. Specificare il nome di destinazione aggiornato RDVH del server nel **specifica Computer di destinazione** pagina e quindi fare clic su **successivo**. 
    1. Scegli **spostare i dati della macchina virtuale a un'unica posizione**, quindi fare clic su **successivo**. 
    1. Selezionare il percorso di destinazione. 
    > [!IMPORTANT]
    > Verificare che questo percorso si trova in una cartella vuota per la macchina virtuale specifica. 

    > [!NOTE]
    > Come accennato, è necessario avere già creato una nuova cartella secondaria di destinazione prima di questo passaggio. La finestra di dialogo Seleziona cartelle non consentirà di creare una sottocartella in questo passaggio. 
    
    Fare clic su **Avanti** e quindi su **Fine**. 
1. Dopo che le macchine virtuali vengono rilocate, aggiungerli come cluster **disponibilità elevata** oggetti:
    1. Aprire Gestione Cluster di Failover in un Server Host di virtualizzazione Desktop remoto aggiornato. 
    1. Fare doppio clic il **ruoli** nodo e quindi fare clic su **Configura ruolo**. Fare clic su **successivo** nel **avviare** pagina della procedura guidata disponibilità elevata. 
    1. Scegli **macchina virtuale** dall'elenco dei ruoli disponibili e quindi fare clic su **successivo**. Verrà visualizzato un elenco di macchine virtuali che non sono configurate. 
    1. Selezionare tutte le macchine virtuali. Fare clic su **successivo** e quindi fare clic su **successivo** nuovamente nella pagina di conferma per avviare l'attività di configurazione.  
1. Dopo che è stato spostato tutte le macchine virtuali, aggiornare i server RDVH rimanenti. Usare i passaggi precedenti per bilanciamento del carico dei percorsi di macchina virtuale come appropriato.

> [!NOTE]  
> Server Hyper-V eterogenee in un cluster non sono supportati. 
