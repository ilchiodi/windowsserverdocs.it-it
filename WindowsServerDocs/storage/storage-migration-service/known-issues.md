---
title: Problemi noti del servizio migrazione archiviazione
description: Problemi noti e supporto per la risoluzione dei problemi per il servizio migrazione archiviazione, ad esempio come raccogliere i log per supporto tecnico Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 10/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 830a2d99443938c25625211f590984819a20d566
ms.sourcegitcommit: 40e4ba214954d198936341c4d6ce1916dc891169
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690447"
---
# <a name="storage-migration-service-known-issues"></a>Problemi noti del servizio migrazione archiviazione

Questo argomento contiene le risposte ai problemi noti quando si usa il [servizio migrazione archiviazione](overview.md) per eseguire la migrazione dei server.

Il servizio migrazione archiviazione è disponibile in due parti: il servizio in Windows Server e l'interfaccia utente nell'interfaccia di amministrazione di Windows. Il servizio è disponibile in Windows Server, canale di manutenzione a lungo termine, oltre a Windows Server, canale semestrale; mentre l'interfaccia di amministrazione di Windows è disponibile come download separato. Sono inoltre incluse periodicamente le modifiche apportate agli aggiornamenti cumulativi per Windows Server, rilasciate tramite Windows Update. 

Ad esempio, Windows Server, versione 1903 include nuove funzionalità e correzioni per il servizio migrazione archiviazione, disponibili anche per Windows Server 2019 e Windows Server, versione 1809 con l'installazione di [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Come raccogliere i file di log quando si lavora con supporto tecnico Microsoft

Il servizio migrazione archiviazione contiene i registri eventi per il servizio agente di orchestrazione e il servizio proxy. Il server urchestrator contiene sempre i registri eventi e i server di destinazione con il servizio proxy installato contengono i log del proxy. Questi log si trovano in:

- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService
- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService-proxy

