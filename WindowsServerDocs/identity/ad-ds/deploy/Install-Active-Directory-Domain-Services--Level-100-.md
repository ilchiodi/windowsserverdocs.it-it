---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: Installare servizi di dominio Active Directory (livello 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>Installare servizi di dominio Active Directory (livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene illustrato come installare Active Directory in Windows Server 2012 usando uno dei metodi seguenti:  
  
-   [Credenziali necessarie per eseguire Adprep.exe e installare servizi di dominio Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [L'installazione di servizi di dominio Active Directory utilizzando Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [L'installazione di servizi di dominio Active Directory tramite Server Manager](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Installazione gestita controller di dominio tramite l'interfaccia utente grafica](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Credenziali necessarie per eseguire Adprep.exe e installare servizi di dominio Active Directory  
Le credenziali seguenti sono necessari per eseguire Adprep.exe e installare Active Directory.  
  
-   Per installare una nuova foresta, è necessario essere connessi come amministratore locale per il computer.  
  
-   Per installare un nuovo dominio figlio o un nuovo albero di dominio, è necessario essere connessi come membro del gruppo Enterprise Admins.  
  
-   Per installare un controller di dominio aggiuntivo in un dominio esistente, è necessario essere un membro del gruppo Domain Admins.  
  
    > [!NOTE]  
    > Se non si esegue il comando adprep.exe separatamente e si installa il primo controller di dominio che esegue Windows Server 2012 in una foresta o dominio esistente, verrà richiesto di fornire le credenziali per eseguire i comandi Adprep. Le credenziali necessarie sono i seguenti:  
    >   
    > -   Per introdurre il primo controller di dominio di Windows Server 2012 nella foresta, è necessario fornire le credenziali per un membro del gruppo Enterprise Admins, gruppo Schema Admins e Domain Admins gruppo nel dominio che ospita il master schema.  
    > -   Per introdurre il primo controller di dominio di Windows Server 2012 in un dominio, è necessario fornire le credenziali per un membro del gruppo Domain Admins.  
    > -   Per introdurre il primo controller di dominio di sola lettura (RODC) nella foresta, è necessario fornire le credenziali per un membro del gruppo Enterprise Admins.  
    >   
    >     > [!NOTE]  
    >     > Se è già stato eseguito adprep /rodcprep in Windows Server 2008 o Windows Server 2008 R2, non è necessario eseguirlo di nuovo per Windows Server 2012.  
  
## <a name="BKMK_PS"></a>L'installazione di servizi di dominio Active Directory utilizzando Windows PowerShell  
A partire da Windows Server 2012, è possibile installare Active Directory utilizzando Windows PowerShell. Dcpromo.exe è deprecato a partire da Windows Server 2012, ma è comunque possibile eseguire dcpromo.exe utilizzando un file di risposta (dcpromo /unattend:<answerfile> o dcpromo /answer:<answerfile>). La possibilità di continuare a eseguire dcpromo.exe con un file di risposte offre alle organizzazioni che hanno investito in fase di automazione esistente per convertire l'automazione da dcpromo.exe a Windows PowerShell risorse. For more information about running dcpromo.exe with an answer file, see [https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034).  
  
Per ulteriori informazioni sulla rimozione di dominio Active Directory mediante Windows PowerShell, vedere [Rimuovi dominio Active Directory mediante Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Iniziare con l'aggiunta del ruolo utilizzando Windows PowerShell. Questo comando installa il ruolo server Servizi di dominio Active Directory e installa i servizi di dominio Active Directory e AD LDS strumenti di amministrazione server, inclusi strumenti basati sull'interfaccia utente grafica, ad esempio Active Directory Users and Computers e strumenti da riga di comando come dcdia.exe. Strumenti di amministrazione server non vengono installati per impostazione predefinita quando si usa Windows PowerShell. You need to specify **"IncludeManagementTools** to manage the local server or install [Remote Server Administration Tools](https://www.microsoft.com/download/details.aspx?id=28972) to manage a remote server.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
Non esiste alcun riavvio fino al termine dell'installazione di servizi di dominio Active Directory.  
  
È quindi possibile eseguire questo comando per visualizzare i cmdlet disponibili nel modulo ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Per visualizzare l'elenco di argomenti che è possibile specificare per un cmdlet e la sintassi:  
  
```  
Get-Help <cmdlet name>  
```  
  
Ad esempio, per visualizzare gli argomenti per la creazione di un account (RODC) controller di dominio di sola lettura non occupato, digitare  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Gli argomenti opzionali appaiono tra parentesi quadre.  
  
È anche possibile scaricare la Guida esempi e concetti per i cmdlet di Windows PowerShell più recenti. For more information, see [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx).  
  
È possibile eseguire i cmdlet di Windows PowerShell sui server remoti:  
  
-   In Windows PowerShell, utilizzare Invoke-Command con il cmdlet ADDSDeployment. Ad esempio, per installare servizi di dominio Active Directory in un server remoto denominato ConDC3 nel dominio contoso.com, digitare:  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
- o -  
  
-   In Server Manager, creare un gruppo di server che includa il server remoto. Fare doppio clic il nome del server remoto e fare clic su **Windows PowerShell**.  
  
Nelle sezioni successive illustrano come eseguire i cmdlet del modulo ADDSDeployment per installare Active Directory.  
  
-   [Argomenti del cmdlet ADDSDeployment](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Specificare le credenziali di Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [Utilizzando i cmdlet di prova](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [Installa un dominio radice della nuova foresta con Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [L'installazione di un nuovo dominio figlio o albero con Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Installazione di un controller di dominio aggiuntivo (replica) con Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>Argomenti del cmdlet ADDSDeployment  
Nella tabella seguente elenca gli argomenti dei cmdlet di ADDSDeployment in Windows PowerShell. Argomenti in grassetto sono obbligatori. Gli argomenti equivalenti di dcpromo.exe sono riportati tra parentesi se hanno un nome diverso in Windows PowerShell.  
  
Parametri di Windows PowerShell di accettare argomenti $TRUE o $FALSE. Non è necessario specificare gli argomenti di $TRUE per impostazione predefinita.  
  
Per sostituire i valori predefiniti, è possibile specificare l'argomento con un valore $False. Ad esempio, poiché **- installdns** viene eseguita automaticamente per un'installazione nuova foresta se non è specificato, l'unico modo per *impedire* installazione DNS quando si installa una nuova foresta è possibile utilizzare:  
  
```  
-InstallDNS:$false  
```  
  
Analogamente, poiché **"installdns** ha un valore predefinito di $False se si installa un controller di dominio in un ambiente che non ospita il Server Windows DNS server, è necessario specificare l'argomento seguente per installare il server DNS:  
  
```  
-InstallDNS:$true  
```  
  
|Argomento|Descrizione|  
|------------|---------------|  
|**ADPrepCredential <PS Credential> ** **Nota:** necessari se si sta installando il primo controller di dominio di Windows Server 2012 in un dominio o foresta e le credenziali dell'utente corrente sono insufficienti per eseguire l'operazione.|Specifies the account with Enterprise Admins and Schema Admins group membership that can prepare the forest, according to the rules of [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) and a PSCredential object.<br /><br />Se viene specificato alcun valore, il valore di **"credential** viene utilizzato l'argomento.|  
|AllowDomainControllerReinstall|Specifica se continuare l'installazione di questo controller di dominio scrivibile, nonostante il fatto che viene rilevato un altro account di controller di dominio scrivibile con lo stesso nome.<br /><br />Utilizzare **$True** solo se si è certi che l'account non è attualmente utilizzato da un altro controller di dominio scrivibile.<br /><br />Il valore predefinito è **$False**.<br /><br />Questo argomento non è valido per un RODC.|  
|AllowDomainReinstall|Consente di specificare se ricreare un dominio esistente.<br /><br />Il valore predefinito è **$False**.|  
|AllowPasswordReplicationAccountName < string [] >|Specifica i nomi di account utente, account di gruppo e gli account computer le cui password possono essere replicate in questo controller di dominio. Utilizzare una stringa vuota "" Se si desidera mantenere vuoto il valore. Per impostazione predefinita, è consentito solo il consentito replica passw e viene originariamente creato vuoto.<br /><br />Specificare i valori come matrice di stringhe. Per esempio:<br /><br />Codice - AllowPasswordReplicationAccountName "JSmith", "JSmithPC", "Utenti delle succursali"|  
|< String [] > ApplicationPartitionsToReplicate **Nota:** non esiste alcuna opzione equivalente nell'interfaccia utente. Se si installa tramite l'interfaccia utente o tramite installazione da supporto, verranno replicate tutte le partizioni applicative.|Specifica le partizioni di directory applicative da replicare. Questo argomento viene applicato solo quando si specifica il **- InstallationMediaPath** argomento per l'installazione da supporto. Per impostazione predefinita, tutte le applicazioni verranno replicate le partizioni dipende dai rispettivi ambiti.<br /><br />Specificare i valori come matrice di stringhe. Per esempio:<br /><br />Codice-<br /><br />-ApplicationPartitionsToReplicate "Partizione1", "Partizione2", "partition3"|  
|Conferma|Chiede conferma prima di eseguire il cmdlet.|  
|CreateDnsDelegation **Nota:** non è possibile specificare questo argomento quando si esegue il cmdlet Add-ADDSReadOnlyDomainController.|Indica se creare una delega DNS che fa riferimento al nuovo server DNS che si sta installando insieme a controller di dominio. Valido per l'integrazione Active Directory"DNS solo. Record di delega possono essere creati solo nei server Microsoft DNS che siano online e accessibile. Non è possibile creare record di delega per domini che siano immediatamente subordinati a domini di primo livello quali. com, gov,. Biz,. edu o domini del codice paese a due lettere quali Nz e. au.<br /><br />Il valore predefinito viene calcolato automaticamente in base all'ambiente.|  
|**Credenziali <PS Credential> ** **Nota:** obbligatorio solo se le credenziali dell'utente corrente sono insufficienti per eseguire l'operazione.|Specifies the domain account that can logon to the domain, according to the rules of [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) and a PSCredential object.<br /><br />Se viene specificato alcun valore, vengono utilizzate le credenziali dell'utente corrente.|  
|CriticalReplicationOnly|Specifica se l'operazione di installazione di servizi di dominio Active Directory esegue solo la parte critica della replica prima del riavvio e continua quindi. La replica della parte non critica si verifica al termine dell'installazione e il riavvio del computer.<br /><br />Uso di questo argomento non è consigliata.<br /><br />È disponibile un equivalente per questa opzione nell'interfaccia utente (UI).|  
|DatabasePath <string>|Specifica il percorso completo, non "percorso Universal Naming Convention (UNC) in una directory su un disco rigido del computer locale che contiene il database del dominio, ad esempio, **C:\Windows\NTDS.**<br /><br />Il valore predefinito è **%SYSTEMROOT%\NTDS**. **Importante:** mentre è possibile archiviare i file di registro e database di Active Directory su volume formattato con Resilient File System (ReFS), non offre vantaggi specifici per l'hosting di dominio Active Directory in ReFS, diverso da normale vantaggi di flessibilità si ottiene per l'hosting di dati in ReFS.|  
|DelegatedAdministratorAccountName <string>|Specifica il nome dell'utente o gruppo che installerà e amministrerà il controller di dominio.<br /><br />Per impostazione predefinita, solo i membri del gruppo Domain Admins possono amministrare un RODC.|  
|DenyPasswordReplicationAccountName < string [] >|Specifica i nomi di account utente, account di gruppo e gli account computer le cui password non devono essere replicate in questo controller di dominio. Utilizzare una stringa vuota "" Se non si desidera consentire la replica delle credenziali di qualunque utente o computer. Per impostazione predefinita, gli amministratori, Server Operators, Backup Operators, Account Operators e il gruppo di replica Password RODC negato viene negato. Per impostazione predefinita, il gruppo di replica Password RODC negato include Cert Publishers, Domain Admins, Enterprise Admins, controller di dominio organizzazione, controller di dominio di sola lettura organizzazione, Group Policy Creator Owners, l'account krbtgt e Schema Admins.<br /><br />Specificare i valori come matrice di stringhe. Per esempio:<br /><br />Codice-<br /><br />-DenyPasswordReplicationAccountName "RegionalAdmins", "AdminPCs"|  
|DnsDelegationCredential <PS Credential> **Nota:** non è possibile specificare questo argomento quando si esegue il cmdlet Add-ADDSReadOnlyDomainController.|Specifies the user name and password for creating DNS delegation, according to the rules of [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) and a PSCredential object.|  
|DomainMode <DomainMode> {Win2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />O<br /><br />DomainMode <DomainMode> {124 # & 2; 3 & #124; 4 & #124; 5 & #124; 6}|Specifica il livello di funzionalità del dominio durante la creazione di un nuovo dominio.<br /><br />Il livello di funzionalità del dominio non può essere inferiore rispetto al livello funzionale di foresta, ma può essere superiore.<br /><br />Il valore predefinito viene calcolato automaticamente e impostare il livello di funzionalità foresta esistente o il valore impostato per **- ForestMode**.|  
|**DomainName**<br /><br />Obbligatorio per il cmdlet Install-ADDSForest e Install-ADDSDomainController.|Specifica il nome di dominio completo del dominio in cui si desidera installare un controller di dominio.|  
|**DomainNetbiosName <string>**<br /><br />Obbligatorio per Install-ADDSForest se il nome prefisso del nome di dominio completo è più di 15 caratteri.|Utilizzare con Install-ADDSForest. Assegna un nome NetBIOS per il dominio radice della nuova foresta.|  
|DomainType <DomainType> {ChildDomain & #124; TreeDomain} o {figlio & #124; albero}|Indica il tipo di dominio che si desidera creare: un nuovo albero di dominio in un oggetto esistente dell'insieme di strutture, un elemento figlio di un dominio esistente o una nuova foresta.<br /><br />Il valore predefinito di DomainType è ChildDomain.|  
|Force|Questo parametro quando si specifica tutti gli avvisi che potrebbero essere visualizzati durante l'installazione e l'aggiunta del controller di dominio verranno bloccati per consentire al cmdlet di completamento dell'esecuzione. Questo parametro può essere utile includere uno script di installazione.|  
|ForestMode <ForestMode> {Win2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />O<br /><br />ForestMode <ForestMode> {124 # & 2; 3 & #124; 4 & #124; 5 & #124; 6}|Specifica il livello di funzionalità quando si crea una nuova foresta.<br /><br />Il valore predefinito è Win2012.|  
|InstallationMediaPath|Indica il percorso del supporto di installazione che verrà utilizzato per installare un nuovo controller di dominio.|  
|InstallDns|Specifica se il servizio Server DNS deve essere installato e configurato nel controller di dominio.<br /><br />Per una nuova foresta, il valore predefinito è **$True** e Server DNS viene installato.<br /><br />Per un nuovo dominio figlio o albero di dominio, se il dominio padre (o dominio radice della foresta per un albero di dominio) sono già ospitati e archiviati i nomi DNS per il dominio, il valore predefinito per questo parametro è $True.<br /><br />Per un'installazione di controller di dominio in un dominio esistente, se questo parametro è specificato e il dominio corrente sono già ospitati e archiviati i nomi DNS per il dominio, il valore predefinito per questo parametro è **$True**. In caso contrario, se i nomi di dominio DNS sono ospitati all'esterno di Active Directory, il valore predefinito è **$False** e non viene installato alcun Server DNS.|  
|LogPath <string>|Specifica il percorso completo non UNC di una directory su un disco rigido del computer locale contenente i file di registro del dominio, ad esempio, **C:\Windows\Logs**.<br /><br />Il valore predefinito è **%SYSTEMROOT%\NTDS**. **Importante:** non archiviare i file di log di Active Directory in un volume di dati formattato con Resilient File System (ReFS).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Specifica se trasferire il master operazioni master infrastrutture (flexible single master operation FSMO, Flexible) al controller di dominio che si sta creando "nel caso è attualmente ospitato in un server di catalogo globale" e non si intende rendere il controller di dominio che si sta creando un server di catalogo globale. Specificare questo parametro per trasferire il ruolo di master infrastrutture al controller di dominio che si sta creando nel caso in cui il trasferimento sia necessario; In questo caso, specificare il **NoGlobalCatalog** opzione se si desidera che il ruolo di master infrastrutture deve rimanere in cui è attualmente.|  
|**NewDomainName <string> ** **Nota:** obbligatorio solo per Install-ADDSDomain.|Specifica il nome di dominio singolo per il nuovo dominio.<br /><br />Ad esempio, se si desidera creare un nuovo dominio figlio denominato **emea.corp.fabrikam.com**, è necessario specificare **emea** come valore di questo argomento.|  
|**NewDomainNetbiosName <string>**<br /><br />Obbligatorio per Install-ADDSDomain se il nome prefisso del nome di dominio completo è più di 15 caratteri.|Utilizzare con Install-ADDSDomain. Assegna un nome NetBIOS al nuovo dominio. Il valore predefinito è derivato dal valore di **"NewDomainName**.|  
|NoDnsOnNetwork|Specifica che il servizio DNS non è disponibile nella rete. Questo parametro viene utilizzato solo quando l'impostazione IP della scheda di rete per il computer non è configurato con il nome di un server DNS per la risoluzione dei nomi. Indica che un server DNS verrà installato nel computer per la risoluzione dei nomi. In caso contrario, le impostazioni IP della scheda di rete devono prima essere configurate con l'indirizzo di un server DNS.<br /><br />Se si omette questo parametro (impostazione predefinita) indica che le impostazioni del client TCP/IP della scheda di rete nel computer server da utilizzare per contattare un server DNS. Pertanto, se non si specifica questo parametro, assicurarsi che le impostazioni del client TCP/IP siano configurate con un indirizzo server DNS preferito.|  
|NoGlobalCatalog|Specifica che non si desidera il controller di dominio a un server di catalogo globale.<br /><br />Per impostazione predefinita, i controller di dominio che eseguono Windows Server 2012 vengono installati con il catalogo globale. In altre parole, questo viene eseguito automaticamente senza calcolo, a meno che non si specifica:<br /><br />Codice-<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Specifica se il riavvio del computer dopo il completamento del comando, indipendentemente dall'esito positivo. Per impostazione predefinita, il computer verrà riavviato. Per impedire il riavvio di server, specificare:<br /><br />Codice-<br /><br />-NoRebootOnCompletion: $True<br /><br />È disponibile un equivalente per questa opzione nell'interfaccia utente (UI).|  
|**ParentDomainName <string> ** **Nota:** necessari per il cmdlet Install-ADDSDomain|Specifica il nome di dominio completo di un dominio padre esistente. Utilizzare questo argomento quando si installa un dominio figlio o un nuovo albero di dominio.<br /><br />Ad esempio, se si desidera creare un nuovo dominio figlio denominato **emea.corp.fabrikam.com**, è necessario specificare **corp.fabrikam.com** come valore di questo argomento.|  
|ReadOnlyReplica|Specifica se installare un controller di dominio di sola lettura (RODC).|  
|/Replicationsourcedc <string>|Indica il nome di dominio completo del controller di dominio partner da cui vengono replicati i dati del dominio. Il valore predefinito viene calcolato automaticamente.|  
|**SafeModeAdministratorPassword <securestring>**|Fornisce la password per l'account amministratore quando il computer viene avviato in modalità provvisoria o in una variante della modalità provvisoria, ad esempio modalità ripristino servizi Directory.<br /><br />Il valore predefinito è una password vuota. È necessario specificare una password. La password deve essere specificata in un formato SecureString, ad esempio restituito da read-host - assecurestring o ConvertTo-SecureString.<br /><br />Operazione dell'argomento SafeModeAdministratorPassword è particolare: se non viene specificato come argomento, il cmdlet chiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo. Se specificato senza un valore e non vi sono altri argomenti non specificato per il cmdlet, il cmdlet richiede di immettere una password nascosta senza conferma. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo. Se è specificato con un valore, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo. Ad esempio, è possibile manualmente richiedere una password tramite il cmdlet Read-Host per richiedere all'utente una stringa sicura:-safemodeadministratorpassword (read-host - prompt "Password:" - assecurestring) è anche possibile fornire una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato. -safemodeadministratorpassword (convertto-securestring "Password1" - asplaintext-force)|  
|**Nome di sito <string>**<br /><br />Necessario per il cmdlet Add-addsreadonlydomaincontrolleraccount|Specifica il sito in cui verrà installato il controller di dominio. Non esiste alcun **"nomesito** argomento quando si esegue **Install-ADDSForest** perché il primo sito creato è nome-predefinito-primo-sito.<br /><br />Il nome del sito deve esistere già quando viene fornito come argomento al **- sitename**. Il cmdlet non crea il sito.|  
|SkipAutoConfigureDNS|Ignora configurazione automatica di impostazioni del client DNS, server d'inoltro e parametri radice. Questo argomento si applica solo se il servizio Server DNS è già installato o installato automaticamente con **- InstallDNS**.|  
|SystemKey <string>|Specifica la chiave di sistema per il supporto da cui vengono replicati i dati.<br /><br />Il valore predefinito è **Nessuno**.<br /><br />Dati devono essere nel formato restituito da read-host - assecurestring o ConvertTo-SecureString.|  
|SysvolPath <string>|Specifica il percorso completo non UNC di una directory su un disco rigido del computer locale, ad esempio, **C:\Windows\SYSVOL**.<br /><br />Il valore predefinito è **%SYSTEMROOT%\SYSVOL**. **Importante:** SYSVOL non può essere archiviata in un volume di dati formattato con Resilient File System (ReFS).|  
|SkipPreChecks|Non esegue controlli dei prerequisiti prima di avviare l'installazione. Non è consigliabile utilizzare questa impostazione.|  
|WhatIf|Mostra che cosa accadrebbe se viene eseguito il cmdlet. Il cmdlet non viene eseguito.|  
  
### <a name="BKMK_PSCreds"></a>Specificare le credenziali di Windows PowerShell  
You can specify credentials without revealing them in plain text on screen by using [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
L'operazione per - SafeModeAdministratorPassword e LocalAdministratorPassword argomenti è particolare:  
  
-   Se non è specificato come argomento, il cmdlet richiede di immettere e confermare una password nascosta. Questo è il metodo di utilizzo quando esegue il cmdlet in modo interattivo.  
  
-   Se è specificato con un valore, il valore deve essere una stringa sicura. Questo non è il metodo di utilizzo quando si esegue il cmdlet in modo interattivo.  
  
Ad esempio, è possibile manualmente richiedere una password utilizzando la **Read-Host** cmdlet per richiedere all'utente una stringa sicura  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Poiché l'opzione precedente non chiede conferma la password, utilizzala con estrema cautela: la password non è visibile.  
  
È anche possibile specificare una stringa sicura come variabile di testo in chiaro convertita, anche se questo metodo è sconsigliato:  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> Non è consigliabile inserire o archiviare una password di testo non crittografato. Chiunque esegua questo comando in uno script o ti spii a conoscenza della password modalità ripristino servizi directory del controller di dominio. Conoscendo possono rappresentare il controller di dominio e innalzare i propri privilegi al massimo livello in una foresta di Active Directory.  
  
### <a name="BKMK_TestCmdlets"></a>Utilizzando i cmdlet di prova  
Ogni cmdlet di ADDSDeployment esiste un corrispondente cmdlet di prova. Il cmdlet di prova eseguono solo i controlli dei prerequisiti per l'operazione di installazione. le impostazioni di installazione non sono configurate. Gli argomenti per ogni cmdlet di prova sono uguali a quelli corrispondenti cmdlet di installazione, ma **"SkipPreChecks** non è disponibile per i cmdlet di prova.  
  
|Cmdlet di prova|Descrizione|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Esegue il controllo dei prerequisiti per l'installazione di una nuova foresta di Active Directory.|  
|Test-ADDSDomainInstallation|Esegue il controllo dei prerequisiti per l'installazione di un nuovo dominio in Active Directory.|  
|Test-ADDSDomainControllerInstallation|Esegue il controllo dei prerequisiti per l'installazione di un controller di dominio in Active Directory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Esegue il controllo dei prerequisiti per l'aggiunta di un controller di dominio di sola lettura account (RODC).|  
  
### <a name="BKMK_PSForest"></a>Installa un dominio radice della nuova foresta con Windows PowerShell  
La sintassi del comando per l'installazione di una nuova foresta è come segue. Gli argomenti opzionali appaiono all'interno di parentesi quadre.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> L'argomento - DomainNetBIOSName è obbligatorio se si desidera modificare il nome di 15 caratteri generato automaticamente in base a prefisso del nome di dominio DNS o se il nome eccede i 15 caratteri.  
  
Ad esempio, per installare una nuova foresta denominata corp.contoso.com e in modo sicuro richiesto di specificare la password modalità ripristino servizi directory, digitare:  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> Server DNS viene installato per impostazione predefinita quando si esegue Install-ADDSForest.  
  
Per installare una nuova foresta denominata corp.contoso.com, creare una delega DNS nel dominio contoso.com, impostare il livello funzionale di dominio a Windows Server 2008 R2 e impostare il livello funzionale di foresta a Windows Server 2008, installare il database di Active Directory e SYSVOL sull'unità D:\, installare i file di log sull'unità E:\ ed essere richiesto di specificare la password modalità ripristino servizi Directory e il tipo :  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>L'installazione di un nuovo dominio figlio o albero con Windows PowerShell  
La sintassi del comando per l'installazione di un nuovo dominio è come segue. Gli argomenti opzionali appaiono all'interno di parentesi quadre.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> Il **-credential** argomento è solo necessario quando non attualmente connessi al computer come membro del gruppo Enterprise Admins.  
>   
> Il **- NewDomainNetBIOSName** argomento è obbligatorio se si desidera modificare il nome di 15 caratteri generato automaticamente in base a prefisso del nome di dominio DNS o se il nome eccede i 15 caratteri.  
  
Ad esempio, per usare le credenziali di corp\EnterpriseAdmin1 per creare un nuovo dominio figlio denominato child.corp.contoso.com, installare il server DNS, creare una delega DNS nel dominio corp.contoso.com, impostare il livello funzionale di dominio a Windows Server 2003, rendere il controller di dominio di un server di catalogo globale in un sito denominato Houston, utilizzare DC1.corp.contoso.com come controller di dominio di origine di replica, installare il database di Active Directory e SYSVOL sull'unità D:\ , installare i file di log sull'unità E:\ e richiesto di specificare la password modalità ripristino servizi Directory ma non viene richiesto di confermare il comando, digitare:  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Installazione di un controller di dominio aggiuntivo (replica) con Windows PowerShell  
La sintassi del comando per l'installazione di un controller di dominio aggiuntivo è come segue. Gli argomenti opzionali appaiono all'interno di parentesi quadre.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Per installare un controller di dominio e server DNS nel dominio corp.contoso.com e venga richiesto di fornire le credenziali di amministratore di dominio e la password modalità ripristino servizi directory, digitare:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Se il computer è già aggiunti a un dominio e si è un membro del gruppo Domain Admins, è possibile utilizzare:  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Per visualizzare la richiesta per il nome di dominio, digitare:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
Il comando seguente utilizzerà le credenziali di Contoso\EnterpriseAdmin1 per installare un controller di dominio scrivibile e un server di catalogo globale in un sito denominato Boston, installare il server DNS, creare una delega DNS nel dominio contoso.com, installare dal supporto archiviato nella cartella c:\ADDS IFM, installare il database di Active Directory e SYSVOL sull'unità D:\, installare i file di log sull'unità E:\ , hanno il server automaticamente riavviato dopo l'installazione di Active Directory è stata completata e richiesto di specificare la password modalità ripristino servizi Directory:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Eseguire un'installazione RODC gestita utilizzando Windows PowerShell  
La sintassi del comando per creare un account di controller di dominio è come segue. Gli argomenti opzionali appaiono all'interno di parentesi quadre.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
La sintassi del comando per associare un server a un account di controller di dominio è come segue. Gli argomenti opzionali appaiono all'interno di parentesi quadre.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Ad esempio, per creare un account di sola lettura denominato RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
Eseguire quindi i comandi seguenti nel server che si desidera collegare all'account RODC1. Il server non può essere aggiunti al dominio. Prima di tutto, installare gli strumenti di gestione e del ruolo server di dominio Active Directory:  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
L'esecuzione, il comando seguente per creare il controller di dominio:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Premere **Y** per confermare o includere il **"conferma** argomento per evitare la richiesta di conferma.  
  
## <a name="BKMK_GUI"></a>L'installazione di servizi di dominio Active Directory tramite Server Manager  
Servizi di dominio Active Directory può essere installato in Windows Server 2012 tramite l'aggiunta guidata ruoli in Server Manager, seguita da Active Directory dominio servizi di configurazione guidata, ovvero nuova funzionalità introdotta in Windows Server 2012. Il dominio servizi di installazione guidata Active Directory (dcpromo.exe) è deprecato a partire da Windows Server 2012.  
  
Le sezioni seguenti illustrano come creare pool di server per installare e gestire servizi di dominio Active Directory in più server e come usare le procedure guidate per installare Active Directory.  
  
### <a name="BKMK_ServerPools"></a>Creazione di pool di server  
Server Manager è possibile creare pool altri server sulla rete, purché siano accessibili dal computer che esegue Server Manager. Una volta pool, scegliere i server per l'installazione remota di servizi di dominio Active Directory o di altre opzioni di configurazione disponibili in Server Manager. Il computer che esegue Server Manager automaticamente inserito nel pool. For more information about server pools, see [Add Servers to Server Manager](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Per gestire un computer aggiunto al dominio utilizzando Server Manager in un server del gruppo di lavoro, o viceversa, sono necessari ulteriori passaggi di configurazione. For more information, see "Add and manage servers in workgroups" in [Add Servers to Server Manager](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>L'installazione di servizi di dominio Active Directory  
**Credenziali amministrative**  
  
Le credenziali necessarie per installare Active Directory variano a seconda della configurazione di distribuzione scelto. Per ulteriori informazioni, vedere [credenziali necessarie per eseguire Adprep.exe e installare servizi di dominio Active Directory](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Utilizzare le procedure seguenti per installare Active Directory utilizzando il metodo di interfaccia utente grafica. La procedura può essere eseguita in locale o in remoto. Per ulteriori informazioni su questi passaggi, vedere gli argomenti seguenti:  
  
-   [Distribuzione di una foresta con Server Manager](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Installare un nuovo Windows Server 2012 Active Directory figlio o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Installare un Controller di dominio di sola lettura di Active Directory di Server 2012 Windows & #40; RODC & #41; & #40; Livello 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Per installare servizi di dominio Active Directory tramite Server Manager  
  
1.  In Server Manager, fare clic su **Gestisci** e fare clic su **Aggiungi ruoli e funzionalità** per avviare l'aggiunta guidata ruoli.  
  
2.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
3.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.  
  
4.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, fare clic sul nome del server in cui si desidera installare Active Directory e quindi fare clic su **Avanti**.  
  
    Per selezionare i server remoti, creare un pool di server e aggiungere i server remoti. For more information about creating server pools, see [Add Servers to Server Manager](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  Nel **Selezione ruoli server** pagina, fare clic su **servizi di dominio Active Directory**, quindi nel **Aggiunta guidata ruoli e funzionalità** la finestra di dialogo, fare clic su **Aggiungi funzionalità**e quindi fare clic su **Avanti**.  
  
6.  Nel **selezionare le funzionalità** pagina, selezionare le funzionalità aggiuntive che si desidera installare e fare clic su **Avanti**.  
  
7.  Nel **servizi di dominio Active Directory** pagina, esaminare le informazioni e quindi fare clic su **Avanti**.  
  
8.  Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
9. Nel **risultati** pagina, verificare che l'installazione ha avuto esito positivo, fare clic su **promuovere il server a un controller di dominio** per avviare la configurazione guidata servizi di dominio Active Directory.  
  
    ![Installare Active Directory](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Se si chiude l'aggiunta guidata ruoli a questo punto senza avviare la configurazione guidata servizi di dominio Active Directory, è possibile riavviarlo facendo clic su attività in Server Manager.  
  
    ![Installare Active Directory](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. Nel **configurazione distribuzione** pagina, scegliere una delle opzioni seguenti:  
  
    -   Se si installa un controller di dominio aggiuntivo in un dominio esistente, fare clic su **aggiungere un controller di dominio a un dominio esistente**e digitare il nome del dominio (ad esempio, emea.corp.contoso.com) o fare clic su **Seleziona... ** per scegliere un dominio e credenziali (ad esempio, specificare un account che sia un membro del gruppo Domain Admins) e quindi fare clic su **Avanti**.  
  
        > [!NOTE]  
        > Il nome del dominio e credenziali dell'utente corrente vengono forniti per impostazione predefinita solo se il computer è aggiunto al dominio e si sta eseguendo un'installazione locale. Se si installa Servizi di dominio Active Directory in un server remoto, è necessario specificare le credenziali, per impostazione predefinita. Se le credenziali dell'utente corrente non sono sufficienti per eseguire l'installazione, fare clic su **modifica... ** per specificare credenziali diverse.  
  
        Per ulteriori informazioni, vedere [installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41; ](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Se si sta installando un nuovo dominio figlio, fare clic su **aggiungere un nuovo dominio a una foresta esistente**, per **Seleziona tipo dominio**selezionare **dominio figlio**, digitare o selezionare il nome del nome DNS del dominio padre (ad esempio, corp.contoso.com), digitare il nome del nuovo dominio figlio (ad esempio emea), digitare le credenziali da utilizzare per creare il nuovo dominio, quindi fare clic su relativo **Avanti**.  
  
        Per ulteriori informazioni, vedere [installare un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Se si sta installando un nuovo albero di dominio, fare clic su **Aggiungi nuovo dominio a una foresta esistente**, per **Seleziona tipo dominio**, scegli **dominio albero**, digitare il nome del dominio radice (ad esempio, corp.contoso.com), digitare il nome DNS del nuovo dominio (ad esempio, fabrikam.com), digitare le credenziali da utilizzare per creare il nuovo dominio, quindi fare clic su **Avanti**.  
  
        Per ulteriori informazioni, vedere [installare un nuovo Windows Server 2012 figlio di Active Directory o dominio albero & #40; Livello 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Se si installa una nuova foresta, fare clic su **aggiungere una nuova foresta** e quindi digitare il nome del dominio radice (ad esempio, corp.contoso.com).  
  
        Per ulteriori informazioni, vedere [installare una nuova foresta Windows Server 2012 Active Directory & #40; Livello 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. Nel **opzioni Controller di dominio** pagina, scegliere una delle opzioni seguenti:  
  
    -   Se si sta creando una nuova foresta o dominio, selezionare i livelli di funzionalità foresta e del dominio, fare clic su **server del sistema DNS (Domain Name)**, specificare la password DSRM e quindi fare clic su **Avanti**.  
  
    -   Se si aggiunge un controller di dominio a un dominio esistente, fare clic su **server del sistema DNS (Domain Name)**, **catalogo globale (GC)**, o **lettura solo Controller di dominio (RODC)** in base alle esigenze, scegliere il nome del sito, digitare la password DSRM e quindi fare clic su **Avanti**.  
  
    Per ulteriori informazioni sulle opzioni in questa pagina disponibili o meno in condizioni diverse, vedere [opzioni Controller di dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. Nel **opzioni DNS** pagina (visualizzata solo se si installa un server DNS), fare clic su **Aggiorna delega DNS** in base alle esigenze. In caso contrario, fornire le credenziali che dispongono dell'autorizzazione per creare record di delega DNS nella zona DNS padre.  
  
    Se non è possibile contattare un server DNS che ospita la zona padre, il **Aggiorna delega DNS** opzione non è disponibile.  
  
    For more information about whether you need to update the DNS delegation, see [Understanding Zone Delegation](https://technet.microsoft.com/library/cc771640.aspx). Se si tenta di aggiornare la delega DNS e si verifichi un errore, vedere [opzioni DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. Nel **Opzioni controller di dominio** pagina (visualizzata solo se si installa un controller di dominio), specificare il nome di un gruppo o utente che sarà gestire tale controller di dominio, aggiungere o rimuovere gli account da gruppi di replica password consentita o negata, e quindi fare clic su account **Avanti**.  
  
    For more information, see [Password Replication Policy](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. Nel **opzioni aggiuntive** pagina, scegliere una delle opzioni seguenti:  
  
    -   Se si sta creando un nuovo dominio, digitare un nuovo nome NetBIOS o verificare il nome NetBIOS predefinito del dominio e quindi fare clic su **Avanti**.  
  
    -   Se si aggiunge un controller di dominio a un dominio esistente, selezionare il controller di dominio che si desidera replicare i dati di installazione di Active Directory (o consentire alla procedura guidata selezionare qualsiasi controller di dominio). Se si installa da supporto, fare clic su **installazione da supporto** digitare e controllare il percorso del file di installazione di origine e quindi fare clic su **Avanti**.  
  
        Non è possibile utilizzare Installazione da supporto per installare il primo controller di dominio in un dominio. IFM non funziona tra versioni diverse del sistema operativo. In altre parole, per poter installare un controller di dominio che esegue Windows Server 2012 tramite IFM, è necessario creare il supporto di backup in un controller di dominio Windows Server 2012. For more information about IFM, see [Installing an Additional Domain Controller by Using IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. Nel **percorsi** pagina, digitare i percorsi per il database di Active Directory, file di log e della cartella SYSVOL (o accettare i percorsi predefiniti) e fare clic su **Avanti**.  
  
    > [!IMPORTANT]  
    > Non archiviare il database di Active Directory, i file di registro o la cartella SYSVOL su un volume di dati formattato con Resilient File System (ReFS).  
  
16. Nel **opzioni di preparazione** pagina, digitare le credenziali sufficienti eseguire adprep. Per ulteriori informazioni, vedere [credenziali necessarie per eseguire Adprep.exe e installare servizi di dominio Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. Nel **verifica opzioni** pagina confermare le selezioni, fare clic su **visualizzare script** se si desidera esportare le impostazioni in uno script di Windows PowerShell, quindi fare clic su **Avanti**.  
  
18. Nel **controllo dei prerequisiti** pagina, verificare che la convalida dei prerequisiti completata e quindi fare clic su **installare**.  
  
19. Nel **risultati** verificare che il server è stato configurato correttamente come controller di dominio. Il server verrà riavviato automaticamente per completare l'installazione di servizi di dominio Active Directory.  
  
## <a name="BKMK_UIStaged"></a>Installazione gestita controller di dominio tramite l'interfaccia utente grafica  
Un'installazione di sola lettura con installazione di appoggio consente di creare un RODC in due fasi. Nella prima fase, un membro del gruppo Domain Admins crea un account di controller di dominio. Nella seconda fase, un server viene associato all'account del controller. La seconda fase può essere completata da un membro del gruppo Domain Admins o un gruppo o utente di dominio delegato.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Per creare un account di controller di dominio utilizzando gli strumenti di gestione di Active Directory  
  
1.  È possibile creare l'account di controller di dominio utilizzando il centro di amministrazione di Active Directory o Active Directory Users e computer.  
  
    1.  Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **centro di amministrazione di Active Directory**.  
  
    2.  Nel riquadro di spostamento (riquadro a sinistra), fare clic sul nome del dominio.  
  
    3.  Nell'elenco di gestione (riquadro centrale), fare clic su di **i controller di dominio** unità Organizzativa.  
  
    4.  Nel riquadro attività (riquadro a destra) fare clic su **pre-creare un account di controller di dominio di sola lettura**.  
  
    - O -  
  
    1.  Fare clic su **Start**, fare clic su **strumenti di amministrazione**, quindi fare clic su **Active Directory Users and Computers**.  
  
    2.  Qualsiasi pulsante destro del mouse il **i controller di dominio** unità organizzativa (OU) o fare clic su di **i controller di dominio** unità Organizzativa e quindi fare clic su **azione**.  
  
    3.  Fare clic su **account Controller di dominio di sola lettura creazione preliminare**.  
  
2.  Nel **iniziale per l'installazione guidata servizi di dominio Active Directory** pagina, se si desidera modificare l'impostazione predefinita, le Password dei criteri di replica, selezionare **utilizza installazione in modalità avanzata**, quindi fare clic su **Avanti**.  
  
3.  Nel **credenziali di rete** nella pagina **specificare le credenziali dell'account da utilizzare per eseguire l'installazione**, fare clic su **le credenziali di accesso corrente** oppure fare clic su **credenziali alternative**e quindi fare clic su **impostare**. Nel **la protezione di Windows** finestra di dialogo immettere il nome utente e la password per un account che è possibile installare il controller di dominio aggiuntivo. Per installare un controller di dominio, è necessario essere un membro del gruppo Enterprise Admins o del gruppo Domain Admins. Dopo aver immesso le credenziali, fare clic su **Avanti**.  
  
4.  Nel **specificare il nome del Computer** , digitare il nome del computer del server che sarà il controller di dominio.  
  
5.  Nel **selezionare un sito** pagina, selezionare un sito dall'elenco oppure selezionare l'opzione per installare il controller di dominio nel sito che corrisponde all'indirizzo IP del computer in cui si esegue la procedura guidata e quindi fare clic su **Avanti**.  
  
6.  Nel **opzioni Controller di dominio aggiuntivo** pagina, effettuare le selezioni seguenti e quindi fare clic su **Avanti**:  
  
    -   **Server DNS**: questa opzione è selezionata per impostazione predefinita, in modo che il controller di dominio possa operare come un server di sistema DNS (Domain Name). Se non vuoi il controller di dominio a un server DNS, deselezionare questa opzione. Tuttavia, se non si installa il ruolo server DNS in tale controller di dominio e il controller di dominio è l'unico controller di dominio nella succursale, gli utenti della succursale non saranno in grado di eseguire la risoluzione dei nomi quando la rete wide area network (WAN) al sito hub è offline.  
  
    -   **Catalogo globale**: questa opzione è selezionata per impostazione predefinita. Aggiunge il catalogo globale, le partizioni di directory di sola lettura al controller di dominio, e consente la funzionalità di ricerca catalogo globale. Se non vuoi il controller di dominio a un server di catalogo globale, deselezionare questa opzione. Tuttavia, se non si installa un server di catalogo globale nella succursale o Abilita caching appartenenza a gruppo universale per il sito che include tale controller di dominio, gli utenti della succursale non saranno in grado di accedere al dominio quando la WAN al sito hub è offline.  
  
    -   **Controller di dominio di sola lettura**. Quando si crea un account di controller, questa opzione è selezionata per impostazione predefinita e non è possibile deselezionarla.  
  
7.  Se si è selezionato il **utilizza installazione in modalità avanzata** casella di controllo di **iniziale** pagina il **specificare i criteri di replica Password** verrà visualizzata la pagina. Per impostazione predefinita, non le password degli account vengono replicati nel RODC e gli account di protezione (ad esempio i membri del gruppo Domain Admins) sono in modo esplicito negati le proprie password replicati nel RODC.  
  
    Per aggiungere altri account ai criteri, fare clic su **Aggiungi**, quindi fare clic su **Consenti password per l'account replicare il RODC** oppure fare clic su **negare le password per l'account da replicare il RODC** e quindi selezionare l'account.  
  
    Al termine (o accettare l'impostazione predefinita), fare clic su **Avanti**.  
  
8.  Nel **delega dell'installazione del controller e l'amministrazione** , digitare il nome dell'utente o gruppo che assocerà il server per l'account che si sta creando. È possibile digitare il nome dell'entità di sicurezza solo una.  
  
    Per cercare la directory per un utente o gruppo specifico, fare clic su **impostare**. In **Seleziona utente o gruppo**, digitare il nome dell'utente o gruppo. È consigliabile delegare l'amministrazione a un gruppo e installazione di controller di dominio.  
  
    Questo utente o gruppo anche avrà diritti amministrativi locali nel RODC dopo l'installazione. Se non si specifica un utente o gruppo, solo i membri del gruppo Domain Admins o del gruppo Enterprise Admins potranno associare il server per l'account.  
  
    Al termine, fare clic su **Avanti**.  
  
9. Nel **riepilogo** pagina, rivedere le selezioni. Fare clic su **Indietro** di modificare le selezioni, se necessario.  
  
    Per salvare le impostazioni selezionate in un file di risposte che è possibile utilizzare per automatizzare le operazioni successive di dominio Active Directory, fare clic su **esportare le impostazioni**. Digitare un nome per il file di risposte, quindi fare clic su **salvare**.  
  
    Quando si è certi che siano esatta anche le selezioni, fare clic su **Avanti** per creare l'account di controller di dominio.  
  
10. Nel **completamento installazione guidata servizi di dominio Active Directory** pagina, fare clic su **fine**.  
  
Dopo aver creato un account di controller di dominio, è possibile collegare un server all'account per completare l'installazione del controller. Può essere completata questa seconda fase nella succursale in cui tale controller di dominio sarà disponibile. Il server in cui viene eseguita questa procedura non deve essere aggiunti al dominio. A partire da Windows Server 2012, utilizzare l'aggiunta guidata ruoli in Server Manager per associare un server a un account di controller di dominio.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Per associare un server a un account di controller di dominio utilizzando Server Manager  
  
1.  Accedere come amministratore locale.  
  
2.  In Server Manager, fare clic su **Aggiungi ruoli e funzionalità**.  
  
3.  Nel **prima di iniziare** pagina, fare clic su **Avanti**.  
  
4.  Nel **Selezione tipo di installazione** pagina, fare clic su **installazione basata su ruoli o basata su funzionalità** e quindi fare clic su **Avanti**.  
  
5.  Nel **server di destinazione** pagina, fare clic su **selezionare un server dal pool di server**, fare clic sul nome del server in cui si desidera installare Active Directory e quindi fare clic su **Avanti**.  
  
6.  Nel **Selezione ruoli server** pagina, fare clic su **servizi di dominio Active Directory**, fare clic su **Aggiungi funzionalità** e quindi fare clic su **Avanti**.  
  
7.  Nel **selezionare le funzionalità** pagina, selezionare le funzionalità aggiuntive che si desidera installare e fare clic su **Avanti**.  
  
8.  Nel **servizi di dominio Active Directory** pagina, esaminare le informazioni e quindi fare clic su **Avanti**.  
  
9. Nel **Conferma selezioni per l'installazione** pagina, fare clic su **installare**.  
  
10. Nel **risultati** verificare **installazione ha avuto esito positivo**e fare clic su **promuovere il server a un controller di dominio** per avviare la configurazione guidata servizi di dominio Active Directory.  
  
    > [!IMPORTANT]  
    > Se si chiude l'aggiunta guidata ruoli a questo punto senza avviare la configurazione guidata servizi di dominio Active Directory, è possibile riavviarlo facendo clic su attività in Server Manager.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. Nel **configurazione distribuzione** pagina, fare clic su **aggiungere un controller di dominio a un dominio esistente**, digitare il nome del dominio (ad esempio, emea.contoso.com) e le credenziali (ad esempio, specificare un account delegato a gestire e installare il controller di dominio), quindi fare clic su **Avanti**.  
  
12. Nel **opzioni Controller di dominio** pagina, fare clic su **Usa account di controller di dominio esistente**, digitare e confermare la password modalità ripristino servizi Directory e quindi fare clic su **Avanti**.  
  
13. Nel **opzioni aggiuntive** pagina, se si installa da supporto, fare clic su **installazione da supporto** digitare e verificare il percorso del file di origine di installazione, selezionare il controller di dominio che si desidera replicare i dati di installazione di Active Directory (o consentire alla procedura guidata selezionare qualsiasi controller di dominio) e quindi fare clic su **Avanti**.  
  
14. Nel **percorsi** pagina, digitare i percorsi per il database di Active Directory, i file di log e della cartella SYSVOL, o accettare i percorsi predefiniti e quindi fare clic su **Avanti**.  
  
15. Nel **verifica opzioni** pagina confermare le selezioni, fare clic su **Visualizza Script** esportare le impostazioni in uno script di Windows PowerShell, quindi fare clic su **Avanti**.  
  
16. Nel **controllo dei prerequisiti** pagina, verificare che la convalida dei prerequisiti completata e quindi fare clic su **installare**.  
  
    Per completare l'installazione di servizi di dominio Active Directory, il server verrà riavviato automaticamente.  
  
## <a name="see-also"></a>Vedere anche  
[Risoluzione dei problemi di distribuzione del Controller di dominio](Troubleshooting-Domain-Controller-Deployment.md)  
[Installare una nuova foresta di Active Directory di Windows Server 2012 e 40 #; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Installare un nuovo Windows Server 2012 Active Directory figlio o dominio albero & #40; Livello 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Installare un Controller di dominio di Replica Windows Server 2012 in un dominio esistente e 40 #; Livello 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



