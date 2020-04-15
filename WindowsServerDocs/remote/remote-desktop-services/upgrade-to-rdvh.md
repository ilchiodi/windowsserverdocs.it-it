---
title: Aggiornamento dell'Host di virtualizzazione Desktop remoto a Windows Server 2016
description: In questo articolo viene descritto come aggiornare le distribuzioni di Servizi Desktop remoto esistente a Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5aed8ba7-f541-4416-b01c-4d3b1712e2b1
author: spatnaik
manager: scottman
ms.openlocfilehash: 7bbf5f6a81a18303d4f9f4b02a1b8dead3c9a53a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857114"
---
# <a name="upgrading-your-remote-desktop-virtualization-host-to-windows-server-2016"></a>Aggiornamento dell'Host di virtualizzazione Desktop remoto a Windows Server 2016

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Aggiornamenti del sistema operativo è supportato con ruolo Servizi Desktop REMOTO
Gli aggiornamenti a Windows Server 2016 sono supportati solo da Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-locally"></a>Server Host di virtualizzazione Desktop remoto nella distribuzione di macchine virtuali archiviate in locale
Questi server devono essere aggiornati contemporaneamente. Per eseguire l'aggiornamento seguire la procedura seguente:

1. Disconnettere tutti gli utenti.
1. Spegnere o salvare tutte le macchine virtuali in ogni host. 
1. Aggiornare i server a Windows Server 2016. 
1. Tutte le raccolte devono essere disponibili e funzionanti dopo aver completato gli aggiornamenti.      

## <a name="rd-virtualization-host-servers-in-the-deployment-where-vms-are-stored-in-cluster-shared-volumes-csv"></a>Server Host di virtualizzazione Desktop remoto nella distribuzione di macchine virtuali archiviate in Volumi condivisi di cluster (CSV) 

1. Determinare una strategia di aggiornamento in cui alcuni server Host di virtualizzazione Desktop remoto (RDVH) verranno aggiornati e altri continueranno a ospitare le macchine virtuali su Windows Server 2012 R2.  
2. Isola uno o più server RDVH, destinati alla fase iniziale di aggiornamento, eseguendo la migrazione di tutte le macchine virtuali ad altri server RDVH "non ancora da aggiornare" che continueranno a far parte del cluster 2012 R2 originale.
    1. Aprire Gestione cluster di failover. 
    1. Fare clic su **Ruoli**. 
    1. Selezionare una o più macchine virtuali. Fare clic con il pulsante destro del mouse per aprire il menu di scelta rapida. 
    1. Fare clic su **Sposta** e scegliere **Live** oppure **Migrazione rapida** per spostare le macchine virtuali in uno o più dei server Host di virtualizzazione Desktop remoto che non fanno parte dell'aggiornamento iniziale. Usare **Live** oppure **Migrazione rapida** in base ad alcuni fattori come la compatibilità hardware o i requisiti online. 
3. Rimuovere i server RDVH, preparati per l'aggiornamento, dal cluster originale. 
4. Aggiornare i server RDVH isolati. 
5. Dopo aver aggiornato correttamente i server RDVH di destinazione, creare un nuovo cluster e un Volume condiviso cluster, che deve trovarsi in un volume SAN completamente diverso.
6. Aggiungere tutti i server RDVH aggiornati al nuovo cluster. 
7. Creare una struttura di cartelle nel nuovo Volume condiviso cluster che simuli la struttura di cartelle esistente nel Volume condiviso cluster esistente. Ciò include le cartelle di raccolta e le sottocartelle di primo livello di ogni macchina virtuale. 
8. Dalle varie cartelle di raccolta delle macchine virtuali sul Volume condiviso cluster originale copiare la cartella /IMGS e il contenuto nelle nuove cartelle di raccolta nelle stesse posizioni sul nuovo Volume condiviso cluster. 
9. Nella macchina RDVH di origine usare Gestione Cluster per rimuovere la configurazione della macchina virtuale per la disponibilità elevata:
    1. Avviare Gestione Cluster. 
    1. Fare clic su **Ruoli**. 
    1. Fare clic con il pulsante destro del mouse sugli oggetti macchina virtuale e quindi fare clic su **Rimuovi**. 
10. In uno dei server RDVH non aggiornati usare Console di gestione di Hyper-V per spostare tutte le macchine virtuali in uno dei server RDVH aggiornati e al nuovo cluster CSV:
    1. Aprire la console di gestione di Hyper-V. 
    2. Selezionare uno dei server RDVH non aggiornati. 
    3. Fare clic con il pulsante destro del mouse su una delle macchine virtuali da spostare e quindi fare clic su **Sposta**. 
    4. Selezionare **Sposta macchina virtuale** quindi fare clic su **Avanti**. 
    5. Fornisci il nome del server RDVH di destinazione aggiornato nella pagina **Specifica computer di destinazione** e quindi fai clic su **Avanti**. 
    6. Scegli **Sposta i dati della macchina virtuale in un unico percorso** e quindi fai clic su **Avanti**. 
    7. Individuare la posizione di destinazione. 
       > [!IMPORTANT]
       > Verificare che questo percorso si trovi in una cartella vuota per la macchina virtuale specifica. 

       > [!NOTE]
       > Come accennato, è necessario aver già creato una nuova sottocartella di destinazione prima di questo passaggio. La finestra di dialogo Seleziona cartella non consentirà di creare una sottocartella in questo passaggio. 
    
       Fare clic su **Avanti**e quindi su **Fine**. 
11. Dopo aver spostato le macchine virtuali, aggiungerle come oggetti cluster a **Disponibilità elevata**:
     1. Aprire Gestione cluster di failover in un server Host di virtualizzazione Desktop remoto aggiornato. 
     1. Fare clic con il pulsante destro del mouse sul nodo **Ruoli** e quindi scegliere **Configura ruolo**. Nella pagina **Inizia** della configurazione guidata disponibilità elevata fare clic su **Avanti**. 
     1. Scegliere **Macchina virtuale** dall'elenco dei ruoli disponibili e quindi fare clic su **Avanti**. Verrà visualizzato un elenco di macchine virtuali non configurate. 
     1. Selezionare tutte le macchine virtuali. Fare clic su **Avanti** e quindi fare di nuovo clic su **Avanti** nella pagina di conferma per avviare l'attività di configurazione.  
12. Dopo aver spostato tutte le macchine virtuali, aggiornare i server RDVH rimanenti. Usare i passaggi precedenti per bilanciare le posizioni delle macchine virtuali nel modo appropriato.

> [!NOTE]  
> I server Hyper-V eterogenei in un cluster non sono supportati. 
