---
ms.assetid: ba7f2b9f-7351-4680-b7d8-a5f270614f1c
title: Novità nelle procedure di installazione e rimozione di Active Directory Domain Services
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 1f24615491391d932609d7f80549985818ced8c1
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323203"
---
# <a name="whats-new-in-active-directory-domain-services-installation-and-removal"></a>Novità nelle procedure di installazione e rimozione di Active Directory Domain Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La distribuzione di Active Directory Domain Services (AD DS) in Windows Server 2012 è più semplice e veloce rispetto alle versioni precedenti di Windows Server. L'installazione di Servizi di dominio Active Directory è ora basata su Windows PowerShell e integrata con Server Manager. Si è ridotto il numero di passaggi necessari per introdurre i controller di dominio in un ambiente Active Directory esistente. La creazione di un nuovo ambiente Active Directory è dunque diventata più semplice ed efficiente. Il nuovo processo di distribuzione di Servizi di dominio Active Directory riduce al minimo le probabilità di errori che altrimenti potrebbero bloccare l'installazione.  
  
È inoltre possibile installare i file binari del ruolo server Servizi di dominio Active Directory (in altri termini, il ruolo server Servizi di dominio Active Directory) su più server simultaneamente. È possibile eseguire anche la procedura guidata di installazione di Servizi di dominio Active Directory in remoto su un singolo server. Questi miglioramenti forniscono una maggiore flessibilità per la distribuzione di controller di dominio che eseguono Windows Server 2012, in particolare per distribuzioni globali su vasta scala, in cui molti controller di dominio devono essere distribuiti in uffici in aree diverse.  
  
L'installazione di Servizi di dominio Active Directory include le funzionalità seguenti:  
  
- **Integrazione di Adprep.exe nel processo di installazione di Servizi di dominio Active Directory.** I complessi passaggi previsti per la preparazione di Active Directory, ad esempio la necessità di usare una serie di credenziali diverse, copiare i file di Adprep.exe o accedere a controller di dominio specifici, sono stati semplificati o vengono eseguiti in automatico. In tal modo si riduce il tempo necessario per installare Servizi di dominio Active Directory e si riducono le probabilità di errori che potrebbero altrimenti bloccare l'innalzamento di livello di un controller di dominio.  

   Per gli ambienti in cui è preferibile eseguire i comandi adprep.exe prima di installare un nuovo controller di dominio, è ancora possibile eseguire i comandi adprep.exe separatamente dall'installazione di Servizi di dominio Active Directory. La versione di Windows Server 2012 di Adprep. exe viene eseguita in remoto, pertanto è possibile eseguire tutti i comandi necessari da un server che esegue una versione a 64 bit di Windows Server 2008 o versione successiva.  

- **La nuova installazione di Servizi di dominio Active Directory è basata su Windows PowerShell e può essere richiamata da una postazione remota.** La nuova installazione di Servizi di dominio Active Directory è integrata con Server Manager e quindi è possibile installare Servizi di dominio Active Directory usando la stessa interfaccia usata per l'installazione di altri ruoli del server. Per gli utenti di Windows PowerShell, i cmdlet di distribuzione di Servizi di dominio Active Directory assicurano un livello più elevato di funzionalità e flessibilità. Le opzioni di installazione della riga di comando e dell'interfaccia utente grafica (GUI) hanno pari funzionalità.  
- **La nuova installazione di Servizi di dominio Active Directory include la convalida dei prerequisiti.** I potenziali errori vengono rilevati prima dell'inizio dell'installazione. È possibile correggere gli errori prima che si verifichino, senza rischiare di eseguire un aggiornamento parziale. Se ad esempio è necessario eseguire il comando adprep /domainprep, la procedura guidata di installazione verifica che l'utente disponga dei diritti necessari per eseguire l'operazione.  
- **Le pagine di configurazione sono raggruppate in una sequenza che rispecchia i requisiti delle opzioni di promozione più comuni, con le opzioni correlate raggruppate in un minor numero di pagine dell'installazione guidata.** Ciò fornisce un contesto migliore per la scelta delle opzioni di installazione.  
- **È possibile esportare uno script di Windows PowerShell contenente tutte le opzioni specificate durante l'installazione grafica.** Al termine di un'installazione o di una rimozione, è possibile esportare le impostazioni in uno script di Windows PowerShell da usare per l'automazione della stessa operazione.  
- **Prima del riavvio viene eseguita solo la replica della parte critica.** Nuova opzione che consente la replica dei dati non critici prima del riavvio. Per ulteriori informazioni, vedere [Argomenti del cmdlet ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params).  

