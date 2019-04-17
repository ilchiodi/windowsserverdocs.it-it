---
title: 'Passaggio 6: Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials'
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
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Passaggio 6: Abbassare di livello e rimuovere il Server di origine dalla nuova rete Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Dopo aver completato l'installazione di Windows Server Essentials e aver completato la migrazione, è necessario eseguire le attività seguenti:  
  
1.  [Rimuovere Servizi certificati Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Disconnettere le stampanti direttamente connesse al Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Abbassare di livello il Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Rimuovere e reimpiegare il Server di origine](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>Rimuovere Servizi certificati Active Directory  
 La procedura è leggermente diversa se più servizi ruolo di servizi certificati Active Directory (AD CS) installati in un singolo server. È possibile utilizzare la procedura seguente per disinstallare un servizio ruolo di servizi certificati Active Directory e conservare gli altri servizi ruolo Servizi certificati Active Directory.  
  
 Per completare questa procedura, è necessario accedere con le stesse autorizzazioni dell'utente che ha installato l'autorità di certificazione (CA). Se si sta disinstallando una CA dell'organizzazione, l'appartenenza al gruppo Enterprise Admins o equivalente è il requisito minimo necessario per completare questa procedura.  
  
#### <a name="to-remove-ad-cs"></a>Per rimuovere Servizi certificati Active Directory  
  
1.  Accedere al Server di origine come amministratore di dominio.  
  
2.  Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Server Manager**.  
  
3.  Fare clic su **continua** nel **controllo dell'Account utente** la finestra di dialogo.  
  
4.  Nel **Riepilogo ruoli** fare clic su **Rimuovi ruoli**.  
  
5.  Nella rimozione guidata ruoli fare clic su **Avanti**.  
  
6.  Cancella il **Servizi certificati Active Directory** casella di controllo, quindi fare clic su **Avanti**.  
  
7.  Nel **Conferma opzioni di rimozione** pagina, esaminare le informazioni e quindi fare clic su **rimuovere**.  
  
    > [!NOTE]
    >  Se Internet Information Services (IIS) è in esecuzione, viene chiesto di arrestare il servizio prima di procedere. Fare clic su **OK**.  
  
    > [!NOTE]
    >  Prima di tutto, potrebbe essere necessario rimuovere **registrazione Web autorità di certificazione**, se è installato.  
  
8.  Al termine della rimozione guidata ruoli, riavviare il server per completare il processo di disinstallazione.  
  
    > [!IMPORTANT]
    >  Riavviare il server anche se non viene richiesto di farlo.  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>Disconnettere le stampanti direttamente connesse al Server di origine  
 Prima di abbassare di livello il Server di origine, scollegare fisicamente le stampanti direttamente connesse al Server di origine e condivise attraverso il Server di origine. Non garantire che nessun oggetto di Active Directory per le stampanti direttamente connesse al Server di origine. Le stampanti possono quindi essere direttamente connesso al Server di destinazione e condivise da Windows Server Essentials.  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>Abbassare di livello il Server di origine  
 Prima di abbassare di livello il Server di origine dal ruolo di controller di dominio Active Directory al ruolo di un server membro del dominio, verificare che le impostazioni di criteri di gruppo siano applicate a tutti i computer client, come descritto nella procedura seguente.  
  
> [!IMPORTANT]
>  Il Server di origine e il Server di destinazione devono essere connessi alla rete mentre le modifiche di criteri di gruppo vengono aggiornate nei computer client.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Per forzare un aggiornamento di criteri di gruppo in un computer client  
  
1.  Accedere al computer client come amministratore.  
  
2.  Aprire una finestra del prompt dei comandi come amministratore.  
  
3.  Al prompt dei comandi, digitare **gpupdate /force**, quindi premere INVIO.  
  
4.  Il processo potrebbe essere necessario disconnettersi e accedere di nuovo per terminare. Fare clic su **Sì** per confermare.  
  
 Se si esegue la migrazione da Windows Server Essentials o versioni precedenti, per abbassare di livello il server, vedere [rimuovere Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Dopo aver aggiunto il Server di origine come membro di un gruppo di lavoro e averlo disconnesso dalla rete, è necessario rimuoverlo da servizi di dominio Active Directory nel Server di destinazione.  
  
 Se si esegue la migrazione da Windows Server Essentials, è possibile utilizzare Server Manager per rimuovere il ruolo di servizi di dominio Active Directory, abbassando così di livello il controller di dominio nel Server di origine utilizzando la procedura seguente:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Per rimuovere il Server di origine da Active Directory  
  
1.  Nel Server di destinazione, aprire **Active Directory Users and Computers**.  
  
2.  Nel **Active Directory Users and Computers** riquadro di spostamento, espandere il nome di dominio e quindi espandere **computer**.  
  
3.  Se il Server di origine esiste ancora nell'elenco dei server, fare clic sul nome del Server di origine, fare clic su **eliminare**, quindi fare clic su **Sì**.  
  
4.  Verificare che il Server di origine non è nell'elenco e quindi Chiudi **Active Directory Users and Computers**.  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>Rimuovere e reimpiegare il Server di origine  
 Disattivare il Server di origine e averlo disconnesso dalla rete. È consigliabile non riformattare il Server di origine per almeno una settimana per garantire che tutti i dati necessari eseguire la migrazione al Server di destinazione. Dopo aver verificato che tutti i dati è stata eseguita la migrazione, è possibile reinstallare questo server nella rete come server secondario per altre attività, se necessario.  
  
> [!NOTE]
>  Dopo aver abbassare di livello e rimuovere il Server di origine, riavviare il Server di destinazione.  
  
 Dopo che si abbassa di livello il Server di origine, non è in uno stato integro. Se si vuole reimpiegare il Server di origine, il modo più semplice è riformattarlo, installare un sistema operativo server, e quindi configurare l'utilizzo come server aggiuntivo.  
  
## <a name="next-steps"></a>Passaggi successivi  
 È stato abbassato di livello e rimosso il Server di origine dalla nuova rete Windows Server Essentials. Passare quindi a [passaggio 7: eseguire le attività post-migrazione per la migrazione di Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Per visualizzare tutti i passaggi, vedere [la migrazione a Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

