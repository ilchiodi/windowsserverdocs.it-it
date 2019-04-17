---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: 'Cosa & #39; s di dominio Active Directory di servizi di installazione e rimozione'
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 814a6f1af9144db28e81163b21cb5da4b6e51b3b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="what39s-new-in-active-directory-domain-services-installation-and-removal"></a>Cosa & #39; s di dominio Active Directory di servizi di installazione e rimozione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento si applica:  
  
-   [Active Directory Domain Services configurazione guidata](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_ADConfigurationWizard)  
  
-   [Integrazione di Adprep.exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep)  
  
-   [Convalida dei prerequisiti di installazione di AD DS](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_PrereqCheck)  
  
-   [Requisiti di sistema](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_SystemReqs)  
  
-   [Problemi noti](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues)  
  
Distribuzione di Active Directory Domain Services (AD DS) in Windows Server 2012 è più semplice e veloce rispetto alle versioni precedenti di Windows Server. Il processo di installazione di servizi di dominio Active Directory è ora basato su Windows PowerShell ed è integrato con Server Manager. Viene ridotto il numero di passaggi necessari per introdurre i controller di dominio in un ambiente Active Directory esistente. In questo modo il processo di creazione di un nuovo ambiente Active Directory più semplice ed efficiente. Il nuovo processo di distribuzione di dominio Active Directory riduce al minimo le probabilità di errori che potrebbero bloccare l'installazione.  
  
Inoltre, è possibile installare i file binari del ruolo server di dominio Active Directory (che è il ruolo del server di dominio Active Directory) su più server contemporaneamente. È anche possibile eseguire l'installazione guidata servizi di dominio Active Directory in modalità remota in un singolo server. Questi miglioramenti offrono maggiore flessibilità per la distribuzione di controller di dominio che eseguono Windows Server 2012, soprattutto per le distribuzioni su larga scala, globale in cui molti controller di dominio devono essere distribuite agli uffici in aree geografiche diverse.  
  
Installazione di Active Directory include le funzionalità seguenti:  
  
-   **Integrazione di Adprep.exe nel processo di installazione di Active Directory.** I complessi passaggi necessari per preparare un Active Directory esistente, ad esempio la necessità di usare un'ampia gamma di credenziali diverse, copiare i file di Adprep.exe o accedere ai controller di dominio specifico, sono stati semplificati o vengono eseguite automaticamente. Ciò riduce il tempo necessario per installare Active Directory e si riduce le probabilità di errori che potrebbero altrimenti bloccare l'innalzamento di livello controller di dominio.  
  
    Per gli ambienti in cui è preferibile eseguire i comandi adprep.exe prima una nuova installazione di controller di dominio, è ancora possibile eseguire i comandi adprep.exe separatamente dall'installazione di servizi di dominio Active Directory. La versione di Windows Server 2012 di adprep.exe viene eseguito in modalità remota, pertanto è possibile eseguire tutti i comandi necessari da un server che esegue una versione a 64 bit di Windows Server 2008 o versione successiva.  
  
-   **La nuova installazione di servizi di dominio Active Directory si basa su Windows PowerShell e può essere richiamata in modalità remota.** La nuova installazione di servizi di dominio Active Directory è integrata con Server Manager, pertanto è possibile utilizzare la stessa interfaccia per installare Active Directory che durante l'installazione di altri ruoli del server. Per gli utenti di Windows PowerShell, i cmdlet di distribuzione di dominio Active Directory forniscono maggiore funzionalità e flessibilità. Esiste parità funzionale tra della riga di comando e opzioni di installazione GUI.  
  
-   **La nuova installazione di servizi di dominio Active Directory include la convalida dei prerequisiti.** I potenziali errori vengono rilevati prima dell'inizio dell'installazione. È possibile correggere le condizioni di errore prima si verifichino, senza rischiare di eseguire un aggiornamento parziale. Ad esempio, se è necessario eseguire adprep /domainprep, l'installazione guidata verifica che l'utente disponga di diritti sufficienti per eseguire l'operazione.  
  
-   **Pagine di configurazione sono raggruppate in una sequenza che rispecchia i requisiti delle opzioni di promozione più comuni con opzioni correlate raggruppate in meno pagine della procedura guidata.** Fornisce un contesto migliore per opzioni di installazione.  
  
