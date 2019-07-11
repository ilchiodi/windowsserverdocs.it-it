---
title: Servizio di migrazione archiviazione problemi noti
description: Problemi noti e risoluzione dei problemi il supporto per il servizio di migrazione di archiviazione, ad esempio come raccogliere i log per il supporto tecnico Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 08156a09491d66016b5fcfe6056ed318d682b987
ms.sourcegitcommit: 514d659c3bcbdd60d1e66d3964ede87b85d79ca9
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/10/2019
ms.locfileid: "67735164"
---
# <a name="storage-migration-service-known-issues"></a>Servizio di migrazione archiviazione problemi noti

In questo argomento è fornite le risposte ai problemi noti quando si usa [servizio di migrazione archiviazione](overview.md) la migrazione di server.

## <a name="collecting-logs"></a> Come raccogliere i file di log quando si lavora con il supporto tecnico Microsoft

Il servizio di migrazione di archiviazione contiene i registri eventi per il servizio Orchestrator e il servizio Proxy. Il server urchestrator include sempre sia i registri eventi e i server di destinazione con installato il servizio proxy contengono i log di proxy. Questi log si trovano in:

- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService
- Registri applicazioni e servizi \ Microsoft \ Windows \ Proxy StorageMigrationService

Se è necessario raccogliere questi log per la visualizzazione non in linea o per l'invio al supporto Microsoft, è presente un open source script di PowerShell disponibile in GitHub:

 [Helper di servizio di migrazione archiviazione](https://aka.ms/smslogs) 

Esaminare il file Leggimi per informazioni sull'utilizzo.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Il servizio di migrazione di archiviazione non viene visualizzata in Windows Admin Center, a meno che la gestione di Windows Server 2019

Quando si usa la versione 1809 di Windows Admin Center per gestire un agente di orchestrazione di Windows Server 2019, opzione non è presente la strumento per il servizio di migrazione di archiviazione. 

L'estensione del servizio migrazione di archiviazione di Windows Admin Center è associato a versione per gestire solo versione di Windows Server 2019 1809 o sistemi operativi successivi. Se si usarlo per gestire i sistemi operativi Windows Server meno recenti o anteprime insider, lo strumento non verrà visualizzata. Questo comportamento dipende dalla progettazione. 

Per risolvere, usare o eseguire l'aggiornamento a Windows Server 2019 build 1809 o versione successiva.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Il servizio di migrazione di archiviazione non è possibile scegliere l'indirizzo IP statico in cutover

Quando si usa la versione 0,57 la migrazione del servizio di archiviazione estensione Windows Admin Center e si raggiunge la fase di migrazione completa, è possibile selezionare un indirizzo IP statico per un indirizzo. È necessario usare il protocollo DHCP.

Per risolvere questo problema, in Windows Admin Center, esaminare **le impostazioni** > **estensioni** per un avviso indicante che tale versione aggiornata del servizio di migrazione archiviazione 0.57.2 è disponibile per l'installazione. Si potrebbe essere necessario riavviare la scheda del browser per interfaccia di amministrazione di Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Archiviazione del servizio di migrazione cutover convalida ha esito negativo con errore "Accesso negato per i criteri di filtro di token nel computer di destinazione"

Quando si esegue la convalida cutover, viene visualizzato l'errore "errore: Accesso negato per i criteri di filtro di token nel computer di destinazione". Ciò si verifica anche se sono fornite le credenziali di amministratore locale corretto per i computer di origine e di destinazione.

Questo problema è causato da un difetto del codice in Windows Server 2019. Il problema si verifica quando si usa il computer di destinazione come agente di orchestrazione del servizio migrazione archiviazione.

Per risolvere questo problema, installare il servizio di migrazione di archiviazione in un computer Windows Server 2019 che non è la destinazione di migrazione desiderato, quindi connettersi al server con Windows Admin Center ed eseguire la migrazione.

Questo è stato risolto in una versione successiva di Windows Server. Aprire un caso di supporto tramite [supporto tecnico Microsoft](https://support.microsoft.com) per richiedere un esegue il backporting di questa soluzione venga creato.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Il servizio di migrazione di archiviazione non è incluso nell'edizione di valutazione di Windows Server 2019

Quando si usa Windows Admin Center per la connessione a un [versione di valutazione di Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) non c'è un'opzione per gestire il servizio di migrazione di archiviazione. Il servizio di migrazione di archiviazione non è incluso anche in ruoli e funzionalità.

Questo problema è causato da un problema di manutenzione nel supporto di valutazione di Windows Server 2019. 

Per risolvere questo problema, installare delle vendite al dettaglio, la versione MSDN, OEM o Volume License di Windows Server 2019 e non l'attivo. Senza attivazione, tutte le edizioni di Windows Server operano in modalità di valutazione di 180 giorni. 

Questo problema è stato risolto in una versione successiva di Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Il servizio di migrazione archiviazione timeout scaricando l'errore di trasferimento CSV

Quando si usa Windows Admin Center o PowerShell per scaricare il trasferimento operazioni solo errori CSV log dettagliato, viene visualizzato l'errore:

 >   Registro trasferimento: verificare la condivisione file è consentita nel firewall. : L'operazione di richiesta inviato a NET.TCP://localhost:8001/NetTcp: 28940/sms/service/1/trasferimento non ha ricevuto una risposta entro il timeout configurato (00: 01:00). Il tempo allocato a questa operazione fosse una parte di un timeout più lungo. Può trattarsi di perché il servizio sta ancora elaborando l'operazione o il servizio è riuscito a inviare un messaggio di risposta. . Provare ad aumentare il timeout dell'operazione (eseguendo il cast del canale/proxy su IContextChannel e impostando la proprietà OperationTimeout) e verificare che il servizio sia in grado di connettersi al client.

Questo problema è causato da un numero estremamente elevato di file trasferiti non è possibile filtrare il timeout di un minuto predefinito consentita dal servizio di migrazione di archiviazione. 

Per risolvere questo problema:

1. Nel computer dell'agente di orchestrazione, modificare il *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* file utilizzando Notepad.exe modificare "sendTimeout" rispetto all'impostazione predefinita di 1 minuto a 10 minuti

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Riavviare il servizio "Servizio di migrazione di archiviazione" sul computer dell'agente di orchestrazione. 
3. Nel computer dell'agente di orchestrazione, avviare Regedit.exe
4. Individuare e fare clic sulla seguente sottochiave del Registro di sistema: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. Dal menu Modifica, scegliere Nuovo e poi fare clic su Valore DWORD. 
6. Digitare "WcfOperationTimeoutInMinutes" per il nome del valore DWORD e quindi premere INVIO.
7. Fare doppio clic su "WcfOperationTimeoutInMinutes" e quindi fare clic su Modifica. 
8. Nella finestra di dati di Base, fare clic su "Decimal"
9. Nella casella dati valore, digitare "10" e quindi fare clic su OK.
10. Uscire dall'editor del Registro di sistema.
11. Tentativo di scaricare nuovamente il file CSV solo gli errori. 

Si prevede di modificare questo comportamento nelle versioni successive di Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Cutover si verifica un errore durante la migrazione tra le reti

Durante la migrazione a un computer di destinazione in esecuzione in una rete diversa rispetto all'origine, ad esempio un'istanza IaaS di Azure, migrazione completa non viene completata quando l'origine usava un indirizzo IP statico. 

Questo comportamento è per impostazione predefinita, per evitare problemi di connettività dopo la migrazione da utenti, applicazioni e gli script che si connettono tramite indirizzo IP. Quando l'indirizzo IP viene spostato dal computer di origine precedente alla nuova destinazione di destinazione, non corrisponderà le nuove informazioni di subnet di rete, ad esempio DNS e WINS.

Per risolvere questo problema, eseguire la migrazione in un computer nella stessa rete. Quindi passare tale computer a una nuova rete e riassegnare le relative informazioni IP. Ad esempio, se la migrazione ad Azure IaaS, prima di tutto la migrazione a una macchina virtuale locale, quindi usare Azure Migrate per spostare la macchina virtuale in Azure.  

Questo problema è stato risolto in una versione successiva di Windows Admin Center. A questo punto si sarà consentono di specificare le migrazioni che non modificano le impostazioni di rete del server di destinazione. L'estensione aggiornata verrà elencata qui quando rilasciato. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avvisi di convalida per i privilegi amministrativi di proxy e le credenziali destinazione

Quando la convalida di un processo di trasferimento, verranno visualizzati gli avvisi seguenti:

 > **La credenziale con privilegi amministrativi.**
 > Avviso: Azione non è disponibile in modalità remota.
 > **Il proxy di destinazione viene registrato.**
 > Avviso: Il proxy di destinazione non è stato trovato.

Se non è installato il servizio di archiviazione della migrazione del servizio Proxy nel computer di destinazione Windows Server 2019 o il computer di destinazione è Windows Server 2016 o Windows Server 2012 R2, questo comportamento è per impostazione predefinita. È consigliabile eseguire la migrazione a un computer Windows Server 2019 con il proxy di installazione per ottenere prestazioni significativamente migliorate trasferimento.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Alcuni file non di inventario o trasferimento, errore 5 "accesso negato"

Quando eseguire l'inventario o il trasferimento dei file di origine ai computer di destinazione, i file da cui un utente ha rimosso le autorizzazioni del gruppo Administrators non eseguire la migrazione. Analisi del debug Proxy del Servizio archiviazione migrazione illustra:

  Nome registro:      Origine Microsoft-Windows-StorageMigrationService-Proxy/Debug:        Microsoft-Windows-StorageMigrationService-Proxy Date:          2/26/2019:04 9:00 l'ID evento:      Categoria attività 10000: Nessuno di livello:         Parole chiave di errore:      
  Utente:          Computer servizio di rete: srv1.contoso.com descrizione:

  Errore di trasferimento 02/26/2019-09:00:04.860 [errore] per \\srv1.contoso.com\public\indy.png: (5) accesso negato.
Analisi dello stack: A Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile (stringa fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes) in Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (percorso della stringa) in Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile (file FileInfo) in Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo() in Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() in Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


Questo problema è causato da un difetto del codice nel servizio di migrazione di archiviazione in cui è non stato viene richiamato il privilegio di backup. 

Per risolvere questo problema, installare [Windows Update 2 aprile 2019: KB4490481 (17763.404 di Build del sistema operativo)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) nel computer dell'agente di orchestrazione e il computer di destinazione se è installato il servizio di proxy non esiste. Assicurarsi che l'account utente di origine della migrazione è un amministratore locale nel computer di origine e l'agente di orchestrazione del servizio di migrazione di archiviazione. Assicurarsi che l'account utente della migrazione di destinazione sia un amministratore locale nel computer di destinazione e l'agente di orchestrazione del servizio di migrazione di archiviazione. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Mancata corrispondenza hash di replica DFS quando si usa il servizio di migrazione di archiviazione per eseguito il preseeding dei dati

Quando si usa il servizio di migrazione di archiviazione per trasferire i file in una nuova destinazione, quindi configurare la replica DFS (DFSR) per replicare i dati con un server di replica DFS esistente tramite la replica presseded o del database DFSR la clonazione, tutti i file experiemce un hash mancata corrispondenza e vengono nuovamente replicate. I flussi di dati, flussi di sicurezza, le dimensioni e attributi sembrano corrispondere perfettamente dopo l'utilizzo di SMS per il trasferimento. Analisi dei file con ICACLS o il log di debug clonazione del Database DFSR rivela:

File di origine:

  Icacls d:\test\Source:

  d:\test\thatcher.png icacls/Salva txt /t thatcher.png D:AI(A;; FA;; BA) (A; 0 x1200a9;; 1!d D) (A; 0 x1301bf;; 1!d U) (A; ID; FA;; BA) (A; ID; FA;; SY) (A; ID; 0X1200A9;; BU)

File di destinazione:

  d:\test\thatcher.png icacls/Salva txt /t thatcher.png D:AI(A;; FA;; BA) (A; 0 x1301bf;; 1!d U) (A; 0 x1200a9;; 1!d D) (A; ID; FA;; BA) (A; ID; FA;; SY) (A; ID; 0X1200A9;; BU)**S:PAINO_ACCESS_CONTROL**

Log di Debug per replica DFS:

  20190308 10:18:53.116 3948 4045 DBCL [avviso] DBClone::IDTableImportUpdate mancata corrispondenza record è stato trovato. 

  Hash: 1BCDFE03 ACL locale-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 32: attributi 

  Clonare l'hash ACL:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0 32: attributi 

Questo problema è causato da un difetto del codice in una libreria usata dal servizio di migrazione di archiviazione per impostare il controllo di sicurezza ACL (SACL). Un SACL non null involontariamente viene impostato quando l'elenco SACL è vuoto, replica DFS per identificare correttamente una mancata corrispondenza hash iniziali. 

Per risolvere questo problema, continuare a usare Robocopy per [DFSR pre-seeding e operazioni di clonazione del Database DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) anziché il servizio di migrazione di archiviazione. Si sta esaminando questo problema e si prevede di risolvere il problema in una versione successiva di Windows Server ed eventualmente un eseguito il backport Windows Update. 

## <a name="error-404-when-downloading-csv-logs"></a>Log degli errori 404 durante il download di CSV

Quando si tenta di scaricare i log di trasferimento o di errore alla fine di un'operazione di trasferimento, viene visualizzato l'errore:

  $jobname: Trasferimento di log: errore di ajax 404

Questo errore è previsto se non è stata abilitata la regola del firewall "Condivisione File e stampanti (SMB-In)" nel server di orchestrator. Download di file Windows Admin Center richiedono la porta TCP/445 (SMB) nei computer connessi.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>Errore "non è stato possibile trasferire archiviazione in uno qualsiasi degli endpoint" quando il trasferimento da Windows Server 2008 R2

Quando si tenta di trasferire i dati da un computer di origine Windows Server 2008 R2, nessun trasferimento di dati e viene visualizzato errore:  

  Non è stato possibile trasferire l'archiviazione in uno qualsiasi degli endpoint.
0x9044

Questo errore è previsto se il computer Windows Server 2008 R2 non è completamente aggiornato con tutte le critici e importanti aggiornamenti da Windows Update. Indipendentemente dal servizio di migrazione di archiviazione, è sempre consigliabile l'applicazione di patch un computer Windows Server 2008 R2 per motivi di sicurezza, come sistema operativo non contiene i miglioramenti della protezione di versioni più recenti di Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Errore "non è stato possibile trasferire archiviazione in uno qualsiasi degli endpoint" e "Controlla se il dispositivo di origine è online - è stato possibile accedervi."

Quando si prova a trasferire i dati da un computer di origine, alcune o tutte le condivisioni non è possibile trasferire, con riepilogo degli errori:

   Non è stato possibile trasferire l'archiviazione in uno qualsiasi degli endpoint.
0x9044

Esaminare i dettagli di trasferimento SMB Mostra l'errore:

   Controllare che se il dispositivo di origine è online - è stato possibile accedervi.

Esaminare il registro eventi StorageMigrationService/Admin Mostra:

   Non è stato possibile trasferire l'archiviazione.

   Processo: ID Job1:  
   Stato: Errore non riuscito: Messaggio di errore 36931: 

   Materiale sussidiario: Controllare l'errore dettagliato e assicurarsi che siano soddisfatti i requisiti di trasferimento. Il processo di trasferimento non è stato possibile trasferire tutti i computer di origine e destinazione. È possibile che il computer di orchestrator non è stato possibile raggiungere qualsiasi computer di origine o destinazione, probabilmente a causa di una regola del firewall, o autorizzazioni mancanti.

Esaminare l'illustra log StorageMigrationService-Proxy/Debug:

   Convalida di trasferimento 07/02/2019-13:35:57.231 [errore] non riuscita. Codice di errore: 40961, endpoint di origine non è raggiungibile o non esiste, le credenziali dell'origine non sono validi o utente autenticato non ha autorizzazioni sufficienti per accedervi.
in Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate() a Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest (FileTransferRequest fileTransferRequest, Guid operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Questo errore è previsto se l'account di migrazione non ha almeno le autorizzazioni di accesso in lettura alle condivisioni SMB. Per risolvere questo errore, aggiungere un gruppo di sicurezza che contiene l'account di origine della migrazione per le condivisioni SMB nel computer di origine e concedere a tale lettura, modifica o controllo completo. Al termine della migrazione, è possibile rimuovere questo gruppo. Una versione futura di Windows Server può modificare questo comportamento e non richiedono più autorizzazioni esplicite per le condivisioni di origine.

## <a name="see-also"></a>Vedere anche

- [Panoramica del servizio di migrazione archiviazione](overview.md)
