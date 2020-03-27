---
title: Abbassare di e rimuovere il server di origine dalla nuova network1 di Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 160c575386feaab5353c97edc1b00b71d1ad7adf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319013"
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Abbassare di e rimuovere il server di origine dalla nuova network1 di Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Dopo aver completato l'installazione di Windows Server Essentials e aver completato le attività della migrazione guidata, è necessario eseguire le attività seguenti:  
  

1.  [Disinstallare Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Disconnettere le stampanti direttamente connesse al server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Abbassare di livello il server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Spostare il ruolo server DHCP dal server di origine al router](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Rimuovere e reimpiegare il server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Disinstallare Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Disconnettere le stampanti direttamente connesse al server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Abbassare di livello il server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Spostare il ruolo server DHCP dal server di origine al router](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Rimuovere e reimpiegare il server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="uninstall-exchange-server-2003"></a><a name="BKMK_UninstallExchangeServer2003"></a>Disinstallare Exchange Server 2003  
  
> [!IMPORTANT]
>  Se si aggiungono account utente dopo lo spostamento delle cassette postali nel server di destinazione e prima di disinstallare Exchange Server 2003 dal server di origine, le cassette postali vengono aggiunte nel server di origine. Si tratta di un comportamento legato alla progettazione del prodotto. È necessario spostare le cassette postali nel server di destinazione per tutti gli account utente aggiunti in questa fase. Ripetere le istruzioni in spostare le cassette postali e le impostazioni di Exchange Server per la migrazione a Windows Server Essentials prima di disinstallare Exchange Server 2003.  
  
 È necessario disinstallare Exchange Server 2003 dal server di origine prima di abbassarlo di più. Verranno rimossi tutti i riferimenti in Active Directory Domain Services (AD DS) a Exchange Server nel server di origine. È necessario disporre del supporto di Windows Small Business Server 2003 per rimuovere Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Per disinstallare Exchange Server 2003 dal server di origine  
  
1. Accedere al server di origine come amministratore.  
  
2. Fare clic su **Start**, su **Pannello di controllo** e infine su **Installazione applicazioni**.  
  
3. Nell'elenco dei programmi selezionare **Windows Small Business Server 2003**, quindi fare clic su **Cambia/Rimuovi**.  
  
4. Nell'Installazione guidata fare clic su **Avanti** fino a visualizzare la pagina **Selezione componenti**.  
  
5. Nella pagina Selezione componenti espandere **Exchange Server** e quindi scegliere **Rimuovi**.  
  
   > [!NOTE]
   > 
   >  Exchange Server eseguirà una verifica per accertarsi che non ci siano cassette postali o cartelle pubbliche sul server. Se rimangono dei dati, viene visualizzato un messaggio di errore quando si fa clic su **Rimuovi**. Per evitare questo problema, assicurarsi di aver completato tutte le procedure descritte nell'argomento [spostare i dati e le impostazioni di SBS 2003 nel server di destinazione](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
   > 
   >  Exchange Server eseguirà una verifica per accertarsi che non ci siano cassette postali o cartelle pubbliche sul server. Se rimangono dei dati, viene visualizzato un messaggio di errore quando si fa clic su **Rimuovi**. Per evitare questo problema, assicurarsi di aver completato tutte le procedure descritte nell'argomento [spostare i dati e le impostazioni di SBS 2003 nel server di destinazione](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6. Fare clic su **Avanti**.  
  
7. Quando richiesto, inserire Windows Small Business Server 2003 CD # 3 e seguire le istruzioni visualizzate.  
  
###  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a>Disconnettere le stampanti direttamente connesse al server di origine  
 Prima di abbassare di livello il server di origine, scollegare fisicamente le stampanti direttamente connesse al server di origine e condivise con il server di origine. Verificare che non rimanga alcun oggetto Active Directory per le stampanti direttamente connesse al server di origine. Le stampanti possono quindi essere direttamente connesse al server di destinazione e condivise da Windows Server Essentials.  
  
###  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a>Abbassare di pagina il server di origine  
 Prima di abbassare di livello il server di origine dal ruolo del controller di dominio di Servizi di dominio Active Directory al ruolo di un server membro di dominio, verificare che le impostazioni di Criteri di gruppo siano applicate a tutti i computer client, come descritto nella procedura seguente.  
  
> [!IMPORTANT]
>  Il server di origine e il server di destinazione devono essere connessi alla rete mentre le modifiche a Criteri di gruppo vengono aggiornate nei computer client.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Per forzare un aggiornamento di Criteri di gruppo in un computer client  
  
1.  Accedere al computer client come amministratore.  
  
2.  Aprire una finestra del prompt dei comandi come amministratore.  
  
3.  Al prompt dei comandi digitare **gpupdate /force**, quindi premere INVIO.  
  
4.  Per completare il processo, potrebbe essere necessario disconnettersi e riconnettersi. Fare clic su **Sì** per confermare.  
  
##### <a name="to-demote-the-source-server"></a>Per abbassare di livello il server di origine  
  
1. Nel server di origine fare clic su **Start**, scegliere **Esegui**, digitare **dcpromo** e quindi fare clic su **OK**.  
  
2. Fare clic due volte su **Avanti**.  
  
   > [!NOTE]
   >  Non selezionare **Questo server è l'ultimo controller di dominio nel dominio**.  
  
3. Digitare una password per il nuovo account amministratore sul server e quindi fare clic su **Avanti**.  
  
4. Nella finestra di dialogo **Riepilogo** si viene informati che servizi di dominio Active Directory verrà rimosso dal computer e che il server diventerà un membro del dominio. Fare clic su **Avanti**.  
  
5. Fare clic su **Fine**. Il server di origine viene riavviato.  
  
6. Dopo che il server di origine è stato riavviato, aggiungerlo come membro di un gruppo di lavoro prima di disconnetterlo dalla rete.  
  
   Dopo aver aggiunto il server di origine come membro di un gruppo di lavoro e averlo disconnesso dalla rete, è necessario rimuoverlo da Servizi di dominio Active Directory sul server di destinazione.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Per rimuovere il server di origine da Active Directory  
  
1.  Nel server di destinazione aprire **Utenti e computer di Active Directory**.  
  
2.  Nella console di **Utenti e computer di Active Directory** espandere il nome di dominio e quindi fare clic su **Computer**.  
  
3.  Fare clic con il pulsante destro del mouse sul nome del server di origine se esiste ancora nell'elenco di server, scegliere **Elimina** e quindi fare clic su **Sì**.  
  
4.  Verificare che il server di origine non sia nell'elenco e quindi chiudere **Utenti e computer di Active Directory**.  
  
###  <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a><a name="BKMK_MoveTheDHCPRole"></a>Spostare il ruolo server DHCP dal server di origine al router  
  
> [!NOTE]
> 
>  Se questa attività è già stata eseguita prima di avviare il processo di migrazione, continuare con la sezione [Rimuovere e reimpiegare il server di origine](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  
> 
>  Se questa attività è già stata eseguita prima di avviare il processo di migrazione, continuare con la sezione [Rimuovere e reimpiegare il server di origine](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Se il server di origine esegue il ruolo DHCP, eseguire i passaggi seguenti per spostare il ruolo DHCP nel router.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Per spostare il ruolo DHCP dal server di origine al router  
  
1.  Disattivare il servizio DHCP sul server di origine, nel modo seguente:  
  
    1.  Nel server di origine fare clic su **Start**, scegliere **Strumenti di amministrazione** e quindi fare clic su **Servizi**.  
  
    2.  Nell'elenco di servizi attualmente in esecuzione fare clic con il pulsante destro del mouse su **Windows Server** e quindi scegliere **Proprietà**.  
  
    3.  Per **Tipo di avvio**, selezionare **Disabilitato**.  
  
    4.  Arrestare il servizio.  
  
2.  Attivare il ruolo DHCP sul router  
  
    1.  Seguire le istruzioni della documentazione del router per attivare il ruolo DHCP sul router.  
  
    2.  Per essere certi che gli indirizzi IP rilasciati dal server di origine rimangano gli stessi, seguire le istruzioni della documentazione del router per configurare un intervallo DHCP sul router uguale a quello sul server di origine.  
  
    > [!IMPORTANT]
    >  Se non è stato configurato un IP statico o prenotazioni DHCP sul router per il server di destinazione e l'intervallo DHCP non è uguale a quello del server di origine, è possibile che il router rilascerà un nuovo indirizzo IP per il server di destinazione. In questo caso, reimpostare le regole di port forwarding del router per l'inoltro al nuovo indirizzo IP del server di destinazione.  
  
###  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a>Rimuovere e reimpiegare il server di origine  
 Spegnere il server di origine e scollegarlo dalla rete. È consigliabile non riformattare il server di origine per almeno una settimana per essere certi che la migrazione al server di destinazione di tutti i dati necessari sia stata eseguita. Dopo aver verificato che sia stata eseguita la migrazione di tutti i dati, è possibile reinstallare questo server nella rete come server secondario per altre attività, se necessario.  
  
> [!NOTE]
>  Dopo aver abbassato di livello e rimosso il server di origine, riavviare il server di destinazione.  
  
 Il server di origine, dopo essere stato abbassato di livello, non è in uno stato integro. Se si vuole reimpiegare il server di origine, il modo più semplice è riformattarlo, installare un sistema operativo server e quindi configurarlo per l'uso come server aggiuntivo.