## <a name="BKMK_ADConfigurationWizard"></a>Configurazione guidata di Active Directory Domain Services

A partire da Windows Server 2012, la configurazione guidata servizi di dominio Active Directory sostituisce il precedente dominio servizi di installazione guidata Active Directory come l'opzione DELL'interfaccia utente per specificare le impostazioni quando si installa un controller di dominio. La Configurazione guidata Servizi di dominio Active Directory viene avviata al termine dell'Aggiunta guidata ruoli.  

> [!WARNING]  
> Il Installazione guidata di Active Directory Domain Services legacy (Dcpromo. exe) è deprecato a partire da Windows Server 2012.  

In [installare i servizi di dominio Active Directory e 40 #; Livello 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md), le procedure dell'interfaccia Utente viene illustrato come avviare Aggiunta guidata ruoli per installare il server di dominio Active Directory di file binari del ruolo e quindi eseguire Active Directory Domain Services configurazione guidata per completare l'installazione di controller di dominio. Gli esempi di Windows PowerShell mostrano come completare entrambi i passaggi utilizzando un cmdlet di distribuzione di Servizi di dominio Active Directory.  
  
## <a name="BKMK_NewAdprep"></a>Integrazione di Adprep. exe

A partire da Windows Server 2012, esiste una sola versione di Adprep. exe (non è disponibile una versione a 32 bit, Adprep32. exe). I comandi adprep vengono eseguiti automaticamente in base alle esigenze quando si installa un controller di dominio che esegue Windows Server 2012 in un dominio o in una foresta Active Directory esistente.  
  
Sebbene le operazioni di adprep siano eseguite automaticamente, è possibile eseguire Adprep.exe anche separatamente. Se ad esempio l'utente che installa Servizi di dominio Active Directory non è un membro del gruppo Enterprise Admins (un prerequisito per poter eseguire Adprep /forestprep), potrebbe essere necessario eseguire il comando separatamente. Tuttavia, è necessario eseguire Adprep. exe solo se si intende aggiornare sul posto il primo controller di dominio Windows Server 2012 (in altre parole, si prevede di aggiornare sul posto il sistema operativo di un controller di dominio che esegue Windows Server 2012).  
  
Adprep. exe si trova nella cartella \support\adprep del disco di installazione di Windows Server 2012. La versione Windows Server 2012 di Adprep è in grado di eseguire in modalità remota.  
  
La versione Windows Server 2012 di Adprep. exe può essere eseguita in qualsiasi server che esegue una versione a 64 bit di Windows Server 2008 o versione successiva. Il server necessita della connettività di rete al master schema per la foresta e al master infrastrutture per il dominio al quale si desidera aggiungere un controller di dominio. Se uno dei due ruoli è ospitato in un server che esegue Windows Server 2003, adprep deve essere eseguito obbligatoriamente in remoto. Il server nel quale viene eseguito adprep non deve essere un controller di dominio: è sufficiente che sia stato aggiunto al dominio o faccia parte di un gruppo di lavoro.  

> [!NOTE]  
> Se si tenta di eseguire la versione di Windows Server 2012 di Adprep. exe in un server che esegue Windows Server 2003, viene visualizzato l'errore seguente:  
>   
> Adprep.exe non è un'applicazione Win32 valida.  

![Novità](media/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal/AdprepNotValid.gif)  

Per informazioni sulla risoluzione degli errori generati da Adprep.exe, vedere la sezione [Problemi noti](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_KnownIssues).  

### <a name="group-membership-check-against-windows-server-2003-operations-master-roles"></a>Verifica di appartenenza a un gruppo e ruoli di master operazioni in Windows Server 2003

Per ogni comando (/forestprep, /domainprep o /rodcprep), Adprep esegue una verifica di appartenenza a un gruppo per stabilire se la credenziale fornita corrisponde o meno a un account in determinati gruppi. Per eseguire questa verifica, Adprep contatta il proprietario del ruolo di master operazioni. Se il master operazioni esegue Windows Server 2003, è necessario specificare i parametri della riga di comando /user e /userdomain nel caso in cui Adprep.exe venga eseguito per assicurare che la verifica di appartenenza a un gruppo avvenga in tutti i casi.  
  
/User e /userdomain sono nuovi parametri di Adprep.exe in Windows Server 2012. Tali parametri specificano rispettivamente il nome account e il dominio utente relativi all'utente che esegue il comando adprep. L'utilità della riga di comando di Adprep.exe blocca l'inserimento di uno dei due parametri, /userdomain e /user, ma omettendo l'altro.  
  
Tuttavia le operazioni di Adprep possono essere eseguite nell'ambito di un'installazione di Servizi di dominio Active Directory utilizzando Windows PowerShell o Server Manager. Tali esperienze hanno in comune con adprep.exe la medesima implementazione sottostante: adprep.dll. Le esperienze Windows PowerShell e Server Manager hanno un inserimento di credenziali diverso, che non impone gli stessi requisiti di adprep.exe. Utilizzando Windows PowerShell o Server Manager è possibile passare ad adprep.dll un valore per /user ma non per /userdomain. Se /user viene specificato ma /userdomain non viene specificato, il dominio del computer locale viene utilizzato per eseguire il controllo. Se il computer non è stato aggiunto a un dominio, l'appartenenza a un gruppo non può essere verificata.  
  
Nel caso in cui non sia possibile verificare l'appartenenza a un gruppo, Adprep visualizzerà un messaggio di attenzione nei file di log di adprep e continuerà:  

```
Adprep was unable to check the specified user's group membership. This could happen if the FSMO role owner <DNS host name of operations master> is running Windows Server 2003 or lower version of Windows.  
```

Se si esegue Adprep.exe senza specificare i parametri /user e /userdomain e il master operazioni esegue Windows Server 2003, Adprep.exe contatterà un controller di dominio nel dominio dell'utente che ha eseguito l'accesso. Se l'utente che ha eseguito l'accesso non è un account di dominio, Adprep.exe non può eseguire la verifica di appartenenza a un gruppo. Inoltre Adprep.exe non può eseguire la verifica di appartenenza a un gruppo se si utilizzano le credenziali della smart card, neppure se si specificano entrambi i parametri /user e /userdomain.  
  
Se Adprep termina senza errori, non sono richieste azioni. Se Adprep non funziona e genera errori di accesso durante l'esecuzione, specificare un account con l'appartenenza corretta. Per ulteriori informazioni, vedere [Credenziali necessarie per eseguire Adprep.exe e installare Servizi di dominio Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
### <a name="syntax-for-adprep-in-windows-server-2012"></a>Sintassi per Adprep in Windows Server 2012

Utilizzare la sintassi seguente per eseguire adprep separatamente dall'installazione di Servizi di dominio Active Directory:  

```
Adprep.exe /forestprep /forest <forest name> /userdomain <user domain name> /user <user name> /password *  
```

Utilizzare /logdsid nel comando per ottenere una registrazione più dettagliata. Il file adprep.log si trova in %windir%\System32\Debug\Adprep\Logs.  

### <a name="running-adprep-using-smartcard"></a>Esecuzione di adprep con smart card

La versione di Windows Server 2012 di Adprep. exe funziona usando la smart card come credenziali, ma non esiste un modo semplice per specificare le credenziali della smart card tramite la riga di comando. Un modo di procedere consiste nell'ottenere le credenziali della smart card tramite il cmdlet Get-Credential di PowerShell. Utilizzare quindi il nome utente dell'oggetto PSCredential restituito, che viene visualizzato nel formato `@@...`. La password è il PIN della smart card.  

In Adprep.exe è necessario specificare /userdomain se è specificato /user. Nel caso delle credenziali della smart card, il parametro /userdomain dovrebbe essere il dominio dell'account utente sottostante, rappresentato dalla smart card.  

### <a name="adprep-domainprep-gpprep-command-is-not-run-automatically"></a>Il comando adprep /domainprep /gpprep non viene eseguito automaticamente

Il comando adprep /domainprep /gpprep non viene eseguito nell'ambito dell'installazione di Servizi di dominio Active Directory. Tale comando imposta le autorizzazioni che sono richieste dalla funzionalità Gruppo di criteri risultante (modalità pianificazione). Per altre informazioni su questo comando, vedere l' [articolo 324392 della Microsoft Knowledge Base](https://support.microsoft.com/kb/324392). Se è necessario che il comando sia eseguito nel dominio Active Directory, è possibile eseguirlo separatamente dall'installazione di Servizi di dominio Active Directory. Se il comando è stato eseguito in precedenza per preparare la distribuzione dei controller di dominio che eseguono Windows Server 2003 SP1 o versione successiva, non è necessario eseguirlo di nuovo.  

È possibile aggiungere in modo sicuro controller di dominio che eseguono Windows Server 2012 a un dominio esistente senza eseguire adprep/domainprep/gpprep, ma la modalità di pianificazione di RSOP non funzionerà correttamente.  

## <a name="BKMK_PrereqCheck"></a>Convalida dei prerequisiti di installazione di servizi di dominio Active Directory

La procedura guidata di installazione di Servizi di dominio Active Directory verifica che i prerequisiti seguenti siano soddisfatti prima di avviare l'installazione. Viene così concessa l'opportunità di risolvere i potenziali problemi che potrebbero ostacolare l'installazione.  
  
I prerequisiti di adprep includono, ad esempio:  

- Verifica delle credenziali per adprep: se è necessario eseguire adprep, la procedura guidata di installazione verifica che l'utente disponga dei diritti necessari per eseguire le operazioni di adprep opportune.  
- Verifica della disponibilità del master schema: se la procedura guidata di installazione rileva che è necessario eseguire adprep /forestprep, verifica che il master schema sia online e, se non lo è, non completa l'operazione.  
- Verifica della disponibilità del master infrastrutture: se la procedura guidata di installazione rileva che è necessario eseguire adprep /domainprep, verifica che il master infrastrutture sia online e, se non lo è, non non completa l'operazione.

Di seguito sono elencate altre verifiche dei prerequisiti che sono state ereditate dalla procedura guidata legacy, Installazione guidata Active Directory (dcpromo.exe):  

- Verifica del nome della foresta: assicura che il nome della foresta è valido e non esiste già.  
- Verifica del nome NetBIOS: assicura che il nome NetBIOS specificato è valido e non causa conflitti con nomi esistenti.  
- Verifica del percorso dei componenti: assicura che i percorsi di database, registri e SYSVOL relativi ad Active Directory sono validi e che lo spazio disponibile sul disco è sufficiente.  
- Verifica del nome del dominio figlio: assicura che il nome del dominio padre e il nome del nuovo dominio figlio sono validi e non causano conflitti con i domini esistenti.  
- Verifica del nome del dominio albero: assicura che il nome dell'albero specificato è valido e non esiste già.  

## <a name="BKMK_SystemReqs"></a>Requisiti di sistema

I requisiti di sistema per Windows Server 2012 sono rimasti invariati rispetto a Windows Server 2008 R2. Per ulteriori informazioni, vedere [requisiti di sistema di Windows Server 2008 R2 con SP1](https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx) (https://www.microsoft.com/windowsserver2008/en/us/system-requirements.aspx).  

Alcune funzionalità potrebbero prevedere requisiti aggiuntivi. Ad esempio, la funzionalità di clonazione del controller di dominio virtuale richiede che l'emulatore PDC esegua Windows Server 2012 e un computer che esegue Windows Server 2012 con il ruolo Hyper-V installato.  

## <a name="BKMK_KnownIssues"></a>Problemi noti

In questa sezione sono elencati alcuni dei problemi che influiscono sull'installazione di Active Directory in Windows Server 2012. Per informazioni su altri problemi noti, vedere [Risoluzione dei problemi relativi alla distribuzione del controller di dominio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  

- Se l'accesso WMI al master schema è bloccato da Windows Firewall quando il comando adprep /forestprep viene eseguito in remoto, l'errore seguente viene registrato nel registro di adprep, in %systemroot%\system32\debug\adprep:  

   ```
   Adprep encountered a Win32 error.
   Error code: 0x6ba Error message: The RPC server is unavailable.  
   ```

   In questo caso è possibile ovviare a questo errore eseguendo direttamente adprep /forestprep sul master schema oppure eseguendo uno dei comandi seguenti per abilitare il traffico WMI attraverso Windows Firewall.

   Per Windows Server 2008 o versione successiva:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=yes
   ```

   Per Windows Server 2003:

   ```
   netsh firewall set service RemoteAdmin enable
   ```

   Al termine dell'esecuzione di adprep, è possibile eseguire uno dei comandi seguenti per bloccare di nuovo il traffico WMI:

   ```
   netsh advfirewall firewall set rule group="windows management instrumentation (wmi)" new enable=no
   ```

   ```
   netsh firewall set service remoteadmin disable
   ```

- È possibile digitare Ctrl + C per annullare il cmdlet Install-ADDSForest. L'annullamento interrompe l'installazione e tutte le modifiche apportate allo stato del server vengono annullate. Dopo l'esecuzione del comando di annullamento, tuttavia, il controllo torna a Windows PowerShell e il cmdlet potrebbe bloccarsi per un periodo di tempo indefinito.  
- **L'installazione di un controller di dominio aggiuntivo tramite le credenziali della smart card non riesce se il server di destinazione non è stato aggiunto al dominio prima dell'installazione.**  

   In questo caso viene restituito il messaggio d'errore seguente:  

   Impossibile connettersi al controller di dominio dell'origine di replica *nome del controller di dominio di origine*. (Eccezione: Accesso non riuscito: Nome utente sconosciuto o password errata)  

   Se si aggiunge il server di destinazione al dominio e in seguito si esegue l'installazione utilizzando una smart card, l'installazione viene completata correttamente.  
  
- **Il modulo ADDSDeployment non viene eseguito con processi a 32 bit.** Se si automatizzano la distribuzione e la configurazione di Windows Server 2012 usando uno script che include un cmdlet ADDSDeployment e qualsiasi altro cmdlet che non supporta i processi a 64 bit nativi, lo script può avere esito negativo con un errore che indica ADDSDeployment Impossibile trovare il cmdlet.  

   In questo caso è necessario eseguire il cmdlet ADDSDeployment separatamente dal cmdlet che non supporta i processi a 64 bit nativi.  

- È disponibile un nuovo file system in Windows Server 2012 denominato Resilient file System. Non archiviare il database di Active Directory, i file di log o SYSVOL su un volume di dati in formato ReFS (Resilient File System). Per altre informazioni su ReFS, vedere il post di blog relativo alla [creazione del file system di prossima generazione per Windows: ReFS](https://blogs.msdn.com/b/b8/archive/2012/01/16/building-the-next-generation-file-system-for-windows-refs.aspx).  
- In Server Manager, i server che eseguono servizi di dominio Active Directory o altri ruoli server in un'installazione Server Core e sono stati aggiornati a Windows Server 2012, il ruolo del server può essere visualizzato con stato rosso, anche se vengono raccolti gli eventi e lo stato come previsto. Possono essere interessati anche i server che eseguono un'installazione dei componenti di base del server di una versione preliminare di Windows Server 2012.  

### <a name="active-directory-domain-services-installation-hangs-if-an-error-prevents-critical-replication"></a>L'installazione di Servizi di dominio Active Directory si blocca se un errore impedisce la replica della parte critica.

Se l'installazione di Servizi di dominio Active Directory rileva un errore durante la fase di replica della parte critica, l'installazione può bloccarsi per un periodo di tempo indefinito. Se, ad esempio, si verificano errori di rete che impediscono il completamento della replica della parte critica, l'installazione non prosegue.  
  
Se si esegue l'installazione con Server Manager, è possibile che una pagina con lo stato di avanzamento dell'installazione rimanga aperta senza che venga visualizzato alcun errore sullo schermo, mentre lo stato di avanzamento resta bloccato per circa 15 minuti. Se si utilizza Windows PowerShell, lo stato di avanzamento visualizzato nella finestra di Windows PowerShell non cambierà per oltre 15 minuti.  
  
Se si verifica questo problema, controllare il file dcpromo.log nella cartella %systemroot%/debug nel server di destinazione. Il file di log indicherà in genere ripetuti errori di replica. Alcune cause note di questo problema sono:  

- I problemi di rete impediscono la replica della parte critica tra il server di destinazione del quale viene innalzato il livello e il controller di dominio dell'origine della replica.  

   Nel file dcpromo.log, ad esempio, è riportato:  

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

   Poiché il processo di installazione ritenta la replica della parte critica per un numero indefinito di volte, l'installazione del controller di dominio prosegue se i sottostanti problemi di rete vengono risolti. Esaminare il problema di rete utilizzando strumenti quali ipconfig, nslookup e netmon in base alle esigenze. Verificare che sia disponibile connettività tra il controller di dominio che si desidera alzare di livello e il partner di replica selezionato durante l'installazione di Servizi di dominio Active Directory. Verificare inoltre il corretto funzionamento della risoluzione dei nomi.  

   I requisiti di installazione di Servizi di dominio Active Directory per la connettività di rete e la risoluzione dei nomi vengono convalidati durante il controllo dei prerequisiti, prima che venga avviata l'installazione. Possono tuttavia verificarsi alcune condizioni di errore successivamente alla convalida dei prerequisiti e prima del completamento dell'installazione, ad esempio se il partner di replica diventa non disponibile durante l'installazione.  

- Durante l'installazione del controller di dominio di replica, nelle credenziali di installazione viene utilizzato l'account amministratore locale del server di destinazione, ma la password dell'account amministratore locale corrisponde alla password di un account del gruppo Domain Admins. In questo caso, è possibile completare l'installazione guidata e iniziare l'installazione prima che si verifichi l'errore "Accesso negato".  

   Nel file dcpromo.log, ad esempio, è riportato:  

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

   Se l'errore è causato dal fatto di aver specificato un account amministratore locale e la relativa password, per correggerlo è necessario reinstallare il sistema operativo, [eseguire la pulizia dei metadati](https://technet.microsoft.com/library/cc816907(WS.10).aspx) dell'account per il controller di dominio che non è stato in grado di completare l'installazione e quindi riprovare l'installazione di Servizi di dominio Active Directory usando credenziali di amministratore di dominio. Il riavvio del server non consente di correggere la condizione di errore, in quanto il server indicherà che Servizi di dominio Active Directory è installato anche se l'installazione non è stata completata.  

### <a name="BKMK_nonnormalDNSNameWarning"></a>La configurazione guidata di Active Directory Domain Services genera un avviso quando viene specificato un nome DNS non normalizzato

Se si crea un nuovo dominio o una nuova foresta e si specifica un nome di dominio DNS che include caratteri internazionali in forma non normalizzata, la Configurazione guidata Servizi di dominio Active Directory avvisa che le query DNS relative al nome potrebbero avere esito negativo. Sebbene il nome di dominio DNS venga specificato nella pagina Configurazione distribuzione, l'avviso viene visualizzato in una pagina successiva della procedura guidata: Controllo dei prerequisiti.  

Se viene specificato un nome di dominio DNS utilizzando un nome non normalizzato ad esempio füßball. com o 'ΣΤ' COM (le versioni normalizzate sono: füssball e βστα.com), le applicazioni client che tentano di accedervi con WinHTTP normalizzeranno il nome prima di chiamare API di risoluzione dei nomi. Se l'utente digita "'ΣΤ'. com" in una finestra di dialogo, la query DNS verrà inviata come "βστα.com" e nessun server DNS troverà corrispondenze con un record di risorse ". com 'ΣΤ'". L'utente non sarà in grado di risolvere il nome.  

L'esempio seguente spiega uno dei problemi che possono verificarsi quando si utilizza un nome IDN non normalizzato:  

1. Il dominio utilizzando un nome non normalizzato viene creato e registrato nel server dns: füßball. com  
2. Macchina "nps" viene aggiunto al dominio e ottiene il nome registrato: nps.füßball.com  
3. Un'applicazione client tenta di connettersi a nps.füßball.com il server  
4. L'applicazione client tenta di risolvere il nome nps.füßball.com chiamata API per la risoluzione nome.  
5. A causa della normalizzazione, il nome viene convertito in füssball e viene eseguita una query tramite la rete come nps.füßball.com  
6. L'applicazione client è in grado di risolvere il nome, poiché il nome registrato è nps.füßball.com  

Se l'avviso viene visualizzato nella pagina Controllo dei prerequisiti della Configurazione guidata Servizi di dominio Active Directory, tornare alla pagina Configurazione distribuzione e specificare un nome di dominio DNS normalizzato. Se si sta installando un nuovo dominio utilizzando Windows PowerShell, specificare un nome DNS normalizzato per l'opzione -DomainName.  

Per ulteriori informazioni sui nomi IDN, vedere [Gestione Internationalized Domain Names (IDN)](https://msdn.microsoft.com/library/windows/desktop/dd318142(v=vs.85).aspx).  
