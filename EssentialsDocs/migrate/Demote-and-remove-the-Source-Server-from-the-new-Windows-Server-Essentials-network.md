---
title: Abbassare di livello e rimuovere il Server di origine il nuovo network1 di Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Abbassare di livello e rimuovere il Server di origine il nuovo network1 di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Dopo aver completato l'installazione di Windows Server Essentials e aver completato le attività della migrazione guidata, è necessario eseguire le attività seguenti:  
  

1.  [Disinstallare Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Disconnettere le stampanti direttamente connesse al Server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Abbassare di livello il Server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Spostare il ruolo Server DHCP dal Server di origine al router](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Rimuovere e reimpiegare il Server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Disinstallare Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Disconnettere le stampanti direttamente connesse al Server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Abbassare di livello il Server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Spostare il ruolo Server DHCP dal Server di origine al router](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Rimuovere e reimpiegare il Server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>Disinstallare Exchange Server 2003  
  
> [!IMPORTANT]
>  Se si aggiungono account utente dopo aver spostato le cassette postali nel Server di destinazione e prima di disinstallare Exchange Server 2003 dal Server di origine, vengono aggiunti le cassette postali nel Server di origine. Questo è per impostazione predefinita. È necessario spostare le cassette postali nel Server di destinazione per tutti gli account utente che vengono aggiunti in questa fase. Ripetere le istruzioni contenute in spostare postali di Exchange e le impostazioni per la migrazione di Windows Server Essentials prima di disinstallare Exchange Server 2003.  
  
 È necessario disinstallare Exchange Server 2003 dal Server di origine prima di abbassarlo. Questo rimuove tutti i riferimenti in servizi Active Directory dominio (AD DS) per Exchange Server nel Server di origine. È necessario il supporto di Windows Small Business Server 2003 per rimuovere Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Per disinstallare Exchange Server 2003 dal Server di origine  
  
1.  Accedere al Server di origine come amministratore  
  
2.  Fare clic su **Start**, fare clic su **Pannello di controllo**, quindi fare clic su **Aggiungi / Rimuovi programmi**.  
  
3.  Nell'elenco dei programmi, selezionare **Windows Small Business Server 2003**, quindi fare clic su **Cambia/Rimuovi**.  
  
4.  Nella configurazione guidata, fare clic su **Avanti** fino a quando non la **Selezione componenti** verrà visualizzata la pagina.  
  
5.  Nella pagina Selezione componenti espandere **Exchange Server**e quindi scegli **rimuovere**.  
  
    > [!NOTE]

    >  Exchange Server controllerà per assicurarsi che siano cassette postali o cartelle pubbliche sul server. Se rimangono dei dati, viene visualizzato un messaggio di errore quando si fa clic **rimuovere**. Per evitare questo problema, assicurarsi di aver completato tutte le procedure riportate nell'argomento [dati al Server di destinazione e le impostazioni di SBS 2003 spostare](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

    >  Exchange Server controllerà per assicurarsi che siano cassette postali o cartelle pubbliche sul server. Se rimangono dei dati, viene visualizzato un messaggio di errore quando si fa clic **rimuovere**. Per evitare questo problema, assicurarsi di aver completato tutte le procedure riportate nell'argomento [dati al Server di destinazione e le impostazioni di SBS 2003 spostare](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6.  Fare clic su **Avanti**.  
  
7.  Quando richiesto, inserire Windows Small Business Server 2003 CD n. 3 e seguire le istruzioni visualizzate.  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>Disconnettere le stampanti direttamente connesse al Server di origine  
 Prima di abbassare di livello il Server di origine, scollegare fisicamente le stampanti direttamente connesse al Server di origine e condivise attraverso il Server di origine. Non garantire che nessun oggetto di Active Directory per le stampanti direttamente connesse al Server di origine. Le stampanti possono quindi essere direttamente connesso al Server di destinazione e condivise da Windows Server Essentials.  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>Abbassare di livello il Server di origine  
 Prima di abbassare di livello il Server di origine dal ruolo di controller di dominio Active Directory al ruolo di un server membro del dominio, verificare che le impostazioni di criteri di gruppo siano applicate a tutti i computer client, come descritto nella procedura seguente.  
  
> [!IMPORTANT]
>  Il Server di origine e il Server di destinazione devono essere connessi alla rete mentre le modifiche di criteri di gruppo vengono aggiornate nei computer client.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Per forzare un aggiornamento di criteri di gruppo in un computer client  
  
1.  Accedere al computer client come amministratore.  
  
2.  Aprire una finestra del prompt dei comandi come amministratore.  
  
3.  Al prompt dei comandi, digitare **gpupdate /force**, quindi premere INVIO.  
  
4.  Il processo potrebbe essere necessario disconnettersi e accedere di nuovo per terminare. Fare clic su **Sì** per confermare.  
  
##### <a name="to-demote-the-source-server"></a>Per abbassare di livello il Server di origine  
  
1.  Nel Server di origine, fare clic su **Start**, fare clic su **eseguire**, tipo **dcpromo**, quindi fare clic su **OK**.  
  
2.  Fare clic su **Avanti** due volte.  
  
    > [!NOTE]
    >  Non selezionare **questo server è l'ultimo controller di dominio nel dominio**.  
  
3.  Digitare una password per il nuovo account amministratore nel server, quindi fare clic su **Avanti**.  
  
4.  Nel **riepilogo** la finestra di dialogo, si viene informati che verrà rimosso dal computer di dominio Active Directory e che il server diventerà membro del dominio. Fare clic su **Avanti**.  
  
5.  Fare clic su **fine **. Il Server di origine viene riavviato.  
  
6.  Dopo avere riavviato il Server di origine, aggiungere il Server di origine come membro di un gruppo di lavoro prima di disconnetterlo dalla rete.  
  
 Dopo aver aggiunto il Server di origine come membro di un gruppo di lavoro e averlo disconnesso dalla rete, è necessario rimuoverlo da servizi di dominio Active Directory nel Server di destinazione.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Per rimuovere il Server di origine da Active Directory  
  
1.  Nel Server di destinazione, aprire **Active Directory Users and Computers**.  
  
2.  Nel **Active Directory Users and Computers** riquadro di spostamento, espandere il nome di dominio e quindi espandere **computer**.  
  
3.  Il nome del Server di origine è ancora presente nell'elenco dei server, fare clic con pulsante destro del mouse **eliminare**, quindi fare clic su **Sì**.  
  
4.  Verificare che il Server di origine non è nell'elenco e quindi Chiudi **Active Directory Users and Computers**.  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>Spostare il ruolo Server DHCP dal Server di origine al router  
  
> [!NOTE]

>  Se questa attività è già stata eseguita prima di avviare il processo di migrazione, continuare con la sezione [rimuovere e reimpiegare il Server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

>  Se questa attività è già stata eseguita prima di avviare il processo di migrazione, continuare con la sezione [rimuovere e reimpiegare il Server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Se il Server di origine è in esecuzione il ruolo DHCP, eseguire i passaggi seguenti per spostare il ruolo DHCP nel router.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Per spostare il ruolo DHCP dal Server di origine al router  
  
1.  Disattivare il servizio DHCP nel Server di origine, come indicato di seguito:  
  
    1.  Nel Server di origine, fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **servizi**.  
  
    2.  Nell'elenco dei servizi attualmente in esecuzione, fare doppio clic su di **Windows Server**, quindi fare clic su **proprietà**.  
  
    3.  Per **tipo di avvio**selezionare **disabilitato**.  
  
    4.  Arrestare il servizio.  
  
2.  Attivare il ruolo DHCP nel router  
  
    1.  Seguire le istruzioni della documentazione del router per attivare il ruolo DHCP sul router.  
  
    2.  Per garantire che gli indirizzi IP rilasciati dal Server di origine rimangono gli stessi, seguire le istruzioni della documentazione del router per configurare un intervallo DHCP sul router per essere lo stesso intervallo DHCP nel Server di origine.  
  
    > [!IMPORTANT]
    >  Se non è configurato un statico IP o prenotazioni DHCP sul router per il Server di destinazione e l'intervallo DHCP non è quello del Server di origine, è possibile che il router rilascerà un nuovo indirizzo IP per Server di destinazione. In questo caso, reimpostare le regole di port forwarding del router per inoltrare il nuovo indirizzo IP del Server di destinazione.  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>Rimuovere e reimpiegare il Server di origine  
 Disattivare il Server di origine e averlo disconnesso dalla rete. È consigliabile non riformattare il Server di origine per almeno una settimana per garantire che tutti i dati necessari eseguire la migrazione al Server di destinazione. Dopo aver verificato che tutti i dati è stata eseguita la migrazione, è possibile reinstallare questo server nella rete come server secondario per altre attività, se necessario.  
  
> [!NOTE]
>  Dopo aver abbassare di livello e rimuovere il Server di origine, riavviare il Server di destinazione.  
  
 Dopo che si abbassa di livello il Server di origine, non è in uno stato integro. Se si vuole reimpiegare il Server di origine, il modo più semplice è riformattarlo, installare un sistema operativo server, e quindi configurare l'utilizzo come server aggiuntivo.
