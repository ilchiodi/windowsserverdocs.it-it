---
title: Problemi noti del servizio migrazione archiviazione
description: Problemi noti e supporto per la risoluzione dei problemi per il servizio migrazione archiviazione, ad esempio come raccogliere i log per supporto tecnico Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/10/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 92742929e3826fca3cf87cb84341d3aecec0d55d
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517496"
---
# <a name="storage-migration-service-known-issues"></a>Problemi noti del servizio migrazione archiviazione

Questo argomento contiene le risposte ai problemi noti quando si usa il [servizio migrazione archiviazione](overview.md) per eseguire la migrazione dei server.

Il servizio migrazione archiviazione è disponibile in due parti: il servizio in Windows Server e l'interfaccia utente nell'interfaccia di amministrazione di Windows. Il servizio è disponibile in Windows Server, canale di manutenzione a lungo termine, oltre a Windows Server, canale semestrale; mentre l'interfaccia di amministrazione di Windows è disponibile come download separato. Sono inoltre incluse periodicamente le modifiche apportate agli aggiornamenti cumulativi per Windows Server, rilasciate tramite Windows Update. 

Ad esempio, Windows Server, versione 1903 include nuove funzionalità e correzioni per il servizio migrazione archiviazione, disponibili anche per Windows Server 2019 e Windows Server, versione 1809 con l'installazione di [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534).

## <a name="collecting-logs"></a>Come raccogliere i file di log quando si lavora con supporto tecnico Microsoft

Il servizio migrazione archiviazione contiene i registri eventi per il servizio agente di orchestrazione e il servizio proxy. Il server dell'agente di orchestrazione contiene sempre i registri eventi e i server di destinazione con il servizio proxy installato contengono i log del proxy. Questi log si trovano in:

- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService
- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService-proxy

