---
title: Risolvere i problemi di connessione dei computer al server in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 52ec9bf1caa4cb4c7ed661eec1448f3f2a4be980
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436056"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Risolvere i problemi di connessione dei computer al server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 In questo argomento contiene informazioni sulla risoluzione dei problemi che possono verificarsi quando ci si connette un computer al server che esegue Windows Server Essentials o Windows Server Essentials.  
  
> [!NOTE]
>  Per informazioni aggiornate sulla risoluzione dei problemi di Windows Server Essentials e della community di Windows Server Essentials, si consiglia di visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). Il forum di Windows Server Essentials è ideale per cercare informazioni o per porre una domanda.  
  
 Questo articolo fornisce le soluzioni per i problemi seguenti:  
  

-   Problema 1: [Problema 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [Problema 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [Problema 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [Problema 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [Problema 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [Problema 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [Problema 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [Problema 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [Problema 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [Problema 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [Problema 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [Problema 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [Problema 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [Problema 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [Problema 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [Problema 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [Problema 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [Problema 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [Problema 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [Problema 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [Problema 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [Problema 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a> Problema 1  
 **Problema**  
  
 Ottenere un pacchetto di installazione non è riuscita. Provare a installare di nuovo il Connettore di Windows Server Essentials. Se il problema persiste, contattare l'errore di amministratore di rete quando ci si connette un computer al server  
  
 **Descrizione**  
  
 Questo problema può verificarsi se ci si connette un computer a un server che esegue Windows Server Essentials anche se sono in sospeso altri aggiornamenti di Windows o installazioni di applicazioni e l'installazione del connettore viene annullata.  
  
 **Soluzione**  
  
 Completare tutti gli altri aggiornamenti o installazioni di applicazioni. Se richiesto, riavviare il computer.  
  
##  <a name="BKMK_ConnectorIssue2"></a> Problema 2  
 **Problema**  
  
 Non è possibile aggiungere un computer a Windows Server Essentials  
  
 **Descrizione**  
  
 Computer che dispongono di caratteri non ASCII nel nome del computer non può essere unito a Windows Server Essentials. Se il nome del computer include caratteri non ASCII, viene visualizzato un messaggio che indica che si è verificato un errore imprevisto.  
  
 **Soluzione**  
  
 Rinominare il computer client con un nome che contenga solo caratteri ASCII e provare ad aggiungere nuovamente il computer a Windows Server Essentials.  
  
##  <a name="BKMK_ConnectorIssue2a"></a> Problema 3  
 **Problema**  
  
 Viene visualizzato un connettore di installazione software annullata errore quando ci si connette un computer al server  
  
 **Descrizione**  
  
 Per poter connettere un computer al server, l'account di sistema deve disporre delle autorizzazioni controllo completo sulle cartelle server visualizzate nel Dashboard di Windows Server Essentials. Se non sono state concesse le autorizzazioni necessarie, viene visualizzato un messaggio di errore che indica che l'installazione del software Connettore è stata annullata.  
  
 **Soluzione**  
  
 Concedere all'account SYSTEM autorizzazioni di tipo **Controllo completo** per ogni cartella del server.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Per concedere all'account SYSTEM autorizzazioni di tipo Controllo completo per una cartella del server  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **Archiviazione** e quindi su **Cartelle server**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella del server e quindi scegliere **Apri la cartella**. Se l'opzione **Apri la cartella** non viene visualizzata, non è necessario impostare le autorizzazioni per la cartella.  
  
4.  Nel percorso di rete nella parte superiore di Esplora risorse fare clic sulla condivisione server per visualizzare le cartelle condivise nel server. Se, ad esempio, il percorso è **Rete > Server01 > Backup Cronologia file**, fare clic su**Server01**.  
  
5.  Fare clic con il pulsante destro del mouse su una cartella del server e quindi scegliere **Proprietà**.  
  
6.  Fare clic sulla scheda **Sicurezza**.  
  
7.  Se l'autorizzazione **Controllo completo** non è disponibile per l'account SYSTEM, fare clic su **Modifica** e quindi su **SYSTEM**. In **Autorizzazioni per SYSTEM**selezionare la casella di controllo **Consenti** accanto a **Controllo completo**.  
  
8.  Fare clic su **OK** due volte per aggiornare le autorizzazioni e chiudere la finestra **Proprietà**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a> Problema 4  
 **Problema**  
  
 Ottenere r per eseguire questa applicazione, è necessario installare una delle seguenti versioni di .NET Framework: V4.5.50709" quando si connette un computer al server  
  
 **Descrizione**  
  
 Quando ci si connette un computer a un server che esegue Windows Server Essentials o Windows Server Essentials, la procedura guidata tenta di installare .NET Framework versione 4.5.50709 nel computer. Tuttavia, se è presenta una versione precedente di .NET Framework versione 4.5, la versione aggiornata non può essere installata e il tentativo di connessione ha esito negativo con questo messaggio di errore: Per eseguire questa applicazione, è necessario installare una delle seguenti versioni di .NET Framework: V4.5.50709. Per istruzioni su come ottenere la versione appropriata di .NET Framework, rivolgersi all'autore.  
  
 **Soluzione**  
  
 Disinstallare .NET Framework 4.5 dal computer e quindi connettere il computer al server.  
  
#### <a name="to-uninstall-net-framework-45"></a>Per disinstallare .NET Framework 4.5  
  
1.  Nella **pagina iniziale** del computer client aprire il **Pannello di controllo**.  
  
2.  Nel Pannello di controllo, in **Programmi**, fare clic su **Disinstalla un programma**.  
  
3.  Fare clic con il pulsante destro del mouse su **Microsoft .NET Framework 4.5**e quindi scegliere **Disinstalla**.  
  
4.  Dopo aver disinstallato .NET Framework 4.5, connettere il computer al server. La versione corretta di .NET Framework 4.5 viene installata insieme al software Connettore.  
  
##  <a name="BKMK_Time"></a> Problema 5  
 **Problema**  
  
 Ottenere un server non è disponibile. Per risolvere questo problema, contattare la persona responsabile della rete. quando si connette un computer al server  
  
 **Descrizione**  
  
 Questo problema può verificarsi se la data e l'ora nel computer connesso non sono sincronizzate con la data e l'ora nel server.  Windows Server Essentials e Windows Server Essentials è possibile usare il servizio di sincronizzazione ora per sincronizzare la data e ora del computer in esecuzione in una rete di Windows Server Essentials o Windows Server Essentials. L'ora sincronizzata è di importanza critica dal momento che il protocollo di autenticazione predefinito utilizza l'ora del server nell'ambito del processo di autenticazione. Ad esempio, se l'orologio in un computer client non verrà sincronizzato con la data e l'ora, Windows Server Essentials o l'autenticazione di Windows Server Essentials potrebbe essere erroneamente interpretato interpretare una richiesta di accesso come tentativo di intrusione e negare l'accesso all'utente.  
  
 Questa situazione può verificarsi se la memoria disponibile nel server è inferiore al 5%.  
  
 Il problema può verificarsi se è già stata stabilita una connessione VPN al server Windows Essentials e si tenta di configurare il software Connettore non in locale usando un indirizzo di dominio.  
  
 **Soluzione**  
  
1.  Sincronizzare la data e l'ora nel computer client con quelle nel server. Connettere quindi il computer al server.  
  
2.  Chiudere alcune applicazioni sul lato server e quindi connettere il computer al server.  
  
3.  Chiudere la connessione VPN e quindi connettere il computer al server.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Per modificare la data e l'ora nel computer client  
  
1. Nella pagina iniziale del computer client aprire il **Pannello di controllo**.  
  
2. Nel Pannello di controllo fare clic su **Orologio e opzioni internazionali**e quindi su **Data e ora**.  
  
3. Fare clic su **Modifica data e ora**, impostare la data e l'ora corrette e quindi fare clic su **OK**.  
  
4. Fare clic su **OK** e quindi chiudere il Pannello di controllo.  
  
5. Provare di nuovo a connettere il computer client al server. Per istruzioni, vedere Connettere computer al server.  
  
   Se ancora non è possibile connettere il computer client al server, verificare che la data e l'ora nel server siano corrette. Se la data e l'ora non sono corrette, modificarle.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Per modificare la data e l'ora nel server  
  
1.  Accedere al server usando la password impostata durante la configurazione o installazione di Windows Server Essentials e Windows Server Essentials.  
  
    > [!NOTE]
    >  Se si sta gestendo il server in remoto, è necessario accedere al server tramite Connessione Desktop remoto.  
  
2.  Nella **pagina iniziale** aprire il **Pannello di controllo**.  
  
3.  Nel Pannello di controllo fare clic su **Orologio e opzioni internazionali**e quindi su **Data e ora**.  
  
4.  Fare clic su **Modifica data e ora**, impostare la data e l'ora corrette e quindi fare clic su **OK**.  
  
5.  Fare clic su **OK** e quindi chiudere il Pannello di controllo.  
  
6.  Nel computer client provare di nuovo a connettersi al server. Per istruzioni, vedere Connettere computer al server.  
  
##  <a name="BKMK_ServiceStopped"></a> Problema 6  
 **Problema**  
  
 Ottenere un An si è verificato un errore imprevisto. Per risolvere questo problema, contattare la persona responsabile della rete. quando si connette un computer al server  
  
 **Descrizione**  
  
 Se viene visualizzato questo messaggio di errore, il servizio WSS Certificate Web Service potrebbe non essere in esecuzione.  
  
 **Soluzione**  
  
 Avviare il servizio WSS Certificate Web Service.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Per avviare il servizio WSS Certificate Web Service  
  
1.  Nella **pagina iniziale** del server aprire **Strumenti di amministrazione**.  
  
     In **Strumenti di amministrazione**fare clic con il pulsante destro del mouse su **Gestione Internet Information Services (IIS)** e quindi scegliere **Apri**.  
  
2.  Nel riquadro di spostamento fare clic su **WSS Certificate Web Service**.  
  
3.  Nel riquadro **Azioni** fare clic su **Avvia**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a> Problema 7  
 **Problema**  
  
 Quando tenta di connettere un computer al server dopo un tentativo di connessione non riuscito, viene visualizzato l'avviso di che un computer con questo nome è già connesso al server  
  
 **Descrizione**  
  
 Se un tentativo precedente di connettere un computer al server è stato annullato o interrotto, potrebbe venire visualizzato l'avviso seguente quando si tenta di connettersi di nuovo: Un computer con questo nome è già connesso al server. Ciò accade perché durante il primo tentativo di connessione al server è stato emesso un certificato.  
  
 **Soluzione** Se si è certi che non ci sia nessun altro computer con lo stesso nome già connesso al server, fare clic su **Avanti** e quindi seguire le istruzioni per completare la procedura guidata **Connetti il computer al server**.  
  
##  <a name="BKMK_JoinWin7"></a> Problema 8  
 **Problema**  
  
 Quando si tenta di connettere un computer client che esegue Windows 7 Home al server, viene visualizzata la pagina Web per l'esecuzione del software Connettore, ma il computer client non può connettersi al server  
  
 **Descrizione**  
  
 Se per il router nella rete è abilitato il multicast, le comunicazioni tra il server e un computer client che esegue Windows 7 Home Basic o Windows 7 Home Premium non funzionano correttamente.  
  
 **Soluzione**  
  
 Disabilitare il multicast nel router. In alcuni router, a tale scopo potrebbe essere necessario disabilitare il protocollo di routing RIP-2M. Per altre informazioni, vedere la documentazione fornita dal produttore del router.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a> Problema 9  
 **Problema**  
  
 L'accesso automatico ha smesso di funzionare dopo aver connesso il computer al server  
  
 **Descrizione**  
  
 Se è impostato l'accesso automatico per l'account utente quando ci si connette il computer a Windows Server Essentials, l'impostazione viene sovrascritta quando il software connettore viene installato nel computer.  
  
 **Soluzione** Per risolvere questo problema, quando si connette il computer al server prendere nota della password usata per l'account utente. Dopo l'installazione del software Connettore, configurare l'accesso automatico per l'uso di tale account.  
  
> [!NOTE]
>  L'account di dominio Windows Server Essentials richiede una password che soddisfi i requisiti dei criteri di password predefinito.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a> Problema 10  
 **Problema**  
  
 La disinstallazione di una versione non definitiva del software Connettore non rimuove i log esistenti  
  
 **Descrizione**  
  
 Dopo l'aggiornamento da una versione non definitiva (Beta o RC) di Windows Server Essentials alla versione rilasciata, è necessario rimuovere il software connettore da ogni computer in cui è stato connesso al server e quindi connettere il computer per completare l'installazione released versione del software connettore.  
  
 Tuttavia, quando si rimuove il software Connettore da un computer di rete, i file di log esistenti nella cartella %ProgramData%\Microsoft\Windows Server\Logs\ in tale computer non vengono eliminati. Se non si elimina la cartella Logs, i file di log possono venire danneggiati quando si connette il computer alla versione rilasciata di Windows Server Essentials.  
  
 **Soluzione** Per evitare che i file di log vengano danneggiati, eliminare manualmente la cartella Logs prima di riconnettere il computer client al server aggiornato.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Per riconnettere un computer dopo l'aggiornamento del server senza danneggiare i file di log  
  
1.  Disinstallare il software Connettore dal computer client.  
  
2.  Eliminare la cartella Logs da %ProgramData%\Microsoft\Windows Server\.  
  
3.  Connettere nuovamente il computer al server. In questo modo verrà installata la versione rilasciata del software Connettore e verranno creati una nuova cartella Logs e nuovi file di log.  
  
##  <a name="BKMK_UpgradeClientOS"></a> Problema 11  
 **Problema**  
  
 Voglio aggiornare il sistema operativo in un computer client  
  
 **Descrizione**  
  
 Durante l'installazione del software Connettore, vengono eseguiti numerosi controlli sul sistema operativo client per verificare che il client soddisfi tutti i prerequisiti del connettore. Se si aggiorna il sistema operativo client dopo aver installato il software Connettore, alcuni dei prerequisiti potrebbero non essere presenti e potrebbe verificarsi un errore del connettore client.  
  
 **Soluzione**  
  
 Prima di aggiornare il sistema operativo client a una versione diversa (ad esempio, aggiornare Windows XP a Windows Vista o Windows Vista a Windows 7), è necessario disinstallare il software Connettore. Usare **Installazione applicazioni** nel Pannello di controllo. Dopo aver completato l'aggiornamento del sistema operativo client, è possibile reinstallare il connettore client aprendo http://&lt*server*> / connect in un Web browser, dove <*server*> è il nome del  Server di Windows Server Essentials.  
  
 Se è già stato eseguito l'aggiornamento del client con il software Connettore installato, usare **Installazione applicazioni** o **Programmi e funzionalità** per disinstallare il software Connettore. Installare quindi di nuovo il software Connettore.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials a ConnectComputer in risoluzione dei problemi (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