Se è necessario raccogliere questi log per la visualizzazione offline o per inviarli a supporto tecnico Microsoft, è disponibile uno script di PowerShell open source su GitHub:

 [Helper servizio migrazione archiviazione](https://aka.ms/smslogs) 

Esaminare il file Leggimi per l'utilizzo.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Il servizio migrazione archiviazione non viene visualizzato nell'interfaccia di amministrazione di Windows a meno che non si gestisca Windows Server 2019

Quando si usa la versione 1809 dell'interfaccia di amministrazione di Windows per gestire un agente di orchestrazione di Windows Server 2019, non viene visualizzata l'opzione dello strumento per il servizio migrazione archiviazione. 

L'estensione del servizio migrazione archiviazione dell'interfaccia di amministrazione di Windows è associata alla versione per gestire solo i sistemi operativi Windows Server 2019 versione 1809 o successiva. Se viene usato per gestire i sistemi operativi Windows Server meno recenti o le anteprime Insider, lo strumento non verrà visualizzato. Questo comportamento è da progettazione. 

Per risolvere, usare o eseguire l'aggiornamento a Windows Server 2019 Build 1809 o versione successiva.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Il servizio migrazione archiviazione non consente di scegliere un indirizzo IP statico in cutover

Quando si usa la versione 0,57 dell'estensione servizio migrazione archiviazione nell'interfaccia di amministrazione di Windows e si raggiunge la fase cutover, non è possibile selezionare un indirizzo IP statico per un indirizzo. Si è costretti a usare DHCP.

Per risolvere questo problema, nell'interfaccia di amministrazione di Windows, cercare in **impostazioni**  > **estensioni** per un avviso che informa che è disponibile per l'installazione il servizio di migrazione archiviazione versione aggiornata 0.57.2. Potrebbe essere necessario riavviare la scheda del browser per l'interfaccia di amministrazione di Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>La convalida cutover del servizio migrazione archiviazione non riesce con l'errore "accesso negato per i criteri di filtro del token nel computer di destinazione"

Quando si esegue la convalida cutover, viene visualizzato l'errore "errore: accesso negato per i criteri di filtro del token nel computer di destinazione". Ciò si verifica anche se sono state fornite le credenziali di amministratore locale corrette per i computer di origine e di destinazione.

Questo problema è causato da un errore del codice in Windows Server 2019. Il problema si verificherà quando si usa il computer di destinazione come agente di orchestrazione del servizio di migrazione archiviazione.

Per risolvere questo problema, installare il servizio migrazione archiviazione in un computer Windows Server 2019 che non è la destinazione di migrazione desiderata, quindi connettersi al server con l'interfaccia di amministrazione di Windows ed eseguire la migrazione.

Questo problema è stato risolto in una versione successiva di Windows Server. Aprire un caso di supporto tramite [supporto tecnico Microsoft](https://support.microsoft.com) per richiedere la creazione di un backporting di questa correzione.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>Il servizio migrazione archiviazione non è incluso nella versione di valutazione di Windows Server 2019 o Windows Server 2019 Essentials

Quando si usa l'interfaccia di amministrazione di Windows per connettersi a una [versione di valutazione di Windows server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o windows server 2019 Essentials, non è possibile gestire il servizio migrazione archiviazione. Anche il servizio migrazione archiviazione non è incluso in ruoli e funzionalità.

Questo problema è causato da un problema di manutenzione nel supporto di valutazione di Windows Server 2019 e Windows Server 2019 Essentials. 

Per risolvere questo problema per la valutazione, installare una versione finale, MSDN, OEM o con contratto multilicenza di Windows Server 2019 e non attivarla. Senza attivazione, tutte le edizioni di Windows Server operano in modalità di valutazione per 180 giorni. 

Questo problema è stato risolto in una versione successiva di Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Timeout del servizio di migrazione archiviazione durante il download del volume CSV errore di trasferimento

Quando si usa l'interfaccia di amministrazione di Windows o PowerShell per scaricare il log degli errori dettagliati per le operazioni di trasferimento, viene visualizzato un errore:

 >   Log di trasferimento: verificare che la condivisione file sia consentita nel firewall. : Questa operazione di richiesta inviata a NET. TCP://localhost: 28940/SMS/Service/1/Transfer non ha ricevuto una risposta entro il timeout configurato (00:01:00). Il tempo allocato a questa operazione potrebbe essere una parte di un timeout più lungo. È possibile che il servizio stia ancora elaborando l'operazione o che il servizio non sia stato in grado di inviare un messaggio di risposta. Prendere in considerazione l'aumento del timeout dell'operazione (eseguendo il cast del canale/proxy a IContextChannel e impostando la proprietà OperationTimeout) e verificare che il servizio sia in grado di connettersi al client.

Questo problema è causato da un numero molto elevato di file trasferiti che non possono essere filtrati nel timeout predefinito di un minuto consentito dal servizio migrazione archiviazione. 

Per ovviare a questo problema:

1. Sul computer dell'agente di orchestrazione, modificare il file *%systemroot%\SMS\Microsoft.StorageMigration.Service.exe.config* utilizzando Notepad. exe per modificare "SendTimeout nell'elemento" dal valore predefinito di 1 minuto a 10 minuti.

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Riavviare il servizio "servizio migrazione archiviazione" sul computer dell'agente di orchestrazione. 
3. Sul computer dell'agente di orchestrazione, avviare Regedit. exe
4. Individuare e fare clic sulla seguente sottochiave del Registro di sistema: 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. Dal menu Modifica, scegliere Nuovo e poi fare clic su Valore DWORD. 
6. Digitare "WcfOperationTimeoutInMinutes" per il nome DWORD, quindi premere INVIO.
7. Fare clic con il pulsante destro del mouse su "WcfOperationTimeoutInMinutes", quindi scegliere modifica. 
8. Nella casella dati di base fare clic su "decimale"
9. Nella casella dati valore digitare "10", quindi fare clic su OK.
10. Uscire dall'editor del Registro di sistema.
11. Provare a scaricare di nuovo il file CSV solo degli errori. 

Si prevede di modificare questo comportamento in una versione successiva di Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Cutover non riesce quando si esegue la migrazione tra le reti

Quando si esegue la migrazione a un computer di destinazione in esecuzione in una rete diversa da quella dell'origine, ad esempio un'istanza di Azure IaaS, non è possibile completare cutover quando l'origine usa un indirizzo IP statico. 

Questo comportamento è progettato per evitare problemi di connettività dopo la migrazione da utenti, applicazioni e script che si connettono tramite indirizzo IP. Quando l'indirizzo IP viene spostato dal computer di origine precedente alla nuova destinazione di destinazione, non corrisponderà alle nuove informazioni sulla subnet di rete e forse DNS e WINS.

Per aggirare questo problema, eseguire una migrazione a un computer nella stessa rete. Quindi spostare il computer in una nuova rete e riassegnare le informazioni IP. Ad esempio, se si esegue la migrazione ad Azure IaaS, eseguire prima la migrazione a una macchina virtuale locale, quindi usare Azure Migrate per spostare la macchina virtuale in Azure.  

Questo problema è stato risolto in una versione successiva dell'interfaccia di amministrazione di Windows. È ora possibile specificare le migrazioni che non modificano le impostazioni di rete del server di destinazione. L'estensione aggiornata verrà elencata qui quando viene rilasciata. 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avvisi di convalida per i privilegi amministrativi del proxy di destinazione e delle credenziali

Quando si convalida un processo di trasferimento, vengono visualizzati gli avvisi seguenti:

 > **La credenziale dispone di privilegi amministrativi.**
 > Avviso: l'azione non è disponibile in modalità remota.
 > **Il proxy di destinazione è registrato.**
 > Avviso: il proxy di destinazione non è stato trovato.

Se il servizio proxy del servizio migrazione archiviazione non è stato installato nel computer di destinazione Windows Server 2019 o se il computer di destinazione è Windows Server 2016 o Windows Server 2012 R2, questo comportamento è da progettazione. Si consiglia di eseguire la migrazione a un computer Windows Server 2019 con il proxy installato per migliorare significativamente le prestazioni di trasferimento.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Alcuni file non vengono inventariati o trasferiti. errore 5 "accesso negato"

Durante l'inventario o il trasferimento di file dai computer di origine a quello di destinazione, la migrazione dei file da cui un utente ha rimosso le autorizzazioni del gruppo Administrators non riesce. Esaminare il servizio migrazione archiviazione-il debug del proxy Mostra:

  Nome registro: Microsoft-Windows-StorageMigrationService-proxy/debug Source: Microsoft-Windows-StorageMigrationService-proxy date: 2/26/2019 9:00:04 AM ID evento: 10000 Categoria attività: None Level: Error keywords:      
  Utente: computer servizio di rete: srv1.contoso.com Descrizione:

  02/26/2019-09:00:04.860 [Error] errore di trasferimento per \\srv1. contoso. com\public\indy.png: (5) accesso negato.
Analisi dello stack: in Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. OpenFile (String fileName, DesiredAccess desiredAccess, ShareMode SHAREMODE, CreationDisposition CreationDisposition, FlagsAndAttributes flagsAndAttributes) at Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. GetTargetFile (percorso stringa) in Microsoft. StorageMigration. proxy. Service. Transfer. FileDirUtils. GetTargetFile (file FileInfo) in Microsoft. StorageMigration. proxy. Service. Transfer. filetransfer. InitializeSourceFileInfo () in Microsoft. StorageMigration. proxy. Service. Transfer. filetransfer. Transfer () at Microsoft. StorageMigration. proxy. Service. Transfer. filetransfer. TryTransfer () [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs:: TryTransfer:: 55]


Questo problema è causato da un errore del codice nel servizio migrazione archiviazione in cui non è stato richiamato il privilegio di backup. 

Per risolvere questo problema, installare [Windows Update 2 aprile 2019, ovvero KB4490481 (build del sistema operativo 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) nel computer dell'agente di orchestrazione e nel computer di destinazione se il servizio proxy è installato. Verificare che l'account utente della migrazione di origine sia un amministratore locale nel computer di origine e l'agente di orchestrazione del servizio di migrazione archiviazione. Verificare che l'account utente della migrazione di destinazione sia un amministratore locale nel computer di destinazione e l'agente di orchestrazione del servizio migrazione archiviazione. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Mancata corrispondenza degli hash DFSR quando si usa il servizio migrazione archiviazione per eseguire il Preseeding dei dati

Quando si usa il servizio migrazione archiviazione per trasferire i file in una nuova destinazione, quindi si configura il Replica DFS (DFSR) per replicare i dati con un server DFSR esistente tramite la replica con seeding o la clonazione del database DFSR, tutti i file experiemce un hash mancata corrispondenza e replica eseguita di nuovo. I flussi di dati, i flussi di sicurezza, le dimensioni e gli attributi sembrano essere perfettamente corrispondenti dopo l'uso di SMS per trasferirli. L'analisi dei file con ICACLS o il log di debug per la clonazione del database DFSR rivela:

File di origine:

  d:\test\Source icacls:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; Bu

File di destinazione:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S:PAINO_ACCESS_CONTROL**

Log di debug DFSR:

  20190308 10:18:53.116 3948 DBCL 4045 [WARN] DBClone:: IDTableImportUpdate record non corrispondente trovato. 

  Hash ACL locale: 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Attributi: 32 

  Clona hash ACL:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime: 20190308 18:09:44.876 FileSizeLow: 1131654 FileSizeHigh: 0 Attributi: 32 

Questo problema è causato da un errore del codice in una libreria usata dal servizio migrazione archiviazione per impostare gli ACL del controllo di sicurezza (SACL). Un SACL non null viene impostato involontariamente quando l'elenco SACL è vuoto, DFSR iniziali per identificare correttamente una mancata corrispondenza dell'hash. 

Per aggirare questo problema, continuare a usare Robocopy per il [pre-seeding di DFSR e le operazioni di clonazione del database DFSR](../dfs-replication/preseed-dfsr-with-robocopy.md) anziché il servizio migrazione archiviazione. Questo problema è stato analizzato e si intende risolverlo in una versione successiva di Windows Server ed eventualmente in un Windows Update con backporting. 

## <a name="error-404-when-downloading-csv-logs"></a>Errore 404 durante il download dei log CSV

Quando si tenta di scaricare i log degli errori o di trasferimento al termine di un'operazione di trasferimento, viene visualizzato un errore:

  $jobname: log di trasferimento: errore Ajax 404

Questo errore è previsto se non è stata abilitata la regola del firewall "condivisione di file e stampanti (SMB-in)" sul server dell'agente di orchestrazione. Il download di file dell'interfaccia di amministrazione di Windows richiede la porta TCP/445 (SMB) nei computer connessi.  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Errore "Impossibile trasferire lo spazio di archiviazione su uno degli endpoint" durante il trasferimento da Windows Server 2008 R2

Quando si tenta di trasferire dati da un computer di origine Windows Server 2008 R2, non vengono trasferiti dati e viene visualizzato un errore:  

  Non è stato possibile trasferire lo spazio di archiviazione su uno degli endpoint.
0x9044

Questo errore è previsto se il computer Windows Server 2008 R2 non è completamente aggiornato con tutti gli aggiornamenti critici e importanti da Windows Update. Indipendentemente dal servizio migrazione archiviazione, è sempre consigliabile applicare patch a un computer Windows Server 2008 R2 per motivi di sicurezza, in quanto il sistema operativo non contiene i miglioramenti della sicurezza delle versioni più recenti di Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Errore "Impossibile trasferire lo spazio di archiviazione su uno degli endpoint" e "verificare se il dispositivo di origine è online-non è stato possibile accedervi".

Quando si tenta di trasferire dati da un computer di origine, alcune o tutte le condivisioni non vengono trasferite, con errore di riepilogo:

   Non è stato possibile trasferire lo spazio di archiviazione su uno degli endpoint.
0x9044

Esaminando i dettagli del trasferimento SMB viene visualizzato l'errore:

   Controllare se il dispositivo di origine è online. non è stato possibile accedervi.

Esaminando il registro eventi di StorageMigrationService/admin viene visualizzato quanto segue:

   Non è stato possibile trasferire l'archiviazione.

   Processo: ID Job1:  
   Stato: errore non riuscito: 36931 messaggio di errore: 

   Indicazioni: controllare l'errore dettagliato e verificare che siano soddisfatti i requisiti di trasferimento. Il processo di trasferimento non è riuscito a trasferire i computer di origine e di destinazione. Il problema potrebbe essere dovuto al fatto che il computer dell'agente di orchestrazione non è riuscito a raggiungere i computer di origine o di destinazione, probabilmente a causa di una regola del firewall o

L'analisi del log StorageMigrationService-proxy/debug Mostra:

   07/02/2019-13:35:57.231 [errore] la convalida del trasferimento non è riuscita. ErrorCode: 40961, endpoint di origine non è raggiungibile o non esiste oppure le credenziali di origine non sono valide o l'utente autenticato non dispone di autorizzazioni sufficienti per accedervi.
in Microsoft. StorageMigration. proxy. Service. Transfer. TransferOperation. Validate () in Microsoft. StorageMigration. proxy. Service. Transfer. TransferRequestHandler. ProcessRequest (FileTransferRequest fileTransferRequest, Guid operationId)    [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

Questo errore è previsto se l'account di migrazione non dispone almeno delle autorizzazioni di accesso di lettura per le condivisioni SMB. Per risolvere questo errore, aggiungere un gruppo di sicurezza contenente l'account di migrazione di origine alle condivisioni SMB nel computer di origine e concedergli la lettura, la modifica o il controllo completo. Al termine della migrazione, è possibile rimuovere questo gruppo.

## <a name="error-0x80005000-when-running-inventory"></a>Errore 0x80005000 durante l'esecuzione dell'inventario

Dopo l'installazione di [KB4512534](https://support.microsoft.com/en-us/help/4512534/windows-10-update-kb4512534) e il tentativo di eseguire l'inventario, l'inventario ha esito negativo con errori:

  ECCEZIONE da HRESULT: 0x80005000
  
  Nome registro: Microsoft-Windows-StorageMigrationService/admin Source: Microsoft-Windows-StorageMigrationService date: 9/9/2019 5:21:42 PM ID evento: 2503 Categoria attività: None Level: Error keywords:      
  Utente: computer servizio di rete: FS02. Descrizione di TailwindTraders.net: non è stato possibile inventariare i computer.
Processo: ID foo2:20ac3f75-4945-41d1-9A79-d11dbb57798b stato: errore non riuscito: 36934 messaggio di errore: inventario non riuscito per tutte le indicazioni sui dispositivi: controllare l'errore dettagliato e verificare che siano soddisfatti i requisiti di inventario. Il processo non è riuscito a inventariare uno dei computer di origine specificati. Questo potrebbe essere dovuto al fatto che il computer dell'agente di orchestrazione non è riuscito a raggiungere la rete, probabilmente a causa di una regola del firewall o di autorizzazioni mancanti.
  
  Nome registro: Microsoft-Windows-StorageMigrationService/admin Source: Microsoft-Windows-StorageMigrationService date: 9/9/2019 5:21:42 PM ID evento: 2509 Categoria attività: None Level: Error keywords:      
  Utente: computer servizio di rete: FS02. Descrizione di TailwindTraders.net: non è stato possibile inventariare un computer.
Processo: foo2 computer: FS01. Stato TailwindTraders.net: errore non riuscito: messaggio di errore-2147463168: informazioni aggiuntive: controllare l'errore dettagliato e verificare che i requisiti di inventario siano soddisfatti. L'inventario non è stato in grado di determinare alcun aspetto del computer di origine specificato. Il problema potrebbe essere dovuto a autorizzazioni o privilegi mancanti nell'origine o in una porta del firewall bloccata.
  
Questo errore è causato da un difetto del codice nel servizio migrazione archiviazione quando si forniscono le credenziali di migrazione sotto forma di nome dell'entità utente (UPN), ad esempio "meghan@contoso.com". Il servizio dell'agente di orchestrazione del servizio migrazione archiviazione non è in grado di analizzare correttamente questo formato, causando un errore in una ricerca del dominio aggiunta per il supporto della migrazione del cluster in KB4512534 e 19H1.

Per aggirare questo problema, fornire le credenziali nel formato dominio\utente, ad esempio ' Contoso\Meghan '.

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Errore "ServiceError0x9006" o "il proxy non è attualmente disponibile". Quando si esegue la migrazione a un cluster di failover di Windows Server

Quando si tenta di trasferire i dati in un file server in cluster, vengono visualizzati errori quali: 

   Verificare che il servizio proxy sia installato e in esecuzione, quindi riprovare. Il proxy non è attualmente disponibile.
0x9006 ServiceError0x9006, Microsoft. StorageMigration. Commands. UnregisterSmsProxyCommand

Questo errore è previsto se la risorsa file server è stata spostata dal nodo proprietario del cluster Windows Server 2019 originale a un nuovo nodo e la funzionalità proxy del servizio migrazione archiviazione non è stata installata in tale nodo.

Come soluzione alternativa, spostare di nuovo la risorsa del file server di destinazione nel nodo del cluster proprietario originale che era in uso al momento della prima configurazione delle associazioni di trasferimento.

Come soluzione alternativa:

1. Installare la funzionalità del proxy del servizio migrazione archiviazione in tutti i nodi di un cluster.
2. Eseguire il seguente comando di PowerShell per il servizio migrazione archiviazione sul computer dell'agente di orchestrazione: 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Errore "Impossibile trovare la dll" durante l'esecuzione dell'inventario da un nodo del cluster

Quando si tenta di eseguire l'inventario con l'agente di orchestrazione del servizio di migrazione archiviazione installato in un nodo del cluster di failover di Windows Server 2019 e si utilizza come destinazione un cluster di failover di Windows Server file server origine, viene visualizzato l'errore seguente:

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

Per aggirare questo problema, installare "strumenti di gestione cluster di failover" (strumenti di gestione del cluster di failover) nel server che esegue l'agente di orchestrazione del servizio di migrazione archiviazione. 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>Errore "non sono disponibili altri endpoint dal mapper degli endpoint" durante l'esecuzione dell'inventario in un computer di origine Windows Server 2003

Quando si tenta di eseguire l'inventario con il server agente di orchestrazione del servizio di migrazione archiviazione con patch con l'aggiornamento cumulativo [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) o versione successiva, viene visualizzato l'errore seguente:

    There are no more endpoints available from the endpoint mapper  

Per aggirare questo problema, disinstallare temporaneamente l'aggiornamento cumulativo di KB4512534 (e qualsiasi altro che lo ha sostituito) dal computer agente di orchestrazione del servizio di migrazione archiviazione. Al termine della migrazione, reinstallare l'aggiornamento cumulativo più recente.  

Si noti che in alcune circostanze, la disinstallazione di KB4512534 o degli aggiornamenti sostitutivi può causare il mancato avvio del servizio migrazione archiviazione. Per risolvere questo problema, è possibile eseguire il backup ed eliminare il database del servizio migrazione archiviazione:

1.  Aprire un prompt dei comandi con privilegi elevati, in cui si è membri degli amministratori nel server dell'agente di orchestrazione del servizio di migrazione archiviazione ed eseguire:

     ```
     TAKEOWN /d /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  Avviare il servizio migrazione archiviazione che creerà un nuovo database.



## <a name="see-also"></a>Vedi anche

- [Panoramica di servizio migrazione archiviazione](overview.md)