Se è necessario raccogliere questi log per la visualizzazione offline o per inviarli a supporto tecnico Microsoft, è disponibile uno script di PowerShell open source su GitHub:

 [Helper servizio migrazione archiviazione](https://aka.ms/smslogs) 

Esaminare il file Leggimi per l'utilizzo.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Il servizio migrazione archiviazione non viene visualizzato nell'interfaccia di amministrazione di Windows a meno che non si gestisca Windows Server 2019

Quando si usa la versione 1809 dell'interfaccia di amministrazione di Windows per gestire un agente di orchestrazione di Windows Server 2019, non viene visualizzata l'opzione dello strumento per il servizio migrazione archiviazione. 

L'estensione del servizio migrazione archiviazione dell'interfaccia di amministrazione di Windows è associata alla versione per gestire solo i sistemi operativi Windows Server 2019 versione 1809 o successiva. Se viene usato per gestire i sistemi operativi Windows Server meno recenti o le anteprime Insider, lo strumento non verrà visualizzato. Questo comportamento è da progettazione. 

Per risolvere, usare o eseguire l'aggiornamento a Windows Server 2019 Build 1809 o versione successiva.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>La convalida cutover del servizio migrazione archiviazione non riesce con l'errore "accesso negato per i criteri di filtro del token nel computer di destinazione"

Quando si esegue la convalida cutover, viene visualizzato l'errore "errore: accesso negato per i criteri di filtro del token nel computer di destinazione". Ciò si verifica anche se sono state fornite le credenziali di amministratore locale corrette per i computer di origine e di destinazione.

Questo problema è stato risolto nell'aggiornamento di [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . 

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>Il servizio migrazione archiviazione non è incluso nella versione di valutazione di Windows Server 2019 o Windows Server 2019 Essentials

Quando si usa l'interfaccia di amministrazione di Windows per connettersi a una [versione di valutazione di Windows server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) o windows server 2019 Essentials, non è possibile gestire il servizio migrazione archiviazione. Anche il servizio migrazione archiviazione non è incluso in ruoli e funzionalità.

Questo problema è causato da un problema di manutenzione nel supporto di valutazione di Windows Server 2019 e Windows Server 2019 Essentials. 

Per risolvere questo problema per la valutazione, installare una versione finale, MSDN, OEM o con contratto multilicenza di Windows Server 2019 e non attivarla. Senza attivazione, tutte le edizioni di Windows Server operano in modalità di valutazione per 180 giorni. 

Questo problema è stato risolto in una versione successiva di Windows Server.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Timeout del servizio di migrazione archiviazione durante il download del volume CSV errore di trasferimento

Quando si usa l'interfaccia di amministrazione di Windows o PowerShell per scaricare il log degli errori dettagliati per le operazioni di trasferimento, viene visualizzato un errore:

 >   Log di trasferimento: verificare che la condivisione file sia consentita nel firewall. : Questa operazione di richiesta inviata a NET. TCP://localhost: 28940/SMS/Service/1/Transfer non ha ricevuto una risposta entro il timeout configurato (00:01:00). È possibile che la durata consentita per l'operazione fosse una porzione di un timeout più lungo. È possibile che il servizio stia ancora elaborando l'operazione o che il servizio non sia stato in grado di inviare un messaggio di risposta. Prendere in considerazione l'aumento del timeout dell'operazione (eseguendo il cast del canale/proxy a IContextChannel e impostando la proprietà OperationTimeout) e verificare che il servizio sia in grado di connettersi al client.

Questo problema è causato da un numero molto elevato di file trasferiti che non possono essere filtrati nel timeout predefinito di un minuto consentito dal servizio migrazione archiviazione. 

Per risolvere questo problema:

1. Sul computer dell'agente di orchestrazione, modificare il file *%systemroot%\SMS\Microsoft.StorageMigration.Service.exe.config* utilizzando Notepad. exe per modificare "SendTimeout nell'elemento" dal valore predefinito di 1 minuto a 10 minuti.

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Riavviare il servizio "servizio migrazione archiviazione" sul computer dell'agente di orchestrazione. 
3. Sul computer dell'agente di orchestrazione, avviare Regedit. exe
4. Individuare e selezionare la sottochiave del Registro di sistema seguente: 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. Dal menu Modifica, scegliere Nuovo e poi fare clic su Valore DWORD. 
6. Digitare "WcfOperationTimeoutInMinutes" per il nome DWORD, quindi premere INVIO.
7. Fare clic con il pulsante destro del mouse su "WcfOperationTimeoutInMinutes", quindi scegliere modifica. 
8. Nella casella dati di base fare clic su "decimale"
9. Nella casella dati valore digitare "10", quindi fare clic su OK.
10. Uscire dall'editor del Registro di sistema.
11. Provare a scaricare di nuovo il file CSV solo degli errori. 

Si prevede di modificare questo comportamento in una versione successiva di Windows Server 2019.  

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>Avvisi di convalida per i privilegi amministrativi del proxy di destinazione e delle credenziali

Quando si convalida un processo di trasferimento, vengono visualizzati gli avvisi seguenti:

 > **La credenziale dispone di privilegi amministrativi.**
 > Avviso: l'azione non è disponibile in modalità remota.
 > **Il proxy di destinazione è registrato.**
 > Avviso: il proxy di destinazione non è stato trovato.

Se il servizio proxy del servizio migrazione archiviazione non è stato installato nel computer di destinazione Windows Server 2019 o se il computer di destinazione è Windows Server 2016 o Windows Server 2012 R2, questo comportamento è da progettazione. Si consiglia di eseguire la migrazione a un computer Windows Server 2019 con il proxy installato per migliorare significativamente le prestazioni di trasferimento.  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>Alcuni file non vengono inventariati o trasferiti. errore 5 "accesso negato"

Durante l'inventario o il trasferimento di file dai computer di origine a quello di destinazione, la migrazione dei file da cui un utente ha rimosso le autorizzazioni del gruppo Administrators non riesce. Esaminare il servizio migrazione archiviazione-il debug del proxy Mostra:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/26/2019 9:00:04 AM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      srv1.contoso.com
    Description:

    02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
    Stack Trace:
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()   

Questo problema è causato da un errore del codice nel servizio migrazione archiviazione in cui non è stato richiamato il privilegio di backup. 

Per risolvere questo problema, installare [Windows Update 2 aprile 2019, ovvero KB4490481 (build del sistema operativo 17763,404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) nel computer dell'agente di orchestrazione e nel computer di destinazione se il servizio proxy è installato. Verificare che l'account utente della migrazione di origine sia un amministratore locale nel computer di origine e l'agente di orchestrazione del servizio di migrazione archiviazione. Verificare che l'account utente della migrazione di destinazione sia un amministratore locale nel computer di destinazione e l'agente di orchestrazione del servizio migrazione archiviazione. 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>Mancata corrispondenza degli hash DFSR quando si usa il servizio migrazione archiviazione per eseguire il Preseeding dei dati

Quando si usa il servizio migrazione archiviazione per trasferire i file in una nuova destinazione, quindi si configura il Replica DFS (DFSR) per replicare i dati con un server DFSR esistente tramite la replica con seeding o la clonazione del database DFSR, tutti i file presentano un hash mancata corrispondenza e replica eseguita di nuovo. I flussi di dati, i flussi di sicurezza, le dimensioni e gli attributi sembrano essere perfettamente corrispondenti dopo l'uso di SMS per trasferirli. L'analisi dei file con ICACLS o il log di debug per la clonazione del database DFSR rivela:

File di origine:

  d:\test\Source icacls:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1200a9;;;D D) (A;; 0 x1301bf;;;D U) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; Bu

File di destinazione:

  icacls d:\test\thatcher.png/Save out. txt/t Thatcher. png D:AI (A;; FA;;; BA) (A;; 0 x1301bf;;;D U) (A;; 0 x1200a9;;;D D) (A; ID; FA;;; BA) (A; ID; FA;;; SY) (A; ID; 0x1200a9;;; BU)**S: PAINO_ACCESS_CONTROL**

Log di debug DFSR:

    20190308 10:18:53.116 3948 DBCL  4045 [WARN] DBClone::IDTableImportUpdate Mismatch record was found. 

    Local ACL hash:1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

    Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

Questo problema è stato risolto dall'aggiornamento di [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>Errore "Impossibile trasferire lo spazio di archiviazione su uno degli endpoint" durante il trasferimento da Windows Server 2008 R2

Quando si tenta di trasferire dati da un computer di origine Windows Server 2008 R2, non vengono trasferiti dati e viene visualizzato un errore:  

    Couldn't transfer storage on any of the endpoints.
    0x9044

Questo errore è previsto se il computer Windows Server 2008 R2 non è completamente aggiornato con tutti gli aggiornamenti critici e importanti da Windows Update. Indipendentemente dal servizio migrazione archiviazione, è sempre consigliabile applicare patch a un computer Windows Server 2008 R2 per motivi di sicurezza, in quanto il sistema operativo non contiene i miglioramenti della sicurezza delle versioni più recenti di Windows Server.

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>Errore "Impossibile trasferire lo spazio di archiviazione su uno degli endpoint" e "verificare se il dispositivo di origine è online-non è stato possibile accedervi".

Quando si tenta di trasferire dati da un computer di origine, alcune o tutte le condivisioni non vengono trasferite, con errore di riepilogo:

    Couldn't transfer storage on any of the endpoints.
    0x9044

Esaminando i dettagli del trasferimento SMB viene visualizzato l'errore:

    Check if the source device is online - we couldn't access it.

Esaminando il registro eventi di StorageMigrationService/admin viene visualizzato quanto segue:

    Couldn't transfer storage.

    Job: Job1
    ID:  
    State: Failed
    Error: 36931
    Error Message: 

   Indicazioni: controllare l'errore dettagliato e verificare che siano soddisfatti i requisiti di trasferimento. Il processo di trasferimento non è riuscito a trasferire i computer di origine e di destinazione. Il problema potrebbe essere dovuto al fatto che il computer dell'agente di orchestrazione non è riuscito a raggiungere i computer di origine o di destinazione, probabilmente a causa di una regola del firewall o

L'analisi del log StorageMigrationService-proxy/debug Mostra:

    07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)    

