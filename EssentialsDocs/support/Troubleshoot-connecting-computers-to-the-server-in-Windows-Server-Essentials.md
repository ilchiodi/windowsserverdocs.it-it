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
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Risolvere i problemi di connessione dei computer al server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 In questo argomento contiene informazioni sulla risoluzione dei problemi che possono verificarsi quando si connette un computer al server che esegue Windows Server Essentials o Windows Server Essentials.  
  
> [!NOTE]
>  Per informazioni aggiornate sulla risoluzione dei problemi dalla community di Windows Server Essentials e Windows Server Essentials, si consiglia di visitare il [Forum di Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). Forum di Windows Server Essentials è un ottimo punto per cercare informazioni o per porre una domanda.  
  
 In questo argomento fornisce soluzioni per i seguenti problemi:  
  

-   Problema 1: [emettere 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emettere 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emettere 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emettere 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emettere 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emettere 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emettere 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emettere 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emettere 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emettere 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emettere 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problema 1: [emettere 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problema 2: [emettere 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problema 3: [emettere 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problema 4: [emettere 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problema 5: [emettere 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problema 6: [emettere 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problema 7: [emettere 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problema 8: [emettere 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problema 9: [emettere 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problema 10: [emettere 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problema 11: [emettere 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>Problema 1  
 **Problema**  
  
 Viene visualizzato un pacchetto di installazione non riuscita. Provare a installare di nuovo il connettore Windows Server Essentials. Se il problema persiste, contattare l'errore di amministratore di rete quando ci si connette un computer al server  
  
 **Descrizione**  
  
 Questo problema può verificarsi se ci si connette un computer a un server che esegue Windows Server Essentials mentre sono in sospeso altri aggiornamenti di Windows o installazioni di applicazioni e l'installazione del connettore viene annullata.  
  
 **Soluzione**  
  
 Completare tutti gli altri aggiornamenti o installazioni di applicazioni. Se richiesto, riavviare il computer.  
  
##  <a name="BKMK_ConnectorIssue2"></a>Problema 2  
 **Problema**  
  
 Impossibile aggiungere un computer a Windows Server Essentials  
  
 **Descrizione**  
  
 Computer che dispongono di caratteri non ASCII nel nome del computer non possono essere aggiunti a Windows Server Essentials. Se il nome del computer include caratteri non ASCII, viene visualizzato il messaggio di errore "si è verificato un errore imprevisto."  
  
 **Soluzione**  
  
 Rinominare il computer client con un nome che contiene solo caratteri ASCII e provare ad aggiungere nuovamente il computer a Windows Server Essentials.  
  
##  <a name="BKMK_ConnectorIssue2a"></a>Problema 3  
 **Problema**  
  
 Viene visualizzato un connettore di installazione del software errore annullati quando si connette un computer al server  
  
 **Descrizione**  
  
 Per essere in grado di connettere un computer al server, l'account di sistema deve disporre delle autorizzazioni controllo completo su cartelle del server visualizzate nel Dashboard di Windows Server Essentials. Se non sono state concesse le autorizzazioni necessarie, viene visualizzato l'errore "l'installazione del software connettore viene annullata" messaggio.  
  
 **Soluzione**  
  
 Concedere all'account SYSTEM **controllo completo** le autorizzazioni per ogni cartella del server.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Per concedere all'account SYSTEM autorizzazioni controllo completo per una cartella del server  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Fare clic sulla cartella del server e quindi fare clic su **aprire la cartella**. (Se non visualizzi il **aprire la cartella** opzione, non è necessario impostare le autorizzazioni sulla cartella.)  
  
4.  Nel percorso di rete nella parte superiore di Esplora risorse, fare clic sulla condivisione server per visualizzare le cartelle condivise sul server. Ad esempio, se il percorso è **rete > Server01 > backup cronologia File**, fare clic su **Server01**.  
  
5.  Fare doppio clic su una cartella del server, quindi fare clic su **proprietà**.  
  
6.  Fare clic su di **sicurezza** scheda.  
  
7.  Se **controllo completo** non è consentito per l'account di sistema, fare clic su **modifica**, quindi fare clic su **sistema**. In **le autorizzazioni per il sistema**, selezionare il **Consenti** casella di controllo accanto a **controllo completo**.  
  
8.  Fare clic su **OK** due volte per aggiornare le autorizzazioni e chiudere **proprietà**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>Problema 4  
 **Problema**  
  
 Ottenere un a eseguire l'applicazione, è necessario installare una delle seguenti versioni di .NET Framework: v 4.5.50709 "quando si connette un computer al server  
  
 **Descrizione**  
  
 Quando si connette un computer a un server che esegue Windows Server Essentials o Windows Server Essentials, la procedura guidata tenta di installare .NET Framework versione 4.5.50709 nel computer. Tuttavia, se è presenta una versione precedente di .NET Framework 4.5, la versione aggiornata non può essere installata e il tentativo di connessione ha esito negativo con questo messaggio di errore: per eseguire l'applicazione, è necessario installare una delle seguenti versioni di .NET Framework: v 4.5.50709. Per istruzioni su come ottenere la versione appropriata di .NET Framework, contattare l'autore dell'applicazione.  
  
 **Soluzione**  
  
 Disinstallare .NET Framework 4.5 dal computer e quindi connettere il computer al server.  
  
#### <a name="to-uninstall-net-framework-45"></a>Per disinstallare .NET Framework 4.5  
  
1.  Nel **Start** pagina del computer client, aprire **Pannello di controllo**.  
  
2.  Nel Pannello di controllo in **programmi**, fare clic su **Disinstalla un programma**.  
  
3.  Fare doppio clic su **Microsoft .NET Framework 4.5**, quindi fare clic su **Disinstalla**.  
  
4.  Dopo aver disinstallato .NET Framework 4.5, connettere il computer al server. La versione corretta di .NET Framework 4.5 viene installata insieme al software connettore.  
  
##  <a name="BKMK_Time"></a>Problema 5  
 **Problema**  
  
 Ottenere un server non è disponibile. Per risolvere questo problema, contatta il responsabile della rete. Quando si connette un computer al server  
  
 **Descrizione**  
  
 Questa situazione può verificarsi se la data e l'ora nel computer connesso non sono sincronizzati con la data e ora del server.  Windows Server Essentials e Windows Server Essentials è possibile utilizzare il servizio di sincronizzazione ora per sincronizzare la data e ora del computer che eseguono una rete di Windows Server Essentials o Windows Server Essentials. La sincronizzazione dell'ora è fondamentale poiché il protocollo di autenticazione predefinito utilizza l'ora di server come parte del processo di autenticazione. Ad esempio, se l'orologio in un computer client non è sincronizzato sui valori corretti di data e ora, Windows Server Essentials o l'autenticazione di Windows Server Essentials potrebbe interpreti per errore una richiesta di accesso come tentativo di intrusione, negando e negare l'accesso all'utente.  
  
 Questa situazione può verificarsi se il server s la memoria disponibile è inferiore al 5%.  
  
 Questa situazione può verificarsi se si dispone già di una connessione VPN per Windows Server Essentials e si tenta di configurare il software di connettore non locali usando un indirizzo di dominio.  
  
 **Soluzione**  
  
1.  Sincronizzare la data e ora del computer client con quelle nel server. Quindi connettere il computer al server.  
  
2.  Chiudere alcune applicazioni sul lato server e quindi connettere il computer al server.  
  
3.  Chiudere la connessione VPN e quindi connettere il computer al server.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Per modificare la data e l'ora nel computer client  
  
1.  Nella pagina iniziale del computer client, aprire **Pannello di controllo**.  
  
2.  Nel Pannello di controllo, fare clic su **orologio, lingua e area geografica**, quindi fare clic su **data e ora**.  
  
3.  Fare clic su **Modifica data e ora**, impostare la data e l'ora sui valori corretti di data e ora e quindi fare clic su **OK**.  
  
4.  Fare clic su **OK**, quindi chiudere il pannello di controllo.  
  
5.  Riprovare a connettere il computer client al server. Per istruzioni, vedere connettere computer al server.  
  
 Se è ancora possibile connettersi il client al server, assicurarsi che la data e ora del server siano corrette. Se la data e ora non sono corrette, modificarle.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Per modificare la data e ora del server  
  
1.  Accedere al server usando la password impostata durante la configurazione o installazione di Windows Server Essentials e Windows Server Essentials.  
  
    > [!NOTE]
    >  Se si gestisce il server in modalità remota, è necessario accedere al server mediante connessione Desktop remoto.  
  
2.  Nel **Start** pagina, aprire **Pannello di controllo**.  
  
3.  Nel Pannello di controllo, fare clic su **orologio, lingua e area geografica**, quindi fare clic su **data e ora**.  
  
4.  Fare clic su **Modifica data e ora**, impostare la data e l'ora sui valori corretti di data e ora e quindi fare clic su **OK**.  
  
5.  Fare clic su **OK**, quindi chiudere il pannello di controllo.  
  
6.  Nel computer client, provare nuovamente a connettere il computer client al server. Per istruzioni, vedere connettere computer al server.  
  
##  <a name="BKMK_ServiceStopped"></a>Problema 6  
 **Problema**  
  
 Ottenere un An si è verificato un errore imprevisto. Per risolvere questo problema, contatta il responsabile della rete. Quando si connette un computer al server  
  
 **Descrizione**  
  
 Se si riceve questo messaggio di errore, WSS Certificate Web Service potrebbe non essere in esecuzione.  
  
 **Soluzione**  
  
 Avviare WSS Certificate Web Service.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Per avviare WSS Certificate Web Service  
  
1.  Il server **Start** pagina, aprire **strumenti di amministrazione**.  
  
     In **strumenti di amministrazione**, fare doppio clic su **Gestione Internet Information Services (IIS)**, quindi fare clic su **aprire**.  
  
2.  Nel riquadro di spostamento, fare clic su **WSS Certificate Web Service**.  
  
3.  Nel **azioni** riquadro, fare clic su **Start**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>Problema 7  
 **Problema**  
  
 Quando si tenta di connettere un computer al server dopo un tentativo di connessione ha esito negativo, viene visualizzato l'avviso di che un computer con questo nome è già connesso al server  
  
 **Descrizione**  
  
 Se un tentativo precedente di connettere un computer al server è stato annullato o interrotto, potresti ricevere l'avviso seguente quando si tenta di connettersi di nuovo: un computer con questo nome è già connesso al server. Ciò accade perché è stato rilasciato un certificato durante il tentativo di connettersi al server la prima volta.  
  
 **Soluzione** se si è certi che nessun altro computer con lo stesso nome è già connesso al server, fare clic su **Avanti**e quindi segui le istruzioni per completare il **connettere il computer al server** procedura guidata.  
  
##  <a name="BKMK_JoinWin7"></a>Problema 8  
 **Problema**  
  
 Quando si tenta di connettere un computer client che esegue Windows 7 Home al server, viene visualizzata la pagina web per l'esecuzione del software connettore, ma il computer client non possa connettersi al server  
  
 **Descrizione**  
  
 Se il router nella rete è abilitato il multicast, le comunicazioni tra il server e un computer client che eseguono Windows 7 Home Basic o Windows 7 Home Premium non funzionano correttamente.  
  
 **Soluzione**  
  
 Disabilitare il multicast nel router. In alcuni router, che potrebbe essere necessario disabilitare il protocollo di routing RIP - 2M. Per ulteriori informazioni, consultare la documentazione fornita dal produttore del router.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>Problema 9  
 **Problema**  
  
 L'accesso automatico ha smesso di funzionare dopo aver connesso il computer al server  
  
 **Descrizione**  
  
 Se l'accesso automatico è impostato per l'account utente quando si connette il computer a Windows Server Essentials, l'impostazione viene sovrascritta quando il software connettore viene installato nel computer.  
  
 **Soluzione** per risolvere questo problema, quando si connette il computer al server, prendere nota della password viene utilizzata per l'account utente. Dopo aver installato il software connettore, configurare l'accesso automatico per l'utilizzo di tale account.  
  
> [!NOTE]
>  L'account di dominio Windows Server Essentials richiede una password che soddisfi i requisiti dei criteri password predefiniti.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>Problema 10  
 **Problema**  
  
 Disinstallazione di una versione provvisoria del software connettore non rimuove i log esistenti  
  
 **Descrizione**  
  
 Dopo l'aggiornamento da una versione non definitiva (Beta o RC) di Windows Server Essentials alla versione rilasciata, è necessario rimuovere il software connettore da ogni computer che era connesso al server e quindi connettere il computer per installare la versione rilasciata del software connettore.  
  
 Tuttavia, quando si rimuove il software connettore da un computer di rete, i file di registro esistenti nella cartella %ProgramData%\Microsoft\Windows Server\Logs\ in tale computer non vengono eliminati. Se non si elimina la cartella Logs, i file di log possono venire danneggiati quando si connette il computer alla versione rilasciata di Windows Server Essentials.  
  
 **Soluzione** per evitare il danneggiamento dei file di registro, eliminare manualmente la cartella Logs prima di riconnettere il computer client al server aggiornato.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Per riconnettere un computer dopo l'aggiornamento del server senza danneggiare il log file  
  
1.  Disinstallare il software connettore dal computer client.  
  
2.  Eliminare la cartella Logs dalla cartella %ProgramData%\Microsoft\Windows server.  
  
3.  Connettere nuovamente il computer al server. Che installa la versione rilasciata del software connettore, la creazione di una nuova cartella registri e file di registro.  
  
##  <a name="BKMK_UpgradeClientOS"></a>Problema 11  
 **Problema**  
  
 È necessario aggiornare il sistema operativo in un computer client  
  
 **Descrizione**  
  
 Durante l'installazione del software connettore, vengono eseguiti numerosi controlli sul sistema operativo client per garantire che il client soddisfi tutti i prerequisiti del connettore. Se si aggiorna il sistema operativo client dopo aver installato il software connettore, alcuni dei prerequisiti potrebbero non essere presenti e il connettore client potrebbe non riuscire.  
  
 **Soluzione**  
  
 Prima di aggiornare il sistema operativo client a una versione diversa (ad esempio, aggiornare Windows XP a Windows Vista o Windows Vista a Windows 7), è necessario disinstallare il software connettore. Utilizzare **Aggiungi o Rimuovi programmi** nel Pannello di controllo. Una volta completata l'aggiornamento del sistema operativo client, è possibile reinstallare il connettore client aprendo http://<*server*> / connect in un Web browser, dove <*server*> è il nome del server di Windows Server Essentials.  
  
 Se il client già stato aggiornato con il software connettore installato, utilizzare **Aggiungi/Rimuovi programmi** o **programmi e funzionalità** per disinstallare il software connettore. Quindi installare nuovamente il software connettore.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows 2012 Server Essentials a ConnectComputer in risoluzione dei problemi (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
