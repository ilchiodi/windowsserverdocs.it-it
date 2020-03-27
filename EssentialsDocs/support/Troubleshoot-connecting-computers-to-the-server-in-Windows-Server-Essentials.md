---
title: Risolvere i problemi di connessione dei computer al server in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4e2a707bf72ca7e371b6503116262e737102c769
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318589"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Risolvere i problemi di connessione dei computer al server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Questo argomento contiene informazioni aggiuntive sulla risoluzione dei problemi che possono verificarsi quando si connette un computer al server che esegue Windows Server Essentials o Windows Server Essentials.  
  
> [!NOTE]
>  Per le informazioni più aggiornate sulla risoluzione dei problemi della community di Windows Server Essentials e Windows Server Essentials, è consigliabile visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). Il forum di Windows Server Essentials è ideale per cercare informazioni o per porre una domanda.  
  
 Questo articolo fornisce le soluzioni per i problemi seguenti:  
  

-   Problema 1: [problema 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [problema 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [problema 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [problema 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [problema 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [problema 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [problema 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [problema 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [problema 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [problema 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [problema 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [problema 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [problema 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [problema 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [problema 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [problema 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [problema 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [problema 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [problema 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [problema 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [problema 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [problema 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="issue-1"></a><a name="BMRK_Package"></a>Problema 1  
 **Problema**  
  
 L'installazione di un pacchetto ha avuto esito negativo. Provare a installare di nuovo il Connettore di Windows Server Essentials. Se il problema persiste, contattare l'amministratore di rete errore quando si connette un computer al server  
  
 **Descrizione**  
  
 Questo problema può verificarsi se si connette un computer a un server che esegue Windows Server Essentials mentre sono in sospeso altri aggiornamenti di Windows o installazioni di applicazioni e l'installazione del connettore viene annullata.  
  
 **Soluzione**  
  
 Completare tutti gli altri aggiornamenti o installazioni di applicazioni. Se richiesto, riavviare il computer.  
  
##  <a name="issue-2"></a><a name="BKMK_ConnectorIssue2"></a>Problema 2  
 **Problema**  
  
 Non è possibile aggiungere un computer a Windows Server Essentials  
  
 **Descrizione**  
  
 I computer con caratteri non ASCII nel nome del computer non possono essere aggiunti a Windows Server Essentials. Se il nome del computer include caratteri non ASCII, viene visualizzato un messaggio che indica che si è verificato un errore imprevisto.  
  
 **Soluzione**  
  
 Rinominare il computer client con un nome che contiene solo caratteri ASCII e provare ad aggiungere di nuovo il computer a Windows Server Essentials.  
  
##  <a name="issue-3"></a><a name="BKMK_ConnectorIssue2a"></a>Problema 3  
 **Problema**  
  
 Si verifica un errore durante la connessione di un computer al server per l'installazione del software del connettore  
  
 **Descrizione**  
  
 Per poter connettere un computer al server, è necessario che l'account di sistema disponga delle autorizzazioni di controllo completo per le cartelle del server visualizzate nel dashboard di Windows Server Essentials. Se non sono state concesse le autorizzazioni necessarie, viene visualizzato un messaggio di errore che indica che l'installazione del software Connettore è stata annullata.  
  
 **Soluzione**  
  
 Concedere all'account SYSTEM autorizzazioni di tipo **Controllo completo** per ogni cartella del server.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Per concedere all'account SYSTEM autorizzazioni di tipo Controllo completo per una cartella del server  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Fare clic su **Archiviazione** e quindi su **Cartelle server**.  
  
3.  Fare clic con il pulsante destro del mouse sulla cartella del server e quindi scegliere **Apri la cartella**. Se l'opzione **Apri la cartella** non viene visualizzata, non è necessario impostare le autorizzazioni per la cartella.  
  
4.  Nel percorso di rete nella parte superiore di Esplora risorse fare clic sulla condivisione server per visualizzare le cartelle condivise nel server. Se, ad esempio, il percorso è **Rete > Server01 > Backup Cronologia file**, fare clic su**Server01**.  
  
5.  Fare clic con il pulsante destro del mouse su una cartella del server e quindi scegliere **Proprietà**.  
  
6.  Fare clic sulla scheda **Security**.  
  
7.  Se l'autorizzazione **Controllo completo** non è disponibile per l'account SYSTEM, fare clic su **Modifica** e quindi su **SYSTEM**. In **Autorizzazioni per SYSTEM** selezionare la casella di controllo **Consenti** accanto a **Controllo completo**.  
  
8.  Fare clic su **OK** due volte per aggiornare le autorizzazioni e chiudere la finestra **Proprietà**.  
  
##  <a name="issue-4"></a><a name="BKMK_ConnectorIssueNetFramework"></a>Problema 4  
 **Problema**  
  
 Quando si esegue questa applicazione, è necessario installare una delle seguenti versioni dell'errore .NET Framework: V 4.5.50709 "durante la connessione di un computer al server  
  
 **Descrizione**  
  
 Quando si connette un computer a un server che esegue Windows Server Essentials o Windows Server Essentials, la procedura guidata tenta di installare .NET Framework versione 4.5.50709 nel computer. Tuttavia, se è presente una versione precedente di .NET Framework versione 4,5, la versione aggiornata non può essere installata e il tentativo di connessione ha esito negativo con questo messaggio di errore: per eseguire l'applicazione, è necessario installare una delle seguenti versioni del .NET Framework: V 4.5.50709. Per istruzioni su come ottenere la versione appropriata del .NET Framework, contattare l'autore dell'allocazione.  
  
 **Soluzione**  
  
 Disinstallare .NET Framework 4.5 dal computer e quindi connettere il computer al server.  
  
#### <a name="to-uninstall-net-framework-45"></a>Per disinstallare .NET Framework 4.5  
  
1.  Nella **pagina iniziale** del computer client aprire il **Pannello di controllo**.  
  
2.  Nel Pannello di controllo, in **Programmi**, fare clic su **Disinstalla un programma**.  
  
3.  Fare clic con il pulsante destro del mouse su **Microsoft .NET Framework 4.5** e quindi scegliere **Disinstalla**.  
  
4.  Dopo aver disinstallato .NET Framework 4.5, connettere il computer al server. La versione corretta di .NET Framework 4.5 viene installata insieme al software Connettore.  
  
##  <a name="issue-5"></a><a name="BKMK_Time"></a>Problema 5  
 **Problema**  
  
 Ricevo un server non disponibile. Per risolvere il problema, contattare la persona responsabile della rete. quando si connette un computer al server  
  
 **Descrizione**  
  
 Questo problema può verificarsi se la data e l'ora nel computer connesso non sono sincronizzate con la data e l'ora nel server.  Windows Server Essentials e Windows Server Essentials usano il servizio di sincronizzazione dell'ora per sincronizzare la data e l'ora dei computer che eseguono in una rete Windows Server Essentials o Windows Server Essentials. L'ora sincronizzata è di importanza critica dal momento che il protocollo di autenticazione predefinito utilizza l'ora del server nell'ambito del processo di autenticazione. Ad esempio, se l'orologio in un computer client non è sincronizzato con la data e l'ora corrette, Windows Server Essentials o l'autenticazione di Windows Server Essentials potrebbe interpretare erroneamente una richiesta di accesso come tentativo di intrusione e negare l'accesso all'utente.  
  
 Questo problema può verificarsi se la memoria disponibile del server è inferiore al 5%.  
  
 Il problema può verificarsi se è già stata stabilita una connessione VPN al server Windows Essentials e si tenta di configurare il software Connettore non in locale usando un indirizzo di dominio.  
  
 **Soluzione**  
  
1.  Sincronizzare la data e l'ora nel computer client con quelle nel server. Connettere quindi il computer al server.  
  
2.  Chiudere alcune applicazioni sul lato server e quindi connettere il computer al server.  
  
3.  Chiudere la connessione VPN e quindi connettere il computer al server.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Per modificare la data e l'ora nel computer client  
  
1. Nella pagina iniziale del computer client aprire il **Pannello di controllo**.  
  
2. Nel Pannello di controllo fare clic su **Orologio e opzioni internazionali** e quindi su **Data e ora**.  
  
3. Fare clic su **Modifica data e ora**, impostare la data e l'ora corrette e quindi fare clic su **OK**.  
  
4. Fare clic su **OK** e quindi chiudere il Pannello di controllo.  
  
5. Provare di nuovo a connettere il computer client al server. Per istruzioni, vedere Connettere computer al server.  
  
   Se ancora non è possibile connettere il computer client al server, verificare che la data e l'ora nel server siano corrette. Se la data e l'ora non sono corrette, modificarle.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Per modificare la data e l'ora nel server  
  
1.  Accedere al server usando la password impostata durante l'installazione e la configurazione di Windows Server Essentials o Windows Server Essentials.  
  
    > [!NOTE]
    >  Se si sta gestendo il server in remoto, è necessario accedere al server tramite Connessione Desktop remoto.  
  
2.  Nella **pagina iniziale** aprire il **Pannello di controllo**.  
  
3.  Nel Pannello di controllo fare clic su **Orologio e opzioni internazionali** e quindi su **Data e ora**.  
  
4.  Fare clic su **Modifica data e ora**, impostare la data e l'ora corrette e quindi fare clic su **OK**.  
  
5.  Fare clic su **OK** e quindi chiudere il Pannello di controllo.  
  
6.  Nel computer client provare di nuovo a connettersi al server. Per istruzioni, vedere Connettere computer al server.  
  
##  <a name="issue-6"></a><a name="BKMK_ServiceStopped"></a>Problema 6  
 **Problema**  
  
 Si verifica un errore imprevisto. Per risolvere il problema, contattare la persona responsabile della rete. quando si connette un computer al server  
  
 **Descrizione**  
  
 Se viene visualizzato questo messaggio di errore, il servizio WSS Certificate Web Service potrebbe non essere in esecuzione.  
  
 **Soluzione**  
  
 Avviare il servizio WSS Certificate Web Service.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Per avviare il servizio WSS Certificate Web Service  
  
1.  Nella **pagina iniziale** del server aprire **Strumenti di amministrazione**.  
  
     In **Strumenti di amministrazione** fare clic con il pulsante destro del mouse su **Gestione Internet Information Services (IIS)** e quindi scegliere **Apri**.  
  
2.  Nel riquadro di spostamento fare clic su **WSS Certificate Web Service**.  
  
3.  Nel riquadro **Azioni** fare clic su **Avvia**.  
  
##  <a name="issue-7"></a><a name="BKMK_ConnectorIssueReconnect"></a>Problema 7  
 **Problema**  
  
 Quando si tenta di connettere di nuovo un computer al server dopo un tentativo di connessione non riuscito, viene visualizzato l'avviso un computer con questo nome è già connesso al server  
  
 **Descrizione**  
  
 Se un tentativo precedente di connettere un computer al server è stato annullato o interrotto, è possibile che venga visualizzato il seguente avviso quando si tenta di connettersi di nuovo: un computer con questo nome è già connesso al server. Ciò accade perché durante il primo tentativo di connessione al server è stato emesso un certificato.  
  
 **Soluzione** Se si è certi che non ci sia nessun altro computer con lo stesso nome già connesso al server, fare clic su **Avanti** e quindi seguire le istruzioni per completare la procedura guidata **Connetti il computer al server**.  
  
##  <a name="issue-8"></a><a name="BKMK_JoinWin7"></a>Problema 8  
 **Problema**  
  
 Quando si tenta di connettere un computer client che esegue Windows 7 Home al server, viene visualizzata la pagina Web per l'esecuzione del software Connettore, ma il computer client non può connettersi al server  
  
 **Descrizione**  
  
 Se per il router nella rete è abilitato il multicast, le comunicazioni tra il server e un computer client che esegue Windows 7 Home Basic o Windows 7 Home Premium non funzionano correttamente.  
  
 **Soluzione**  
  
 Disabilitare il multicast nel router. In alcuni router, a tale scopo potrebbe essere necessario disabilitare il protocollo di routing RIP-2M. Per altre informazioni, vedere la documentazione fornita dal produttore del router.  
  
##  <a name="issue-9"></a><a name="BKMK_ConnectorIssueAutologon"></a>Problema 9  
 **Problema**  
  
 L'accesso automatico ha smesso di funzionare dopo aver connesso il computer al server  
  
 **Descrizione**  
  
 Se per l'account utente è impostato l'accesso automatico quando si connette il computer a Windows Server Essentials, l'impostazione viene sovrascritta quando il software connettore viene installato nel computer.  
  
 **Soluzione** Per risolvere questo problema, quando si connette il computer al server prendere nota della password usata per l'account utente. Dopo l'installazione del software Connettore, configurare l'accesso automatico per l'uso di tale account.  
  
> [!NOTE]
>  L'account di dominio di Windows Server Essentials richiede una password che soddisfi i requisiti dei criteri password predefiniti.  
  
##  <a name="issue-10"></a><a name="BKMK_ConnectorIssueOldLogs"></a>Problema 10  
 **Problema**  
  
 La disinstallazione di una versione non definitiva del software Connettore non rimuove i log esistenti  
  
 **Descrizione**  
  
 Dopo aver eseguito l'aggiornamento da una versione non definitiva (beta o RC) di Windows Server Essentials alla versione rilasciata, è necessario rimuovere il software connettore da ogni computer connesso al server e quindi connettere di nuovo il computer per installare la versione rilasciata versione del software connettore.  
  
 Tuttavia, quando si rimuove il software Connettore da un computer di rete, i file di log esistenti nella cartella %ProgramData%\Microsoft\Windows Server\Logs\ in tale computer non vengono eliminati. Se non si elimina la cartella logs, i file di log possono risultare danneggiati quando si connette il computer alla versione rilasciata di Windows Server Essentials.  
  
 **Soluzione** Per evitare che i file di log vengano danneggiati, eliminare manualmente la cartella Logs prima di riconnettere il computer client al server aggiornato.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Per riconnettere un computer dopo l'aggiornamento del server senza danneggiare i file di log  
  
1.  Disinstallare il software Connettore dal computer client.  
  
2.  Eliminare la cartella Logs da %ProgramData%\Microsoft\Windows Server\.  
  
3.  Connettere nuovamente il computer al server. In questo modo verrà installata la versione rilasciata del software Connettore e verranno creati una nuova cartella Logs e nuovi file di log.  
  
##  <a name="issue-11"></a><a name="BKMK_UpgradeClientOS"></a>Problema 11  
 **Problema**  
  
 Voglio aggiornare il sistema operativo in un computer client  
  
 **Descrizione**  
  
 Durante l'installazione del software Connettore, vengono eseguiti numerosi controlli sul sistema operativo client per verificare che il client soddisfi tutti i prerequisiti del connettore. Se si aggiorna il sistema operativo client dopo aver installato il software Connettore, alcuni dei prerequisiti potrebbero non essere presenti e potrebbe verificarsi un errore del connettore client.  
  
 **Soluzione**  
  
 Prima di aggiornare il sistema operativo client a una versione diversa (ad esempio, aggiornare Windows XP a Windows Vista o Windows Vista a Windows 7), è necessario disinstallare il software Connettore. Usare **Installazione applicazioni** nel Pannello di controllo. Al termine dell'aggiornamento del sistema operativo client, è possibile reinstallare il connettore client aprendo http://<*server*>/Connect in un Web browser, dove <*Server*> è il nome del server di Windows Server Essentials.  
  
 Se è già stato eseguito l'aggiornamento del client con il software Connettore installato, usare **Installazione applicazioni** o **Programmi e funzionalità** per disinstallare il software Connettore. Installare quindi di nuovo il software Connettore.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Risoluzione dei problemi di ConnectComputer per Windows 2012 Server Essentials (TechNet wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