Si tratta di un difetto del codice che verrebbe manifesto se l'account di migrazione non dispone almeno delle autorizzazioni di lettura per le condivisioni SMB. Questo problema è stato risolto per la prima volta nell'aggiornamento cumulativo [4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062). 

## <a name="error-0x80005000-when-running-inventory"></a>Errore 0x80005000 durante l'esecuzione dell'inventario

Dopo l'installazione di [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) e il tentativo di eseguire l'inventario, l'inventario ha esito negativo con errori:

    EXCEPTION FROM HRESULT: 0x80005000
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2503
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory the computers.
    Job: foo2
    ID: 20ac3f75-4945-41d1-9a79-d11dbb57798b
    State: Failed
    Error: 36934
    Error Message: Inventory failed for all devices
    Guidance: Check the detailed error and make sure the inventory requirements are met. The job couldn't inventory any of the specified source computers. This could be because the orchestrator computer couldn't reach it over the network, possibly due to a firewall rule or missing permissions.
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2509
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory a computer.
    Job: foo2
    Computer: FS01.TailwindTraders.net
    State: Failed
    Error: -2147463168
    Error Message: 
    Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.
  
    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/14/2020 1:18:21 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      2019-rtm-orc.ned.contoso.com
    Description:
    02/14/2020-13:18:21.097 [Erro] Failed device discovery stage SystemInfo with error: (0x80005000) Unknown error (0x80005000)   
  