-   **È possibile esportare uno script di Windows PowerShell contenente tutte le opzioni specificate durante l'installazione grafica.** Al termine di un'installazione o rimozione, è possibile esportare le impostazioni in uno script di Windows PowerShell per l'utilizzo con l'automazione della stessa operazione.  
  
-   **Si verifica solo parte critica della replica prima del riavvio.** Nuova opzione che consente la replica dei dati non critici prima del riavvio. Per ulteriori informazioni, vedere [gli argomenti del cmdlet ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  
  
### <a name="BKMK_ADConfigurationWizard"></a>Active Directory Domain Services configurazione guidata  
A partire da Windows Server 2012, la configurazione guidata servizi di dominio Active Directory sostituisce il precedente dominio servizi di installazione guidata Active Directory come la possibilità di interfaccia utente di specificare le impostazioni quando si installa un controller di dominio. La configurazione guidata servizi di dominio Active Directory viene avviata al termine dell'Aggiunta guidata ruoli.  
  
> [!WARNING]  
> Il precedente dominio servizi di installazione guidata Active Directory (dcpromo.exe) è deprecato a partire da Windows Server 2012.  
  
In [installare servizi di dominio Active Directory & #40; Livello 100 & #41; ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), le procedure dell'interfaccia utente mostrano come avviare Aggiunta guidata ruoli per installare il server di dominio Active Directory di file binari del ruolo e quindi eseguire il dominio servizi di configurazione guidata Active Directory per completare l'installazione di controller di dominio. Gli esempi di Windows PowerShell mostrano come completare entrambi i passaggi utilizzando un cmdlet di distribuzione di dominio Active Directory.  
  
### <a name="BKMK_NewAdprep"></a>Integrazione di Adprep.exe  
A partire da Windows Server 2012, esiste solo una versione di Adprep.exe (non è una versione a 32 bit, adprep32.exe). I comandi adprep vengono eseguiti automaticamente all'occorrenza quando si installa un controller di dominio che esegue Windows Server 2012 in una foresta o dominio di Active Directory esistente.  
  
Benché le operazioni di adprep siano eseguite automaticamente, è possibile eseguire Adprep.exe separatamente. Se, ad esempio, l'utente che installa Servizi di dominio Active Directory non è un membro del gruppo Enterprise Admins, che è necessario per eseguire Adprep /forestprep, potrebbe essere necessario eseguire il comando separatamente. Tuttavia, è necessario eseguire adprep.exe. Se si intende eseguire l'aggiornamento sul posto del primo controller di dominio di Windows Server 2012 (in altre parole, si intende sul posto del sistema operativo di un controller di dominio che esegue Windows Server 2012).  
  
Adprep.exe si trova nella cartella \support\adprep del disco di installazione di Windows Server 2012. La versione di Windows Server 2012 di adprep è in grado di eseguire in modalità remota.  
  
La versione di Windows Server 2012 di adprep.exe possibile eseguire in qualsiasi server che esegue una versione a 64 bit di Windows Server 2008 o versione successiva. Il server necessita di connettività di rete al master schema per la foresta e il master infrastrutture del dominio in cui si desidera aggiungere un controller di dominio. Se uno dei due ruoli è ospitato in un server che esegue Windows Server 2003, quindi che adprep deve essere eseguito in modalità remota. Il server in cui viene eseguito adprep non è necessario essere un controller di dominio. Può essere aggiunto al dominio o un gruppo di lavoro.  
  
> [!NOTE]  
> Se si tenta di eseguire la versione di Windows Server 2012 di adprep.exe in un server che esegue Windows Server 2003, viene visualizzato l'errore seguente:  
>   
> Adprep.exe non è un'applicazione Win32 valida.  
  
![Novità](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  
  
Per informazioni sulla risoluzione di altri errori generati da Adprep.exe, vedere [problemi noti](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  
  
#### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Verifica di appartenenza a gruppo contro i ruoli di master operazioni di Windows Server 2003  
Per ogni comando (/ forestprep, /domainprep o /rodcprep), Adprep esegue una verifica di appartenenza al gruppo per determinare se la credenziale rappresenta un account in determinati gruppi. Per eseguire questa verifica, Adprep contatta il proprietario del ruolo master operazioni. Se il master operazioni esegue Windows Server 2003, è necessario specificare /user e /userdomain parametri della riga di comando se si esegue Adprep.exe per garantire che la verifica di appartenenza a gruppo viene eseguita in tutti i casi.  
  
/User e /userdomain sono nuovi parametri per Adprep.exe in Windows Server 2012. Questi parametri specificano il nome dell'account utente e il dominio dell'utente, rispettivamente, dell'utente che esegue il comando adprep. L'utilità della riga di comando di Adprep.exe blocca l'inserimento di uno dei /userdomain e /user, ma omettendo l'altro.  
  
Tuttavia, operazioni di Adprep possono essere eseguite anche come parte di un'installazione di servizi di dominio Active Directory utilizzando Windows PowerShell o Server Manager. Tali esperienze di condividono la medesima implementazione sottostante (adprep.dll) come adprep.exe. Le esperienze Windows PowerShell e Server Manager dispone di credenziali diverso di input, che non impone gli stessi requisiti come da adprep.exe. Con Windows PowerShell o Server Manager, è possibile passare un valore per /user ma non a adprep.dll. Se /user viene specificato ma /userdomain non viene specificato, il dominio del computer locale viene utilizzato per eseguire la verifica. Se il computer non appartenenti a un dominio, non sarà possibile controllare l'appartenenza al gruppo.  
  
Quando non sarà possibile controllare l'appartenenza al gruppo, Adprep Mostra un messaggio di avviso nei file di log adprep e continuerà:  
  
```  
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```  
  
Se si esegue Adprep.exe senza specificare i parametri di /userdomain e /user e il master operazioni esegue Windows Server 2003, Adprep.exe contatterà un controller di dominio nel dominio dell'utente corrente. Se l'utente di accesso corrente non è un account di dominio, Adprep.exe non può eseguire la verifica di appartenenza al gruppo. Adprep.exe non può eseguire anche la verifica di appartenenza al gruppo se vengono utilizzate le credenziali di smart card, anche se si specifica sia /user e /userdomain.  
  
Se Adprep termina senza errori, non è richiesta alcuna azione. Se Adprep non riesce durante l'esecuzione con errori di accesso, fornire un account con l'appartenenza corretta. Per ulteriori informazioni, vedere [credenziali necessarie per eseguire Adprep.exe e installare servizi di dominio Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
#### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintassi per Adprep in Windows Server 2012  
Utilizzare la sintassi seguente per eseguire adprep separatamente da un'installazione di servizi di dominio Active Directory:  
  
```  
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```  
  
Utilizzare /logdsid nel comando per generare una registrazione più dettagliata. Il file adprep.log si trova in windir%\System32\Debug\Adprep\Logs.  
  
#### <a name="running-adprep-using-smartcard"></a>Esecuzione di adprep con smart card  
La versione di Windows Server 2012 di adprep.exe. funziona con smart card come credenziali, ma non esiste alcun metodo semplice per specificare le credenziali della smart card tramite la riga di comando. Un modo per eseguire questa operazione è ottenere le credenziali della smart card tramite il cmdlet Get-Credential di PowerShell. Quindi utilizzare il nome utente dell'oggetto PSCredential restituito, che viene visualizzato come `@@...`. La password è il PIN della smart card.  
  
Adprep.exe è necessario specificare /userdomain se /user viene specificato. Le credenziali smart card, il parametro /userdomain dovrebbe essere il dominio dell'account utente sottostante rappresentato dalla smart card.  
  
#### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Comando adprep /domainprep /gpprep non viene eseguito automaticamente  
Il comando adprep /domainprep /gpprep non viene eseguito come parte dell'installazione di servizi di dominio Active Directory. Questo comando imposta le autorizzazioni necessarie per gruppo di criteri (RSOP) funzionalità modalità di pianificazione. Per ulteriori informazioni su questo comando, vedere [articolo della Microsoft Knowledge Base 324392](https://support.microsoft.com/kb/324392). Se il comando deve essere eseguito nel dominio Active Directory, è possibile eseguirlo separatamente dall'installazione di servizi di dominio Active Directory. Se è già stato eseguito il comando in preparazione della distribuzione di controller di dominio che eseguono Windows Server 2003 SP1 o versioni successive, il comando non è necessario eseguire di nuovo.  
  
È possibile aggiungere in modo sicuro i controller di dominio che eseguono Windows Server 2012 a un dominio esistente senza esecuzione di adprep /domainprep /gpprep, ma tale modalità non funzionerà correttamente.  
  
### <a name="BKMK_PrereqCheck"></a>Convalida dei prerequisiti di installazione di AD DS  
L'installazione guidata servizi di dominio Active Directory verifica che i seguenti prerequisiti siano soddisfatti prima dell'inizio dell'installazione. Questo offre l'opportunità di risolvere i problemi che potrebbero ostacolare l'installazione.  
  
Ad esempio, i prerequisiti di Adprep includono:  
  
-   Verifica delle credenziali per adprep: se è necessario eseguire adprep, la procedura guidata di installazione verifica che l'utente disponga di diritti sufficienti per eseguire le operazioni di Adprep necessarie.  
  
-   Verifica della disponibilità del master schema: se l'installazione guidata determina che deve essere eseguita da adprep /forestprep, verifica che il master schema sia online e in caso contrario non riesce.  
  
-   Verifica della disponibilità del master infrastrutture: se l'installazione guidata determina che deve essere eseguita da adprep /domainprep, verifica che il master infrastrutture sia online e in caso contrario non riesce.  
  
Altri controlli dei prerequisiti che vengono ereditati dal legacy installazione guidata Active Directory (dcpromo.exe) includono:  
  
-   Verifica del nome della foresta: assicura che il nome della foresta sia valido e non esiste già.  
  
-   Verifica del nome NetBIOS: i controlli forniti nome NetBIOS valido e non è in conflitto con nomi esistenti.  
  
-   Verifica del percorso dei componenti: verifica che i percorsi per il database di Active Directory, i registri e SYSVOL siano validi e che sia disponibile spazio su disco insufficiente.  
  
-   Verifica del nome del dominio figlio: assicura che l'elemento padre e i nuovi nomi di dominio figlio siano validi e che non sono in conflitto con i domini esistenti.  
  
-   Verifica del nome del dominio albero: assicura che il nome dell'albero specificato sia valido e che non attualmente esiste.  
  
## <a name="BKMK_SystemReqs"></a>Requisiti di sistema  
Requisiti di sistema per Windows Server 2012 sono identiche a Windows Server 2008 R2. Per ulteriori informazioni, vedere [Windows Server 2008 R2 con SP1 requisiti di sistema](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  
  
Alcune funzionalità potrebbero prevedere requisiti aggiuntivi. Ad esempio, la funzionalità di clonazione di controller di dominio virtuale richiede che l'emulatore PDC esegue Windows Server 2012 e un computer che esegue Windows Server 2012 con il ruolo Hyper-V installato.  
  
## <a name="BKMK_KnownIssues"></a>Problemi noti  
In questa sezione sono elencati alcuni dei problemi che influiscono sull'installazione di Active Directory in Windows Server 2012. Per altri problemi noti, vedere [risoluzione dei problemi di distribuzione del Controller di dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
-   Se l'accesso WMI al master schema è bloccato da Windows Firewall quando si esegue in modalità remota adprep /forestprep, nel Registro di adprep in systemroot%\system32\debug\adprep viene registrato l'errore seguente:  
  
    ```  
    Adprep encountered a Win32 error.   
    Error code: 0x6ba Error message: The RPC server is unavailable.  
    ```  
  
    In questo caso, è possibile risolvere l'errore da entrambi in esecuzione di adprep /forestprep direttamente sul master schema oppure è possibile eseguire uno dei seguenti comandi per consentire il traffico WMI attraverso Windows Firewall.  
  
    Per Windows Server 2008 o versione successiva:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes  
    ```  
  
    Per Windows Server 2003:  
  
    ```  
    netsh firewall set service RemoteAdmin enable  
    ```  
  
    Al termine di adprep è possibile eseguire uno dei comandi seguenti per bloccare di nuovo traffico WMI:  
  
    ```  
    netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no  
    ```  
  
    ```  
    netsh firewall set service remoteadmin disable  
    ```  
  
-   È possibile digitare Ctrl + C per annullare il cmdlet Install-ADDSForest. L'annullamento interrompe l'installazione e le modifiche apportate allo stato del server vengono annullate. Ma dopo il comando di annullamento, il controllo non viene restituito a Windows PowerShell e il cmdlet potrebbe bloccarsi per un tempo indefinito.  
  
-   **Installazione di un controller di dominio utilizzando le credenziali della smart card non riesce se il server di destinazione non è stato aggiunto al dominio prima dell'installazione.**  
  
    In questo caso è il messaggio di errore restituito:  
  
    Impossibile connettersi al controller di dominio di origine di replica *nome controller di dominio di origine*. (Eccezione: accesso non riuscito: nome utente sconosciuto o password errata)  
  
    Se unire il server di destinazione al dominio e quindi eseguire l'installazione utilizzando una smart card, l'installazione ha esito positivo.  
  
-   **Il modulo ADDSDeployment non viene eseguito con processi a 32 bit.** Se si automatizza la distribuzione e configurazione di Windows Server 2012 tramite uno script che include un cmdlet ADDSDeployment e qualsiasi altro cmdlet che non supporta i processi a 64 bit nativi, lo script può verificarsi un errore che indica che non è possibile trovare il cmdlet ADDSDeployment.  
  
    In questo caso, è necessario eseguire il cmdlet ADDSDeployment separatamente dal cmdlet che non supporta i processi a 64 bit nativi.  
  
-   Esiste un nuovo file system in Windows Server 2012 denominato Resilient File System. Non archiviare il database di Active Directory, i file di log o SYSVOL su un volume di dati formattato con Resilient File System (ReFS). Per ulteriori informazioni su ReFS, vedere [creazione del file system di prossima generazione per Windows: ReFS](http://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
  
-   In Server Manager, i server che eseguono servizi di dominio Active Directory o altri ruoli server in un'installazione Server Core e sono stati aggiornati a Windows Server 2012, il ruolo del server può essere visualizzati con stato rosso, anche se vengono raccolti gli eventi e lo stato come previsto. Server che eseguono un'installazione Server Core di una versione preliminare che possono essere interessati anche Windows Server 2012.  
  
### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>L'installazione di Active Directory Domain Services si blocca se un errore impedisce la parte critica della replica  
Se l'installazione di Active Directory rileva un errore durante la fase della parte critica della replica, l'installazione può bloccarsi per un tempo indefinito. Ad esempio, se gli errori di rete impediscono completamento della replica critica, l'installazione non prosegue.  
  
Se si installa tramite Server Manager, è possibile vedere la pagina di avanzamento installazione restano aperte, ma non esiste alcun errore sullo schermo e lo stato di avanzamento non può cambiare per circa 15 minuti. Se si utilizza Windows PowerShell, lo stato di avanzamento visualizzato nella finestra di Windows PowerShell non cambierà per oltre 15 minuti.  
  
Se si verifica questo problema, controllare il file dcpromo.log nella cartella %SystemRoot%\Debug. nel server di destinazione. Il file di log indicherà in genere ripetuti errori di replica. Alcune cause note di questo problema sono:  
  
-   I problemi di rete impedisce parte critica della replica tra il server di destinazione viene innalzato di livello e il controller di dominio di origine di replica.  
  
    Ad esempio, il log dcpromo.log Mostra:  
  
    ```  
  
      05/02/2012 14:16:46 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963  
      Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    500  
    Reported error information:  
    Error value:   
    Could not find the domain controller for this domain. (1908)  
    directory service:   
    <domain>.com  
    Extensive error information:  
    Error value:   
    A security package specific error occurred. 1825  
    directory service:   
    <DC Name>  
    ```  
  
    Poiché il processo di installazione Ritenta la parte critica della replica per un tempo indefinito, l'installazione del controller di dominio prosegue se vengono risolti i problemi di rete sottostante. Analizzare il problema di rete utilizzando strumenti quali ipconfig, nslookup e netmon in base alle esigenze. Assicurarsi che sia disponibile connettività tra il controller di dominio che si alzano di livello e il partner di replica selezionato durante l'installazione di servizi di dominio Active Directory. Assicurarsi inoltre che la risoluzione dei nomi funzioni.  
  
    Requisiti di installazione di Active Directory per la risoluzione dei nomi e la connettività di rete vengono convalidati durante il controllo dei prerequisiti prima dell'inizio dell'installazione. Ma possono verificarsi alcune condizioni di errore nel tempo dopo la convalida dei prerequisiti e prima del completamento dell'installazione, ad esempio se il partner di replica non è più disponibile durante l'installazione.  
  
-   Durante l'installazione di controller di dominio di replica, l'account amministratore locale del server di destinazione specificato per le credenziali di installazione e la password dell'account amministratore locale corrisponde alla password di un account amministratore di dominio. In questo caso, è possibile completare l'installazione guidata e iniziare l'installazione prima che si verifichi l'errore "Accesso negato".  
  
    Ad esempio, il log dcpromo.log Mostra:  
  
    ```  
  
    03/30/2012 11:36:51 [INFO] Creating the NTDS Settings object for this Active Directory Domain Controller on the remote AD DC DC2.contoso.com...  
    03/30/2012 11:36:51 [INFO] EVENTLOG (Error): NTDS Replication / DS RPC Client : 1963Internal event: The following local directory service received an exception from a remote procedure call (RPC) connection. Extensive RPC information was requested. This is intermediate information and might not contain a possible cause.  
    Process ID:   
    508  
    Reported error information:  
    Error value:   
    Access is denied. (5)  
    directory service:   
    DC2.contoso.com  
  
    ```  
  
    Se l'errore è causato specificando un account amministratore locale e la password, per correggerlo è necessario reinstallare il sistema operativo, [eseguire la pulizia dei metadati](https://technet.microsoft.com/library/cc816907(WS.10).aspx) dell'account per il controller di dominio che non è riuscito a completare l'installazione e quindi riprovare l'installazione di dominio Active Directory utilizzando credenziali di amministratore di dominio. Il riavvio del server non consente di correggere questa condizione di errore perché il server indicherà che è installato Active Directory, anche se l'installazione non è stata completata.  
  
### <a name="BKMK_nonnormalDNSNameWarning"></a>Active Directory Domain Services configurazione guidata Avvisa quando viene specificato un nome DNS forma non normalizzata  
Se si crea un nuovo dominio o foresta e si specifica un nome di dominio DNS che include caratteri internazionali in forma non normalizzata, la configurazione guidata servizi di dominio Active Directory Visualizza un avviso che le query DNS per il nome possono avere esito negativo. Anche se il nome di dominio DNS viene specificato nella pagina di configurazione della distribuzione, l'avviso viene visualizzato nella pagina controllo dei prerequisiti più avanti nella procedura guidata.  
  
Se viene specificato un nome di dominio DNS usando un nome non normalizzato quali. com füßball.com o 'ΣΤ' (le versioni normalizzato: füssball.com e βστα.com), le applicazioni client che tentano di accedervi con WinHTTP normalizzare il nome prima di chiamare l'API di risoluzione dei nomi. Se l'utente digita "'ΣΤ'. com" nella finestra di dialogo di alcuni, la query DNS verrà inviata come "βστα.com" e nessun server DNS verrà corrispondono con un record di risorse per "'ΣΤ'. com". L'utente sarà in grado di risolvere il nome.  
  
L'esempio seguente illustra uno dei problemi che possono verificarsi quando si utilizza un nome IDN non normalizzata:  
  
1.  Il dominio utilizzando un nome di forma non normalizzata viene creato e registrato nel server dns: füßball.com  
  
2.  Macchina "dei criteri di rete" è unito al dominio e il proprio nome registrato: nps.füßball.com  
  
3.  Un'applicazione client tenta di connettersi a nps.füßball.com il server  
  
4.  L'applicazione client tenta di risolvere il nps.füßball.com nome chiamata API risoluzione dei nomi.  
  
5.  A causa di normalizzazione, il nome viene convertito in nps.füssball.com e viene eseguita una query attraverso la rete come nps.füßball.com  
  
6.  L'applicazione client è in grado di risolvere il nome, dato che il nome registrato è nps.füßball.com  
  
Se l'avviso viene visualizzato nella pagina controllo dei prerequisiti nella configurazione guidata servizi di dominio Active Directory, tornare alla pagina di configurazione della distribuzione e specificare un nome di dominio DNS normalizzato. Se si sta installando un nuovo dominio utilizzando Windows PowerShell, specificare un nome DNS normalizzato per l'opzione - DomainName.  
  
Per ulteriori informazioni sui nomi IDN, vedere [gestisce i nomi (Internationalized Domain)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
  

