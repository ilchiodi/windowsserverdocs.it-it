---
title: 'Passaggio 6: Abbassare di livello e rimuovere il server di origine dalla nuova rete Windows Server Essentials'
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6842137dd498b11bccc2216023d648d61edbb87e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432540"
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Passaggio 6: Abbassare di livello e rimuovere il server di origine dalla nuova rete Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Dopo aver finito di installare Windows Server Essentials e aver completato la migrazione, è necessario eseguire le attività seguenti:  
  
1.  [Rimuovere Servizi certificati Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Disconnettere le stampanti direttamente connesse al Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Abbassare di livello il Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Rimuovere e reimpiegare il Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a> Rimuovere Servizi certificati Active Directory  
 La procedura è leggermente diversa se più servizi ruolo di Servizi certificati Active Directory sono installati in un solo server. È possibile usare la procedura seguente per disinstallare un servizio ruolo di Servizi certificati Active Directory e conservare gli altri servizi ruolo di Servizi certificati Active Directory.  
  
 Per completare questa procedura, è necessario accedere con le stesse autorizzazioni dell'utente che ha installato l'Autorità di certificazione (CA). Se si deve disinstallare una CA globale (enterprise), per eseguire questa procedura è richiesta almeno l'appartenenza al gruppo Enterprise Admins oppure a un gruppo equivalente.  
  
#### <a name="to-remove-ad-cs"></a>Per rimuovere Servizi certificati Active Directory  
  
1.  Accedere al server di origine come amministratore di dominio.  
  
2.  Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Server Manager**.  
  
3.  Fare clic su **Continua** nella finestra di dialogo **Controllo account utente** .  
  
4.  Nella sezione **Riepilogo ruoli** fare clic su **Rimuovi ruoli**.  
  
5.  Nella Rimozione guidata ruoli fare clic su **Avanti**.  
  
6.  Deselezionare la casella di controllo **Servizi certificati Active Directory** e quindi fare clic su **Avanti**.  
  
7.  Nella pagina **Conferma opzioni di rimozione** esaminare le informazioni e quindi fare clic su **Rimuovi**.  
  
    > [!NOTE]
    >  Se Internet Information Services (IIS) è in esecuzione, viene richiesto di arrestare il servizio prima di continuare. Fare clic su **OK**.  
  
    > [!NOTE]
    >  Prima di tutto, può essere necessario rimuovere ruolo **Registrazione Web Autorità di certificazione**, se è installato.  
  
8.  Al termine della Rimozione guidata ruoli, riavviare il server per completare il processo di disinstallazione.  
  
    > [!IMPORTANT]
    >  Riavviare il server anche se non viene richiesto.  
  
##  <a name="BKMK_PhysicallyDisconnect"></a> Disconnettere le stampanti direttamente connesse al Server di origine  
 Prima di abbassare di livello il server di origine, scollegare fisicamente le stampanti direttamente connesse al server di origine e condivise con il server di origine. Verificare che non rimanga alcun oggetto Active Directory per le stampanti direttamente connesse al server di origine. Le stampanti possono quindi essere direttamente connesso al Server di destinazione e condivisi da Windows Server Essentials.  
  
##  <a name="BKMK_DemoteTheSourceServer"></a> Abbassare di livello il Server di origine  
 Prima di abbassare di livello il server di origine dal ruolo del controller di dominio di Servizi di dominio Active Directory al ruolo di un server membro di dominio, verificare che le impostazioni di Criteri di gruppo siano applicate a tutti i computer client, come descritto nella procedura seguente.  
  
> [!IMPORTANT]
>  Il server di origine e il server di destinazione devono essere connessi alla rete mentre le modifiche a Criteri di gruppo vengono aggiornate nei computer client.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Per forzare un aggiornamento di Criteri di gruppo in un computer client  
  
1. Accedere al computer client come amministratore.  
  
2. Aprire una finestra del prompt dei comandi come amministratore.  
  
3. Al prompt dei comandi digitare **gpupdate /force**, quindi premere INVIO.  
  
4. Per completare il processo, potrebbe essere necessario disconnettersi e riconnettersi. Fare clic su **Sì** per confermare.  
  
   Se si esegue la migrazione da Windows Server Essentials o versioni precedenti, per abbassare di livello server, vedere [rimuovere Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Dopo aver aggiunto il server di origine come membro di un gruppo di lavoro e averlo disconnesso dalla rete, è necessario rimuoverlo da Servizi di dominio Active Directory sul server di destinazione.  
  
   Se si esegue la migrazione da Windows Server Essentials, usare Server Manager per rimuovere il ruolo Servizi di dominio Active Directory, abbassando così di livello il controller di dominio nel Server di origine usando la procedura seguente:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Per rimuovere il server di origine da Active Directory  
  
1.  Nel server di destinazione aprire **Utenti e computer di Active Directory**.  
  
2.  Nella console di **Utenti e computer di Active Directory** espandere il nome di dominio e quindi fare clic su **Computer**.  
  
3.  Se il server di origine esiste ancora nell'elenco di server, fare clic con il pulsante destro del mouse sul nome del server di origine, scegliere **Elimina** e quindi fare clic su **Sì**.  
  
4.  Verificare che il server di origine non sia nell'elenco e quindi chiudere **Utenti e computer di Active Directory**.  
  
##  <a name="BKMK_RemoveTheSourceServer"></a> Rimuovere e reimpiegare il Server di origine  
 Spegnere il server di origine e scollegarlo dalla rete. È consigliabile non riformattare il server di origine per almeno una settimana per essere certi che la migrazione al server di destinazione di tutti i dati necessari sia stata eseguita. Dopo aver verificato che sia stata eseguita la migrazione di tutti i dati, è possibile reinstallare questo server nella rete come server secondario per altre attività, se necessario.  
  
> [!NOTE]
>  Dopo aver abbassato di livello e rimosso il server di origine, riavviare il server di destinazione.  
  
 Il server di origine, dopo essere stato abbassato di livello, non è in uno stato integro. Se si vuole reimpiegare il server di origine, il modo più semplice è riformattarlo, installare un sistema operativo server e quindi configurarlo per l'uso come server aggiuntivo.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato abbassato di livello e rimosso il Server di origine dalla nuova rete Windows Server Essentials. Passare quindi a [passaggio 7: Eseguire attività post-migrazione per la migrazione di Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Per visualizzare tutti i passaggi, vedere [eseguire la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