Questo errore è causato da un difetto del codice nel servizio migrazione archiviazione quando si forniscono le credenziali di migrazione sotto forma di nome dell'entità utente (UPN), ad esempio "meghan@contoso.com". Il servizio dell'agente di orchestrazione del servizio migrazione archiviazione non è in grado di analizzare correttamente questo formato, causando un errore in una ricerca del dominio aggiunta per il supporto della migrazione del cluster in KB4512534 e 19H1.

Per aggirare questo problema, fornire le credenziali nel formato dominio\utente, ad esempio ' Contoso\Meghan '.

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>Errore "ServiceError0x9006" o "il proxy non è attualmente disponibile". Quando si esegue la migrazione a un cluster di failover di Windows Server

Quando si tenta di trasferire i dati in un file server in cluster, vengono visualizzati errori quali: 

    Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
    0x9006
    ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand

Questo errore è previsto se la risorsa file server è stata spostata dal nodo proprietario del cluster Windows Server 2019 originale a un nuovo nodo e la funzionalità proxy del servizio migrazione archiviazione non è stata installata in tale nodo.

Come soluzione alternativa, spostare di nuovo la risorsa del file server di destinazione nel nodo del cluster proprietario originale che era in uso al momento della prima configurazione delle associazioni di trasferimento.

Come soluzione alternativa:

1. Installare la funzionalità del proxy del servizio migrazione archiviazione in tutti i nodi di un cluster.
2. Eseguire il seguente comando di PowerShell per il servizio migrazione archiviazione sul computer dell'agente di orchestrazione: 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>Errore "Impossibile trovare la dll" durante l'esecuzione dell'inventario da un nodo del cluster

Quando si tenta di eseguire l'inventario con il servizio migrazione archiviazione e come destinazione di un cluster di failover di Windows Server, usare file server origine, vengono visualizzati gli errori seguenti:

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
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  Avviare il servizio migrazione archiviazione che creerà un nuovo database.

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>Errore "CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO non riuscito per la risorsa netName" e cutover cluster Windows Server 2008 R2 non riesce

Quando si tenta di eseguire il trasferimento di un'origine cluster Windows Server 2008 R2, il taglio viene bloccato alla fase "ridenominazione del computer di origine..." viene visualizzato l'errore seguente:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

Questo problema è causato da un'API mancante nelle versioni precedenti di Windows Server. Attualmente non è possibile eseguire la migrazione di cluster Windows Server 2008 e Windows Server 2003. È possibile eseguire l'inventario e il trasferimento senza problemi nei cluster Windows Server 2008 R2, quindi eseguire manualmente cutover modificando manualmente l'origine del cluster file server risorsa NetName e l'indirizzo IP, quindi modificando il cluster di destinazione NetName e IP indirizzo corrispondente all'origine originale. 

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>Cutover si blocca su "38% mapping delle interfacce di rete nel computer di origine..." 

Quando si tenta di eseguire il trasferimento di un computer di origine, dopo aver impostato il computer di origine per l'utilizzo di un nuovo indirizzo IP statico (non DHCP) in una o più interfacce di rete, il trasferimento viene bloccato alla fase "38% del mapping delle interfacce di rete nel comnputer di origine..." viene visualizzato il seguente messaggio di errore nel registro eventi SMS:

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          11/13/2019 3:47:06 PM
    Event ID:      20494
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      orc2019-rtm.corp.contoso.com
    Description:
    Couldn't set the IP address on the network adapter.

    Computer: fs12.corp.contoso.com
    Adapter: microsoft hyper-v network adapter
    IP address: 10.0.0.99
    Network mask: 16
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

L'analisi del computer di origine indica che non è possibile modificare l'indirizzo IP originale. 

Questo problema non si verifica se è stata selezionata l'opzione "Usa DHCP" nella schermata "Configura cutover" dell'interfaccia di amministrazione di Windows, solo se si specifica un nuovo indirizzo IP statico, una subnet e un gateway. 

Questo problema è causato da una regressione nell'aggiornamento [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) . Sono attualmente disponibili due soluzioni alternative per questo problema:

  - Prima del trasferimento: anziché impostare un nuovo indirizzo IP statico in cutover, selezionare "Usa DHCP" e assicurarsi che l'ambito DHCP copra la subnet. SMS configurerà il computer di origine per l'utilizzo di DHCP nelle interfacce del computer di origine e il trasferimento proseguirà normalmente. 
  
  - Se il trasferimento è già bloccato: accedere al computer di origine e abilitare DHCP sulle interfacce di rete, dopo aver verificato che un ambito DHCP copra tale subnet. Quando il computer di origine acquisisce un indirizzo IP fornito da DHCP, SMS procederà con il taglio in modo normale.
  
In entrambe le soluzioni alternative, dopo il completamento del trasferimento, è possibile impostare un indirizzo IP statico nel computer di origine precedente, in base alle esigenze e all'arresto dell'utilizzo di DHCP.   

## <a name="slower-than-expected-re-transfer-performance"></a>Prestazioni di ritrasferimento previste più lente

Dopo aver completato un trasferimento, dopo aver eseguito un nuovo trasferimento degli stessi dati, è possibile che non si verifichi un notevole miglioramento del tempo di trasferimento anche quando sono stati modificati pochi dati nel frattempo nel server di origine.

Si tratta di un comportamento previsto quando si trasferisce un numero molto elevato di file e cartelle nidificate. La dimensione dei dati non è pertinente. Sono stati apportati miglioramenti a questo comportamento in [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) e si continua a ottimizzare le prestazioni di trasferimento. Per ottimizzare ulteriormente le prestazioni, vedere [ottimizzazione delle prestazioni di inventario e trasferimento](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#optimizing-inventory-and-transfer-performance).

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>I dati non vengono trasferiti, rinominati dall'utente durante la migrazione a o da un controller di dominio

Dopo l'avvio del trasferimento da o a un controller di dominio:

 1. Non viene eseguita la migrazione dei dati e non viene creata alcuna condivisione nella destinazione.
 2. Nell'interfaccia di amministrazione di Windows è visualizzato un simbolo di errore rosso senza messaggio di errore
 3. Uno o più utenti di Active Directory e gruppi locali di dominio hanno il nome e/o l'attributo di accesso precedente a Windows 2000 modificato
 4. Nell'agente di orchestrazione SMS viene visualizzato l'evento 3509:
 
        Log Name:      Microsoft-Windows-StorageMigrationService/Admin
        Source:        Microsoft-Windows-StorageMigrationService
        Date:          1/10/2020 2:53:48 PM
        Event ID:      3509
        Task Category: None
        Level:         Error
        Keywords:      
        User:          NETWORK SERVICE
        Computer:      orc2019-rtm.corp.contoso.com
        Description:
        Couldn't transfer storage for a computer.

        Job: dctest3
        Computer: dc02-2019.corp.contoso.com
        Destination Computer: dc03-2019.corp.contoso.com
        State: Failed
        Error: 53251
        Error Message: Local accounts migration failed with error System.Exception: -2147467259
           at Microsoft.StorageMigration.Service.DeviceHelper.MigrateSecurity(IDeviceRecord sourceDeviceRecord, IDeviceRecord destinationDeviceRecord, TransferConfiguration config, Guid proxyId, CancellationToken cancelToken)

Si tratta di un comportamento previsto se si tenta di eseguire la migrazione da o a un controller di dominio con il servizio migrazione archiviazione ed è stata usata l'opzione "Esegui la migrazione di utenti e gruppi" per rinominare o riutilizzare gli account. anziché selezionare "non trasferire utenti e gruppi". La migrazione del controller [di dominio non è supportata con il servizio migrazione archiviazione](faq.md). Poiché un controller di dominio non dispone di veri utenti e gruppi locali, il servizio migrazione archiviazione gestisce queste entità di sicurezza come se si trattasse di una migrazione tra due server membri e tenti di modificare gli ACL come indicato, causando gli errori e gli account modificati o copiati. 

Se è già stato eseguito il trasferimento di una o più volte:

 1. Usare il comando AD PowerShell seguente in un controller di dominio per individuare gli utenti o i gruppi modificati (modificando SearchBase in modo che corrisponda al nome dinstringuished del dominio): 

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```
   
 2. Per tutti gli utenti restituiti con il nome originale, modificare il nome di accesso utente (precedente a Windows 2000) per rimuovere il suffisso di carattere casuale aggiunto dal servizio migrazione archiviazione, in modo che questo perdente possa accedere.
 3. Per tutti i gruppi restituiti con il nome originale, modificare il nome del gruppo (precedente a Windows 2000) per rimuovere il suffisso di carattere casuale aggiunto dal servizio migrazione archiviazione.
 4. Per tutti gli utenti o i gruppi disabilitati con nomi che ora contengono un suffisso aggiunto dal servizio migrazione archiviazione, è possibile eliminare questi account. È possibile verificare che gli account utente siano stati aggiunti in un secondo momento perché contengono solo il gruppo Domain Users e avranno una data/ora di creazione corrispondente all'ora di inizio del trasferimento del servizio migrazione archiviazione.
 
 Se si vuole usare il servizio migrazione archiviazione con i controller di dominio a scopo di trasferimento, assicurarsi di selezionare sempre "non trasferire utenti e gruppi" nella pagina impostazioni di trasferimento dell'interfaccia di amministrazione di Windows.
 
 ## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>Errore 53, "Impossibile eseguire l'inventario di tutti i dispositivi specificati" durante l'esecuzione dell'inventario, 

Quando si tenta di eseguire l'inventario, viene visualizzato quanto segue:

    Failed to inventory all specified devices 
    
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          1/16/2020 8:31:17 AM
    Event ID:      2516
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    Couldn't inventory files on the specified endpoint.
    Job: ned1
    Computer: ned.corp.contoso.com
    Endpoint: hithere
    State: Failed
    File Count: 0
    File Size in KB: 0
    Error: 53
    Error Message: Endpoint scan failed
    Guidance: Check the detailed error and make sure the inventory requirements are met. This could be because of missing permissions on the source computer.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/16/2020 8:31:17 AM
    Event ID:      10004
    Task Category: None
    Level:         Critical
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    01/16/2020-08:31:17.031 [Crit] Consumer Task failed with error:The network path was not found.
    . StackTrace=   at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)
       at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetEnvironmentPathFolders(String ServerName, Boolean IsServerLocal)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.ScanUtils.<ScanSMBEndpoint>d__3.MoveNext()
       at Microsoft.StorageMigration.Proxy.EndpointScanOperation.Run()
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(EndpointScanRequest scanRequest, Guid operationId)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(Object request)
       at Microsoft.StorageMigration.Proxy.Common.ProducerConsumerManager`3.Consume(CancellationToken token)    
       
    01/16/2020-08:31:10.015 [Erro] Endpoint Scan failed. Error: (53) The network path was not found.
    Stack trace:
       at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)

In questa fase, l'agente di orchestrazione del servizio migrazione archiviazione sta tentando di leggere il registro di sistema remoto per determinare la configurazione del computer di origine, ma è stato rifiutato dal server di origine. Ciò può essere causato da una serie di fattori:

 - Il servizio Registro di sistema remoto non è in esecuzione nel computer di origine.
 - il firewall non consente le connessioni del registro di sistema remoto al server di origine dall'agente di orchestrazione.
 - L'account di migrazione di origine non dispone delle autorizzazioni del registro di sistema remoto per la connessione al computer di origine.
 - L'account di migrazione di origine non dispone delle autorizzazioni di lettura nel registro di sistema del computer di origine, in "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows NT\CurrentVersion" o in "HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\ LanmanServer

## <a name="see-also"></a>Vedere anche

- [Panoramica di servizio migrazione archiviazione](overview.md)
